---
description: '�� ภาษา C# เวอร์ชั่น 8.0 มีอะไรใหม่ๆบ้าง'
---

# 💡 C\# version 8.0

หลายๆคนอาจจะเคยเห็นในอินเตอร์เน็ทเขาเขียนภาษา C\# ในรูปแบบที่เราไม่เคยเห็นมาก่อน แล้วพอเราไปทำตามก็เขียนแบบนั้นไม่ได้ นั่นเป็นเพราะภาษา C\# ในแต่ละเวอร์ชั่นเขาได้มีการเพิ่มความสามารถใหม่ๆเข้าไปเสมอนั่นเอง แล้วถ้าเราอยากใช้ความสามารถใหม่ๆพวกนั้น เราก็ต้องทำการอัพเดทตัว C\# เวอร์ชั่นของเราด้วยถึงจะใช้ได้ ดังนั้นในรอบนี้เราจะมาดูกันว่า C\# เวอร์ชั้น 8.0 จะมีความสามารถอะไรใหม่ๆมาให้เราลองเล่นกันบ้าง

## 🔥 Readonly members

ใน struct ได้ถูกเพิ่มความสามารถให้ใช้ **`readonly`** ให้กับ member ของมันได้ เพื่อเป็นการกำหนดให้ member ตัวนั้นๆต้องไม่ไปทำการเปลี่ยนค่าของ member อื่นๆ เช่นโค้ดด้านล่าง จะเป็น struct ที่มีการไปแก้ไขค่าให้กับ member อื่น

```csharp
 public struct Something
 {
     public int X { get; set; }
     
     public override string ToString()
     {
         X = 77.9;
         return $"The value of X is: {X}";
     }
 }
```

จากโค้ดด้านบนถ้าเราไม่อยากให้ **ToString\(\)** แก้ไขเปลี่ยนแปลง member อื่นๆได้ เราก็สามารถใส่ `readonly` เข้าไปได้ตามโค้ดด้านล่างครับซึ่งมันจะบังคับให้เราแก้ไข member อื่นไม่ได้เลย เพราะมันจะ compile ไม่ผ่านนั่นเอง

```csharp
public struct Something
 {
     public int X { get; set; }
     
     public readonly override string ToString()
     {
         // X = 77.9; ต้องเอาบรรทัดนี้ออกไม่งั้น compile ไม่ผ่าน
         return $"The value of X is: {X}";
     }
 }
```

## 🔥 Default interface members

โดยปรกติ Interface นั้นเราจะไม่สามารถมี member ที่มี implementation ภายในได้ แต่ด้วยความสามารถใหม่นี้จะทำให้เราสร้าง member ที่มี implementation ภายใน interface ได้แล้ว โดยโค้ดด้านล่างเป็นการเขียน interface แบบเดิม

```csharp
public interface ICalculator
{
    int Add(int a, int b);
}
```

จากโค้ดด้านบนเราจะเห็นว่ามันไม่สามารถมี implementation ภายใน method **Add** ได้ ส่วนโค้ดด้านล่างจะเห็นว่าเราสามารถใส่ implementation เข้าไปได้แล้ว

```csharp
public interface ICalculator
{
    int Add(int a, int b)
    {
        return a + b;
    }
}
```

{% hint style="success" %}
ความต่างของ interface กับ abstract กับจะเหลือเรื่อง multi inheritance เท่านั้นละ
{% endhint %}

## 🔥 More patterns in more places

ต่อยอดความสามารถของ C\# version 7 เรื่องการทำ pattern matching ของคำสั่ง `switch` โดยมันมีความสามารถเพิ่มเข้ามาตามนี้เลย

### 💡 Switch expressions

เราสามารถย่อการทำงานของคำสั่ง `switch`ให้มันสั้นลงได้แล้ว ยกตัวอย่างเช่นเราต้องทำงานกับ enum ที่มีค่าตามโค้ดล่างนี้

```csharp
public enum MainColor
{
    Red,
    Green,
    Blue
}
```

ถ้าเป็นโค้ดแบบเดิมเราจะต้องเขียนการทำงานภายใน method เป็นแบบนี้

```csharp
public static RGBColor FromMainColor(MainColor colorBand)
{
    switch (colorBand)
    {
        case Rainbow.Red:
            return new RGBColor(0xFF, 0x00, 0x00);
        case Rainbow.Green:
            return new RGBColor(0x00, 0xFF, 0x00);
        case Rainbow.Blue:
            return new RGBColor(0x00, 0x00, 0xFF);
        default:
            throw new ArgumentException();
    };
}
```

