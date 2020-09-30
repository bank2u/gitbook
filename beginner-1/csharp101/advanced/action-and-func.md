# Action & Func

## 😢 ปัญหา

ทุกครั้งที่เราจะทำงานกับ delegate มันวุ่นวาย เพราะเราต้องสร้าง delegate ที่มี signature ที่อยากได้ก่อนเสมอถึงจะเริ่มใช้งานมันได้ มีวิธีไหนที่ทำงานกับมันได้ง่ายๆอีกไหม ?

ถ้าอ่านแล้ว งงๆ ผมขอยกตัวอย่างว่า สมมุติผมอยากมีจะเก็บ method ในโค้ดด้านล่างนี้

```csharp
static void AwesomeMethod()
{
    Console.WriteLine("Hello world");
}
```

ไว้ในตัวแปรซักตัว ผมก็ต้องสร้าง delegate ขึ้นมาก่อน ตามนี้

```csharp
delegate void SALADPUK_DELEGATE();
```

พอมีโค้ดด้านบนละ เราก็ถึงจะเริ่มไปสร้างตัวแปรมาเก็บ method AwesomeMethod ได้นั่นเอง ตามโค้ดด้านล่าง

```csharp
static void Main(string[] args)
{
    SALADPUK_DELEGATE d = AwesomeMethod;
    d();  // Hello world
}
```

ซึ่งปัญหาคือถ้าจะทำแบบนี้เราก็ต้องไปสร้าง delegate ทุกครั้งเลยใช่ไหม ? มีง่ายกว่านี้ไหม?

