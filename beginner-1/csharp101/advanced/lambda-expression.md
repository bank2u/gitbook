# Lambda expression

จากที่เคยพูดถึง `Generic`, `Delegate`, `Action` และ `Func` ไปเรียบร้อยแล้ว  คราวนี้ตัว **Lambda expression** จะเป็นตัวจะช่วยให้เราทำงานกับเจ้าของพวกนั้นได้ง่ายขึ้นนั่นเอง ซึ่งโดยปรกติการเขียน lambda มันมี 2 แบบครับคือ

### 1.เขียนแบบเต็ม \(Statement lambda\)

การเขียนแบบเต็มก็คือมีการสร้าง body ของมันออกมา แล้วข้างใน body นั้นเราจะให้มัน statements อยู่กี่ตัวก็ได้ ตามโค้ดด้านล่าง

```text
(input-parameters) => { <sequence-of-statements> }
```

### 2.เขียนแบบย่อ \(Expression lambda\)

การเขียนแบบย่อคือมันจะต้องไม่มี body และมีได้เพียง statement เดียวเท่านั้น ตามโค้ดด้านล่าง

```text
(input-parameters) => expression
```

หลังจากที่ได้เห็นโค้ดตัวอย่างทั้ง 2 แบบไป เราจะเห็นสัญลักษณ์แปลกๆนั่นก็คือเจ้า `=>` นั่นเองซึ่งเจ้าตัวนี้แหละคือการบอกว่า **มันคือ Lambda Expression นะ** โดยการใช้งาน lambda จะต้องประกอบด้วยของ 2 อย่างคือ 

* **Input parameters** - เป็นตัวที่เอาไว้กำหนดว่า ถ้ามีการเรียกใช้งาน lambda มันจะทำการ map parameter แต่ละตัวเข้ามาในชื่ออะไร
* **Expression** - เป็นตัวที่เอาไว้บอกว่า ถ้ามีการเรียกใช้งาน lambda แล้ว lambda จะต้องทำงานยังไง

จากที่ว่ามาอาจจะ งงๆ ว่ามันทำงานยังไง ดังนั้นเราไปดูตัวอย่างการใช้งานจริงกันเลยดีกว่าว่า ถ้าเราอยากได้ method ที่ทำงานเหมือนโค้ดด้านล่างนี้ เราจะเขียนโดยใช้ Lambda ยังไงดี

```csharp
static void SimpleMethod()
{
    Console.WriteLine("SimpleMethod");
}
```

## 🔥 Action

จากโค้ดตัวอย่าง ถ้าเราต้องการใช้ lambda มาช่วยกำหนดค่าให้กับ action ของเรา ก็จะได้ออกมาราวๆนี้

### **เขียนแบบเต็ม**

```csharp
Action simpleMethod = () =>
{
    Console.WriteLine("SimpleMethod");
};
```

### **เขียนแบบย่อ**

```csharp
Action simpleMethod = () => Console.WriteLine("SimpleMethod");
```

### แบบมี parameter ตัวเดียว

แล้วถ้า method ตัวนั้นมี parameter อยู่ด้วยล่ะ สมมุติว่าเป็นแบบนี้

```csharp
static void MethodWithParameter(int a)
{
    Console.WriteLine(a);
}
```

เราก็สามารถกำหนด parameter ในส่วนของ \(input-parameters\) ได้ยังไงล่ะ ตามนี้เลย

```csharp
Action<int> simpleMethod = (a) =>
{
    Console.WriteLine(a);
};
```

โดยเจ้าโค้ดด้านบนมันจะทำการ map เองว่า int ที่รับเข้ามา มันจะไปกำหนดค่าให้กับตัวแปร a นั่นเอง

### แบบมี parameters หลายตัว

คราวนี้ถ้ามันมี parameters หลายๆตัวบ้างล่ะ? ประมาณนี้

```csharp
static void MethodWithParameter(int a, string b)
{
    Console.WriteLine(a);
    Console.WriteLine(b);
}
```

เราก็สามารถเขียน lambda ออกมาเป็นแบบนี้ได้

```csharp
Action<int, string> simpleMethod = (a, b) =>
{
    Console.WriteLine(a);
    Console.WriteLine(b);
};
```

## 🔥 Func

คราวนี้ลองมาดู method แบบที่มี return type ดูบ้างว่ามันจะเป็นยังไง โดยสมมุติว่าเราต้องการแปลงเจ้าโค้ดตัวนี้ให้เป็น func บ้าง

```csharp
static int SimpleMethod()
{
    return 99;
}
```

### เขียนแบบเต็ม

```csharp
Func<int> simpleMethod = () =>
{
    return 99;
};
```

### เขียนแบบย่อ

```csharp
Func<int> simpleMethod = () => 99;
```

ดังนั้นในกรณีที่มี parameter ตัวเดียว หรือแบบมีหลายตัวก็จะเขียนเหมือนกับ action นะครับลองเอาไปรับเล่นดู

## 🔥 Delegate

ถึงคราวของเจ้า delegate ดูบ้างละ ซึ่งปรกติเราไม่น่าจะเจอโค้ดแบบนี้แล้วนะ เพราะมันเก่ามากละ โดยสมมุติว่าเรามี Method ง่ายๆอยู่ตัวนึง ประมาณนี้ละกัน

```csharp
static void SimpleMethod()
{
    Console.WriteLine("Saladpuk");
}
```

แล้วเราอยากทำ delegate ไปหา method ตัวนี้ โดยปรกติเราก็จะเขียนแบบนี้ชิมิ

```csharp
// สร้าง delegate ออกมาก่อน
delegate void DEMO_DELEGATE();

// ตอนเอา delegate ไปใช้งาน
DEMO_DELEGATE handler = Awesome;
```

จากโค้ดด้านบนทั้งหมด จะเห็นว่ามันเสียเวลาที่เราต้องไปสร้าง **SimpleMethod** ก่อน แล้วค่อยเอามากำหนดค่าให้กับเจ้า delegate ดังนั้นในรอบนี้เราก็จะเอา Lambda มาช่วยเพื่อให้โค้ดของเรากระชับขึ้น ตามโค้ดด้านล่างนี้

### เขียนแบบเต็ม

```csharp
DEMO_DELEGATE handler = () =>
{
    Console.WriteLine("Saladpuk");
};
```

### เขียนแบบย่อ

```csharp
DEMO_DELEGATE handler = () => Console.WriteLine("Saladpuk");
```

## 🎯 บทสรุป

โดยรวมๆการใช้ Lambda expression ก็จะเป็นช่องทางให้เราสามารถเขียนโค้ดได้ง่ายและไม่เวิ่นเว้อจนเกินไป โดยการสร้าง anonymous method ผ่านการใช้ lambda นั่นเอง ซึ่งจริงๆมันมีวิธีการใช้อีกเยอะม๊วก เช่น การทำงานกับ `async`, `await`, การทำงานกับ `tuples` ซึ่งผมจะยังไม่ขออธิบายไว้ในตรงนี้ละกันนะ เดี๋ยวมาอัพเดทเป็นเรื่อง advance ของ Lambda expression เอาดีกว่า

{% hint style="info" %}
**แนะนำให้อ่าน**  
เรื่องนี้สามารถไปศึกษาเพิ่มเติมได้จากต้นทางตามลิงค์ด้านล่างนี้เบย  
[Microsoft document - Lambda expression](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)
{% endhint %}

