# 🏆 Inheritance & Polymorphism

หลังจากที่เข้าใจหลักการของ **Inheritance** กับ **Polymorphism** กันไปเรียบร้อยแล้ว ดังนั้นเรามาสรุปความเข้าใจเจ้า 2 ตัวนี้ก่อนว่ามันเกี่ยวเนื่องกันยังไง

## 🔥 **Inheritance**

> เป็นการนำ Model ที่มีอยู่แล้วไปสร้างรูปแบบการทำงานใหม่ๆได้

![](../../.gitbook/assets/image%20%28385%29.png)

## 🔥 Polymorphism

> เป็นการรองรับให้ object สามารถอยู่ได้หลากหลายรูปแบบ และทำงานกับ object ที่แท้จริงของมัน

![](../../.gitbook/assets/image%20%2885%29.png)

## 💖 Abstraction + Encapsulation

เมื่อเราเอาทั้ง 2 แนวคิดนี้มารวมกัน **เราจะสามารถเพิ่มความสามารถใหม่ๆได้ แถมยังไม่ต้องยึดติดกับ Model ที่แท้จริงอีกด้วย** นั่นเอง ซึ่งประโยชน์ที่เราจะได้ก็คือ โค้ดเดิมไม่ถูกแก้ไข โค้ดมีความยืดหยุ่นสูงขึ้น

{% hint style="danger" %}
**ข้อควรระวัง**

* **อย่าใช้ Inheritance** เพราะเราขี้เกียจเขียนโค้ดซ้ำๆ เพราะ Inheritance มันเป็นความสัมพันธ์เป็นแบบ **Is A** นั่นเอง \(มันไม่ดียังไงตัวอย่างอยู่ด้านล่าง\)
* **ให้ใช้ Inheritance** เมื่อมันเป็นของประเภทเดียวกันเท่านั้น
{% endhint %}

## **👎 Bad Design**

การทำ Inheritance ที่ไม่ถูก เช่น เราทำแอพขายหนังสือและแมวโดยมีระบบสมาชิกด้วย เลยทำให้เรามี Model 3 ตัวนั่นคือ **หนังสือ, แมว** และ **สมาชิก** ซึ่งมีโค้ดเป็นแบบนี้

```csharp
public class Book
{
   public int Id { get; set; }
   public string Name { get; set; }
   public DateTime CreatedDate { get; set; }
   public string ISBN { get; set; }
   public string Author { get; set; }
}

public class Cat
{
   public int Id { get; set; }
   public string Name { get; set; }
   public DateTime CreatedDate { get; set; }
   public string OwnerName { get; set; }
}

public class Member
{
   public int Id { get; set; }
   public string Name { get; set; }
   public DateTime CreatedDate { get; set; }
   public string LastName { get; set; }
   public string Address { get; set; }
}
```

หลายครั้งที่เราจะพบว่า Developer จะมองว่า มันมีของที่เหมือนกัน ดังนั้นก็ใช้ Inheritance ซิมันจะได้ลดโค้ดที่ซ้ำกันออก กลายเป็นแบบนี้

```csharp
public class ProductInformation
{
   public int Id { get; set; }
   public string Name { get; set; }
   public DateTime CreatedDate { get; set; }
}

public class Book : ProductInformation
{
   public string ISBN { get; set; }
   public string Author { get; set; }
}

public class Cat : ProductInformation
{
   public string Ownername { get; set; }
}

public class Member : ProductInformation
{
   public string LastName { get; set; }
   public string Address { get; set; }
}
```

ซึ่งมันก็ทำงานได้นะ ลดโค้ดที่ซ้ำกันได้ด้วยแต่ **มันใช้ของผิดวัตถุประสงค์** \(เฟร้ยยยย\) มันเหมือนกับเรากำลังเอาตะเกียบไปกินไอติม ซึ่งดูผิวเผินก็คีบกินได้ไม่มีไร แต่ถ้าปล่อยทำแบบนี้ต่อไปเรื่อยๆ เมื่อไหร่ที่ไอติมละลายเราก็จะกินต่อไม่ได้นั่นเอง เมื่อเทียบกลับมาที่โค้ดด้านบน มันก็จะทำงานได้ แต่ถ้าปล่อยไปเรื่อยๆมีการใช้ของแบบนี้ต่อไปเรื่อยๆเดี๋ยวมันจะมีปัญหาตามมาอีกที เพราะในกรนี้นี้เราใช้ inheritance โดยไม่ได้คำนึงถึงความสัมพันธ์แบบ **IS A** ยังไงล่ะ

