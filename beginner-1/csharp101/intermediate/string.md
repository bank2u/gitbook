# 29.ลงลึกกับ string

💬 ในรอบนี้เราจะมาลองทำความเข้าใจเจ้า string กันแบบลึกๆบ้างว่ามันเป็นยังไงกันแน่นะ

{% embed url="https://www.youtube.com/watch?v=eFDNoLLqsDg&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=48" caption="" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 รูปแบบของ string ที่แท้จริง

ตัว string จริงๆมันจะคล้ายๆกับ array แต่มันจะเป็น array ของ char เช่นคำว่า "Hello" มันก็จะเหมือนกับมันเก็บข้อมูลแยกเป็นทีละตัว H, e, l, l, o ตามลำดับ ดังนั้นเราจะสามารถเรียกดูข้อมูลของ string ในแต่ละตำแหน่งได้ เช่น

```csharp
string txt = "Hello world";
Console.WriteLine(txt[0]); // บรรทัดนี้จะได้ H
```

{% hint style="info" %}
**Read-only collection of Char**  
ตัว collection ของ string เราสามารถเข้าถึงตำแหน่งของมันแต่ละตัวได้ แต่เราไม่สามารถเปลี่ยนแปลงค่ามันได้นะจ๊ะ
{% endhint %}

{% hint style="info" %}
**IndexOutOfRangeException**  
คือ error เวลาที่เราเรียกข้อมูลใน collection ในตำแหน่งที่มันไม่มีอยู่
{% endhint %}

{% hint style="info" %}
**string กับ String**  
มันเหมือนกันนะ เพียงแค่ตัว string \(ตัวเล็ก\) มันเป็นตัวย่อของ class String นี่แหละ
{% endhint %}

### 👨‍🚀 Immutability

ตัว string ที่ถูกสร้างขึ้นมาทุกตัวจะมีความเป็น immutable เสมอ หรือแปลง่ายๆว่า มันแก้ไขเปลี่ยนแปลงอะไรอีกไม่ได้ ถ้าสร้างมันขึ้นมาแล้ว \(ไปดูบทถัดไปจะเห็นภาพที่ชัดขึ้น\)

### 👨‍🚀 Regular & Verbatim

string เวลาใช้งานมันสามารถใช้อักขระพิเศษทำงานด้วยเพื่อให้เกิดการทำงานแบบพิเศษๆได้ เช่น ขึ้นบรรทัดใหม่ด้วย \n หรือเป็นการเว้นยาวๆด้วย \t

{% hint style="warning" %}
**ข้อควรระวังในการใช้อักษรพิเศษ**  
โดยปรกติเราไม่ควรจะใช้อักษรพิเศษในการทำงานร่วมกัน string \(แม้ว่ามันจะทำงานได้ก็ตาม\) เพราะมันจะทำงานได้เฉพาะแค่กับ OS ใด OS นึงเท่านั้น เช่นการขึ้นบรรทัดใหม่ของ **Windows** กับ **Linux** หรือ **Mac** ก็ไม่เหมือนกันละ \(ตัวนึงเป็น LF อีกตัวนึงเป็น CRLF\) ซึ่งวิธีที่เหมาะสมในการทำงานจริงๆคือการใช้คลาส **Environment** เข้ามาช่วยนั่นเอง
{% endhint %}

### 👨‍🚀 Environment class

ตัวช่วยในการเลือกของที่เหมาะสมกับสภาพที่โปรแกรมกำลังทำงานอยู่ เช่นถ้าโปรแกรมทำงานอยู่บน Windows มันก็จะเลือกการขึ้นบรรทัดใหม่ในรูปแบบของ Windows แต่ถ้าโปรแกรมทำงานอยู่บนเครื่อง Mac มันก็จะขึ้นบรรทัดใหม่ในรูปแบบเครื่อง Mac ตามตัวอย่างโค้ดด้านล่าง

```csharp
Environment.NewLine // ใช้ตัวนี้แทน "\n" นะจ๊ะ
```

### 👨‍🚀 Format strings

เราสามารถจัดการรูปแบบการแสดงผลของ string ได้หลายรูปแบบ

**Placeholder -** การเว้นพื้นที่เพื่อระบุว่าพื้นที่นั้นๆจะใช้ค่าอะไรมาใส่

```csharp
string name = "Au";
int age = 18;
string txt = string.Format("Hello, {0}, Age: {1}", name, age);
```

**Interpolation -** เหมือนกับ placeholder เลย เพียงแค่กำหนดไปเลยว่าจุดนั้นๆใช้ตัวแปรอะไร

```csharp
string name = "Au";
int age = 18;
string txt = $"Hello, {name}, Age: {age}";
```

{% hint style="info" %}
**String interpolation**  
ใช้ได้กับ C\# version 6 ขึ้นไปน่าจ๊า
{% endhint %}

### 👨‍🚀 Method ต่างๆของ string

เจ้าคลาส string มี methods ต่างๆให้เราเล่นเยอะม๊วกๆๆๆๆ ซึ่งถ้าเอามาเขียนในนี้ทั้งหมดผมว่ามันจะยาวเป็นราชโองการในหนังจีนแน่เลย ดังนั้นผมจะขอเอาแค่คราวๆมาให้ดูก็พอ ส่วนถ้าอยากดูทั้งหมดก็ดูได้จากลิงค์นี้เลย [Microsoft document](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=netcore-2.2#methods)

**Substring** - ตัด string ออกจากกัน โดยต้องระบุตำแหน่งที่จะตัดและจำนวนตัวอักษร

```csharp
string txt = "Hello world!";
string result = txt.Substring(0, 4);  // result มีค่าเป็น Hell
```

**IndexOf** - เป็นการค้นหาว่าคำที่ระบุอยู่ในตำแหน่งที่เท่าไหร่

```csharp
string txt = "Hello world!";
int result = txt.IndexOf("world");  // result มีค่าเป็น 6
```

**Replace** - แก้ไขในข้อความที่กำหนดให้เป็นค่าใหม่

```csharp
string txt = "Hello world!";
string result = txt.Replace("world", "Cat");  // result มีค่าเป็น "Hello Cat"
```

### 👨‍🚀 การตรวจสอบ null กับ string ว่าง

การตรวจสอบว่า string มีค่าเป็น null หรือเป็นค่าว่างหรือเปล่า ทาง Microsoft นิยมตรวจสอบโดยใช้ method ที่ชื่อว่า **string.IsNullOrWhiteSpace\(\)**

