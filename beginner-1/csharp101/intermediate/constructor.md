# 18.มารู้จักกับ Constructor กันบ้าง

💬 เวลาที่เราสร้างคลาสขึ้นมาซักตัว เคยรำคาญไหมว่าเวลาที่เราสร้าง object ของคลาสนั้นๆขึ้นมา บางทีเราต้องไปคอยกำหนดค่าพื้นฐานให้มันเรื่อยๆ จะมีวิธีไหนไหมที่กำหนดค่าพื้นฐานให้กับ object นั้นมาให้เราเลย? คำตอบก็คือการได้รู้จักกับสิ่งที่เรียกว่า Constructor นั่นเอง

{% embed url="https://www.youtube.com/watch?v=GM8Qop3anqo&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=33" caption="" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 Constructor

มีหน้าที่กำหนดข้อมูลพื้นฐานให้กับตัวแปรต่างๆของคลาสนั้นๆ โดยมันจะมีชื่อเดียวกับคลาสของเราเป๊ะๆเลย ตามโค้ดด้านล่าง \(บรรทัดที่ 3~5 นั่นแหละเจ้า constructor\)

```csharp
public class Student
{
   public Student()
   {
   }
}
```

แล้วถ้าเราอยากกำหนดค่าพื้นฐานให้มันล่ะ? ก็ทำตามตัวอย่างด้านล่างได้เบย

```csharp
public class Student
{
   public int Name;
   public Student()
   {
      Name = "Unknow";
   }
}
```

> ทุกครั้งที่เราสร้าง object ใหม่จากคลาส Student เราจะได้ตัวแปร Name มีค่าเป็น Unknow เสมอ

* Constructor สามารถมี parameter ได้เหมือน method เบย เพียงแค่มันไม่มี return type เท่านั้นเอง
* ภายในคลาส 1 คลาส สามารถมี Constructor ได้มากกว่า 1 ตัว แต่ว่า แต่ละตัวจะต้องรับ parameter ที่ไม่เหมือนกันเลยนะ

{% hint style="success" %}
**Default constructor**  
โดยปรกติเวลาที่เราสร้างคลาสมา แม้ว่าเราจะไม่เขียน constructor ไว้ แต่เวลาที่มันเอาไป compile มันจะแอบเติม constructor ที่ไม่รับ parameter ขึ้นมา 1 ตัวเสมอ ซึ่งเจ้า constructor แบบนี้เราเรียกมันว่า **default constructor** แต่ในทางกลับด้านกันถ้าเรามี constructor อยู่แล้ว โปรแกรมมันจะไม่ใส่ default constructor ให้นะกั๊ฟ \(ขอแค่มีแค่ 1 ตัวมันก็ไม่ใส่ให้ละ\)
{% endhint %}