**🤨 ยังไม่เข้าใจอ่ะ การใช้ความสัมพันธ์แบบ IS A แบบไม่ถูกมันจะมีปัญหาไร?**

**IS A** ถ้าแปลเป็นภาษาไทยมันแปลได้ว่า **"มันเป็น"** นั่นเอง ซึ่งลองเอา Model ตามโค้ดด้านบนมาดูนะว่ามันฟังแล้วมันแปลกๆหรือเปล่า

1. Book **IS A** ProductInformation - หนังสือ**เป็น**ข้อมูลสินค้าประเภทหนึ่ง
2. Cat **IS A** ProductInformation - แมว**เป็น**ข้อมูลสินค้าประเภทหนึ่ง
3. Member **IS A** ProductInformation - สมาชิก**เป็น**ข้อมูลสินค้าประเภทหนึ่ง

จากด้านบนข้อ 1 กับ 2 อ่านแล้วยังพอเข้าใจได้ เพราะเราขาย หนังสือ และ ขายแมว ดังนั้นทั้ง 2 อย่างนี้ยังพอเข้าใจได้ แต่ข้อ 3 สมาชิกมันไม่ใช่ข้อมูลสินค้าแน่ๆ แต่เนื่องจากเราเอา Inheritance มาใช้ มันเลยทำให้มันเกิดความสัมพันธ์แบบนั้นเกิดขึ้น ดังนั้นในอนาคตมันจะเกิดปัญหาขึ้นโดยไม่ตั้งใจได้

เช่น เราจะสร้าง Method เอาไว้คิดเงินละว่าสินค้าที่เลือกเข้ามาทั้งหมดราคาเท่าไหร่ ซึ่งเราก็อาจจะเขียนแบบง่ายๆออกมาเป็น

```csharp
public double GetTotalPrice(ProductInformation[] products)
{
   ...
}
```

เนื่องจากเราทำเป็น Inheritance ดังนั้นมันเลยได้ความสามารถของ Polymorphism เข้ามาด้วย เลยทำให้เราสามารถส่ง object ที่เป็น Book และ Cat เข้ามาใน method ตัวนี้ได้ ... แต่ก็อย่าลืมนะว่ามันก็ส่ง object Member เข้ามาคิดราคาได้ด้วยเหมือนกัน!!

## 👍 Good Design

จากตัวอย่างแอพขายหนังสือและแมวโดยมีระบบสมาชิกถ้าเราจะออกแบบให้ดีนั้นมันมีหลักการง่ายๆแค่

{% hint style="success" %}
มันมีเหตุผลอะไรที่ต้องใช้ Inheritance ไหม? ใช้แล้วได้ประโยชน์อะไร? จำเป็นจริงๆหรือเปล่า?
{% endhint %}

เพียงแค่ตอบคำถามด้านบนให้ได้เสียก่อน ซึ่งถ้าเราตอบไม่ได้หรือได้คำตอบว่าไม่จำเป็น หรือประโยชน์คือเขียนโค้ดง่ายขึ้นนิดหน่อยไม่เกี่ยวข้องกับความสัมพันธ์อะไรเลยแล้วล่ะก็ **อย่าใช้ Inheritance เพราะมันจะทำให้โค้ดเราซับซ้อนโดยไม่จำเป็น**

แต่ถ้าได้คำตอบซึ่งมีเหตุผลรองรับที่ดีแล้วล่ะก็ ใช้ได้เลย แต่**จงทำให้ถูกในมุมของความสัมพันธ์ด้วย** เช่น ถ้าหนังสือและแมว มันเป็นสินค้าในร้าน เราจะให้มันมาจาก Base class ที่เป็น Product ก็ได้

```csharp
public class ProductInformation
{
   public int Id { get; set; }
   public string Name { get; set; }
   public DateTime CreatedDate { get; set; }
}

public class Book : ProductInformation
{
   public string ISBN { get; set; }
   public string Author { get; set; }
}

public class Cat : ProductInformation
{
   public string OwnerName { get; set; }
}
```

ส่วนเจ้าสมาชิกถ้ามันมี Model แค่ตัวนี้ตัวเดียวก็ปล่อยไปแบบเดิมก็ได้ อย่าไปทำอะไรที่มันซับซ้อนเลย

```csharp
public class Member
{
   public int Id { get; set; }
   public string Name { get; set; }
   public DateTime CreatedDate { get; set; }
   public string LastName { get; set; }
   public string Address { get; set; }
}
```

