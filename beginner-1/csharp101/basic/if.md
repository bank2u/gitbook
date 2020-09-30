# 8.การตัดสินใจด้วย IF statements

💬 หลังจากที่เราเปรียบเทียบค่าต่างๆเป็นละ คราวนี้เราจะลองให้โปรแกรมทำการตัดสินใจดูบ้าง เช่น จะเดินไปทางซ้ายหรือทางขวาดี ซึ่งการตัดสินใจของโปรแกรมเราเรียกมันว่า **IF statements**

{% embed url="https://www.youtube.com/watch?v=bKknZqJhA7w&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=12" caption="" %}

## 🎥 ตัวอย่างการใช้คำสั่ง IF statements

{% embed url="https://www.youtube.com/watch?v=yShfRLBXJ6M&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=13" caption="" %}

{% embed url="https://www.youtube.com/watch?v=RqP1yq05Q3A&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=14" caption="" %}

{% embed url="https://www.youtube.com/watch?v=m11eco7qCgw&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=15" caption="" %}

{% embed url="https://www.youtube.com/watch?v=YilIcDVV81k&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=16" caption="" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 รูปแบบต่างๆของ IF statements

คำสั่งในการตัดสินใจหรือ IF จริงๆมันก็มีแบบเดียวนั่นแหละ แต่เราสามารถเขียนมันได้ทั้งหมด 5 วิธีหลักๆตามด้านล่างครับ

1.if

```csharp
if( เงื่อนไข )
{
  // ทำใน block นี้ถ้าเงื่อนไขเป็นจริง
}
```

1. if..else

```csharp
if( เงื่อนไข )
{
  // ทำใน block นี้ถ้าเงื่อนไขเป็นจริง
}
else
{
  // ทำใน block นี้ถ้าเงื่อนไขเป็นเท็จ
}
```

3.if..else..if

```csharp
if( เงื่อนไขที่ 1 )
{
  // ทำใน block นี้ถ้าเงื่อนไขที่ 1 เป็นจริง
}
else if( เงื่อนไขที่ 2 )
{
  // ทำใน block นี้ถ้าเงื่อนไขที่ 2 เป็นจริง (และเงื่อนไขด้านบนทุกตัวเป็นเท็จ)
}
else
{
  // ทำใน block นี้ถ้าเงื่อนไขทุกตัวเป็นเท็จ
}
```

4.Nested if

```csharp
if( เงื่อนไข 1 )
{
  if( เงื่อนไข 2)
  {
    // ทำใน block นี้ถ้าเงื่อนไข 1 และ 2 เป็นจริง
  }
  else
  {
    // ทำใน block นี้ถ้าเงื่อนไข 1 เป็นจริงแต่ 2 เป็นเท็จ
  }
}
```

5.Inline if หรือ short if

```csharp
string message = เงื่อนไข ? "กรณีเงื่อนเป็นจริงจะใช้ค่านี้" : "กรณีเงื่อนไขเป็นเท็จจะใช้ค่านี้";
```

{% hint style="warning" %}
**คำเตือน**  
การใช้ Inline if หรือ short if ไม่ใช่เรื่องความเท่ใดๆ ถ้าทีมที่เราทำงานด้วยไม่คุ้นเคยหรือไม่ชำนาญในการใช้คำสั่งลัดพวกนี้ **ดช.แมวน้ำ** ขอแนะนำว่าอย่าใช้ครับ เพราะจะทำให้เราทีมเสียเวลาและอาจะเกิดข้อผิดพลาดได้ง่ายขึ้นโดยใช่เหตุ แต่ถ้าทีมเริ่มมีความชำนาญมากขึ้นผมแนะนำให้ใช้เพราะมันทำให้โค้ดเรา clean ขึ้นครับ \(ซึ่งผมจะขอยกเรื่อง Clean code ไปไว้ในบทของมันเองนะครับ\)
{% endhint %}

{% hint style="info" %}
**การใช้วงเล็บ**  
การเขียน if หรือ else ไม่จำเป็นต้องใส่วงเล็บ **{ }**ให้มันก็ได้นะครับ ซึ่งตัวโปรแกรมจะถือว่าคำสั่งถัดไป \(แค่คำสั่งเดียว\) คือของที่อยู่ในวงเล็บของมันครับ
{% endhint %}

