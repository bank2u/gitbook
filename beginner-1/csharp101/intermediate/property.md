# 20.มารู้จักกับ Property กัน

💬 ถ้าเราอยากให้ตัวแปรที่เป็น private ถูกภายนอกกำหนดค่าหรืออ่านค่าได้ โดยปรกติเราก็ต้องสร้าง method ขึ้นมา 2 ตัว สำหรับเขียนและสำหรับอ่านชิมิ ดังนั้นถ้าเรามีตัวแปรแบบนั้น 20 ตัว นั่นก็หมายความว่าเราก็จะมี 40 method อะจิ!! แบบนี้เราคงไม่ต้องทำอะไรกันพอดีเพราะคลาสเราจะรกไปด้วย method นั่นเอง ดังนั้นในรอบนี้เราจะมารู้จักกับ **Property** ที่จะเป็นพระเอกมาช่วยเราในเรื่องนี้เองงับ

{% embed url="https://www.youtube.com/watch?v=P6fJLX-LdqM&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=36" caption="" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 Property คือ

Method พิเศษตัวนึงที่ช่วยให้เราเข้าถึงตัวแปรได้ง่ายๆ ผ่าน **accessor** ที่ชื่อว่า **get** กับ **set** โดยเราสามารถเลือกได้ว่า property ที่เราสร้างขึ้นมาจะทำงานกับตัวแปรไหนได้ ตามโค้ดด้านล่าง

```csharp
public class MyClass
{
   private string name;

   public string Name
   {
      get { return name; }
      set { name = value; }
   }
}
```

{% hint style="info" %}
**Get** คือตัวช่วยให้เราสามารถเข้าไปเรียกดูข้อมูลได้
{% endhint %}

{% hint style="info" %}
**Set** คือตัวช่วยให้เราสามารถเข้าไปเขียนข้อมูลได้
{% endhint %}

{% hint style="info" %}
**value keyword**  
คำสั่ง value ที่อยู่ใน set นั่นหมายถึงค่าที่ผู้ใช้ส่งมากำหนดให้ตอนเรียกใช้ property Name
{% endhint %}

### 👨‍🚀 Auto-Implemented Property

คือ property ที่เราไม่ต้องไปกำหนดว่ามันจะทำงานกับตัวแปรตัวไหนเลย ซึ่ง C\# จะเป็นคนจัดการให้ เรามีหน้าที่แค่กำหนด get set ของมันก็พอ ตามโค้ดด้านล่างเลย

```csharp
public class MyClass
{
   public string Name { get; set; }
}
```

{% hint style="success" %}
**เกร็ดความรู้**  
โดยปรกติ Auto-Implemented Property ตอนที่มัน compile มันจะไปแอบสร้างตัวแปรชื่อมั่วๆขึ้นมา เพื่อให้ Property ที่ชื่อว่า Name สามารถทำงานกับตัวแปรที่ถูกแอบสร้างขึ้นมาผ่าน **get** และ **set** ได้นั่นเอง
{% endhint %}

### 👨‍🚀 Property คัดกรองข้อมูลให้กับตัวแปร

เราสามารถเขียนโค้ดเพื่อจัดการกับเวลาที่มีคน แก้ไขข้อมูล หรือ เรียกดูข้อมูล ผ่าน property ได้ตามโค้ดด้านล่าง

```csharp
public class MyClass
{
   private bool isMale;

   private string name;

   public string Name
   {
      get
      {
         var title = isMale? "Mr." : "Ms.";
         return title + name;
      }
      set
      {
         name = value.ToLower();
      }
   }
}
```

{% hint style="danger" %}
**ข้อความระวัง**  
ในตัวอย่างบรรทัดที่ 12 จะเห็นว่าผมเขียนโค้ดต่อ string ด้วยคำสั่ง **+ \(concatenation\)** ซึ่งโค้ดก็ทำงานได้อยู่นะ แต่เอาจริงๆ**เราไม่ควรเขียนแบบนั้นเพราะมันจะทำให้ประสิทธิภาพของแอพตกลง** ซึ่งเราควรเขียนยังไงผมจะขอยกไปอธิบายในเรื่องของ **string** ในบทถัดๆไปนะครับ
{% endhint %}

### 👨‍🚀 Access Modifier กับ Proper

เราสามารถกำหนด access modifier ให้กับพวก accessors ได้นะครับ เช่นผมอยากให้ ทุกคนเรียกดูตัวแปร Name ได้ แต่ให้คลาสมันเองเท่านั้นที่แก้ไขได้ ผมก็จะได้โค้ดตามรูปเบย

```csharp
public class MyClass
{
   public string Name { get; private set; }
}
```

### 👨‍🚀 Accessors ของ Property

พวก accessors จริงๆจะมีทั้ง 2 ตัว หรือจะมีแค่ตัวใดตัวนึงก็ได้นะ ตามโค้ดด้านล่างเลยงับ

```csharp
public class MyClass
{
   public string Name { get; set; }
   public int Age { get; }
   public string Address { set; }
}
```

{% hint style="danger" %}
**ข้อควรระวัง**  
โค้ดด้านบนไม่ค่อยเหมาะสมที่จะเขียนแบบนั้นนะครับ แต่ผมว่ามันเห็นแล้วเข้าใจง่ายดี ฮา
{% endhint %}