แต่ด้วยความสามารถใหม่เราจะสามารถย่อมันเข้ามาเป็นแบบนี้ได้

```csharp
public static RGBColor FromMainColor(MainColor colorBand) =>
    colorBand switch
    {
        Rainbow.Red    => new RGBColor(0xFF, 0x00, 0x00),
        Rainbow.Green  => new RGBColor(0x00, 0xFF, 0x00),
        Rainbow.Blue   => new RGBColor(0x00, 0x00, 0xFF),
        _              => throw new ArgumentException(),
    };
```

### 💡 Property patterns

ถัดมาเราสามารถ map ค่าของ property ของ object ภายในคำสั่ง `switch` ได้เลย เช่นเรามีคลาส `Address` ที่เก็บรหัสจังหวัดไว้ใน property ที่ชื่อว่า `State` ดังนั้นเราก็สามารถเอา object ของ address มาใช้กับคำสั่ง switch ได้ตามด้านล่างเลย

```csharp
public string GetStateCode(Address location) =>
    location switch
    {
        { State: "BKK" } => "10",
        { State: "KKN" } => "40",
        { State: "UBN" } => "76",
        _ => string.Empty
    };
```

### 💡 Tuple patterns

ความสามารถนี้ต่อยอดจากความสามารถของ C\# version 7 เรื่อง tuple ซึ่งมันจะทำให้เราสามารถ map tuple เข้ากับคำสั่ง `switch` ได้แล้ว เช่นผมจะเขียนโค้ดในการตรวจว่าเกมเป่ายิงฉุบถ้าผลออกมาแบบนี้แล้วผลลัพท์จะเป็นแพ้ชนะหรือเสมอ โดยการส่ง string เข้าไป 2 ตัว เราก็สามารถใช้ความสามารถใหม่แบบเขียนเป็นแบบนี้ได้เลย

```csharp
public string checkGameResult(string first, string second)
    => (first, second) switch
    {
        ("rock", "paper") => "Lose",
        ("rock", "scissors") => "Win",
        ("paper", "rock") => "Win",
        ("paper", "scissors") => "Lose",
        ("scissors", "rock") => "Lose",
        ("scissors", "paper") => "Win",
        (_, _) => "tie"
    };
```

### 💡 Positional patterns

จากความสามารถเดิมของ C\# version 7 เราสามารถทำการจับคู่ระหว่างตำแหน่งของ `deconstructor` ของ object ได้ เช่นจากเดิมเวลาที่เราจับคู่เราจะต้องจับแบบนี้

```csharp
public string Describe(object obj)
{
    switch(obj)
    {
        case Rectangle r when r.Length == 10 && r.Width == 10:
            return "Found 10x10 rectangle";
        ...
    }
}
```

แต่ด้วยความสามารถใหม่เราจะสามารถจับคู่แบบนี้ได้

```csharp
public string Describe(object obj)
{
    switch(obj)
    {
        case Rectangle (10, 10): 
            return "Found 10x10 rectangle";
        ...
    }
}
```

### 💡  R**ecursive patterns**

จากที่ว่ามาทั้งหมด เราสามารถเขียน pattern ที่ซ้อน pattern เข้าไปได้ด้วย ตามนี้เลย

```csharp
switch( p.FirstName, p.MiddleName, p.LastName)
{
    case (string f, string m, string l):
        return $"{f} {m[0]}. {l}";
        
    case (string f, null, string l):
        return $"{f} {l}";
    ...
}
```

## 🔥 using declarations

แต่เดิมเราสามารถใช้คำสั่ง `using` เพื่อสร้างตัวแปรที่มันจะคืนทรัพยากรแบบอัตโนมัติเมื่อจบ block ของมันได้แล้ว ตามโค้ดด้านล่างเราจะเห็นว่า using แบบเดิมเมื่อ block ของมันจบลง ตัวแปรนั้นก็ทำการคืนทรัพยากรหลังจาก block ทันที

{% code title="" %}
```csharp
public void WriteSomeThing()
{
    using (var writer = new StreamWriter("file.txt"))
    {
        ...
    } // ตัวแปร writer จะเรียก Dispose ที่นี่
    ...
}
```
{% endcode %}

ด้วย C\# version 8 เราสามารถสร้างตัวแปรที่มันอยู่ได้ตลอดภายใน block ที่มันอยู่ และจะคืนทรัพยากรเมื่อมันจบ block ของมันแล้วได้ ตามตัวอย่างด้านล่าง

{% code title="" %}
```csharp
public void WriteSomeThing()
{
    using var writer = new StreamWriter("file.txt");
    ...
    // ตัวแปร writer จะเรียก Dispose ที่นี่
}
```
{% endcode %}

