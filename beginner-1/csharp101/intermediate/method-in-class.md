# 19.มาเขียน Method ใน Class กัน

💬 อะเช หลังจากที่รู้เรื่องคลาสมาพอประมาณละ เราก็จะเห็นว่ามันใส่ตัวแปรต่างๆเข้าไปในคลาสได้ชิมิ ดังนั้นเพื่อนๆก็น่าจะรู้แล้วว่ามันก็น่าจะใส่ **method** ลงไปได้เหมือนกัน แต่ method ที่อยู่ในคลาสมันมีลูกเล่นอะไรกันบ้างนะไปดูกันเบย

{% embed url="https://www.youtube.com/watch?v=oBpFypR8\_Eo&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=34" caption="" %}

{% hint style="danger" %}
**ขออภัย**  
ในวีดีโอผมพึ่งเห็นว่าผมเมาอธิบายเรื่อง method overloading ว่า return type ไม่เกี่ยว จริงๆคือ เกี่ยวนะครับ
{% endhint %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 Method overloading

คือการสร้าง method ที่มีชื่อเดียวกันและอยู่ภายในคลาสเดียวกัน โดยมันจะมีกฎที่ให้เราทำแบบนั้นได้คือ  
**method พวกนั้นจะต้องมี parameter ที่ไม่เหมือนกัน** หรือ **มี return type ต่างกัน**

{% hint style="info" %}
**parameter ไม่เหมือนกัน**  
เช่น ลำดับของ data type ไม่เหมือนกัน หรือ จำนวนของ parameter ไม่เท่ากัน \(ชื่อของ parameter ไม่เกี่ยวนะจ๊ะ\)
{% endhint %}

```csharp
public class MyClass
{
   public void MyMethod()
   {
   }

   public void MyMethod(int p)
   {
   }

   public void MyMethod(string p)
   {
   }

   public int MyMethod(int p)
   {
      return p;
   }
}
```

### 👨‍🚀 Method overriding

ในวีดีโอจะไม่ได้พูดเรื่องนี้นะครับ เพราะมันต้องเข้าใจเรื่อง **inheritance** เสียก่อน แต่ขออธิบายไว้คร่าวๆว่า มันคือการเขียนการทำงานของ method ใหม่จาก class ลูกนั่นเอง โดยคลาสแม่จะต้องประกาศว่า method นั้นเป็น **virtual** ด้วย

