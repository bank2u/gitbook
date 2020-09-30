# 💖 Inheritance

## 🤔 มันคืออะไร ?

คำว่า **Inheritance** มีใช้อยู่ในหลายวงการเลย แต่ในวงการซอฟต์แวร์ใน **Wikipedia** ถูกเขียนไว้ว่า

> **Inheritance** is the mechanism of basing an [object](https://en.wikipedia.org/wiki/Object_%28computer_science%29) or [class](https://en.wikipedia.org/wiki/Class_%28computer_programming%29) upon another object \([prototype-based inheritance](https://en.wikipedia.org/wiki/Prototype-based_programming)\) or class \([class-based inheritance](https://en.wikipedia.org/wiki/Class-based_programming)\), retaining similar implementation. Also defined as deriving new classes \([sub classes](https://en.wikipedia.org/wiki/Inheritance_%28object-oriented_programming%29#Subclasses_and_superclasses)\) from existing ones \(super class or [base class](https://en.wikipedia.org/wiki/Fragile_base_class)\) and forming them into a hierarchy of classes. In most class-based object-oriented languages, an object created through inheritance \(a "child object"\) acquires all the properties and behaviors of the parent object \(except: [constructors](https://en.wikipedia.org/wiki/Constructor_%28object-oriented_programming%29), destructor, [overloaded operators](https://en.wikipedia.org/wiki/Operator_overloading) and [friend functions](https://en.wikipedia.org/wiki/Friend_function) of the base class\). Inheritance allows programmers to create classes that are built upon existing classes,[\[1\]](https://en.wikipedia.org/wiki/Inheritance_%28object-oriented_programming%29#cite_note-1) to specify a new implementation while maintaining the same behaviors \([realizing an interface](https://en.wikipedia.org/wiki/Class_diagram#Realization/Implementation)\), to [reuse code](https://en.wikipedia.org/wiki/Code_reuse) and to independently extend original software via public classes and interfaces. The relationships of objects or classes through inheritance give rise to a [directed graph](https://en.wikipedia.org/wiki/Directed_graph). Inheritance was invented in 1969 for [Simula](https://en.wikipedia.org/wiki/Simula).[\[2\]](https://en.wikipedia.org/wiki/Inheritance_%28object-oriented_programming%29#cite_note-2)

😑 แค่อ่านก็ปวดกบาลละ แต่ก็ถอดหัวใจสำคัญของมันออกมาได้ว่า

> **Inheritance** คือการ**สืบทอดคุณสมบัติ** จาก Model A ไปยัง Model อื่นๆได้ ซึ่งมันจะช่วยให้เราเพิ่มความสามารถใหม่ๆเข้าไปได้โดยที่ไม่ต้องไปยุ่งกับโค้ดเก่าที่เคยเขียนไว้

## 🤨 ก็ยัง งง อยู่ดีขอตัวอย่างหน่อย

### 😢 ปัญหา

สมมุติว่าเราต้องเขียน **โปรแกรมบัญชีธนาคาร** ซึ่งบัญชีออมทรัพย์สามารถเก็บข้อมูล **เงินในบัญชี** และ **เจ้าของบัญชี** ได้ และอย่าลืมนะว่าเราต้อง **ฝากเงินเข้าบัญชี** ได้ด้วย ดังนั้นเราก็น่าจะได้ Model ออกมาประมาณนี้

```csharp
public class SavingAccount
{
    private double balance;
    public double Balance { get => balance; }
    public string OwnerName { get; set; }

    public void Deposit(double amount)
    {
        if (amount > 0)
        {
            balance += amount;
        }
    }
}
```

ซึ่งบัญชีมันไม่ได้มีแค่ประเภทเดียวนะ เช่น บัญชีกระแสรายวัน ซึ่งมันก็ต้องเก็บข้อมูล **เงินในบัญชี, เจ้าของบัญชี, ฝากเงินเข้าบัญชี** ได้เหมือนกันด้วย ดังนั้นเราก็จะสร้าง Model ขึ้นมาอีกตัวหน้าตาประมาณนี้

```csharp
public class CurrentAccount
{
    private double balance;
    public double Balance { get => balance; }
    public string OwnerName { get; set; }

    public void Deposit(double amount)
    {
        if (amount > 0)
        {
            balance += amount;
        }
    }
}
```

เราจะเห็นว่าโค้ด **บัญชีออมทรัพย์** และ **บัญชีกระแสรายวัน** มันเหมือนกันเลย! ซึ่งถามว่าผิดไหม คำตอบคือไม่ผิดครับ แต่**ถ้าเราปล่อยมันไว้แบบนี้เราจะมีปัญหาในอนาคต** เช่น ถ้าบัญชีแต่ละประเภทต้องเก็บรหัสประชาชนเข้าไปด้วยล่ะ หรือ เรามาพบทีหลังว่าโค้ดมันเขียนผิดนะ แต่เราก็ copy ไปวางไว้ที่อื่นเรียบร้อยแล้ว

{% hint style="danger" %}
**Needless Repetition**  
นี่คือตัวอย่างการเขียนโค้ดที่ไม่ดี นั่นคือการ copy งานที่เหมือนๆกันไปใช้ในแต่ละที่ ซึ่งหลักในการออกแบบที่ดี เราไม่ควรจะทำงานเดิมซ้ำ Don't Repeat Yourself \(DRY\) ซึ่งเพื่อนๆสามารถศึกษาเรื่อง Bad Code ได้จากบทความ [👶 Code Smells](https://saladpuk.gitbook.io/learn/basic/code-smells)
{% endhint %}

### 😄 วิธีแก้ปัญหา

เราสามารถนำหลักการ **Inheritance** มาช่วยในเรื่องนี้ได้ โดยการสร้าง **Model ต้นแบบ** ขึ้นมา แล้วทำการย้ายของที่ซ้ำกันไปไว้ในตัวต้นแบบตัวนั้น ซึ่งในตัวอย่างของที่เรากำลังทำอยู่มันคือ **บัญชีธนาคาร** ดังนั้นเราเลยสร้างคลาส **BankAccount** ขึ้นมาใหม่

```csharp
public class BankAccount
{
}
```

แล้วทำการ**ย้ายของที่มันซ้ำกัน**จาก _บัญชีออมทรัพย์_ กับ _\*\*บัญชีกระแสรายวัน_ มาไว้ในตัวต้นแบบซะ

```csharp
public class BankAccount
{
    private double balance;
    public double Balance { get => balance; }
    public string OwnerName { get; set; }

    public void Deposit(double amount)
    {
        if (amount > 0)
        {
            balance += amount;
        }
    }
}
```

สุดท้ายเราก็สั่งให้ _บัญชีออมทรัพย์_ และ _บัญชีกระแสรายวัน_ ไป**สืบทอดความสามารถมาจากตัวต้นแบบ**ที่สร้างไว้

```csharp
public class SavingAccount : BankAccount
{
}

public class CurrentAccount : BankAccount
{
}
```

เพียงเท่านี้ _บัญชีออมทรัพย์_ และ _บัญชีกระแสรายวัน_ ก็จะมีความสามารถทุกอย่างที่ตัวต้นแบบมีนั่นเอง ดังนั้นลองเขียนโค้ดเล่นกับบัญชีทั้ง 2 ตัวของเราดู

```csharp
var sa = new SavingAccount();
sa.OwnerName = "(Saving) Saladpuk";
sa.Deposit(500);
Console.WriteLine($"{sa.OwnerName}, has THB {sa.Balance}.");

var ca = new CurrentAccount();
ca.OwnerName = "(Current) Saladpuk";
ca.Deposit(700);
Console.WriteLine($"{ca.OwnerName}, has THB {ca.Balance}.");
```

> **Output**  
> \(Saving\) Saladpuk, has THB 500.  
> \(Current\) Saladpuk, has THB 700.

## 😵 Inheritance คือไรกันแน่ ?

**Inheritance** มันมีหลายเรื่องอยู่ในนั้นเยอะเลย ดังนั้นมาค่อยๆทำความเข้าใจกันทีละเรื่องก่อนละกันนะ โดยสิ่งที่เราต้องรู้สิ่งแรกคือ

### Inheritance \(การสืบทอด\)

{% hint style="success" %}
✨ เราสามารถนำคลาสที่มีอยู่แล้ว มาต่อยอดความสามารถให้กับคลาสใหม่ได้
{% endhint %}

เราได้เห็นตัวอย่างโค้ดด้านบนไปแล้วว่า _บัญชีออมทรัพย์_ และ _บัญชีกระแสรายวัน_ ได้**ต่อยอดความสามารถ**จาก **BankAccount** มา เลยทำให้บัญชีทั้ง 2 ประเภทมี _OwnerName, Balance และ Deposit\(\)_ ทั้งๆที่ตัวมันเองไม่ได้มีโค้ดอะไรอยู่ด้านในเลย ซึ่งสิ่งนี้เราเรียกว่า **การสืบทอด** หรือ **Inheritance** นั่นเอง

### Base class \(คลาสแม่\)

โดยคลาสที่เป็นต้นแบบเราเรียกมันว่า **Base class** \(บางตำราจะเรียกว่า Super class, Parent class บลาๆ\) ซึ่งในตัวอย่างนี้คลาส **BankAccount** เป็น base class ของ **SavingAccount** และ **CurrentAccount** นั่นเอง

### Sub class \(คลาสลูก\)

ส่วนคลาสที่ไปสืบทอดความสามารถจากคนอื่นเราเรียกมันว่า **Sub Class** \(บางตำราเรียกว่า Derived class, Child class บลาๆ\) ซึ่งในตัวอย่างนี้คลาส **SavingAccount** และ **CurrentAccount** เป็น sub class ของ **BankAccount** นั่นเอง

### Generalization

{% hint style="success" %}
✨ ของที่อยู่ใน Base class จะถูกส่งลงมาให้ Sub class ส่วน sub class จะใช้มันได้หรือเปล่าขึ้นอยู่กับ accessibility ของ property พวกนั้น
{% endhint %}

เราจะเห็นว่าแม้ **SavingAccount** จะไม่ได้เขียนโค้ดอะไรไว้ข้างในเลย แต่เราก็จะสามารถเรียกใช้ Balance หรือเมธอต Deposit ได้ ตามโค้ดด้านล่าง

```csharp
var sa = new SavingAccount();
sa.OwnerName = "(Saving) Saladpuk";
sa.Deposit(500);
```

เพราะสิ่งที่คลาสแม่มี คลาสลูกจะมีด้วย **แต่จะใช้งานได้หรือเปล่าขึ้นอยู่กับ accessibility ที่ตัวแม่ตั้งไว้** เช่นตัวแปร balance มันเป็น private คลาสลูกจะไม่สามารถอ้างถึงได้นั่นเอง

### Override \(การเปลี่ยนพฤติกรรม\)

อีกสิ่งหนึ่งที่เราได้จากการทำ Inheritance นั่นคือ เราสามารถทำให้การทำงานของ Sub class ทำงานแตกต่างกันกับ Base class ของมันได้ เช่น ตัวชัญชีนั้นจะต้องสามารถ **ถอนเงิน** ได้ ซึ่งจุดที่น่าสนใจของมันคือ

* **บัญชีออมทรัพย์** - ถอนเงินได้สูงสุด**ไม่เกินเงินที่มีอยู่ในบัญชี**
* **บัญชีกระแสรายวัน** - ถอนเงินได้**เกินเงินที่มีอยู่ในบัญชี แต่ไม่เกินวงเงินที่ตั้งไว้**

ดังนั้นเราก็จะทำการเขียนโค้ดถอนเงินไว้ที่ตัว **BankAccount** ที่เป็น Base class ของมันก่อน ซึ่งได้ประมาณนี้

```csharp
public class BankAccount
{
    ...

    public double Balance
    {
        get => balance;
        protected set => balance = value;
    }

    public virtual void Withdraw(double amount)
    {
        if (amount <= balance)
        {
            balance -= amount;
        }
    }
}
```

> คำสั่ง virtual เป็นคำสั่งพิเศษของภาษา C\# เพื่อที่จะให้ Sub class สามารถไปแก้ไขการทำงานของเมธอตนั้นๆได้ และผมได้แก้ให้ตัวแปร Balance สามารถเข้าไปแก้ไขผ่าน Sub class ได้เท่านั้น

เพียงเท่านี้เราก็จะสามารถถอนเงินได้ละ ส่วน บัญชีกระแสรายวัน เราก็ไปเขียนเพิ่มให้มันสามารถถอนเงินได้เกินเงินที่มี อยู่ในบัญชี ซึ่งสมมุติว่าวงเงินกู้ได้ไม่เกิน 1,000 ละกัน ดังนั้นเราก็จะได้โค้ดออกมาตามนี้

```csharp
public class CurrentAccount : BankAccount
{
    private double credit = 1000;
    public override void Withdraw(double amount)
    {
        if (amount <= Balance + credit)
        {
            Balance -= amount;
        }
    }
}
```

ลองทดสอบกันดู

```csharp
static void Main(string[] args)
{
    var sa = new SavingAccount();
    sa.OwnerName = "(Saving) Saladpuk";
    sa.Deposit(500);
    sa.Withdraw(700);
    Console.WriteLine($"{sa.OwnerName}, has THB {sa.Balance}.");

    var ca = new CurrentAccount();
    ca.OwnerName = "(Current) Saladpuk";
    ca.Deposit(700);
    ca.Withdraw(1000);
    Console.WriteLine($"{ca.OwnerName}, has THB {ca.Balance}.");
}
```

> **Output**  
> \(Saving\) Saladpuk, has THB 500.  
> \(Current\) Saladpuk, has THB -300.

เรียบร้อยแล้ว บัญชีออมทรัพย์ ถอนเงินได้ไม่เกินเงินที่มีอยู่ในบัญชี แต่ในขณะที่ บัญชีกระแสรายวันถอนเกินเงินที่มีอยู่ได้นั่นเอง

## 🤔 ทำ Inheritance ไปทำไม ?

กลับมาที่บัญชีธนาคารเหมือนเดิม ซึ่งนอกจากการฝากเงินแล้ว เรายังต้องทำให้มัน **ปิดบัญชี** ได้ด้วย ซึ่งถ้าเราใช้ความสามารถของ Inheritance เราเลยสามารถไปเขียนเมธอตในการปิดบัญชีไว้ที่ Base class เพียงที่เดียว แล้วเจ้า Sub class ของมันก็จะได้รับความสามารถนี้ไปด้วยทันทีนั่นเอง \(ขอเขียนแบบย่อๆนะ\)

```csharp
public class BankAccount
{
    ...

    private bool isClosed;
    public bool IsClosed { get => isClosed; }

    public void CloseAccount()
    {
        isClosed = true;
    }
}
```

## ความสัมพันธ์แบบ Is a

เมื่อไหร่ก็ตามที่เราใช้ Inheritance ปุ๊ป เจ้าพวก **Sub class** ทั้งหลายจะมีความสัมพันธ์ที่เรียกว่า **"Is a"** ทันที หมายความว่า เราก็จะมองว่า **บัญชีออมทรัพย์** และ **บัญชีกระแสรายวัน** มันเป็น **บัญชีธนาคาร** ประเภทหนึ่งทันที ซึ่งมันจะมีบทบาทที่สำคัญในเรื่องของการนำไปใช้กับ **Polymorphism** ในบทถัดไปอย่างมาก

{% hint style="danger" %}
**ข้อควรระวัง**  
อย่าใช้ Inheritance เพราะเราขี้เกียจเขียนโค้ดซ้ำๆ เพราะมันจะได้ความสัมพันธ์แบบ Is A เข้าไปด้วย \(เดี๋ยวไปดูความร้ายแรงของมันในบทของ Polymorphism เอาละกัน\)
{% endhint %}

{% hint style="success" %}
**จงทำ Inheritance เมื่อ**  
Class พวกนั้นมันเป็นประเภทเดียวกันจริงๆ จากมุมมองของ Abstraction เท่านั้น
{% endhint %}

## ลำดับชั้น

ในการทำ Inheritance มันจะมีลำดับชั้นความสัมพันธ์กันว่าใครเป็น **คลาสแม่ คลาสลูก** กันเสมอ ซึ่งจากโค้ดตัวอย่างด้านบนทั้งหมดก็สามารถเอามาเขียนเป็นแผนภาพ UML ง่ายๆได้ประมาณนี้

![](../../.gitbook/assets/image-952.png)

โดยเราจะเห็นว่า BankAccount เป็นคลาสแม่ ซึ่งมีลูก 2 ตัวคือ SavingAccount และ CurrentAccount

แต่มันยังไม่จบเพียงเท่านี้เพราะทุกคลาสจริงๆมันจะสืบสอดมาจากคลาสที่ชื่อว่า Object อีกที ดังนั้นแผนภาพที่แท้จริงคือ

![](../../.gitbook/assets/image%20%28743%29.png)

ซึ่งจากรูปนี้คลาส BankAccount ก็เป็นคลาสลูกของคลาส Object อีกต่อหนึ่งนั่งเอง เลยทำให้มันมีความสามารถต่างๆของคลาสแม่ติดมาด้วยเสมอยังไงล่ะ เช่น เราใช้คำสั่ง **`.ToString()`** ได้เลยนั่นเอง

```csharp
static void Main(string[] args)
{
    var acc = new BankAccount();
    acc.OwnerName = "Saladpuk";
    acc.Deposit(500);

    var result = acc.ToString();
    Console.WriteLine(result);
}
```

> **Output**  
> demo.BankAccount
>
> _หมายเหตุ: โดยปรกติ **.ToString\(\)** ที่เขียนไว้ใน Base Class จะเป็นการแสดงชื่อเต็มๆโดยขึ้นต้นจาก Name space แล้วต่อด้วยชื่อคลาสของเรานั่นเอง_

{% hint style="warning" %}
**หมายเหตุ**  
ใน C\# เราสามารถทำการสืบทอดได้ทีละ 1 คลาสเท่านั้น หรือพูดง่ายๆคือ **มีแม่ได้เพียงคนเดียว** นะจ๊ะ แต่ในหลักการเรื่องนี้มันมีอีกหลายแบบเลย เช่น มีแม่ได้มากกว่า 1 ตัว \(**Multiple Inheritance**\) แบบลูกครึ่ง \(**Hybrid inheritance**\) ซึ่งเรื่องเหล่านั้นลองไปหาอ่านดูเอาเองต่อนะจ๊ะ
{% endhint %}

{% hint style="info" %}
**แนะนำให้อ่าน**  
ใครที่สนใจอยากรู้การแปลงโค้ดให้เป็นภาพที่เข้าใจได้ง่ายๆ สามารถศึกษาเพิ่มเติมได้ที่บทความตัวนี้เลย [👶 UML พื้นฐาน](https://saladpuk.gitbook.io/learn/basic/uml)
{% endhint %}

## 🎥 วีดีโอประกอบความเข้าใจ

### ทฤษฎี

{% embed url="https://www.youtube.com/watch?v=ewhjQ6GaKFY&feature=emb\_title" caption="" %}

### ตัวอย่างการใช้งาน

{% embed url="https://www.youtube.com/watch?v=hXpZ00f6cFI&feature=emb\_title" caption="" %}

## ตัวอย่างโค้ดทั้งหมด

{% tabs %}
{% tab title="BankAccount" %}
```csharp
public class BankAccount
{
    private bool isClosed;
    private double balance;

    public bool IsClosed { get => isClosed; }
    public double Balance
    {
        get => balance;
        protected set => balance = value;
    }
    public string OwnerName { get; set; }

    public void Deposit(double amount)
    {
        if (amount > 0)
        {
            balance += amount;
        }
    }

    public virtual void Withdraw(double amount)
    {
        if (amount <= balance)
        {
            balance -= amount;
        }
    }

    public void CloseAccount()
    {
        isClosed = true;
    }
}
```
{% endtab %}

{% tab title="SavingAccount" %}
```csharp
public class SavingAccount : BankAccount
{
}
```
{% endtab %}

{% tab title="CurrentAccount" %}
```csharp
public class CurrentAccount : BankAccount
{
    private double credit = 1000;

    public override void Withdraw(double amount)
    {
        if (amount <= Balance + credit)
        {
            Balance -= amount;
        }
    }
}
```
{% endtab %}

{% tab title="Main" %}
```csharp
static void Main(string[] args)
{
    var sa = new SavingAccount();
    sa.OwnerName = "(Saving) Saladpuk";
    sa.Deposit(500);
    sa.Withdraw(700);
    Console.WriteLine($"{sa.OwnerName}, has THB {sa.Balance}.");

    var ca = new CurrentAccount();
    ca.OwnerName = "(Current) Saladpuk";
    ca.Deposit(700);
    ca.Withdraw(1000);
    Console.WriteLine($"{ca.OwnerName}, has THB {ca.Balance}.");
}
```
{% endtab %}
{% endtabs %}