## 🔥 Static local functions

ความสามารถใหม่ตัวนี้ต่อยอดจาด C\# version 7 เราจะสามารถสร้าง local function ได้ แตมาใน version 8 นี้เราจะสามารถสร้าง local function ที่เป็น **static** ได้ โดยความสามารถใหม่นี้จะป้องกันไม่ให้เราเผลอไปเรียกใช้งานตัวแปรที่เป็นพวก local variable นั่นเอง ลองดูโค้ดด้านล่างนี้ที่เขียนด้วย local function ของ C\# version 7 ซึ่งเราอาจจะเผลอไปเรียกใช้งาน local variable ได้

{% code title="" %}
```csharp
public void SomeMethod()
{
    int a = 7;
    
    void LocalFunction() => a = 0;
}
```
{% endcode %}

โค้ดด้านล่างคือความสามารถใหม่ที่เป็น static local function ซึ่งมันจะป้องกันไม่ให้เราไปเรียกใช้ local variable ได้เลย ซึ่งถ้าเผลอไปเรียกมันจะ compile ไม่ผ่านทันที

{% code title="" %}
```csharp
public void SomeMethod()
{
    int a = 7;
    
    static void LocalFunction(int b) => b = 0;
}
```
{% endcode %}

## 🔥 Disposable ref structs

ใน C\# version 7 เขาได้เพิ่มความสามารถใหม่เข้ามานั่นคือคำสั่ง `ref` ซึ่งจะทำให้เราสามารถอ้างอิงของในรูปแบบ reference type ได้ ซึ่งจากไอเดียนี้เราก็จะสามารถสร้าง struct ที่เป็น ref ได้เช่นกันตามโค้ดด้านล่าง

{% code title="" %}
```csharp
ref struct Something
{
}
```
{% endcode %}

แต่ด้วยข้อจำกัดบางประการเลยทำให้เจ้า struct ไม่สามารถ implement interface ได้เลย ดังนั้นเวลาที่เราสร้าง struct ที่จำเป็นต้องคืนทรัพยากรหลังใช้งานเสร็จก็จะมีปัญหา เพราะเราจะไป implement **IDisposable** ไม่ได้นั่นเอง ทำให้มีโอกาสสูงที่จะลืมคืนทรัพยากร และ แน่นอนว่าคำสั่ง **`using`** ก็ไม่สามารถใช้ได้ด้วยนั่นเอง

จากที่เกริ่นมาทั้งหมดนั้นเจ้า C\# version 8 ได้แก้ปัญหาให้ struct มีความสามารถใช้ Dispose ได้แล้วโดยที่ไม่ต้อง implement IDisposable เลย โดยแค่สร้าง method Dispose ให้เป็น **public** เพียงเท่านี้เอง ตามโค้ดด้านล่าง

{% code title="" %}
```csharp
ref struct Something
{
    public void Dispose()
    {
    }
}
```
{% endcode %}

ส่วนใครที่ต้องการเรียกใช้ struct ตัวนี้ก็สามารถใช้คำสั่ง `using` ลงไปดื้อๆได้เลยตามโค้ดด้านล่างนี้

{% code title="" %}
```csharp
using (var st = new Something())
{
    ...
}
```
{% endcode %}

## 🔥 Nullable reference types

เวลาที่เราทำงานกับ reference type นั้นมีบ่อยครั้งที่เราอาจเจอ error ประเภท **NullReferenceException** เด้งมาให้เห็นบ่อยๆ ดังนั้นใน C\# version 8 นี้เขาได้เพิ่มการแจ้งเตือนแบบใหม่ขึ้นมา เพื่อแยกของต่างๆออกจากกันว่าข้อมูลตัวนี้เป็น null ได้ ตัวนี้ไม่มีทางเป็น null ซึ่งการแจ้งเตือนทั้งหมดจะแสดงผลผ่าน **warning** ออกมานั่นเอง ส่วนถ้าอยากเปิดการใช้การแจ้งเตือนแบบใหม่ เราจะต้องใส่แทก **`#nullable enable`** ตัวนี้เข้าไป เพื่อบอกว่าเราจะเปิดใช้งานการตรวจสอบ null แล้วนะตามโค้ดด้านล่าง

{% code title="" %}
```csharp
#nullable enable
```
{% endcode %}

ส่วนในการใช้งานเราจะต้องทำการระบุว่าตัวแปร reference type ไหนบ้างที่เป็น null ได้ โดยตัวแปรพวกนั้นก็จะใช้คำสั่ง `?` ด้านหลัง data type เพื่อเป็นการประกาศว่ามันเป็น nullable นั่นเอง ตามโค้ดด้านล่างเลย

