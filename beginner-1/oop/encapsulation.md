# 💖 Encapsulation

## 🤔 มันคืออะไร ?

คำว่า **Encapsulation** มีใช้อยู่ในหลายวงการเลย แต่ในวงการซอฟต์แวร์ใน **Wikipedia** ถูกเขียนไว้ว่า

> **Encapsulation** refers to the bundling of data with the methods that operate on that data, or the restricting of direct access to some of an object's components.[\[1\]](https://en.wikipedia.org/wiki/Encapsulation_%28computer_programming%29#cite_note-Rogers01-1) Encapsulation is used to hide the values or state of a structured data object inside a [class](https://en.wikipedia.org/wiki/Class_%28computer_programming%29), preventing unauthorized parties' direct access to them. Publicly accessible methods are generally provided in the class \(so-called ["getters" and "setters"](https://en.wikipedia.org/wiki/Mutator_method)\) to access the values, and other client classes call these methods to retrieve and modify the values within the object.

ซึ่งถ้าถอดความหมาย เราก็จะได้หัวใจสำคัญของมันออกมาว่า

> **Encapsulation** เป็นการจัดการข้าวของภายในตัว Model ของเรา เพื่อให้คนอื่นเรียกใช้งานได้ง่ายๆ ในขณะที่เรายังป้องกันข้อมูลที่สำคัญจากภายนอกเอาไว้ได้

## 🤨 ก็ยัง งง อยู่ดีขอตัวอย่างหน่อย

สมมุติว่าเราต้องเขียน **โปรแกรมบัญชีธนาคาร** ซึ่งตัวบัญชีสามารถเก็บข้อมูล **เจ้าของบัญชี** และ **เงินในบัญชี** ได้ ดังนั้นเราก็น่าจะได้ Model จากหลักการของ Abstraction ออกมาประมาณนี้

```csharp
public class BankAccount
{
    public string OwnerName;
    public double Balance;
}
```

โค้ดตัวอย่างด้านบนไม่ได้มีอะไรผิดถูกต้องตาม Abstraction ทุกประการ **แต่มันไม่ใช่ Model ที่ดีเพราะมันยังขาดการนำ Encapsulation มาใช้อยู่** ดังนั้นมาดูตัวอย่างของปัญหาเพื่อให้เห็นภาพกันว่ามันไม่ดีตรงไหน

เพื่อนๆลองคิดถึงตอนที่เราจะใช้งานเจ้า **BankAccount** ดูนะ สมมุติว่าเราจะสร้างบัญชีให้ตัวผมเอง พร้อมฝากเงินเข้าไปในบัญชีนั้น 500 บาท ผมก็จะเขียนโค้ดง่ายๆออกมาได้ประมาณนี้

```csharp
static void Main(string[] args)
{
    var acc = new BankAccount();
    acc.OwnerName = "Saladpuk";
    acc.Balance += 500;

    Console.WriteLine($"Account: {acc.OwnerName}, has THB {acc.Balance}.");
}
```

> **Output**  
> Account: Saladpuk, has THB 500.

ซึ่งดูเหมือนจะไม่มีปัญหาอะไรชิมิ งั้นลองเพิ่มเงินเข้าไป -1200 ดูดิ๊

```csharp
static void Main(string[] args)
{
    var acc = new BankAccount();
    acc.OwnerName = "Saladpuk";
    acc.Balance += 500;
    acc.Balance -= 1200;

    Console.WriteLine($"Account: {acc.OwnerName}, has THB {acc.Balance}.");
}
```

> **Output**  
> Account: Saladpuk, has THB -700.

จะเห็นว่าเงินในบัญชีติดลบ -700 ไปเรียบร้อยแล้ว ซึ่งในบัญชีปรกติเราไม่ควรมีเงินติดลบอยู่ในนั้นใช่ป่ะ

จากที่ว่ามาเดี๋ยวเราจะลองเอาหลักของ **Encapsulation** มาใช้ดูบ้างว่ามันจะมาช่วยเราได้ยังไง ซึ่งเราไปทำความเข้าใจแนวคิดของมันนิดหนึ่งก่อนว่า

{% hint style="info" %}
**แนวคิด**  
ในการทำ Model นั้นเราจะต้องแยกของที่เป็นข้อมูลที่ **Sensitive data** ออกมาดูแลเอง และให้เฉพาะคนที่สมควรเท่านั้น ถึงจะสามารถเข้าถึง sensitive data เหล่านั้นได้
{% endhint %}

ซึ่งในโค้ดตัวอย่างสิ่งที่เป็น sensitive data ก็คือ เงินในบัญชี นั่นเอง ซึ่งเราไม่ควรให้คนอื่นเข้าไปแก้ไขมันได้มั่วๆ ดังนั้นเราต้องทำการซ่อนมันไว้ก่อน เลยทำให้ Model ของเราถูกแก้เป็นแบบนี้

```csharp
public class BankAccount
{
    public string OwnerName;

    // แก้มันเป็น private member เพราะเราไม่อยากให้คนอื่นเข้ามาแก้มันได้มั่วซั่ว
    private double balance;
}
```

ส่วนถ้าใครอยากจะเข้าถึงข้อมูลตัวนี้ เราก็จะมีช่องทางให้คนอื่นเข้ามาใช้งานมันได้ง่ายๆ แต่แก้ไขมันไม่ได้นะ ดังนั้น Model เราก็จะออกมาประมาณนี้

```csharp
public class BankAccount
{
    public string OwnerName;
    private double balance;

    // ใครอยากจะดูยอดเงินก็เข้ามาดูผ่าน property ตัวนี้ ซึ่งมันดูได้อย่างเดียวแก้ไม่ได้
    public double Balance { get => balance; }
}
```

สุดท้ายใครอยากจะแก้ไข **ยอดเงิน** ก็ต้องทำผ่านช่องทางที่เรากำหนดไว้เท่านั้น เพื่อป้องกันการแก้ไขโดยใส่ค่าที่ไม่ถูกต้องเข้ามา

```csharp
public class BankAccount
{
    public string OwnerName;
    private double balance;
    public double Balance { get => balance; }

    // ช่องทางให้คนอื่นเข้ามาเพิ่มจำนวนเงินได้ โดยที่ไม่ทำให้ sensitive data มีปัญหา
    public void Deposit(double amount)
    {
        if (amount > 0)
        {
            balance += amount;
        }
    }
}
```

ดังนั้นจากที่ว่ามาเราก็ลองกลับไปลองแกล้งมันใหม่ดูว่าจะเกิดอะไรขึ้น

```csharp
static void Main(string[] args)
{
    var acc = new BankAccount();
    acc.OwnerName = "Saladpuk";
    acc.Deposit(500);
    acc.Deposit(-1200);

    Console.WriteLine($"Account: {acc.OwnerName}, has THB {acc.Balance}.");
}
```

> **Output**  
> Account: Saladpuk, has THB 500.

จะเห็นว่าต่อให้เราฝากเงินติดลบลงไป บัญชีเราก็ไม่ทำงานผิดพลาดเหมือนเดิมแล้ว เพราะเรานำหลักของ Encapsulation มาใส่ในการออกแบบ Model แล้วนั่นเอง

## 😵 Encapsulation คือไรกันแน่ ?

หลักการของ Encapsulation มีง่ายๆคือ **มันเอาไว้ป้องกันไม่ให้คนอื่นเข้าถึงสิ่งที่เป็นของสำคัญภายใน Model** ของเรา \(ซึ่งในตัวอย่างคือยอดเงิน\) ดังนั้นเราก็จะใช้พวก **Access modifier** \(public, private, protected\) ต่างๆเข้ามาช่วยจัดการสิทธิ์ในการเข้าถึง member ต่างๆใน Model ของเรา และเราก็**สามารถสร้างช่องทางให้คนภายนอกเข้ามาทำงานกับ Model** ของเราโดยที่เราสามารถควบคุมการทำงานให้เป็นแบบที่เราอยากจะให้เป็นได้นั่นเอง

ซึ่งคำว่า Encapsulation มันหมายถึง **การเอาเข้าแคปซูล** ตามรูปเลย

![https://www.scientecheasy.com/2018/06/encapsulation-in-java-real-time-examples-advantages.html](../../.gitbook/assets/image%20%28245%29.png)

โดยความหมายของมันจริงๆก็คือ

* **เราเปิดให้คนอื่นเข้าใช้งานได้เท่าที่เราอยากเปิด** - ตามรูปคือโซนแคปซูลโปร่งใสที่มองเห็นตัวแปรและเมธอตที่เขาอยากให้เราเห็น
* **เราจะปิดข้อมูลในส่วนที่เราไม่อยากให้คนอื่นเข้ามายุ่ง** - ตามรูปคือโซนแคปซูลสีแดงที่เรามองไม่เห็นเลยว่าข้างในมีอะไรอยู่

ซึ่งหลักในการทำ **Encapsulation** เลยทำให้เราสรุปสั้นๆตามที่สรุปไว้ด้านบนสุดได้ว่า

> ✨_Encapsulation เป็นการจัดการข้าวของภายในตัว Model ของเรา เพื่อให้คนอื่นเรียกใช้งานได้ง่ายๆ ในขณะที่เรายังป้องกันข้อมูลที่สำคัญจากภายนอกเอาไว้ได้_

ซึ่งหลักในการคิดเรื่อง Encapsulation ก็จะมีหลักในการทำ **Information Hiding** รวมอยู่ด้วย

## 🤔 ทำ Encapsulation แล้วดีไง ?

หนึ่งในข้อดีที่เราเห็นได้ชัดเจนคือ **เราสามารถควบคุมสิ่งต่างๆจากภายนอกได้** แต่มันก็ยังมีอีกเรื่องนั่นคือ การทำ Encapsulation มันเกิดจากแนวคิดของ **Component base** ซึ่งคำว่า Component ในวงการนี้เราหมายถึง **มันสมบูรณ์แบบด้วยตัวมันเอง** นั่นหมายความว่า Model ที่เราสร้างไว้นั้น เราสามารถเอาไปใช้งานได้เลยไม่ต้องทำอะไรกับมันเพิ่มเติมอีกนั่นเอง ยกตัวอย่างง่ายๆคือ ถ้าเราอยากจะใช้ **BankAccount** แล้วต้องการฝากเงินเข้าไปก็แค่เรียกใช้แบบโค้ดด้านล่างได้เลย

```csharp
var acc = new BankAccount();
acc.OwnerName = "Saladpuk";
acc.Deposit(500);
```

จากการใช้งานโค้ดด้านบนจะเห็นว่า เราไม่ต้องสนใจว่าโค้ดภายใน BankAccount มันทำงานจริงๆยังไง แต่เรารู้ว่าแค่เรียกใช้งานแบบนี้มันก็ทำการฝากเงินเข้าบัญชีให้เราได้ก็พอ เพราะว่า BankAccount มันเป็น Component ที่สมบูรณ์แบบด้วยตัวมันเองแล้ว

ดังนั้นเวลาที่เราใช้งาน Component ต่างๆ เลยทำให้เราเขียนโปรแกรมได้ไวขึ้น เพราะแต่ละ Component มันก็สามารถทำงานได้โดยที่เราไม่ต้องไปยุ่งกับการทำงานภายในของมัน เรามีหน้าที่แค่เรียกใช้งานมันอย่างเดียวก็พอ สุดท้ายเราก็เอา Component ต่างๆมาใช้งานร่วมกันทำให้เกิดการทำงานแบบใหม่ได้นั่นเอง

## 💡 หลักในการคิด

เวลาที่เราจะทำ Encapsulation ให้ดูตัว Model ที่เรากำลังออกแบบก่อน ว่ามันมีข้อมูลที่เป็น sensitive data อะไรบ้าง? แล้วเราจะจัดการกับข้อมูลพวกนั้นยังไง? คนอื่นเข้าถึงได้ไหม? ถ้าเข้าถึงได้เราจะควบคุมคนที่เข้ามาเรียกใช้ยังไง? โดยทั้งหมดนี่มีวัตถุประสงค์เดียวคือ **การควบคุม** เพื่อให้เกิดเป็น Component ที่พร้อมนำไปใช้งานนั่นเอง

