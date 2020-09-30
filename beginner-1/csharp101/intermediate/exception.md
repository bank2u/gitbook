# 28.Exception handler

💬 เคยไหมโปรแกรมที่เราเขียนพอทำงานอยู่ดีๆก็เด้งปิดโปรแกรมไป หรือไม่ก็เจอข้อความแปลกๆเต็มจอไปหมดแล้วโปรแกรมก็เด้งไป ถ้าเจอปัญหาพวกนั้นแสดงว่าในโค้ดของเรามันมี error เกิดขึ้น ซึ่งเราจะต้องจัดการกับ error เหล่านั้นไม่ให้โปรแกรมเด้งออก ซึ่งตัวที่จะมาช่วยเราจัดการ error นั่นก็คือสิ่งที่เรียกว่า **Exception handler** นั่นเอง

{% embed url="https://www.youtube.com/watch?v=yxyy6S2QYXA&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=46" caption="" %}

{% embed url="https://www.youtube.com/watch?v=FMRssmk\_0Tc&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=47" caption="" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 Exception handler คือ

ตัวช่วยในการดักจับข้อผิดพลาดต่างๆในโปรแกรม ซึ่งเราจะใช้ตัวดักจับ error ไปครอบโค้ดที่มีความเสี่ยงว่ามันอาจจะเกิด error เอาไว้ ส่วนถ้ามันเกิด error ขึ้นจริงๆ เราก็จะสามารถเขียนโค้ดในการตัดสินใจต่อได้ว่าควรจะทำอะไรกับมันต่อ ตามโค้ดด้านล่าง

```csharp
try
{
   // โค้ดที่มีความเสี่ยงว่าอาจจะเกิด error ขึ้นได้
}
catch (Exception e)
{
   // ถ้าเกิด error ขึ้นภายในบรรทัดที่ 2~4 มันจะกระโดดมาทำภายใน block นี้
}
```

### 👨‍🚀 การดักจับ exception แบบระบุ

ตอนที่เกิด error ขึ้น เราสามารถเลือกได้ว่า "ถ้าเกิด error แบบ A เราจะจัดการแบบนี้ หรือ "ถ้าเกิด error แบบ B ให้จัดการแบบโน้น" ก็ได้เหมือนกันนะ โดยการระบุประเภทของ exception ลงไปใน catch ตามตัวอย่างด้านล่าง

```csharp
try
{
   // โค้ดที่มีความเสี่ยงว่าอาจจะเกิด error ขึ้นได้
}
catch (DivideByZeroException e)
{
   // ถ้าเกิด error ขึ้นภายในบรรทัดที่ 2~4 
   // และเป็น error จากการที่ตัวหารเป็น 0 มันจะกระโดดมาทำภายใน block นี้
}
catch (NullReferenceException e)
{
   // ถ้าเกิด error ขึ้นภายในบรรทัดที่ 2~4
   // และเป็น error จากการที่เราใช้ตัวแปรที่เป็น null มันจะกระโดดมาทำภายใน block นี้
}
catch (Exception e)
{
   // ถ้าเกิด error ขึ้นภายในบรรทัดที่ 2~4
   // และ error ที่เกิดขึ้นไม่ใช่เกิดจาก ตัวหารเป็น 0 หรือ เรียกใช้ตัวแปรที่เป็น null
   // มันจะกระโดดมาทำภายใน block นี้
}
```

{% hint style="info" %}
**Exception**  
คือต้นตระกูลของ exception ทุกตัว ดังนั้นถ้าเกิด error ที่เกิดขึ้น ไม่ตรงกับ catch ตัวไหนเลย มันจะเข้าไปที่ Exception
{% endhint %}

{% hint style="danger" %}
**Exception**  
ถ้ามี catch หลายๆตัว ห้ามเอา Exception ไปไว้ใน catch ตัวแรก เพราะถ้ามันเกิด error ขึ้นมันจะเข้า catch\(Exception e\) ตัวแรกที่วางไว้โดยไม่สนใจ catch อื่นๆด้านล่างเลย
{% endhint %}

### 👨‍🚀 Finally keyword

คือ block ที่ระบุว่า ไม่ว่าจะทำแล้วเกิด error ขึ้นหรือไม่ หลังจากทำ try หรือ catch เสร็จ มันก็จะมาทำโค้ดใน block ของ finally เป็นลำดับถัดไปนั่นเอง

```csharp
try
{
   // โค้ดที่มีความเสี่ยงว่าอาจจะเกิด error ขึ้นได้
}
catch (Exception e)
{
   // ถ้าเกิด error ขึ้นภายในบรรทัดที่ 2~4 มันจะกระโดดมาทำภายใน block นี้
}
finally
{
   // เมื่อทำ try หรือ catch เสร็จ มันจะกระโดดมาทำภายใน block นี้เสมอ
}
```

### 👨‍🚀 throw keyword

คือคำสั่งที่บอกว่า ณ โค้ดจุดนั้นจะไม่ขอจัดการ error นี้แล้ว และจะทำการส่ง error นั้นๆไปให้กับ โค้ดที่เรียกมันรับหน้าที่จัดการต่อ

```csharp
try
{
   // โค้ดที่มีความเสี่ยงว่าอาจจะเกิด error ขึ้นได้
}
catch (Exception e)
{
   throw e;
}
```

หรืออยู่ๆ เราอยากจะโยน error ออกจาก method ดื้นๆทั้งๆที่ยังไม่เกิด error ก็ได้  
เช่น ผมเขียน method ที่รับชื่อเข้ามา ซึ่งถ้าส่งชื่อเข้ามามันจะทักทาย แต่ถ้าส่ง null หรือช่องว่างเข้ามามันจะส่ง error ออกไปแทน เราก็จะได้โคีดตามตัวอย่างด้านล่างนี้ครับ

```csharp
public void Greeting(string name)
{
    if (string.IsNullOrWhiteSpace(name))
    {
        throw new ArgumentNullException();
    }

    Console.Write("Greeting " + name);
}
```

### 👨‍🚀 การส่งต่อ Exception โดยมี message

เวลาที่เราส่งต่อ exception เราสามารถกำหนด message ให้กับ exception ที่ส่งต่อได้ เพื่อให้คนที่รับผิดชอบสามารถรู้ได้ว่า error ที่เกิดขึ้นนั้น เกิดจากอะไร ตามตัวอย่างด้านล่าง

```csharp
throw new ArgumentNullException("name", "กรุณากำหนดชื่อเข้ามาด้วย");
```

{% hint style="info" %}
**Exception message**  
เวลาที่เราใส่ message ให้กับ exception object ลองกดดู overload ของมันด้วยนะ เพราะมันมี overload ให้เราเลือกเยอะม๊วก
{% endhint %}

### 👨‍🚀 Custom Exception

นอกเหนือจาก exception มาตรฐานที่ Microsoft เตรียมไว้ให้แล้ว ถ้าเรามี exception แบบพิเศษในแบบของเราเอง เราก็สามารถทำได้เหมือนกัน โดยการสร้างคลาสธรรมดานี่แหละไปสืบทอดจากพวก exception class

```csharp
public class MyCustomException : Exception
{
}
```

{% hint style="warning" %}
**คำเตือน**  
โดยปรกติเราจะไม่สร้าง Custom exception เท่าไหร่นะ เพราะของที่มีมากับ .NET อยู่แล้วมันค่อนข้างจะสมบูรณ์ด้วยตัวมันเอง ผมแนะนำว่าลองศึกษา Exception ที่มีมาให้เสียก่อนว่าเพียงพอต่อการใช้งานของเราหรือเปล่า ซึ่งถ้าดูแล้วมันไม่เพียงพอต่อการใช้งานจริงๆเราค่อยทำการ Custom มันก็ได้ครับ
{% endhint %}

### 👨‍🚀 ข้อควรระวังในการใช้ try/catch

โดยปรกติการใช้ try/catch มันจะทำให้ความเร็วในการทำงานตกลง ดังนั้นในทางปฏิบัติจริงๆเราควรจะหลีกเลี่ยงการใช้ try/catch ให้ได้มากที่สุดเท่าที่จะทำได้ เช่นการตรวจสอบก่อนว่าค่านั้นๆเป็น null หรือเปล่า หรือตัวหารเป็นศูนย์หรือเปล่า ก่อนที่จะนำตัวแปรพวกนั้นไป execute จริงๆนะครับ ตามตัวอย่างโค้ดด้านล่างเลยเป็นการป้องกันตัวหารเป็นศูนย์

```csharp
public void Divide(double dividend, double divisor)
{
   if(divisor == 0)
   {
      Console.WriteLine("ตัวหารไม่สามารถเป็นศูนย์ได้นะ!")
   }
   else
   {
      Console.WriteLine(dividend / divisor);
   }
}
```

