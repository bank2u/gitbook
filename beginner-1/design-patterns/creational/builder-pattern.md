---
description: แนวคิดในการรับมือกับ object ที่มีขั้นตอนการสร้างซับซ้อน
---

# 🏗️ Builder Pattern

เจ้าตัวนี้ผมขอตั้งชื่อเป็นภาษาไทยว่า **ผู้สร้าง** และมันอยู่ในกลุ่มของ 🤰 [**Creational Patterns**](https://saladpuk.gitbook.io/learn/beginner-1/design-patterns/creational) ซึ่งเจ้าตัวนี้จะมาช่วยแก้ปัญหาเมื่อ **เรามีคลาสที่มีขั้นตอนในการสร้างซับซ้อน** และ **การสร้าง object หลายๆอย่างที่มีขั้นตอนในการสร้างคล้ายๆกัน** \(อย่าพึ่งเสียเวลาอ่านตัวหนาพวกนั้นเลย\) ไปดูโจทย์ของเรากันเลยดีกว่าจะได้เข้าใจได้เร็วขึ้น

{% hint style="info" %}
**แนะนำให้อ่าน**  
บทความนี้เป็นส่วนหนึ่งของมหากาพย์ Design Patterns ที่จะมาเป็น guideline ในการแก้ปัญหาในการออกแบบซอฟต์แวร์โปรเจค หากใครสนใจอยากเข้าใจตั้งแต่ต้นว่ามันคืออะไร และเจ้า patterns ทั้ง 23 ตัวมีอะไรบ้าง ก็สามารถจิ้มตรงนี้เพื่อไปอ่านบทความหลักได้เบยครัช [👦 **Design Patterns**](https://saladpuk.gitbook.io/learn/beginner-1/design-patterns)
{% endhint %}

{% hint style="warning" %}
**หมายเหตุ**  
เนื้อหาของบทความนี้จะเน้นให้เข้าใจหลักการทำงานของ Design Patterns แต่ละตัว โดยใช้เกม Ragnarok เป็นการอธิบาย ซึ่งบางอย่างอาจจะไม่ตรงกับตัวเกมจริงๆนะขอรับ _Gravity อย่ามาจับผมนะผมโดนแมวน้ำครอบงำ + รู้เท่าไม่ถึงการ + ผมเป็นคนดี + ผมมีลูกมีเมียมีสามีที่ต้องดูแล_ 😭
{% endhint %}

## 🧐 โจทย์

สมมุติว่าเกมของเรามี **ระบบสร้างอาวุธ** โดยอาวุธที่สร้างออกมานั้นมี 2 ประเภทคือ

* **อาวุธธรรมดา** - อาวุธประเภทนี้จะ **มีช่องว่าง 2 ช่อง** ให้ใส่การ์ดเพิ่มความสามารถให้กับอาวุธได้
* **อาวุธธาตุ**- อาวุธประเภทนี้จะ **เลือกธาตุได้** เช่น ดิน น้ำ ลม ไฟ 
* **อาวุธธรรมดา** - จะต้องไม่มีธาตุ
* **อาวุธธาตุ**- จะต้องไม่มีช่องว่างสำหรับใส่การ์ด

และอาวุธที่ถูกสร้างขึ้นมานั้นจะต้องถูก **สลักชื่อคนสร้าง** เอาไว้ในตัวอาวุธด้วย ตามรูปด้านล่างเบย

![](../../../.gitbook/assets/image%20%28277%29.png)

นอกจากนี้ยังมีเรื่องของ **ประเภทอาวุธ** ด้วย ซึ่งอาวุธแต่ละประเภทก็จะมีลักษณะที่แตกต่างกัน เช่น

* **ดาป** - เป็นการ**โจมตีทางกายภาพ** และ ใช้ได้ใน**ระยะประชิด**เท่านั้น
* **คฑาเวทมนต์** - เป็นการ**โจมตีทางเวทมนต์** และ ใช้ได้ใน**ระยะใกล้**
* **ธนู** - เป็นการ**โจมตีทางกายภาพ** และ ใช้ได้ใน**ระยะไกล**

และอย่างลืมว่าอาวุธพวกนี้จะต้องสามารถถูกสร้างโดยใส่ธาตุได้นะ **ยกเว้นอาวุธพวกคฑาเวทมนต์** จะไม่สามารถใส่ธาตุได้ ตามรูปเลย

![](../../../.gitbook/assets/image%20%28841%29.png)

แล้วเราจะเขียนโค้ดออกมายังไงดี เพื่อให้มันสามารถตอบโจทย์ความต้องการแบบนี้ ได้อย่างไม่มีปัญหากันนะ ?

## 🧒 แก้โจทย์ครั้งที่ 1

จากโจทย์ที่ได้มาเราก็ออกแบบ **คลาสอาวุธ** ของเรากันได้ประมาณนี้

![](../../../.gitbook/assets/image%20%28116%29.png)

สุดท้ายเราก็ไปเขียนเมธอดที่ใช้ในการสร้างอาวุธออกมา โดยการโยน parameters ต่างๆเพื่อใช้ในการสร้างอาวุธ พร้อมกับเขียนเงื่อนไขต่างๆลงไป เช่น ถ้าเป็นอาวุธธาตุจะต้องไม่มีช่องใส่การ์ด บลาๆ ดังนั้นเราก็จะได้เมธอดออกมาหน้าตาประมาณนี้

> หากดูโค้ดตัวอย่างด้านล่างไม่รู้เรื่อง ให้กดที่ tab ข้างๆที่ชื่อว่า **ตัวอย่างที่ 2 \(แบบง่าย\)** ละกันนะ

{% tabs %}
{% tab title="ตัวอย่างที่ 1" %}
```csharp
public Weapon CreateNewWeapon(string name, 
    string element, 
    string creatorName, 
    bool isPhysical, 
    string type,
    int range)
{
    const int DefaultAttack = 10;
    const int NoneElementSlots = 2;
    var isElementWeapon = !string.IsNullOrWhiteSpace(element);
    var weapon = new Weapon
    {
        Name = name,
        CreatorName = creatorName,
        Element = isElementWeapon ? element : string.Empty,
        MaximumSlots = isElementWeapon ? 0 : NoneElementSlots,
        PhysicalAttack = isPhysical ? DefaultAttack : 0,
        MagicalAttack = isPhysical ? 0 : DefaultAttack,
        WeaponType = type,
        MinimumAttackRange = range,
    };
    var isStaff = type == "staff";
    weapon.Element = isStaff ? string.Empty : weapon.Element;
    weapon.MaximumSlots = isStaff ? NoneElementSlots : weapon.MaximumSlots;
    return weapon;
}
```
{% endtab %}

{% tab title="ตัวอย่างที่ 2 \(แบบง่าย\)" %}
```csharp
public Weapon CreateNewWeapon(string name,
    string element,
    string creatorName,
    bool isPhysical,
    string type,
    int range)
{
    const int DefaultAttack = 10;
    const int NoneElementSlots = 2;
    var weapon = new Weapon
    {
        Name = name,
        CreatorName = creatorName,
        WeaponType = type,
        MinimumAttackRange = range,
    };

    var isElementWeapon = !string.IsNullOrWhiteSpace(element);
    var isStaff = type == "staff";
    if (isElementWeapon && !isStaff)
    {
        weapon.Element = element;
    }
    else
    {
        weapon.Element = string.Empty;
        weapon.MaximumSlots = NoneElementSlots ;
    }

    if (isPhysical)
    {
        weapon.PhysicalAttack = DefaultAttack;
    }
    else
    {
        weapon.MagicalAttack = DefaultAttack;
    }

    return weapon;
}
```
{% endtab %}
{% endtabs %}

ซึ่งจากโค้ดด้านบนก็ไม่ได้ทำอะไรผิดนะ แก้โจทย์ที่ของเราได้ตามปรกติเลย แต่มันจะเกิดอะไรขึ้นถ้า ประเภทของอาวุธมีเพิ่มขึ้นเรื่อยๆ ... **เราก็ต้องไปไล่แก้โค้ดใหม่ทุกครั้งอะดิ** **แถมยิ่งแก้ยิ่งพันกันเป็นสปาเก็ตตี้ขึ้นไปเรื่อย** 😨

เพราะโค้ดของเรามันมีหลายอย่างที่ไม่ตรงหลักในการออกแบบที่ดี เช่น **SRP**, **OCP** ยังไงล่ะ

{% hint style="danger" %}
**Single-Responsibility Principle \(SRP\)**  
การออกแบบที่ละเมิดหลักในการออกแบบนี้จะทำให้ เวลาที่ Requirement เปลี่ยนมาทีนึง มันก็จมีโอกาสสูงมากที่การเปลี่ยนนั้นมันจะไปกระทบเจ้าสิ่งนั้น ทำให้เราต้องแก้ไขมัน ซึ่งผองเพื่อนอื่นๆที่มันดูแลอยู่นั้นไม่ได้เกี่ยวข้องเลยก็มีผลกระทบด้วยนั่นเอง ส่วนใครที่ลืมหรืออยากทบทวนเรื่อง SRP สามารถเข้าไปอ่านได้จากลิงค์นี้เบย [**Single-Responsibility Principle**](https://saladpuk.gitbook.io/learn/basic/solid/srp)
{% endhint %}

{% hint style="danger" %}
**Open & Close Principle \(OCP\)**  
การออกแบบที่ละเมิดหลักในการออกแบบนี้จะทำให้ทุกครั้งที่มีของใหม่ๆถูกเพิ่มเข้าไปปุ๊ป เราก็ต้องไปแก้โค้ดเดิมเสมอ สำหรับใครที่ลืมหลักในการออกแบบเรื่องนี้ไปแล้วให้กดอ่านได้จากตรงนี้ [**Open & Close Principle**](https://saladpuk.gitbook.io/learn/basic/solid/ocp)
{% endhint %}

อาวล่ะเมื่อมาถึงตรงนี้ ผมขอแปลงโค้ดทั้งหมดของเราให้ไปเป็นภาพประมาณนี้ละกันจะได้เข้าใจกันได้ง่ายๆ แล้วเดี๋ยวเราลองไปวิเคราะห์ปัญหา แล้วค่อยๆแก้ปัญหากันทีละจุดกันดีกว่า

![](../../../.gitbook/assets/image%20%28893%29.png)

## 🧒 แก้โจทย์ครั้งที่ 2

### 🔥 วิเคราะห์ปัญหา

จากปัญหาที่ว่ามาเราจะพบว่า ทุกครั้งที่มีอาวุธประเภทใหม่ๆ หรือ เงื่อนไขใหม่ๆ เข้ามามันจะทำให้ เราต้องไปแก้เจ้าเมธอด **CreateNewWeapon** เสมอ เลยทำให้ในอนาคตมันจะ **บวมฉ่ำ** อย่างไม่ต้องสงสัยเลย เพราะแค่ดูจากรูปก็เห็นแล้วว่ามันรับ parameters ยั้วเยี๊ยไปหมด ดังนั้นเงื่อนไขภายในก็น่าจะเยอะไม่แพ้กัน เมื่อมีการเปลี่ยนแปลงแก้ไขเกิดขึ้น มันก็จะนำปัญหามาหาเรายังไงล่ะ เราเรียกการบวมแบบนี้ว่า **บวมออกข้าง**

![&#xE1A;&#xE27;&#xE21;&#xE2D;&#xE2D;&#xE01;&#xE02;&#xE49;&#xE32;&#xE07;](../../../.gitbook/assets/image%20%28553%29.png)

หรือต่อให้เราเอา Parameters ทั้งหลายไปยัดรวมไว้ภายในคลาสเดียวกันก็ตาม มันก็จะเป็นการย้ายที่บวมไปอยู่ที่เจ้าคลาสใหม่นั่นเอง

![&#xE22;&#xE49;&#xE32;&#xE22;&#xE17;&#xE35;&#xE48;&#xE1A;&#xE27;&#xE21;](../../../.gitbook/assets/image%20%2893%29.png)

และต่อให้เราแยกเมธอดเพื่อใช้ในการสร้างอาวุธมันก็ยังจะบวมออกเหมือนเดิมแต่เปลี่ยนเป็น **บวมในแนวดิ่งแทน**

![&#xE1A;&#xE27;&#xE21;&#xE41;&#xE19;&#xE27;&#xE14;&#xE34;&#xE48;&#xE07;](../../../.gitbook/assets/image%20%281005%29.png)

ส่วนหนึ่งในสาเหตุการบวมนั้นเกิดจาก **มันรับผิดชอบหลายเรื่อง** ยังไงล่ะ ดังนั้นเดี๋ยวเรามาแก้ปัญหาเรื่องนี้กันด้วยหลักในการออกแบบด้วย **SRP** กันเลย

### 🔥 แก้ไขปัญหา

จากที่อธิบายไปปัญหาหลักของเราคือ **ของหลายๆอย่างมันพันกันอยู่ภายในเมธอดเดียว** ดังนั้นเราจะต้องค่อยๆ แก้ปมมันทีละเรื่องตามด้านล่างนี้เบย

#### **หน้าที่ในการสร้างอาวุธ**

เราไม่ควรจะรวมไว้ในเมธอดเดียว แต่มันควรจะแยกการสร้างอาวุธออกเป็นอาวุธแต่ละประเภทนั่นเอง เช่น ตัวสร้างดาป ตัวสร้างคฑาเวทมนต์ ตัวสร้างธนู ตามรูปด้านล่าง

![](../../../.gitbook/assets/image%20%28795%29.png)

#### จัดการความซับซ้อนของการสร้าง

คราวนี้เจ้าพวก parameters ที่จะต้องส่งเข้าไปให้กับเมธอดนั้น **มันค่อนข้างวุ่นวายมาก** เพราะถ้าในอนาคตมี parameters ต่างๆเพิ่มเข้ามาอีก มันก็จะทำให้มันบวมอย่างเลี่ยงไม่ได้นั่นเอง และ บางที parameter ที่เพิ่มเข้ามาใหม่ มันก็อาจจะไม่เกี่ยวข้องกับการสร้างของแบบเดิมเลยด้วยซ้ำ แต่เราก็ต้องส่งเข้าไป เพราะไม่อย่างนั้นของเดิมจะทำงานไม่ได้ เช่น มีเงื่อนไขเพิ่มว่าดาปธาตุนั้นถ้าถูกสร้างตอนเที่ยงคืนมันจะเพิ่มพลังโจมตี x 2 ไรงี้ โค้ดเราก็จะประมาณนี้

```csharp
public Weapon CreateWeapon(string name,
    string element,
    string creatorName,
    bool isPhysical,
    string type,
    int range,
    DateTime? currentTime)
{ ... }
```

ซึ่งเจ้าโค้ดด้านบนมันจะ**บังคับให้เราใส่เวลาลงไปเสมอ** แม้ว่าจะเป็นการสร้างดาปธรรมดาก็ตาม เลยทำให้โค้ดตอนเรียกใช้เมธอดนี้มันจะดูตลก ถ้ามีของพวกนี้อยู่เยอะๆนั่นเอง ตามโค้ดด้านล่างนั่นเอง

```csharp
var weapon = CreateWeapon("a", "", true, "sword", 1, null);
```

ซึ่งปัญหาเรื่องนี้มันเกิดจาก **เราส่งข้อมูลทุกอย่างที่เกี่ยวข้องกับการสร้างอาวุธไปทั้งหมดภายในครั้งเดียว** นั่นเอง แต่ถ้าเรามองมันใน**มุมของการสร้างสิ่งของ** เราก็จะพบว่า เราจะกำหนดค่าต่างๆให้ตอนที่มันจำเป็นจริงๆเท่านั้น ไม่ใช่โยนเข้าไปหมดภายในครั้งเดียว ดังนั้นเราจะแยกของต่างๆออกมาให้ง่ายต่อการต่อเติมเป็น เมธอดย่อยๆ เช่น เมธอดกำหนดชื่อ, เมธอดกำหนดระยะการโจมตี บลาๆ ตามรูปด้านล่าง

![&#xE02;&#xE2D;&#xE17;&#xE33;&#xE41;&#xE04;&#xE48;&#xE14;&#xE32;&#xE1B;&#xE43;&#xE2B;&#xE49;&#xE14;&#xE39;&#xE2D;&#xE31;&#xE19;&#xE40;&#xE14;&#xE35;&#xE22;&#xE27;&#xE19;&#xE30; &#xE44;&#xE21;&#xE48;&#xE07;&#xE31;&#xE49;&#xE19;&#xE23;&#xE39;&#xE1B;&#xE08;&#xE30;&#xE23;&#xE01;&#xE21;&#xE32;&#xE01;](../../../.gitbook/assets/image%20%28975%29.png)

ซึ่งจากโครงสร้างใหม่ด้านบน เมื่อเราอยากจะกำหนดอะไรเราก็แค่เรียกใช้เมธอดพวกนั้นในการกำหนดค่าต่างๆก็พอ เช่น อยากสร้างดาปธาตุไฟ ก็จะเขียนออกมาราวๆนี้

```csharp
var swordMaker = new SwordMaker();
swordMaker.SetName("Katana");
swordMaker.SetElement("Fire");
swordMaker.SetCreatorName("Saladpuk");
swordMaker.SetAttackType(true);
swordMaker.SetType("Sword");
swordMaker.SetAttackRange(1);
```

จากโค้ดด้านบนจะเห็นว่าจะเพิ่มเติมอะไรซักที มันจะเขียนค่อนข้างยาว ดังนั้นเราเลยแก้ไขให้มันสามารถ **เชื่อมคำสั่ง \(Chain\)** กันได้นั่นเอง โดยการแก้ return type ให้มันส่ง object ตัวมันเองกลับมา ตามรูปเรย

![&#xE15;&#xE31;&#xE27;&#xE17;&#xE35;&#xE48;&#xE02;&#xE35;&#xE14;&#xE40;&#xE2A;&#xE49;&#xE19;&#xE43;&#xE15;&#xE49;&#xE2A;&#xE35;&#xE41;&#xE14;&#xE07;&#xE04;&#xE37;&#xE2D;&#xE2A;&#xE34;&#xE48;&#xE07;&#xE17;&#xE35;&#xE48;&#xE41;&#xE01;&#xE49;&#xE44;&#xE02;&#xE44;&#xE1B;&#xE19;&#xE30;](../../../.gitbook/assets/image%20%28444%29.png)

ดังนั้นโค้ดเวลาที่เราเรียกใช้มันก็จะเป็นแบบนี้

```csharp
var swordMaker = new SwordMaker();
swordMaker
    .SetName("Katana")
    .SetElement("Fire")
    .SetCreatorName("Saladpuk")
    .SetAttackType(true)
    .SetType("Sword")
    .SetAttackRange(1);
```

ซึ่งเจ้าคลาสตระกูล **Maker จะต้องทำหน้าที่รับผิดชอบในการสร้างอาวุธ**ให้เรานั่นเอง และของบางอย่างเราก็อาจจะไม่ต้องกำหนดค่าให้มันก็ได้ เช่น SwordMaker ก็จะรู้ว่าตัวเองนั้นจะต้องกำหนดพวก รูปแบบการโจมตี ประเภทของอาวุธ และ ระยะการโจมตีเป็นเท่าไหร่นั่นเอง และ มันก็ควรจะ**มีเมธอดที่สั่งให้มันสร้างอาวุธออกมา**ให้เราได้ด้วยนั่นเอง ตามนี้เบย

![](../../../.gitbook/assets/image%20%2856%29.png)

ส่วนโค้ดตอนที่เราต้องการจะสร้างดาปธาตุไฟก็จะออกมาเป็นแบบนี้

```csharp
var fireSword = new SwordMaker()
    .SetName("Katana")
    .SetElement("Fire")
    .SetCreatorName("Saladpuk")
    .GetWeapon();
```

ส่วนโค้ดของคลาส SwordMaker ก็จะเป็นแบบนี้นั่นเอง

```csharp
public class SwordMaker
{
    private int cardSlots = 2;
    private string productName;
    private string elementType;
    private string creatorName;

    public SwordMaker SetName(string name)
    {
        productName = name;
        return this;
    }

    public SwordMaker SetElement(string element)
    {
        cardSlots = 0;
        elementType = element;
        return this;
    }

    public SwordMaker SetCreatorName(string name)
    {
        creatorName = name;
        return this;
    }

    public Weapon GetWeapon()
    {
        return new Weapon
        {
            Name = productName,
            Element = elementType,
            CreatorName = creatorName,
            PhysicalAttack = 10,
            MaximumSlots = cardSlots,
            MinimumAttackRange = 1,
            WeaponType = "sword"
        };
    }
}
```

จากที่ว่าไปทั้งหมดเราก็จะสามารถสร้าง interface กลางที่ใช้ในการสร้างอาวุธต่างๆได้ออกมาราวๆนี้

![](../../../.gitbook/assets/image%20%28675%29.png)

ซึ่งจากที่ทำมาทั้งหมด เราก็จะได้สิ่งที่เรียกว่า **Builder** แล้วนั่นเองครัช โดยเจ้าคลาส Builder นั้นมีหน้าที่ในการช่วยให้การสร้าง object ต่างทำได้ง่ายขึ้น โดยที่ Builder แต่ละตัวก็จะต้องรู้เงื่อนไข ข้อจำกัด หรือกฎต่างๆที่ตัวมันเองต้องรับผิดชอบด้วย เลยทำให้เรามี builder ในการสร้างของแตกต่างกันได้หลายๆแบบนั่นเอง \(ลองไปดูโค้ดจริงๆที่ด้านล่างสุดได้นะ ว่า builder มันรับผิดชอบเงื่อนไขของมันต่างกันยังไง\)

### 🤔 มันใช้งานยากไปหน่อยไหม ?

แม้ว่าเราจะทำให้โค้ดเราเรียกใช้งานได้ง่ายขึ้นเยอะเมื่อเทียบกับโค้ดแรกเริ่มของเราละ แต่ถ้าทุกครั้งที่เราต้องการสร้างดาป เราก็ต้องเขียนโค้ดแบบด้านล่างนี้เสมอ มันจะทำให้โปรแกรมของเรามีโค้ดซ้ำกันเต็มไปหมดเลยเหรอ ? แบบนี้ก็ขัดกับหลักในการออกแบบเรื่อง **Don't Repeat Yourself \(DRY\)** อะดิ ??

```csharp
var fireSword = new SwordMaker()
    .SetName("Katana")
    .SetElement("Fire")
    .SetCreatorName("Saladpuk")
    .GetWeapon();
```

ดังนั้นเพื่อแก้ปัญหาของพวกนี้ มันเลยมีอีกตัวนึงที่จะมาช่วยให้เราทำงานกับ Builder ได้ง่ายขึ้น นั่นก็คือสิ่งที่เรียกว่า **Director** นั่นเอง

โดยเจ้า director จะทำหน้าที่ในการช่วยให้เราสร้างของต่างๆออกมา โดยที่เราไม่จำเป็นต้องรู้อะไรเลย แค่บอกมันว่าเราอยากได้อะไร กับ อยากได้รูปแบบไหนก็พอ เช่น ในกรณีของเราก็จะมีการสร้างอาวุธ 2 แบบนั่นคือ อาวุธธรรมดา กับ อาวุธธาตุ เลยทำให้เรามี director หน้าตาประมาณนี้

![](../../../.gitbook/assets/image%20%28462%29.png)

ซึ่งเจ้า Director จะรู้ว่าถ้าเราอยากจะได้อาวุธธรรมดา หรือ อาวุธธาตุ มันจะต้องสั่งงาน Builder ให้ทำงานยังไงถึงจะได้ของที่เราต้องการกลับมาเสมอนั่นเอง

```csharp
public class WeaponDirector
{
    public Weapon CreateBasicWeapon(IWeaponMaker maker)
    {
        return maker
                .SetCreatorName("Saladpuk")
                .GetWeapon();
    }

    public Weapon CreateFireWeapon(IWeaponMaker maker)
    {
        return maker
                .SetElement("Fire")
                .SetCreatorName("Saladpuk")
                .GetWeapon();
    }

    // ขอเขียนแค่นี้นะ บทความยาวม๊วกละ
}
```

ยินดีด้วยในตอนนี้คุณได้ใช้สิ่งที่เรียกว่า **Builder Pattern** เรียบร้อยแล้ว ไม่ว่าจะรู้ตัวหรือไม่ก็ตาม เย่ๆ 👏

## 🤔 Builder คือไย ?

### 🔥 เป้าหมายในการแก้ปัญหา

* ช่วยให้เรา **สร้าง object ที่มีขั้นตอนในการสร้างที่ซับซ้อน ให้ถูกสร้างได้ง่ายๆ**
* ช่วยให้ **โค้ดขั้นตอนการสร้างที่เหมือนๆกันรวมอยู่ที่เดียวกัน** แต่สามารถสร้าง object ที่แตกต่างกันได้

### 🔥 วิธีการใช้

ตรงจุดนี้จะขออธิบายออกเป็นทีละขั้นตอนแบบนี้ละกัน คนที่พึ่งหัดออกแบบจะได้เข้าใจได้ง่ายๆนะ

เวลาที่เราอยากสร้างอะไรก็ตาม เจ้าสิ่งนั้นเราจะเรียกมันว่า **Product** ซึ่งเจ้าตัวนี้แนะนำว่าให้มันเป็น interface ได้ก็จะดีมาก

![](../../../.gitbook/assets/image%20%28660%29.png)

ส่วนการสร้าง product นั้นมันอาจจะมี options ให้เลือกจุกจิกมากมาย ดังนั้นเราก็จะทำเป็น interface กลางในการสร้างเอาไว้ซะ โดยเราจะเรียกมันว่า **Builder** และพวก options ต่างๆที่เราสามารถเพิ่มเข้าไปได้เราก็จะแปลงให้มันเป็นเมธอดต่างๆเพื่อให้เราสามารถเพิ่มลดได้นั่นเอง และก็อย่าลืมให้มันมีมีธอดในการสร้าง product จริงๆด้วยนะตามรูปเลย

![](../../../.gitbook/assets/image%20%28598%29.png)

เจ้าตัวที่จะสร้าง product ที่แท้จริง ก็จะมา implement IBuilder ต่ออีกทีนึง และมันก็จะรู้เงื่อนไข กฎต่างๆในการสร้าง product ที่มันรับผิดชอบอยู่นั่นเอง ซึ่งเราเรียกมันว่า **ConcreteBuilder** ซึ่งถ้าเรามี product หลายๆแบบ ก็จะมี ConcreteBuilder หลายๆตัวนั่นเอง ตามรูปด้านล่าง

![](../../../.gitbook/assets/image%20%2863%29.png)

สุดท้ายขั้นตอนในการสร้างที่เหมือนๆกัน เราก็จะให้มันไปอยู่กับสิ่งที่เรียกว่า **Director** นั่นเอง ซึ่งเราอาจจะมีการสร้างหลายๆแบบไว้กับ Director ก็ได้

![](../../../.gitbook/assets/image%20%28228%29.png)

ดังนั้นภาพรวมทั้งหมดของ **Builder Pattern** เลยออกมาเป็นประมาณนี้

![](../../../.gitbook/assets/image%20%28197%29.png)

ไหนลองเอาที่เราออกแบบมาเทียบกันดูดิ๊ ... เหมือนกันเปี๊ยบเบย

![](../../../.gitbook/assets/image%20%28939%29.png)

{% hint style="success" %}
**ข้อแนะนำ**  
ของทุกอย่างใน Design Patterns ทุกตัว **เราไม่จำเป็นต้องทำตาม หรือ มีครบเหมือนตามที่เขาบอกไว้ก็ได้ \(ถ้าเข้าใจ + มีเหตุผลที่ดีพอ\)** เพราะสิ่งที่ Pattern แต่ละตัวต้องการจะบอกเราคือ **แนวทาง** และเหตุผลในการออกแบบเพียงเท่านั้น ซึ่งสิ่งที่เราต้องทำต่อก็คือนำมันไปประยุกต์ให้เข้ากับปัญหาที่เราเจออยู่ให้เหมาะสมนั่นเอง
{% endhint %}

## 🤔 ทำไมต้องใช้ด้วย ?

### 🥰 **ลดการบวม**

จากปัญหาด้านบนที่เวลามีของใหม่ๆเพิ่มเข้ามามันจะทำให้คลาสของเราบวมออก ไม่บวมออกข้าง ก็ออกบวมออกในแนวดิ่ง แถม parameters ที่ส่งไปบางทีก็ไม่จำเป็นต้องส่งไปก็ได้สำหรับางการณี ก็น่าจะพอเห็นข้อดีของการใช้ Builder เพียงอย่างเดียวกันแล้วนะ เพราะมันทำให้เราปรับแต่งเพิ่มลดได้ตามใจชอบเลย

### 🥰 **ลดความซ้ำซ้อน**

จะเห็นว่าขั้นตอนในการสร้างอาวุธไม่ว่าจะเป็นแบบ อาวุธธรรมดา หรือ อาวุธธาตุ มันก็มีขั้นตอนในการสร้างที่ตายตัวในรูปแบบของมัน ดังนั้นแทนที่เราจะต้องไปเขียนโค้ดเดิมซ้ำๆ เราก็สามารถนำโค้ดพวกนั้นไปไว้ใน director เพื่อลดการเขียนโค้ดซ้ำได้แล้ว แถมโค้ดตัวนั้นยังสามารถผลิต product ที่มีขั้นตอนการสร้างแบบเดียวกันได้อีกไม่จำกัดรูปแบบเลย เพียงแค่ส่ง Builder style ที่ต้องการเข้ามานั่นเอง

{% hint style="info" %}
**หมายเหตุ**  
การใช้ Builder Pattern นั้นจริงๆมีอีกหลายเรื่องเลยที่เป็นข้อดี เช่น

* ลด Hardcode ในการสร้าง object
* ลดปัญหาเรื่อง Dependencies ต่างๆ
* ลดปัญหาเรื่องการเทส

ซึ่งผมเขียนอธิบายไว้ใน **Factory Pattern** น่าจะละเอียดพออยู่แล้ว ดังนั้นลองไปอ่านกันดูได้จากลิงค์นี้เบยครัช [**Factory Pattern ทำไมต้องใช้ด้วย ?**](https://saladpuk.gitbook.io/learn/beginner-1/design-patterns/creational/factory-method-pattern#undefined-1)
{% endhint %}

## 📝 ตัวอย่างโค้ดทั้งหมด

{% tabs %}
{% tab title="Main" %}
```csharp
static void Main(string[] args)
{
    var director = new WeaponDirector();

    var swordMaker = new SwordMaker();
    var basicSword = director.CreateBasicWeapon(swordMaker);
    var earthSword = director.CreateFireWeapon(swordMaker);
    Console.WriteLine(basicSword);
    Console.WriteLine(earthSword);

    var staffMaker = new StaffMaker();
    var basicStaff = director.CreateBasicWeapon(staffMaker);
    var waterStaff = director.CreateWaterWeapon(staffMaker);
    Console.WriteLine(basicStaff);
    Console.WriteLine(waterStaff);

    var bowMaker = new BowMaker();
    var basicBow = director.CreateBasicWeapon(bowMaker);
    var windBow = director.CreateWindWeapon(bowMaker);
    Console.WriteLine(basicBow);
    Console.WriteLine(windBow);
}
```
{% endtab %}

{% tab title="Weapon" %}
```csharp
public class Weapon
{
    public string Name { get; set; }
    public string Element { get; set; }
    public string CreatorName { get; set; }
    public int MaximumSlots { get; set; }
    public int PhysicalAttack { get; set; }
    public int MagicalAttack { get; set; }
    public string WeaponType { get; set; }
    public int MinimumAttackRange { get; set; }

    public override string ToString()
    {
        var builder = new System.Text.StringBuilder()
            .Append($"{CreatorName} ({Element}) {Name} ")
            .Append($"{WeaponType} [{MaximumSlots}] slots, ")
            .Append($"Atk: {PhysicalAttack}, ")
            .Append($"MAtk: {MagicalAttack}, ")
            .Append($"Range: {MinimumAttackRange}");
        return builder.ToString();
    }
}
```
{% endtab %}

{% tab title="IWeaponMaker" %}
```csharp
public interface IWeaponMaker
{
    IWeaponMaker SetElement(string elementType);
    IWeaponMaker SetCreatorName(string name);
    Weapon GetWeapon();
}
```
{% endtab %}

{% tab title="SwordMaker" %}
```csharp
public class SwordMaker : IWeaponMaker
{
    private int cardSlots = 2;
    private string elementType;
    private string creatorName;

    public IWeaponMaker SetElement(string element)
    {
        cardSlots = 0;
        elementType = element;
        return this;
    }

    public IWeaponMaker SetCreatorName(string name)
    {
        creatorName = name;
        return this;
    }

    public Weapon GetWeapon()
    {
        return new Weapon
        {
            Name = "Katana",
            Element = elementType ?? "-",
            CreatorName = creatorName,
            PhysicalAttack = 10,
            MaximumSlots = cardSlots,
            MinimumAttackRange = 1,
            WeaponType = "sword"
        };
    }
}
```
{% endtab %}

{% tab title="StaffMaker" %}
```csharp
public class StaffMaker : IWeaponMaker
{
    private string creatorName;

    public IWeaponMaker SetElement(string element)
    {
        return this;
    }

    public IWeaponMaker SetCreatorName(string name)
    {
        creatorName = name;
        return this;
    }

    public Weapon GetWeapon()
    {
        return new Weapon
        {
            Name = "Soul",
            Element = "-",
            CreatorName = creatorName,
            MagicalAttack = 10,
            MaximumSlots = 2,
            MinimumAttackRange = 5,
            WeaponType = "staff"
        };
    }
}
```
{% endtab %}

{% tab title="BowMaker" %}
```csharp
public class BowMaker : IWeaponMaker
{
    private int cardSlots = 2;
    private string elementType;
    private string creatorName;

    public IWeaponMaker SetElement(string element)
    {
        cardSlots = 0;
        elementType = element;
        return this;
    }

    public IWeaponMaker SetCreatorName(string name)
    {
        creatorName = name;
        return this;
    }

    public Weapon GetWeapon()
    {
        return new Weapon
        {
            Name = "Hunter",
            Element = elementType ?? "-",
            CreatorName = creatorName,
            PhysicalAttack = 10,
            MaximumSlots = cardSlots,
            MinimumAttackRange = 12,
            WeaponType = "bow"
        };
    }
}
```
{% endtab %}

{% tab title="WeaponDirector" %}
```csharp
public class WeaponDirector
{
    public Weapon CreateBasicWeapon(IWeaponMaker maker)
    {
        var product = maker
            .SetCreatorName("Saladpuk")
            .GetWeapon();
        return product;
    }

    public Weapon CreateFireWeapon(IWeaponMaker maker)
        => createElementWeapon(maker, "Fire");

    public Weapon CreateWindWeapon(IWeaponMaker maker)
        => createElementWeapon(maker, "Wind");

    public Weapon CreateWaterWeapon(IWeaponMaker maker)
        => createElementWeapon(maker, "Water");

    public Weapon CreateEarthWeapon(IWeaponMaker maker)
        => createElementWeapon(maker, "Earth");

    private Weapon createElementWeapon(IWeaponMaker maker, string element)
    {
        var product = maker
            .SetElement(element)
            .SetCreatorName("Saladpuk")
            .GetWeapon();
        return product;
    }
}
```
{% endtab %}
{% endtabs %}

> **ผลลัพท์**  
> Saladpuk \(-\) Katana sword \[2\] slots, Atk: 10, MAtk: 0, Range: 1  
> Saladpuk \(Fire\) Katana sword \[0\] slots, Atk: 10, MAtk: 0, Range: 1  
> Saladpuk \(-\) Soul staff \[2\] slots, Atk: 0, MAtk: 10, Range: 5  
> Saladpuk \(-\) Soul staff \[2\] slots, Atk: 0, MAtk: 10, Range: 5  
> Saladpuk \(-\) Hunter bow \[2\] slots, Atk: 10, MAtk: 0, Range: 12  
> Saladpuk \(Wind\) Hunter bow \[0\] slots, Atk: 10, MAtk: 0, Range: 12

## 🎯 บทสรุป

### 👍 ข้อดี

การนำ **Builder Pattern** มาใช้งานนั้นจะช่วย ****ลดการผูกกันของโค้ดลง ทำให้เราสามารถเปลี่ยนแปลง แก้ไข รองรับสิ่งต่างๆได้มากขึ้น แถมยังช่วยลดโค้ดการสร้าง object ที่มีขั้นตอนในการสร้างเหมือนๆกันได้อีกด้วย

### 👎 ข้อเสีย

แค่จะสร้าง object ใหม่เฉยๆ ก็เพิ่มโค้ดเข้าไปมหาศาลแล้ว ดังนั้นโครงสร้างจะซับซ้อนขึ้นอีกเยอะเลย ดังนั้นก่อนใช้ให้คิดให้ดีก่อนว่า เรามีปัญหาถึงขนาดที่ต้องใช้มันหรือเปล่า?

### 🤙 ทางเลือก

เราสามารถนำ Framework พวก **Dependency Injection \(DI\)** เข้ามาใช้แทนได้นะจ๊ะ โค้ดกระชับหลับสบายเต็มตื่นด้วย

{% hint style="danger" %}
**ข้อควรระวัง**  
**อย่านำ Builder Pattern ไปใช้มั่วซั่ว** เพราะมันทำให้โค้ดของเราซับซ้อนขึ้นเยอะเลยแทนที่เราจะใช้คำสั่ง **new** แบบปรกติ ดังนั้นให้ชั่งน้ำหนักให้ดีเสียก่อนว่าปัญหาที่เราเจออยู่นั้น มันวุ่นวาย เทสยาก โค้ดมันผูกกันอยู่เยอะหรือเปล่า ถ้าชั่งน้ำหนักแล้ว + มีเหตุผลที่เพียงพอที่จะใช้ก็จงใช้ให้สบายใจไปเถิด
{% endhint %}

{% hint style="success" %}
เกลียด ชอบ ถูกใจ อยากติดตาม อยากติชมแนะนำด่าทอ หรืออะไรก็แล้วแต่ \(ห้ามมายืมเงิน\) จิ้มลงมาที่เพจนี้ได้เลย [**Mr.Saladpuk**](https://www.facebook.com/mr.saladpuk) และจะเป็นประคุณอันล้นพ้นถ้ากด Like + Follow + Share ให้ด้วยขอรับ น้ำตาจิไหล 🥺
{% endhint %}

