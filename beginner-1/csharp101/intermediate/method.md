# 16.ลดงานซ้ำๆด้วย Method

💬 เคยรู้สึกไหมว่าในโค้ดของเราบางทีก็มีงานที่เขียนซ้ำๆกันออกมาให้เจอบ่อยๆ \(ซ้ำในที่นี้ไม่ใช่ในแบบของ loop นะ\) ซึ่งมันทำให้เราต้องไปคอยนั่งก๊อปปี้มาวางจุดนั้นจุดนู้นตลอดเวลา แล้วยิ่งเราเอาไปวางไว้เยอะ ถ้าเราต้องแก้ไขมันเราก็ต้องไล่ไปตามแก้ทุกจุดด้วยอะดิ จากปัญหาที่ว่ามาในรอบนี้เราจะลองมารู้จักกับสิ่งที่เรียกว่า **Method** ซึ่งจะมาช่วยคลี่คลายปัญหาที่ว่ามานี้ครับ

{% embed url="https://www.youtube.com/watch?v=1fQkrqWwf2I&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=31" caption="" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 Method คือ

โค้ดธรรมดานี่แหละ แต่เราสามารถเรียกใช้มันซ้ำๆได้ และมันก็ยืดหยุ่นพอที่จะทำให้เราทำให้มันเปลี่ยนพฤติกรรมการทำงานของมันตามข้อมูลที่เราส่งไปให้มันได้ด้วยนะ ซึ่งสิ่งที่เราส่งไปให้มันเราเรียกว่า **Parameter**

### 👨‍🚀 Method แบบไม่มี parameter

```csharp
public void MyMethod()
{
   // Do something
}
```

### 👨‍🚀 Method แบบมี parameter

แบบมี parameter ตัวเดียว

```csharp
public void MyMethod(int parameterA)
{
   // Do something
}
```

แบบมี parameter หลายตัว \(ใช่ comma คั่น\)

```csharp
public void MyMethod(int parameterA, string parameterB)
{
   // Do something
}
```

### 👨‍🚀 Method แบบมีการส่งข้อมูลกลับ \(return type\)

แบบมี return type แต่ไม่มี parameter

```csharp
public int MyMethod()
{
   return 5;
}
```

แบบมี return type และมี parameter

```csharp
public int Add(int firstValue, int secondValue)
{
   return firstValue + secondValue;
}
```

### 👨‍🚀 out keyword

เป็นการบอกว่า parameter ที่ส่งเข้ามานั้นจะให้ method เป็นคนกำหนดค่าให้มัน ดังนั้นเมื่อ method นั้นๆทำงานเสร็จ ตัวแปรที่ส่งเข้าไปให้ด้วย **out keyword** นั้นก็จะถูกกำหนดค่ามาให้เสร็จเรียบร้อยเลย

```csharp
public void MyMethod(out int parameter)
{
   parameter = 99;
}

// ตอนเรียกใช้
int a;
MyMethod(out a); // พอทำงานเสร็จตัวแปร a จะมีค่าเป็น 99
```

{% hint style="warning" %}
นั่นหมายความว่า method ที่รับ parameter เป็น **out keyword จะต้องมีการกำหนดค่าให้กับ parameter นั้นๆด้วย** ซึ่งถ้าไม่ทำการกำหนดค่าให้เราจะไม่สามารถ compile ไฟล์นั้นได้ครับ
{% endhint %}

### 👨‍🚀 ref keyword

เป็นการบอกว่า parameter ที่ส่งเข้ามานั้น ถ้า method มีการแก้ไขค่าให้เป็นอะไร ตัวแปรจริงๆที่ถูกส่งเข้ามาก็จะถูกแก้ไขตามไปด้วย ซึ่งส่วนใหญ่เราจะใช้กับ **value type** นั่นเอง

```csharp
public void MyMethod(ref int parameter)
{
   parameter = 99;
}

// ตอนเรียกใช้
int a = 5;
MyMethod(ref a); // พอทำงานเสร็จตัวแปร a จะมีค่าเป็น 99
```

{% hint style="warning" %}
นั่นหมายความว่า method ที่รับ parameter เป็น **ref keyword ไม่จำเป็นต้องมีการกำหนดค่าให้กับ parameter นั้นๆ** เพียงแต่ถ้าเราเปลี่ยนค่ามันภายใน method มันก็จะถูกเปลี่ยนตามไปด้วยนั่นเอง
{% endhint %}