{% hint style="info" %}
**แนะนำให้อ่าน**  
หากยังไม่ได้อ่านเรื่อง **delegate** จงไปอ่านก่อนซะ ไม่นานหรอก  
[Saladpuk - Delegates](https://saladpuk.gitbook.io/learn/beginner-1/csharp101/advanced/delegates)
{% endhint %}

## 😄 วิธีแก้ปัญหา

การแก้ปัญหาสั้นสุดแสนจะง่าย เพราะใน C\# เขาได้เตรียมสิ่งที่เรียกว่า `Action` กับ `Func` มาให้เราเล่นแล้วยังไงล่ะ ดังนั้นไปดูกันทีละตัวเลยละกันนะ

## 🔥 Action

สมมุติว่าเราอยากเก็บ method ไว้ในตัวแปรซักตัว เราก็สามารถใช้ Action ไปเก็บเจ้า method นั้นได้เลย แต่เงื่อนไขคือเจ้า **method ที่จะเก็บลงใน Action ได้นั้น มันจะต้องมี return type เป็น void เท่านั้น**นะจ๊ะ

เช่น ผมต้องการจะเก็บ method ด้านล่างนี้ไว้ในตัวแปรซักตัว

```csharp
static void AwesomeMethod()
{
    Console.WriteLine("Hello world");
}
```

เราก็สามารถสร้าง Action มาเก็บมันได้เลย เห็นมะเรียบง่ายดี ไม่ต้องสร้าง delegate ให้วุ่นวาย

```csharp
static void Main(string[] args)
{
    Action a = AwesomeMethod;
    a();
}
```

**แบบรองรับ parameters**

หรือถ้าเราอยากเก็บ method ที่มี parameter เอาไว้ในตัวแปรบ้างล่ะ เช่น ในโค้ดด้านล่าง

```csharp
static void CalculatorMethod(int a, int b, int c)
{
    var result = a + b + c;
    Console.WriteLine($"The result is: {result}");
}
```

เราก็สามารถสร้าง Action ที่มี signature ตรงกับ method ของเราได้เช่นกัน โดยการใช้ Generic เข้ามาช่วย ตามโค้ดด้านล่าง

```csharp
static void Main(string[] args)
{
    Action<int, int, int> a = CalculatorMethod;
    a(1, 5, 7);
}
```

{% hint style="info" %}
**แนะนำให้อ่าน**  
ใครยังไม่ได้อ่านเรื่อง Generic แนะนำให้ไปอ่านซะเพราะหลังๆเราจะใช้มันบ่อยมาก จนเรียกว่าไม่รู้จักมันแล้วละก็ชีวิตจะวุ่นวายขึ้นเยอะเลยเวลาเขียน C\#  
[Saladpuk - Generic](https://saladpuk.gitbook.io/learn/beginner-1/csharp101/advanced/generic)
{% endhint %}

## 🔥 Func

หลังจากที่เราเห็น action ไปเรียบร้อยละ คราวนี้ก็จะเป็นคราวของ Func บ้าง ซึ่งสิ่งที่มันทำได้นั้นก็จะเหมือนกับ action เลย ต่างกันแค่ **มันเอาไว้เก็บ method ที่มี return type นั่นเอง**

เช่นผมต้องการจะเก็บ method ตัวนี้ไว้ในตัวแปรซักตัว

```csharp
static double Add(int first, int second)
{
    var result = first + second;
    return result;
}
```

สิ่งที่เราต้องทำก็คือ ทำการสร้าง Func ที่มี signature ตรงกับ method ที่เราต้องการจะเก็บค่ามันไว้นั่นเอง ตามโค้ดด้านล่าง

```csharp
static void Main(string[] args)
{
    Func<int, int, double> f = Add;
    var result = f(5, 9);
    Console.WriteLine(result);
}
```

**อธิบายโค้ดด้านบนนิดหน่อย**  
เจ้า Func ก็เหมือนกับ Action แหละ ต่างกันแค่ตัวตัว Generic ตัวสุดท้ายของมันจะต้องเป็น return type ของ method นั่นเอง ซึ่งเจ้า method Add มี return type เป็น double ดังนั้นเลยทำให้ Func ของเราจะต้องมี Generic ตัวสุดท้ายเป็น double ด้วยนั่นเอง

## 💡 Anonymous

ในการเขียนโค้ดบางทีเราก็อยากได้ method ใหม่ แต่ไม่อยากเขียน method นั้นไว้ในคลาส เพราะมันจะทำให้คลาสของเรารกใช่ป่ะ ดังนั้นเราก็สามารถสร้าง method ในรูปแบบที่เรียกว่า anonymous ได้ครับ ซึ่งมันทำได้หลายแบบเลย ตามนี้

### Delegate

เป็นการสร้าง anonymous method โดยการใช้ delegate เข้ามาช่วย

{% hint style="info" %}
**คำแนะนำ**  
วิธีการนี้เราจะไม่ค่อยได้เห็นแล้วนะ เพราะส่วนใหญ่เราจะใช้ **Lambda** แทนแล้วครับ แต่แค่อยากให้รู้ว่ามันก็เขียนแบบนี้ได้นะ เพราะในสมัยก่อนที่ C\# 3.0 ยังไม่ออก เรายังไม่มี lambda มาใช้ยังไงล่ะ
{% endhint %}

สมมุติว่าผมอยากได้ method ซักตัว ที่มันทำงานแบบโค้ดตัวนี้

```csharp
static void Add(int first, int second)
{
    var result = first + second;
    Console.WriteLine(result);
}
```

เราก็สามารถใช้ delegate ร่วมกับ Action ได้ครับ ก็จะได้ออกมาเป็นแบบนี้

```csharp
static void Main(string[] args)
{
    Action<int, int> a = delegate (int first, int second)
    {
        var result = first + second;
        Console.WriteLine(result);
    };
    a(5, 9);
}
```

เพียงเท่านี้เราก็สามารถเพิ่ม method ใหม่ๆเข้าไปได้ละ โดยที่โค้ดเราก็จะรถเรื่อง method ละแต่ไปรถเรื่องตัวแปรแทน ฮ่าๆ

### Lambda

เป็นการสร้าง anonymous method โดยการใช้ lambda เข้ามาช่วย ซึ่งเราจะพบโค้ดลักษณะนี้บ่อยมากถึงมากที่สุดในการเขียน C\# ครับ

สมมุติว่าผมอยากได้ method ซักตัว ที่มันทำงานแบบโค้ดตัวนี้

```csharp
static void Add(int first, int second)
{
    var result = first + second;
    Console.WriteLine(result);
}
```

เราก็สามารถใช้ lambda ร่วมกับ Action ได้ครับ ก็จะได้ออกมาเป็นแบบนี้

```csharp
static void Main(string[] args)
{
    Action<int, int> a = (first, second)=>
    {
        var result = first + second;
        Console.WriteLine(result);
    };
    a(5, 9);
}
```

{% hint style="info" %}
**แนะนำให้อ่าน**  
Lambda คืออะไรสามารถอ่านได้จากบทความนี้ครัช  
[Saladpuk - Lambda expression](https://saladpuk.gitbook.io/learn/beginner-1/csharp101/advanced/lambda-expression)
{% endhint %}

## 🤔 ทำไมต้องใช้ของพวกนี้ด้วย ?

หลายๆคนอาจจะเริ่มมีคำถามว่า ทำไมไม่เรียกใช้ method มันไปตรงๆเลยล่ะ จะมามัวเก็บเป็นตัวแปรไว้ทำไมใช่ป่ะ

คำตอบก็คือ **ใช่ครับเราไม่ต้องใช้ของพวกนี้เลยก็ได้**เพราะเราเรียกใช้ method ได้เลยสะดวกดี แต่ในบางสถานะการณ์ ถ้าเราไม่ใช้ความสามารถของพวกนี้มันจะทำให้เราทำงานลำบากมาก ยกตัวอย่างเช่น เราอยากจะให้โปรแกรมของเราไปทำงาน A ให้เสร็จก่อน และเมื่อมันทำงานเสร็จแล้ว ค่อยกลับมาเรียกใช้งาน B ต่อนะ ซึ่งเราเรียกลักษณะการทำงานแบบนี้ว่า **Callback** ครับ ดังนั้นมาดูกันว่าการใช้ action หรือ func มาช่วยเรื่องการทำ callback นั้นมันจะช่วยแก้ปัญหานี้ได้ยังไงกัน

สมมุติว่าผมมีคลาสตัวนึงที่มี method 2 อัน

```csharp
static void A()
{
    Console.WriteLine("A");
}

static void B()
{
    Console.WriteLine("B");
}
```

ซึ่งผมต้องการให้มันทำงาน method A เสร็จก่อน แล้วค่อยเรียกใช้ method B เราจะทำยังไง? สมมุติว่าการทำงานของ A มันเป็น asynchronous

{% hint style="info" %}
**Asynchronous & Synchronous**  
โดยปรกติโค้ดที่เราเขียนอยู่นั้นมันจะทำงานในลักษณะที่เรียกว่า **Synchronous** ซึ่งมันจะรอจนกว่าโค้ดบรรทัดนั้นจะทำงานจนเสร็จถึงจะไปทำงานบรรทัดอื่นต่อ แต่การทำงานของ **Asynchronous** จะตรงกันข้ามกัน คือมันจะไม่รอ เมื่อสั่งทำงานโค้ดที่เป็น asynchronous ไปละ มันก็จะไปทำงานบรรทัดต่อไปเลย

ซึ่งในตัวอย่างคือถ้าเราเรียกใช้งาน method A ปุ๊ป มันก็จะไปทำงานอย่างอื่นต่อ ไม่รอให้ method A ทำงานจนเสร็จ ดังนั้นถ้าเราเรียก method B ต่อจากการเรียก method A มันจะมีโอกาสทำงาน A กับ B พร้อมกันได้ ซึ่งทำให้ลำดับการทำงานไม่ตรงตามที่เราต้องการนั่นเอง
{% endhint %}

คำตอบก็คือ เราก็แค่ส่ง method B เข้าไปเป็น parameter ของ method A ยังไงล่ะ ซึ่งพอ method A ทำงานเสร็จ มันก็จะเรียกใช้ method B ต่อเลยนั่นเอง ดังนั้นโค้ดเราก็จะออกมาเป็นภาพนี้

```csharp
static void A(Action callback)
{
    Console.WriteLine("A");
    callback();
}

static void B()
{
    Console.WriteLine("B");
}
```