{% code title="" %}
```csharp
public class Student
{
    public string FirstName { get; set; }
    public string? LastName { get; set; } // เป็น null ได้
}
```
{% endcode %}

## 🔥 Asynchronous streams

จาก C\# version 5 นั่นเราได้รู้จักกับคำสั่ง `async` กันไปแล้ว แต่ถ้าเราใช้งานเราจะพบว่ามันมีปัญหาในการทำงานเวลาที่เราจะค่อยๆส่งข้อมูลออกมาเรื่อยๆ ในลักษณะของ streaming ดังนั้นใน C\# version 8 นี้เขาก็ได้แก้ปัญหานี้ให้เราแล้วโดยใช้คำสั่ง `IAsyncEnumerable<T>` นั่นเอง จากโค้ดด้านล่างผมจะส่งเลข 1-20 มาเป็น streaming ผมก็จะเขียนได้ว่า

{% code title="" %}
```csharp
public async IAsyncEnumerable<int> GenerateSequence()
{
    for(var i = 1; i <= 20; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}
```
{% endcode %}

ส่วนคนที่เรียกใช้ method นี้ก็สามารถใช้คำสั่ง `await` เพื่อทำงานร่วมกับคำสั่ง `foreach` ได้เลยด้วย ตามโค้ดด้านล่าง

{% code title="" %}
```csharp
await foreach (var number in GenerateSequence())
{
    Console.WriteLine(number);
}
```
{% endcode %}

## 🔥 Indices and ranges

ความสามารถใหม่ในการเข้าถึงข้อมูลที่เป็นตระกูล Array collection

### 💡 Indices

สมัยก่อนถ้าเราจะเข้าไปทำอะไรซักอย่างกับ Array แล้วล่ะก็ เราจะต้องเข้าถึงผ่านเจ้าตัวที่เรียกว่า `indexer` เช่น ขอเข้าถึงข้อมูลตัวแรก \[0\] หรือข้อมูลตัวที่ 3 \[2\] ประมาณนี้ แต่ด้วยความสามารถใหม่นี้เราจะมีตัวอ้างอิง Array collection แบบกลับหลังได้ด้วย ตามโค้ดด้านล่างนี้เลย

{% code title="" %}
```csharp
var words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};              // 9 (words.Length)    ^0
```
{% endcode %}

จากโค้ดด้านบนจะเห็นว่าเราสามารถอ้างอิงข้อมูล Array collection แบบกลับหลังได้โดยใช้เครื่องหมาย `^` นั่นเอง เช่นผมอยากได้ข้อมูลตัวสุดท้ายก็จะเขียนออกมาแบบนี้ได้เลย

{% code title="" %}
```csharp
Console.WriteLine($"The last word is {words[^1]}");
// "dog"
```
{% endcode %}

### 💡 Ranges

นอกจากการเข้าถึง Array collection แบบใหม่แล้ว เขายังรองรับการทำงานกับ Array collection แบบเข้าถึงข้อมูลเป็นช่วงได้ด้วย เช่นผมอยากได้ข้อมูลตัวที่ 2 ถึงตัวที่ 4 นั่นก็คือ `quick` `brown` `fox` ผมก็สามารถเขียนโค้ดง่ายๆตามด้านล่างนี้เลย

{% code title="" %}
```csharp
var quickBrownFox = words[1..4];
// { quick, brown, fox }
```
{% endcode %}

จากโค้ดด้านบนเราจะมีคำสั่งใหม่นั่นคือคำสั่ง `..` ซึ่งจะเป็นตัวบอกว่าจะเอาข้อมูลตั้งแต่ตัวไหนและสิ้นสุดที่ตัวไหนนั่นเอง และแน่นอนมันก็ใช้กับการอ้างอิงแบบกลับด้านได้เช่นกัน เช่นผมอยากได้ข้อมูล 2 ตัวสุดท้าย `lazy` `dog` ผมก็สามารถเขียนแบบนี้ได้เช่นกัน

{% code title="" %}
```csharp
var lazyDog = words[^2..^0];
// { lazy, dog }
```
{% endcode %}

และที่เจ๋งไปกว่านั้นของคำสั่ง `..` ก็คือถ้าเราไม่ระบุว่าจะเอาข้อมูลจากไหนถึงไหน มันก็จะกวาดมาทั้งหมดเลย เหมือนกับคำสั่ง `*` ของพวก SQL นั่นเอง

{% code title="" %}
```csharp
var allWords = words[..]; // เอาข้อมูลมาทั้งหมดเลย
var firstPhrase = words[..4]; // เอาตั้งแต่ index แรกถึง index 4
var lastPhrase = words[6..]; // เอาตั้งแต่ index 6 ถึงตัวสุดท้าย
```
{% endcode %}