แต่ถ้าเรามีสมาชิกหลายแบบ เช่น VIP หรืออะไรก็แล้วแต่ ให้เราถามคำถามเดิมต่อว่า มันมีเหตุผลอะไรที่ควรจะต้องทำ Inheritance ไหม? ซึ่งถ้าไม่มี ผมเสนอว่าทำตัวแปรมาคัดแยกประเภทเอาจะง่ายกว่า ไม่ซับซ้อนด้วย

```csharp
public class Member
{
   public int Id { get; set; }
   public string Name { get; set; }
   public DateTime CreatedDate { get; set; }
   public string LastName { get; set; }
   public string Address { get; set; }

   public bool IsVIP { get; set; }
}
```

แต่ถ้าเรามีเหตุผลที่เพียงพอก็จะแตกกลุ่มออกมาอีกก็ได้

```csharp
public class Membership
{
   public int Id { get; set; }
   public string Name { get; set; }
   public DateTime CreatedDate { get; set; }
   public string LastName { get; set; }
   public string Address { get; set; }
}

public class NormalMember : Membership
{
}

public class VIPMember : Membership
{
   public DateTime ExpiredDate { get; set; }
}
```

สมมุติว่าเรามีเหตุผลที่ดีพอในการทำ Inheritance แล้วล่ะก็ อย่าลืมเอาความสัมพันธ์มันมาตรวจสอบดีๆอีกครั้งด้วยล่ะ ดังนั้นเราลองเอาแผนภาพ **UML** มาดูคร่าวๆกัน

![](../../.gitbook/assets/image%20%28987%29.png)

ลองตรวจความสัมพันธ์ของแต่ละกลุ่มดูว่ามันเหมาะสมหรือยัง

**กลุ่มสินค้า**

1. หนังสือ **เป็น** สินค้าประเภทหนึ่ง
2. แมว **เป็น** สินค้าประเภทหนึ่ง

**กลุ่มสมาชิก**

1. สมาชิกธรรมดา **เป็น** สมาชิกประเภทหนึ่ง
2. สมาชิก VIP **เป็น** สมาชิกประเภทหนึ่ง

เมื่อลองอ่านด้านบนแล้วก็น่าจะไม่มีอะไรผิดปรกติแล้วใช่ไหม เพียงเท่านี้เราก็สามารถนำไปใช้งานได้อย่างสบายใจในระดับหนึ่งละ

{% hint style="success" %}
จะทำ Inheritance ให้มองย้อนถึง **IS A Relationship ก่อนทำเสมอ** และ **เหตุผลที่มีน้ำหนักคุ้มที่จะทำ**
{% endhint %}

## 🎯 บทสรุป

การใช้ **Inheritance & Polymorphism** นั้นจะช่วยทำให้โค้ดเรามีความยืดหยุ่นเพิ่มขึ้นในหลายๆด้าน แต่ก็แลกมาด้วยความซับซ้อนที่สูงขึ้น และการออกแบบที่ต้องระวังมากขึ้น ซึ่งมันจะเป็นจุดที่ทำให้แอพเราแก้ไขปรับปรุงได้ยากขึ้นหลายเท่าตัว ดังนั้นก่อนใช้อะไรก็ตามแต่ให้คำนึงถึง **ความเหมาะสม** และข้อดีข้อเสียด้วยว่าคุ้มหรือไม่

{% hint style="danger" %}
**คำเตือน**  
การใช้ Inheritance โดยไม่คำนึงถึงความสัมพันธ์แบบ **IS A** มันจะส่งผลร้ายเป็น bug ที่ไม่ส่งเสียงในอนาคต รอวันที่เราเผลอลืมของที่สร้างไว้ จนวันหนึ่งมีคนในทีมไปเหยียบกับระเบิดเข้า ก็จะส่งส่งสมาชิกเข้าไปเป็นสินค้าเพื่อคำนวณราคาออกมา ซึ่งมันอาจจะทำงานได้ หรือ ไม่ได้ไม่รู้ล่ะ แต่ในมุมของการออกแบบและการทำงานจริง มันไม่ถูกทั้งคู่ ดังนั้นมันจะทำให้โปรแกรมของเรามีของแปลกๆอยู่ในนั้น เหมือนกับเวลาเราเห็นคนเอาตะเกียบไปกินไอติมนั่นแหละ
{% endhint %}

{% hint style="info" %}
**แนะนำให้อ่าน**  
หลักในการออกแบบขั้นพื้นฐาน หรือที่เราเรียกว่า SOLID Design เพื่อนๆสามารถไปศึกษาอ่านได้จากลิงค์ด้านล่างนี้เบย  
[👦 SOLID Design Principles](https://saladpuk.gitbook.io/learn/basic/solid)
{% endhint %}

