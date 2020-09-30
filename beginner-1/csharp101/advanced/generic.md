# Generic

## 😢 ปัญหา

ในบางครั้งที่เราเขียนโค้ด เราก็อาจจะไม่รู้ก็ได้ว่าเจ้าตัวแปรตัวนี้มันควรจะมี Data type เป็นอะไรดี เพราะมันขึ้นอยู่กับว่าคนที่เรียกใช้มันจะส่งอะไรมาให้นั่นเอง 

**ตัวอย่าง:** เราอยากมีคลาสที่เอาไว้**เก็บข้อมูลอะไรก็ได้ไว้ 1 ตัว** ซึ่งตอนแรกอาจจะเก็บค่าตัวเลขไว้ ก็จะได้โค้ดออกมาแบบนี้

```csharp
public class SimpleStorage
{
    public int Value;
}
```

คำถามคือ แล้วถ้าเราอยากให้คลาสนั้นมันเก็บ double เข้าไปบ้างล่ะ หรืออาจจะเก็บ string บ้างล่ะ เราจะทำยังไงดี ?

{% hint style="danger" %}
**คำเตือน**  
เพื่อนๆบางคนอาจจะบอกว่า งั้นก็กำหนดให้มันเป็น **object** ไปดิ จะเก็บอะไรก็เก็บได้หมดเบย!! คำตอบคือใช่ครับทำแบบนั้นก็ทำงานได้ แต่โค้ดแบบนั้นมันจะทำให้ performance ตกลง เพราะเราต้องไปแปลงของต่างๆให้ไปเป็น object และ ตอนที่เราจะเอาค่ามันกลับมา เราก็ต้องทำการแปลงกลับมาด้วย ซึ่งเราเรียกเรื่องนี้ว่าการทำ [Boxing and Unboxing](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/boxing-and-unboxing) **แนะนำว่าอย่าทำ**
{% endhint %}