ยังไม่หมดเพียงเท่านี้ เรายังสามารถสร้างตัวแปรแบบ `Range` เก็บเอาไว้ใช้งานได้ด้วยนะ

{% code title="" %}
```csharp
Range phrase = 1..4;
var text = words[phrase];
```
{% endcode %}

## 🔥 Null-coalescing assignment

เวลาที่เราทำงานกับ reference type หรือ Nullable หรือพูดง่ายๆคือข้อมูลที่มันอาจจะเป็น null ได้ เราจะต้องคอยระวังเวลาใช้งานมันเสมอเพราะไม่งั้นจะได้ NullReferenceException โผล่มาจ๊ะเอ๋ได้ ดังนั้นเราอาจจะต้องตรวจค่ามันว่าเป็น null หรือเปล่าเพื่อป้องกันมันเป็น null ก่อนเรียกใช้งานราวๆนี้

{% code title="" %}
```csharp
IEnumerable<int> numbers = null;
if (numbers == null)
{
    numbers = Enumerable.Empty<int>();
}
```
{% endcode %}

แต่ด้วยความสามารถใหม่ของ C\# version 8 เราสามารถใช้คำสั่ง `??=` เพื่อเป็นการบอกว่า ถ้าค่าด้านซ้ายเป็น null ให้เอาค่าด้านขวาไปกำหนดให้ค่าด้านซ้ายได้เลย แต่ถ้าไม่เป็น null ให้ข้ามไป ดังนั้นโค้ดใหม่เราก็จะออกมาเหลือเพียงแค่นี้

{% code title="" %}
```csharp
IEnumerable<int> numbers = null;
numbers ??= Enumerable.Empty<int>();
```
{% endcode %}

## 🔥 Unmanaged constructed types

ตั้งแต่ C\# version 7.3 ลงมานั้นตัว `constructed type` จะไม่สามารถเป็น `unmanaged type` ได้ แต่ในตัว C\# version 8 นั้นถ้า construct value นั้นมีแต่ members ที่เป็น unmanaged types แล้วล่ะก็ตัวมันก็จะเป็น unmanaged เช่นกัน เช่นผมมี constructed type ตัวนึงตามโค้ดด้านล่างนี้

{% code title="" %}
```csharp
public struct Coords<T>
{
    public T X;
    public T Y;
}
```
{% endcode %}

แล้วผมไปทำการสร้าง Coords โดยส่ง T เป็น unmanaged types เช่น `Coords<int>` `Coords<bool>` บลาๆ ซึ่งภายในของ Coords นั้นมีแต่ unmanged types อย่างเดียว ผมก็จะสามารถใช้ `pointer` หรือ `stackalloc` ไปเล่นกับมันได้นั่นเอง

{% code title="" %}
```csharp
Span<Coords<int>> coordinates = stackalloc[]
{
    new Coords<int> { X = 0, Y = 0 },
    new Coords<int> { X = 0, Y = 3 },
    new Coords<int> { X = 4, Y = 0 }
};
```
{% endcode %}

ปรกติผมไม่ได้เล่นกับ pointer มานักถ้าสนใจก็สามารถไปศึกษาเพิ่มเติมได้จากลิงค์ด้านล่างนี้ครับ

* [Microsoft Document - Stackalloc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/stackalloc) 
* [Microsoft Document - Unmanaged types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/unmanaged-types)

## 🔥 Enhancement of interpolated verbatim strings

หลังจากที่ C\# version 7 ได้นำเสนอความสามารถในการสร้างตัวอักษรแบบใหม่ด้วยเครื่องหมาย **`$`** กันแล้ว ถ้าเพื่อนๆหลายๆคนได้ลองใช้คู่กับเจ้าเครื่องหมาย **`@`** นี้แล้วละก็จะพบว่า มันทำงานร่วมกันไม่ได้ แต่ตอนนี้ C\# version 8 ก็ได้ออกมาแก้ไขให้มันสามารถใช้งานร่วมกันได้แล้ว เย่ๆ โดยจะเอาอันไหนขึ้นก่อนขึ้นหลังก็ได้ครับ ตามตัวอย่างด้านล่างเลย

{% code title="" %}
```csharp
 var fileName = test.jpg;
 var path1 = $@"c:\{filename}";
 var path2 = @$"c:\{filename}";
```
{% endcode %}

## 🎥 วีดีโอประกอบความเข้าใจ

{% embed url="https://www.youtube.com/watch?v=VdC0aoa7ung" %}



