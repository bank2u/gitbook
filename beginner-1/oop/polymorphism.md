# 💖 Polymorphism

## 🤔 มันคืออะไร ?

คำว่า **Polymorphism** มีใช้อยู่ในหลายวงการเลย แต่ในวงการซอฟต์แวร์ใน **Wikipedia** ถูกเขียนไว้ว่า

> **Polymorphism** is the provision of a single [interface](https://en.wikipedia.org/wiki/Interface_%28computing%29) to entities of different [types](https://en.wikipedia.org/wiki/Data_type) or the use of a single symbol to represent multiple different types.

😑 ผมเชื่อว่าถ้าไม่เคยรู้จักมันมาก่อนจริงๆอ่านไปก็ไม่เข้าใจหรอก แต่ก็ถอดหัวใจสำคัญของมันออกมาได้ว่า

> **Polymorphism** คือทำให้ object เปลี่ยนรูปได้

## 🤨 ก็ยัง งง อยู่ดีขอตัวอย่างหน่อย

### 😢 ปัญหา

สมมุติว่าเราต้องเขียน **โปรแกรมบัญชีธนาคาร** เหมือนเดิมละกัน โดยมันมี **บัญชีออมทรัพย์** กับ **บัญชีกระแสรายวัน** โดยมีโค้ดแบบง่ายๆไว้ประมาณนี้

```csharp
public class SavingAccount
{
   public void Withdraw(double amount) { ... }
}

public class CurrentAccount
{
   public void Withdraw(double amount) { ... }
}
```

แล้วสมมุติว่าเราต้องเขียนให้มัน **ถอนเงิน** ให้กับบัญชีทั้ง 2 แบบนี้ได้ล่ะ เราจะเขียนยังไง?

ถ้าเอาแบบง่ายก่อน ก็สร้าง method ให้มันถอนเงินได้ใช่ป่ะ ตามโค้ดด้านล่าง

```csharp
static void Main(string[] args)
{
    var sa = new SavingAccount();
    Withdraw(sa, 500);

    var ca = new CurrentAccount();
    Withdraw(ca, 700);
}

static void Withdraw(SavingAccount account, double amount)
{
    account.Withdraw(amount);
}

static void Withdraw(CurrentAccount account, double amount)
{
    account.Withdraw(amount);
}
```

มันก็ทำงานได้นะ แต่ลองคิดดูว่าถ้าเรามีบัญชีอีกหลายๆประเภทเข้ามาล่ะ? เราจะมี method พวกนี้กี่ตัว?

### 🤔 วิเคราะห์ปัญหากันนิสนุง

ปัญหาที่เราเจออยู่ตอนนี้คือ object ที่เราสร้างขึ้นมา มันจะต้องใช้กับ data type ที่มันเป็นอยู่เท่านั้น มันเลยทำให้เราไม่สามารถส่ง object ของ SavingAccount ไปยัง CurrentAccount ได้ และทางตรงข้ามกันเราก็ส่ง object ของ CurrentAccount ไปยัง SaveingAccount ไม่ได้ด้วยเช่นกัน เพราะ **object มันผูกกับ data type ที่มันเป็นอยู่นั่นเอง**

### 😄 วิธีแก้ปัญหา

ตามหัวข้อคือเราจะใช้ **Polymorphism** มาแก้ปัญหาในรอบนี้ยังไงล่ะ ซึ่งการใช้ใช้ Polymorphism ได้เราจะต้องใช้หลักของ Inheritance เข้ามาช่วยด้วยดังนั้นก็แก้โค้ดให้มันเป็นแม่ลูกกันหน่อยนึง ตามตัวอย่างในเรื่อง [**Inheritance** ](https://saladpuk.gitbook.io/learn/beginner-1/oop/inheritance)เลย \(ถ้าจำไม่ได้กดที่ชื่อเพื่อกลับไปทบทวนซะ\) ซึ่งเราก็จะได้ออกมาราวๆนี้

```csharp
public class BankAccount { ... }
public class SavingAccount : BankAccount { }
public class CurrentAccount : BankAccount { ... }
```

ซึ่งหลักจากที่เราทำ Inheritance มาเรียบร้อย เราจะได้ความสามารถของ Polymorphism มาด้วย นั่นคือ

{% hint style="success" %}
**การเปลี่ยนรูปได้** \(ผมชอบใช้คำว่าโค้ดมันดิ้นได้\)  
นั่นหมายความว่า object นั้นๆมันจะไม่ได้ผูกติดกับ data type ที่มันอยู่แล้วนั่นเอง
{% endhint %}

ดังนั้นเราจะสามารถเขียนโค้ดแบบนี้ได้

```csharp
BankAccount acc1 = new SavingAccount();
BankAccount acc2 = new CurrentAccount();
```

จะเห็นว่าตัว acc1 และ acc2 ทั้งคู่มันเป็นคลาส์ BankAccount แต่มันก็สามารถเก็บ object ที่ไม่ใช่ BankAccount ได้นั่นเอง ดังนั้นเราก็สามารถที่จะทำให้โค้ดของเรารองรับการทำงานของ บัญชีหลายๆรูปแบบในอนาคตได้แล้ว โดยการเขียนโค้ดเพียงแค่ตัวนี้เท่านั้น

```csharp
static void Withdraw(BankAccount account, double amount)
{
    account.Withdraw(amount);
}
```

ไหนทดลองทำงานดูดิ๊

```csharp
static void Main(string[] args)
{
    BankAccount acc1 = new SavingAccount { OwnerName = "(Saving) Saladpuk" };
    Withdraw(acc1, 500);
    Console.WriteLine($"{acc1.OwnerName}, has THB {acc1.Balance}.");

    BankAccount acc2 = new CurrentAccount { OwnerName = "(Current) Saladpuk" };
    Withdraw(acc2, 700);
    Console.WriteLine($"{acc2.OwnerName}, has THB {acc2.Balance}.");
}
```

> **Output**  
> \(Saving\) Saladpuk, has THB 0.  
> \(Current\) Saladpuk, has THB -700.

จะเห็นว่ามันทำงานได้ตามปรกติแถมยังทำงานถูกต้องตามแต่ละประเภทบัญชีด้วย บัญชีออมทรัพย์ถอนเงินเกินในบัญชีไม่ได้ แต่บัญชีกระแสรายวันถอนเงินเกินได้

## 😵 Polymorphism คือไรกันแน่ ?

ก่อนอื่นผมอยากให้เข้าใจตรงกันก่อนว่า **โดยปรกติตัวแปรมันจะทำงานตามที่ type ที่มันเป็นอยู่เท่านั้น** เช่น เรามีคลาส **Dog** กับ **Cat**

```csharp
public class Dog { }
public class Cat { }
```

ถ้าเราจะไปสร้าง object ของคลาส Dog ออกมา เราก็ต้องใช้ตัวแปรที่เป็นคลาส Dog มาเก็บมันเท่านั้น

```csharp
Dog d1 = new Dog();
```

ไม่สามารถสร้าง object ของคลาส Dog ไปเก็บไว้กับตัวแปรที่เป็นคลาส Cat ได้เลย

```csharp
Cat c1 = new Dog(); // Error
```

นี่แหละคือสิ่งที่เรียกว่า **object มันผูกกับ data type ที่มันเป็นอยู่** นั่นเอง

การที่โปรแกรมของเราสามารถทำ Polymorphism ได้มันจะเป็นการ **ปลดล๊อค** การผูกกันที่ว่านี้ เลยทำให้โค้ดเรายืดหยุ่นขึ้น นำกลับมาใช้งานใหม่ได้ง่ายขึ้น โดยตัวหนึ่งที่เรานำมาทำให้เกิด Polymorphism ได้นั่นก็คือการทำ **Inheritance** นั่นเอง ดังนั้นเมื่อเราแก้โค้ดให้เป็นแบบนี้

```csharp
public class Animal { }
public class Dog : Animal { }
public class Cat : Animal { }
```

มันเลยทำให้คลาส Dog และ Cat สามารถ เปลี่ยนรูป ตัวมันเองให้ตัวแปรที่เป็น Animal สามารถเก็บ object เหล่านั้นได้

```csharp
Animal dog = new Dog();
Animal cat = new Cat();
```

### การเปลี่ยนรูป

จากตัวอย่างด้านบนคือการเปลี่ยนรูป จาก Dog หรือ Cat กลายเป็น Animal เลยทำให้ตัวแปร Animal สามารถทำงานร่วมกัน Dog และ Cat ได้นั่นเอง โดยมันมีกฏอยู่ว่า

{% hint style="warning" %}
Base Class สามารถเก็บ Sub Class ได้ แต่ทำตรงข้ามกันไม่ได้
{% endhint %}

ถ้าเราแปลงโค้ดด้านบนเป็นรูปแล้วเราจะได้ออกมาเป็นแบบนี้

![](../../.gitbook/assets/image%20%28325%29.png)

นั่นหมายความว่า Animal สามารถเก็บ object ที่เป็น Sub Class ของมันได้นั่นคือ Dog และ Cat แต่เราไม่สามารถทำตรงข้ามกันได้ นั่นคือ Dog และ Cat ไม่สามารถเก็บ object ที่เป็น Animal ได้นั่นเอง

```csharp
Dog dog = new Animal(); // Error
Cat cat = new Animal(); // Error
```

และถ้าเรามองย้อนกลับไปถึงลำดับชั้นของการทำ Inheritance แล้วเราจะพบว่ามันเป็นรูปนี้

![](../../.gitbook/assets/image%20%28465%29.png)

เลยเป็นผลว่าทำไม Object เลยสามารถเก็บตัวแปรทุกอย่างในโค้ดเราได้นั่นเอง \(มันเป็น base class สูงสุดนั่นเอง\)

```csharp
object animal = new Animal();
object dog = new Dog();
object cat = new Cat();
```

### Casting

นอกจากที่เราจะเปลี่ยนรูปจาก Sub class ไปให้ Base class เก็บไว้ได้แล้ว เรายังสามารถเปลี่ยนรูปมันกลับมาก็ได้นะ เช่น

```csharp
public class Animal
{
    public string Name { get; set; }
}

public class Dog : Animal
{
    // พลักในการกัด
    public int BitePower { get; set; }
}

static void Main(string[] args)
{
    Animal animal = new Dog
    {
        Name = "Saladpuk",
        BitePower = 50,
    };
    Dog dog = (Dog)animal;
    Console.WriteLine($"Name: {dog.Name}, BitePower: {dog.BitePower}");
}
```

> **Output**  
> Name: Saladpuk, BitPower: 50

จากโค้ดด้านบนจะเห็นว่าแม้ Dog จะถูกเปลี่ยนรูปไปเป็น Animal ในบรรทัดที่ 14 แล้ว แต่เราก็สามารถแปลงมันกลับมาเป็น Dog ได้ตามปรกติ ในบรรทัดที่ 19

### Interface

อีกตัวอย่างหนึ่งในการทำ Polymorphism นั่นคือการใช้ **Interface** นั่นเอง เช่น เครื่องใช้ไฟฟ้าสามารถเปิด/ปิดได้ โดยมีทีวีและมือถือเป็นเครื่องใช้ไฟฟ้า

![](../../.gitbook/assets/image%20%28906%29.png)

จากรูปเราก็สามารถทำ Polymorphism ได้เช่นกัน ตามนี้

```csharp
public interface IElectronic
{
    void TunrOn();
    void TunrOff();
}

public class Television : IElectronic
{
    public void TunrOff() => Console.WriteLine("Tv - off");
    public void TunrOn() => Console.WriteLine("Tv - on");
}

public class Mobile : IElectronic
{
    public void TunrOff() => Console.WriteLine("Mobile - off");
    public void TunrOn() => Console.WriteLine("Mobile - on");
}
```

ลองใช้งานมันดู

```csharp
IElectronic e1 = new Television();
e1.TunrOn();
e1.TunrOff();

IElectronic e2 = new Mobile();
var mobile = (Mobile)e2;
mobile.TunrOn();
mobile.TunrOff();
```

> **Output**  
> Tv - on  
> Tv - off  
> Mobile - on  
> Mobile - off

{% hint style="info" %}
**เกร็ดความรู้**  
**Polymorphism** มันมีหลามันมาจากคำ 2 คำรวมกันนั่นคือ \(**Poly** = หลากหลาย\) + \(**Morph** = รูปร่าง\) ดังนั้นพอรวมกันแล้วเลยเป็นกลายทำให้โค้ดของเรา **เปลี่ยนรูป** ได้นั่นเอง ซึ่งมันจะช่วยให้โค้ดของเรายืดหยุ่นขึ้นยังไงล่ะ
{% endhint %}

## 🎥 วีดีโอประกอบความเข้าใจ

### ทฤษฎี

{% embed url="https://www.youtube.com/watch?v=sG0C6PyW9e8" caption="" %}

### ตัวอย่างการใช้งาน

{% embed url="https://www.youtube.com/watch?v=rTlmBkTXggE" caption="" %}

