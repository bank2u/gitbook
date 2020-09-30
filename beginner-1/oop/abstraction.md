# 💖 Abstraction

## 🤔 มันคืออะไร ?

คำว่า **Abstraction** มีใช้อยู่ในหลายวงการเลย แต่ในวงการซอฟต์แวร์ใน **Wikipedia** ถูกเขียนไว้ว่า

> [Abstraction, in general](https://en.wikipedia.org/wiki/Abstraction), is a fundamental concept to computer science and [software development](https://en.wikipedia.org/wiki/Software_development)[\[4\]](https://en.wikipedia.org/wiki/Abstraction_%28computer_science%29#cite_note-4). The process of abstraction can also be referred to as **modeling** and is closely related to the concepts of [theory](https://en.wikipedia.org/wiki/Theory) and [design](https://en.wikipedia.org/wiki/Design)[\[5\]](https://en.wikipedia.org/wiki/Abstraction_%28computer_science%29#cite_note-5). [Models](https://en.wikipedia.org/wiki/Conceptual_model) can also be considered types of abstractions per their generalization of aspects of [reality](https://en.wikipedia.org/wiki/Reality). \(Wikipedia\)

ซึ่งถ้าถอดความหมาย เราก็จะได้หัวใจสำคัญของมันออกมาว่า

> **Abstraction** เป็นการสร้าง Model ให้สอดคล้องกับโลกของความเป็นจริง

## 🤨 ก็ยัง งง อยู่ดีขอตัวอย่างหน่อย

ในการทำ abstraction นั้นมันมีของอยู่ 3 อย่างให้เราต้องคิดในการสร้าง Model ซึ่งพูดไปก็จะ งง อีก ดังนั้นไปรู้จักมันพร้อมกับตัวอย่างกันเลยละกัน

### 🔥 องค์ประกอบ

ขอยกตัวอย่าง **โปรแกรมส่งจดหมาย** ละกัน ซึ่งถ้าเราจะเขียนโค้ดออกมาเราจะต้องมองว่า **มันจะต้องมีองค์ประกอบอะไรบ้าง?** ถึงจะทำให้เราส่งจดหมายได้ โดยเราก็จะมองกลับมาที่โลกจริงๆของเราว่าจดหมายมันต้องประกอบด้วย

1. ซองจดหมาย \(Envelope\)
2. ตัวจดหมาย \(Letter\)
3. คนส่งจดหมาย \(Mailman\)

ซึ่งเราก็จะเอาองค์ประกอบเหล่านี้แปลงมาเป็น Class ที่โปรแกรมเมอร์เข้าใจ ดังนั้นเราก็จะได้ class ออกมา 3 ตัว

```csharp
public class Envelope { }
public class Letter { }
public class Mailman { }
```

### 🔥 Property & Behavior

ในแต่ละองค์ประกอบเราจะต้องคิดต่อว่า **มันต้องมีอะไรอยู่ในนั้นบ้าง \(property\) และมันทำอะไรได้บ้าง \(behavior\)** อีกด้วย ดังนั้นเราก็จะกลับมามองต่อว่า

* **ซองจดหมาย** - มันจะต้องมีการระบุว่า **ผู้ส่งเป็นใคร ผู้รับเป็นใคร และส่งไปที่ไหน \(properties\)**
* **ตัวจดหมาย** - มันควรจะต้องมี **หัวเรื่อง** กับ **เนื้อหา \(properties\)**
* **คนส่งจดหมาย** - เขาควรที่จะ **รวบรวมซองจดหมายเพื่อเตรียมไปส่ง \(behavior\)**

ดังนั้นเราก็จะเอามาเติมใส่ใน class ของเราต่อ ก็จะออกมาเป็น

```csharp
public class Envelope
{
   public string Sender;
   public string Receiver;
   public string Destination;
}

public class Letter
{
   public string Title;
   public string Content;
}

public class Mailman
{
   public void CollectAnEnvelope(Envelope env)
   {
      // ...
   }
}
```

### 🔥 ทำงานร่วมกัน

สุดท้ายเราก็จะมองกลับมาว่า **ของพวกนั้นมันจะทำงานร่วมกันยังไง** นั่นเอง โดยโลกของความเป็นจริง เราก็จะเอา ตัวจดหมาย **ใส่** ซองจดหมายนั่นเอง เลยทำให้เราได้โค้ดตามด้านล่าง

```csharp
public class Envelope
{
   public string Sender;
   public string Receiver;
   public string Destination;

   public void Enclose(Letter letter)
   {
      // ...
   }
}
```

จากที่ว่ามาทั้งหมดนี่แหละคือเรื่องการทำ **Abstraction** ดังนั้นผมสามารถพูดในอีกแง่นึงว่า หลักในการทำ abstraction มันคือการ นำของใน physical แปลงมาอยู่ในรูปแบบของ class เพื่อให้เหล่าโปรแกรมเมอร์สามารถเข้าใจการทำงานได้ง่ายขึ้น และ ของแต่ละอย่างก็จะถูกแบ่งสัดส่วนที่ชัดเจน

## ❓ แล้วสร้างคลาสไปทำไม ?

สุดท้ายในโลกของ **OOP** เราก็จะเอา class เหล่านั้นไปสร้างเป็น object เพื่อใช้งานต่อ เลยทำให้เขานิยมเรียกเจ้าคลาสที่สร้างออกมาว่า **พิมพ์เขียว \(Blueprint\)** นั่นเอง

![Blueprint](../../.gitbook/assets/image%20%28436%29.png)

## 🔎 มุมมอง

สุดท้ายของทุกอย่างนั้นย่อมมีหลายมุมมอง ดังนั้นเวลาที่เราออกแบบ เราก็ต้องออกแบบให้ตรงกับมุมมองของเจ้าสิ่งที่เรากำลังดูแลด้วย เช่น แม้กระทั่งเรื่องจดหมายเจ้าเดิมนี่แหละ ในโค้ดตัวอย่างด้านบนทั้งหมดมันเป็นมุมมองของ **คนเขียน คนอ่าน และ คนส่งจดหมาย** แต่ถ้าเรามองไปที่มุมของ **โรงงานผลิตจดหมาย** เราจะพบว่า มุมมองที่เขาสนใจจะไม่เหมือนกับที่ว่ามานี้เลย ดังนั้นเราก็ต้องดูด้วยว่า **Context** ที่เราอยู่มันอยู่ในมุมไหน เราถึงจะออกแบบมาได้ถูกต้อง

ตัวอย่าง จดหมายในมุมของโรงงานผลิต เขาก็มีคล้ายๆกับที่เคยเขียน เช่น **ซองจดหมาย**

```csharp
public class Envelope { }
```

แต่ในมุมมองของโรงงานผลิต เขาอาจจะมองว่า **มันทำมาจากวัสดุอะไร ขนาดเท่าไหร่ ลวดลายเป็นแบบไหน** ก็ได้

```csharp
public class Envelope
{
   public string Size;
   public string PaperMaterial;
   public string Pattern;
}
```

จากที่ว่ามาก็จะเห็นแล้วว่า แม้จะเป็น ซองจดหมายเหมือนกัน แต่เมื่ออยู่ต่าง **Context** แล้วล่ะก็ มันอาจจะเป็นคนละเรื่องกันเลยก็ได้

{% hint style="danger" %}
**คำเตือน**  
Abstraction ในที่นี้ **ไม่ใช่ abstract keyword ที่ใช้ในการสร้าง abstract class** นะขอรับ ซึ่งเรื่องนี้เข้าใจกันผิดได้บ่อยๆเพราะมันเป็นคำเดียวกัน แต่จำง่ายๆว่า abstraction ของ OOP คือ

**การนำของที่เป็น Physical แปลงมาเป็น Conceptual หรือที่เราเรียกว่า Modeling นั่นเองครัช**
{% endhint %}

{% hint style="warning" %}
**หมายเหตุ**  
ในการที่เราจะสร้าง Model ได้นั้น เราจะต้องใช้วิธีการคิดแบบ Abstraction เพื่อได้ให้สิ่งต่างๆที่สามารถทำงานได้ และมีความสัมพันธ์กันออกมา แต่ Model ที่ได้ออกมา มันจะยังไม่ใช่ของที่ดี ถ้าขาดการนำหลักการของ **Encapsulation** มาใช้งาน
{% endhint %}

{% hint style="success" %}
**แนะนำให้อ่าน**  
ในเรื่องของการออกแบบโดยดูจาก Context สามารถไปศึกษาต่อได้ในเรื่องของ **Domain Driven Design** หรือที่เราเรียกกันติดปากว่า **DDD** นั่นเองครัช
{% endhint %}

