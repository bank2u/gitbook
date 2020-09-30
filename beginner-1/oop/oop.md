---
description: "\U0001F914 เวลาเขาเอาหลัก Object-Oriented Programming มาใช้จริงๆมันเป็นยังไงนะ ?"
---

# 📝 ตัวอย่าง OOP

หลังจากที่เราได้เห็นหัวใจหลักของ OOP ไปเรียบร้อยแล้ว ดังนั้นในรอบนี้เราจะมาดูกันว่าเวลาที่ไปเจอโจทย์เราจะนำ OOP มาใช้แก้ปัญหาของเราได้ยังไง

{% hint style="info" %}
**แนะนำให้อ่าน**  
หัวใจหลักของ OOP มีทั้งหมด 4 อย่างคือ [Abstraction](https://saladpuk.gitbook.io/learn/beginner-1/oop/abstraction), [Encapsulation](https://saladpuk.gitbook.io/learn/beginner-1/oop/encapsulation), [Inheritance ](https://saladpuk.gitbook.io/learn/beginner-1/oop/inheritance)และ [Polymorphism ](https://saladpuk.gitbook.io/learn/beginner-1/oop/polymorphism)ถ้าสนใจอยากทบทวนเรื่องไหรก็จิ้มไปอ่านที่ชื่อมันได้เลย หรือจะไปดูจากเมนูด้านซ้ายมือก็ได้
{% endhint %}

## 🧐 โจทย์ 01

มีบริษัทมาจ้างเขียน **เกมออนไลน์** ที่มีแค่ตัวละครเดียวคือ **เด็กฝึกหัด \(Novice\)** ซึ่งมันจะมี พลังชีวิต, พลังโจมตี, ประสบการณ์ที่จะใช้ในการอัพเลเวล และสามารถ เดิน, นั่ง, โจมตี และตายได้ ประมาณนี้ เราจะเขียนออกมายังไงดี ?

## 🧒 แก้โจทย์

ในการคิดแบบ OOP เราจะต้องแปลง **ปัญหา** ให้มาอยู่ในรูปแบบของ Model เสียก่อน โดยใช้หลักการของ [Abstraction ](https://saladpuk.gitbook.io/learn/beginner-1/oop/abstraction)ดังนั้นเพื่อให้เข้าใจตรงกันผมกำลังจะสร้าง Model เพื่อใช้ในการเขียนโค้ด โดยมันจะต้องมี **Properties** และ **Behaviors** ต่างๆประมาณนี้

![](../../.gitbook/assets/image%20%28634%29.png)

ดังนั้นผมก็จะสร้าง Model ออกมา ซึ่งมีหน้าตาประมาณนี้

```csharp
public class Novice
{
    public int HP { get; set; }
    public int Exp { get; set; }
    public int Atk { get; set; }
    public int Level { get; set; }
    public string Status { get; set; }

    public void Walk() { }
    public void Sit() { }
    public void Attack() { }
    public void Dead() { }
}
```

มันก็ใช้งานได้แต่ใครอยากจะแก้พลังชีวิต พลังโจมตี หรืออะไรก็ตามในกลุ่มของ Properties ก็แก้ได้เลย ดังนั้นเราเลยต้องมาคิดในมุมของ [Encapsulation](https://saladpuk.gitbook.io/learn/beginner-1/oop/encapsulation) ด้วย เช่น

* ถ้าพลังชีวิตถูกลดลงมันจะต้องห้ามต่ำกว่า 0 และถ้าพลังชีวิตหมดก็แสดงว่าตัวละครจะต้องตาย
* ถ้าได้รับค่าประสบการณ์เกิน 100 ให้ตัวละครเลเวลอัพได้เลย
* พวก Properties ต่างๆต้องไม่ถูกแก้ได้มั่วซั่ว

ซึ่งแทนที่จะให้คนที่เรียกใช้เป็นคนคอยกำหนดค่าเงื่อนไขพวกนี้ เราก็จะให้คลาส **Character** นี่แหละเป็นตัวดูแลรับผิดชอบความถูกต้องของเหล่านั้น

```csharp
public class Novice
{
    private int hp;
    public int HP
    {
        get => hp;
        set
        {
            var hpTemp = hp;
            hp -= value;
            if (hp <= 0)
            {
                hp = 0;
                Status = "Dead";
                Dead();
            }
            else
            {
                hp = hpTemp;
                Status = "Alive";
            }
        }
    }

    private int exp;
    public int Exp
    {
        get => exp;
        set
        {
            var expTemp = exp;
            expTemp += value;
            if (expTemp >= 100)
            {
                exp = 0;
                Level++;
            }
            else
            {
                exp = expTemp;
            }
        }
    }
    
    public int Atk { get; private set; }
    public int Level { get; private set; }
    public string Status { get; private set; } = "Alive";
    
    public Novice()
    {
        HP = 100;
        Atk = 3;
    }

    ...
}
```

จาดโค้ดด้านบนก็จะเห็นแล้วว่า ของหลายๆอย่างเราซ่อนความวุ่นวายที่ภายนอกไม่จำเป็นต้องรู้ไว้ได้หมดแล้ว โดยการใช้ Encapsulation มาช่วย ซึ่งถ้าเราออกแบบ encapsulation ออกมาได้ดีมันก็จะช่วย **เติมเต็ม** ความเป็น Abstraction ได้มากขึ้น

{% hint style="info" %}
**Abstraction + Encapsulation**  
จะซ่อนความวุ่นวายทั้งหลายไว้ เหลือแค่เพียงความเรียบง่ายในการใช้งาน เพราะหลักการคือการสร้าง **Component** ที่มันรับผิดชอบตัวเองได้นั่นเอง
{% endhint %}

## 🧐 โจทย์ 02

เกมที่เราเขียนขายดีมาก เลยทำให้เราต้องเพิ่มตัวละครแบบอื่นๆเข้าไปบ้างนั่นคือ **นักดาป \(Swordman\)** และ **พระ \(Acolyte\)** เพื่อให้เกมมีความหลากหลายขึ้น และมีเงื่อนไขเพิ่มมาอีกนิสนุงนั่นคือ

* **นักดาป** - จะมีพลังโจมตีเริ่มต้นที่ 10 หน่วย และ สามารถใช้ ท่าโจมตีพิเศษ ได้อีกด้วย
* **พระ** - มีพลังโจมตีเริ่มต้นที่ 5 หน่วย และ สามารถใช้ การรักษาให้กับตัวเองได้ด้วย

แล้วเราจะเขียนโปรแกรมยังไงดี เพื่อให้มีตัวละครใหม่ 2 ตัวเพิ่มขึ้นมา ในขณะที่ตัว **เด็กฝึกหัด \(Novice\)** ก็ยังต้องใช้งานได้เหมือนเดิมด้วยนะ

## 🧒 แก้โจทย์

จากที่ว่ามาเราก็จะกลับมาออกแบบโดยใช้หลัก Abstraction เพื่อตีโจทย์ก่อนว่า **ปัญหา** ที่เรากำลังเจอมันประกอบไปด้วยอะไรบ้างนั่นเอง ดังนั้นผมก็จะได้รูปประมาณนี้

**นักดาป \(Swordman\)**

![](../../.gitbook/assets/image%20%285%29.png)

**พระ \(Acolyte\)**

![](../../.gitbook/assets/image%20%28378%29.png)

เมื่อเราเห็นข้อมูลตัวอย่างของ **นักดาป** และ **พระ** เรียบร้อยแล้วเราน่าจะเห็นว่ามันมี Properties และ Behaviors ต่างๆที่คล้ายกับ **เด็กฝึกหัด** เลยใช่ไหม ซึ่งของที่มันมีเหมือนกันนั่นคือ

```csharp
// Properties
HP
Exp
Atk
Level
Status

// Behaviors
Walk()
Sit()
Attack()
Dead()
```

แล้วทั้ง 3 ตัวก็เป็นสิ่งที่เรียกว่า ตัวละคร ที่ให้ผู้ใช้สามารถเลือกเล่นได้ ดังนั้นมันอยู่ในรูปแบบความสัมพันธ์ **IS A** อยู่แล้ว ดังนั้นในจุดนี้เราเลยสามารถใช้ Inheritance ได้เลย โดยการสร้าง Model กลางที่ชื่อว่า **Character** ตามด้านล่างเลย

```csharp
public class Character
{
    public int HP { get; set; }
    public int Exp { get; set; }
    public int Atk { get; set; }
    public int Level { get; set; }
    public string Status { get; set; }

    public void Walk() { }
    public void Sit() { }
    public void Attack() { }
    public void Dead() { }
}
```

อ่อ อย่าลืมนะว่าตัว Model กลางก็ต้องนำหลัก Abstraction + Encapsulation มาใช้ด้วยเช่นกัน ดังนั้น เราจะได้โค้ดจริงๆออกมาเป็นแบบนี้ \(อย่าลืมเปลี่ยน private เป็น protected ด้วยนะ คลาสลูกจะได้เข้าถึงตัวแปรพวกนั้นได้\)

```csharp
public class Character
{
    private int hp;
    public int HP
    {
        get => hp;
        set
        {
            var hpTemp = hp;
            hp -= value;
            if (hp <= 0)
            {
                hp = 0;
                Status = "Dead";
                Dead();
            }
            else
            {
                hp = hpTemp;
                Status = "Alive";
            }
        }
    }

    public int Atk { get; protected set; } = 3;

    private int exp;
    public int Exp
    {
        get => exp;
        set
        {
            var expTemp = exp;
            expTemp += value;
            if (expTemp >= 100)
            {
                exp = 0;
                Level++;
            }
            else
            {
                exp = expTemp;
            }
        }
    }
    public int Level { get; protected set; }
    public string Status { get; protected set; } = "Alive";
    
    public Character()
    {
        HP = 100;
    }

    public void Walk() { }
    public void Sit() { }
    public void Attack() { }
    public void Dead() { }
}
```

คราวนี้เราก็เอา Model เด็กฝึกหัด, นักดาป, พระ มาทำการต่อยอดความสามารถของ Character โดยใช้ความรู้จากเรื่อง [Inheritance ](https://saladpuk.gitbook.io/learn/beginner-1/oop/inheritance)เข้ามาใช้นั่นเอง

```csharp
public class Novice : Character
{
}

public class Swordman : Character
{
}

public class Acolyte : Character
{
}
```

สุดท้ายมันก็จะเหลือแค่ของพิเศษที่มีเฉพาะตัวของ **เด็กฝึกหัด** **นักดาป** และ **พระ** เท่านั้น ซึ่งก็คือ

* **นักดาป** - จะมีพลังโจมตีเริ่มต้นที่ 10 หน่วย และ สามารถใช้ ท่าโจมตีพิเศษ ได้อีกด้วย
* **พระ** - มีพลังโจมตีเริ่มต้นที่ 5 หน่วย และ สามารถใช้ การรักษาให้กับตัวเองได้ด้วย

![](../../.gitbook/assets/image%20%28614%29.png)

ดังนั้นเราก็จะแก้ไข Model ของทั้ง 3 อาชีพให้กลายเป็นแบบนี้

```csharp
public class Novice : Character
{
    public Novice()
    {
        Atk = 3;
    }
}

public class Swordman : Character
{
    public Swordman()
    {
        Atk = 10;
    }
    
    public void SuperAttack() { }
}

public class Acolyte : Character
{
    public Acolyte()
    {
        Atk = 5;
    }

    public void Heal() { }
}
```

เพียงเท่านี้เราก็สามารถเลือกตัวละครเป็นตัวไหนก็ได้แล้ว โดยที่แต่ละตัวละครก็มีความสามารถที่ไม่เหมือนกันอีกด้วย ซึ่งโค้ดที่มีโครงสร้างแบบนี้ก็ง่ายที่จะเพิ่มตัวละครใหม่ๆเข้าไปได้แล้ว เพราะเราก็แค่ต่อยอด Model เดิมโดยการใช้ Inheritance เข้ามาช่วยนั่นเอง

## 🧐 โจทย์ 03

ตอนนี้คนเล่มถล่มทลายเลย แถมเขาอยากให้เล่นได้พร้อมกันหลายๆคนอีกด้วย ทำให้มีของที่เพิ่มเข้ามาในส่วนที่เรารับผิดชอบคือ

* **พระ** - สามารถรักษาให้กับตัวเองและ**ผู้เล่นคน**อื่นได้ด้วย

แล้วเราจะแก้ไขโค้ดเรายังไงให้รองรับความต้องการใหม่อันนี้ ?

## 🧒 แก้โจทย์

ในตอนนี้โจทย์ของเราอยู่ที่เมธอด Heal ของคลาส Acolyte ตามโค้ดด้านล่าง

```csharp
public class Acolyte : Character
{
    public void Heal() { }
}
```

ปัญหาของมันคือ มันรักษาได้เฉพาะตัวเอง ดังนั้นถ้าเราอยากให้รักษาคนอื่นได้ เราก็ต้องส่ง parameter บางอย่างเข้าไปให้มันนั่นเอง แต่ว่าเราควรจะส่ง parameter อะไรเข้าไปดีล่ะ

```csharp
public void Heal( ??? ) { }
```

จากตรงนี้ผมขอเขียนแผนภาพ [UML ](https://saladpuk.gitbook.io/learn/basic/uml)ก่อนละกัน จะได้เข้าใจภาพรวมทั้งหมดในตอนนี้ ซึ่งจะได้ออกมาตามรูปด้านล่าง

![](../../.gitbook/assets/image%20%28351%29.png)

จากรูปด้านบนจะเห็นว่า ถ้าเราใช้ Novice, Swordman หรือ Acolyte เข้าไปเป็น parameter นั่นหมายความว่าเราจะต้องมี method แบบนี้อย่างน้อย 3 ตัว

```csharp
public void Heal(Novice target) { }
public void Heal(Swordman target) { }
public void Heal(Acolyte target) { }
```

มันก็ทำงานได้นะ แต่ถ้าในอนาคตมันมีตัวละครใหม่ๆล่ะ เราจะต้องไปเพิ่มเมธอดใหม่ทุกครั้งที่มีอาชีพใหม่เหรอ?

ดังนั้นในจุดนี้เราจะใช้ความรู้เรื่อง [Polymorphism](https://saladpuk.gitbook.io/learn/beginner-1/oop/polymorphism) เข้าช่วยแก้ปัญหา โดยการส่งคลาส **Character** เข้าไปเป็น parameter นั่นเอง เพราะ Base class สามารถทำงานกับ Sub class ได้ทุกตัวยังไงล่ะ ดังนั้นโค้ดเราก็จะออกมาเป็นแบบนี้

```csharp
public void Heal(Character target) { }
```

## 🧐 โจทย์ 04

ตอนนี้ผู้เล่นเริ่มบ่นอยากแต่งตัวละครสวยๆได้ ทางบริษัทเลยคิดว่า **ทุกตัวละครสามารถใส่หมวกบนหัวได้** และ **หมวกแต่ละแบบก็จะเพิ่มความสามารถต่างๆให้กับผู้เล่นได้**ด้วย ดังนั้นหน้าที่นี้เลยตกมาที่เรา แล้วเราจะรับมือกับเรื่องนี้ยังไงดีกันนะ ?

## 🧒 แก้โจทย์

จำง่ายๆเวลาที่มีงานเข้ามาให้ดูก่อนว่างานนั้นเป็นงานที่ **โค้ดเดิมทำได้อยู่แล้ว** หรือ **มันเป็นเรื่องใหม่** 

* **ถ้าโค้ดเดิมทำได้อยู่แล้ว** เช่น เพิ่มตัวละครตัวใหม่เข้าไป อันนี้เราก็แค่ต่อยอด Model เดิมก็จะใช้งานได้ละ
* **ถ้ามันเป็นเรื่องใหม่** เช่นกรณีนี้ ให้เรานำหลักของ Abstraction กลับมาวิเคราะห์ดูเสมอนั่นเอง

ดังนั้นในรอบนี้เราก็จะเอา Abstraction กลับมาช่วยตีโจทย์นั่นเอง โดยผมมองว่าไม่ว่าจะเป็นตัวละครตัวไหนก็ตาม ก็ควรที่จะใส่หมวกได้ และแม้แต่ตัวละครในอนาคตก็ควรจะต้องใส่หมวกได้เช่นกัน และหมวกก็ควรมีความหลากหลายด้วย ตามรูปเลย

![](../../.gitbook/assets/image%20%28679%29.png)

แล้วก็อย่าลืมมองกลับมาที่ Model เดิมที่เรามีด้วย ตามรูปด้านล่างเลย

![](../../.gitbook/assets/image%20%28351%29.png)

ซึ่งถ้าดูตามความเหมาะสมแล้ว เราควรจะเพิ่ม Property ของหมวกให้กับ Character นั่นเอง ดังนั้นเราลองคิดนิสนุงว่า Model หมวกที่สามารถเพิ่มสถานะให้กับผู้ใส่ได้ควรจะมีอะไรบ้าง \(ผมแอบคิดมาละประมาณนี้ละกัน\)

![](../../.gitbook/assets/image%20%28246%29.png)

นั่นก็หมายความว่าผมควรที่จะมี Model ของมันออกมาประมาณนี้

```csharp
public class Hat
{
    public string Description { get; set; }
    public int EffectOnHP { get; set; }
    public int EffectOnAttack { get; set; }
}
```

ดังนั้นเราก็จะเอา Model ตัวใหม่อันนี้ไปใส่ไว้ใน Character ตามรูปด้านล่าง เพื่อให้ตัวละครรองรับการใส่หมวกได้ \(ซึ่งบางตัวละครอาจจะไม่ใส่หมวกก็ได้นะ\)

![](../../.gitbook/assets/image%20%28648%29.png)

ดังนั้นโค้ดเราก็จะออกมาราวๆนี้

```csharp
public class Character
{
    public Hat headEquipment { get; protected set; }
    
    public void EquipHead(Hat gear) { }
    
    ...
}
```

## 🎯 บทสรุป

น่าจะพอเห็นตัวอย่างการนำหลักของ Object-Oriented Programming ไปใช้ในการเขียนโค้ดกันชัดเจนมากยิ่งขึ้นแล้วนะ ซึ่งถ้าเรามองของต่างๆให้เป็น Component แล้วล่ะก็ เราจะสามารถเพิ่มลดความสามารถต่างๆเข้าไปในโปรแกรมได้ง่ายขึ้น เพราะตัวโค้ดเราแต่ละส่วนมันจะเหมือนกับ **เลโก้** นั่นเอง ซึ่งมันจะช่วยทำให้เราเปลี่ยนชิ้นส่วนที่ไม่ต้องการ หรือ อยากให้มันมีการทำงานแบบอื่นๆก็สามารถทำได้ง่ายๆเลยนั่นเอง

![](../../.gitbook/assets/image%20%28496%29.png)

{% hint style="danger" %}
**คำเตือน**  
ในการออกแบบโดยใช้แนวคิดของ OOP ซึ่งมีหัวใจหลัก 4 ตัวนั้น **ไม่ได้หมายความว่าเราจะต้องพยายามเอาหัวใจมันมาใช้งานทุกตัวนะ** เพราะไม่อย่างนั้นเราจะทำให้ของมันยากขึ้นโดยใช่เหตุ ซึ่งถ้าใช้แค่หลักของ Abstraction เพื่อสร้าง Model แล้วทำงานได้หมด ก็ใช้แค่นั้นก็เพียงพอแล้วครับ
{% endhint %}