{% hint style="info" %}
**แนะนำให้อ่าน**  
การทำ Boxing and Unboxing นั้นจริงๆมันเป็นยังไง สามารถอ่านได้จากบทความนี้เลย [💡 Boxing & Unboxing](https://saladpuk.gitbook.io/learn/beginner-1/csharp101/advanced/boxing-and-unboxing)
{% endhint %}

## 😄 วิธีแก้ปัญหา

เราสามารถใช้สิ่งที่เรียกว่า **Generic** มาช่วยแก้ปัญหาที่ว่าได้ โดยหลักการคือมันจะให้คนที่เรียกใช้เป็นคนกำหนดเองว่า data type ที่เขาจะทำงานด้วยคืออะไรนั่นเอง

### 🔥 ใช้กับตัวแปร

จากโค้ดตัวอย่างด้านบน เราก็สามารถเอา Generic เติมเข้าไปได้เลยเป็นแบบนี้

```csharp
public class SimpleStorage<T>
{
    public T Value;
}
```

จะเห็นว่าบรรทัดที่ 1 ถูกเพิ่ม &lt;T&gt; ต่อท้ายเข้าไป ซึ่งเจ้านี่แหละคือ generic และส่วนบรรดทัดที่ 2 แทนที่เราจะกำหนดให้เป็น int เราก็เอาเจ้า T จากบรรทัดที่ 1 มาใส่ไว้นั่นเอง **อ่านแล้ว งงๆ อยู่ไม่เป็นไร ไปดูต่อกัน**

คราวนี้เวลาที่เราจะเรียกใช้คลาสนี้ เราก็จะต้องสร้าง object ของมันพร้อมกับบอกว่าเจ้าคลาสนี้จะทำงานกับ data type อะไรนั่นเอง เช่น อยากทำงานกับ int เราก็จะเขียนโค้ดออกมาเป็นแบบนี้

```csharp
static void Main(string[] args)
{
    SimpleStorage<int> s = new SimpleStorage<int>();
    s.Value = 55;
    Console.WriteLine(s.Value);
}
```

จากโค้ดด้านบนจะเห็นว่าตอนที่สร้าง object ของคลาส **SimpleStorage** เราจะต้องบอกว่า เจ้าคลาสนี้จงทำงานกับ **int** ซะ \(ในบรรทัดที่ 3\) ดังนั้นตัวแปรที่ชื่อว่า **Value** เลยมี data type เป็น int นั่นเอง เลยทำให้มันเก็บค่าตัวเลขลงไปได้ \(ในบรรทัดที่ 4\)

จากที่ว่ามาเราก็อาจจะสร้าง object ของคลาส SimpleStorage เพื่อให้มันทำงานกับ data type ที่เราอยากทำงานด้วยหลายๆแบบก็ได้ ตามโค้ดนี้เลย

```csharp
var s1 = new SimpleStorage<int>();
s1.Value = 55;

var s2 = new SimpleStorage<string>();
s2.Value = "Hello world";

var s3 = new SimpleStorage<bool>();
s3.Value = false;
```

### 🔥 ใช้กับ Method

นอกจากที่เราจะใช้กับตัวแปรได้แล้ว เรายังสามารถเอา Generic มาใช้กับ method ได้ด้วยนะ ซึ่งถ้าเราใช้กับ method เราสามารถกำหนด generic ไว้กับ method ได้เลย ประมาณนี้

```csharp
public class Awesome
{
    public void ShowIt<T>(T value)
    {
        Console.WriteLine(value);
    }
}
```

ซึ่งตอนเรียกใช้ก็จะราวๆนี้

```csharp
int a = 55;
var awe = new Awesome();
awe.ShowIt(a);
```

และรวมถึงเราจะเอาไปให้มันเป็น return type ก็ได้นะ

```csharp
public T ShowIt<T>(T value)
{
    Console.WriteLine(value);
    return value;
}
```

### 🔥 Generic แบบหลายตัว

ในบางทีเราก็อยากให้มันทำ generic ไว้หลายๆตัวเราก็สามารถทำได้นะ โดยการคั่น generic แต่ละตัวด้วยคอมม่ายังไงล่ะ

```csharp
public class Awesome<T,U>
{
    public T Value1 { get; set; }
    public U Value2 { get; set; }
}
```

คนเรียกใช้งานก็จะต้องเรียกราวๆนี้

```csharp
var awe = new Awesome<int, string>();
awe.Value1 = 55;
awe.Value2 = "Hello world";
```

## 🤔 ขอตัวอย่างที่เอาไปใช้งานจริงๆหน่อย

มีตรึมเลยที่ Microsoft เขาเขียนไว้เบื้องต้นให้เราแล้ว เช่นเราอยากเก็บข้อมูลในรูปแบบ Stack เราก็สามารถใช้คลาส **Stack&lt;T&gt;** ได้เลย

```csharp
var stack = new Stack<int>();
stack.Push(5);
stack.Push(9);
var lastedValue = stack.Pop();
Console.WriteLine(lastedValue);  // 9
```

## 🎯 บทสรุป

ความสามารถของ generic จะช่วยให้ คนที่เรียกใช้งานเป็นคนกำหนดเองได้ว่า เขาอยากทำงานกับ data type อะไร ซึ่งจะช่วยทำให้โค้ดของเรามีความยืดหยุ่นมากขึ้น และนำกลับมาใช้ใหม่ได้มากขึ้นนั่นเอง

{% hint style="success" %}
**แนะนำให้อ่าน**  
ในการใช้งาน generic นั้นจริงๆมันทำได้เยอะม๊วกกกกก เช่นใช้กับ interface , method, delegate บลาๆ แนะนำให้ไปอ่านเอาต่อนะเพราะไม่งั้นบทความนี้จะยาวเป็นหางว่าวเลย  
[Microsoft document - Generics](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/generics/)
{% endhint %}

