# 25.Interface

💬 เคยหงุดหงิดไหมว่าเวลาที่เราเรียกใช้ method ประเภทเดิมๆ แต่ชื่อมันไม่ซ้ำกันเลย เช่น จะเปิดทีวีต้องเรียกใช้ method TurnOn พอจะเปิดพัดลมต้องเรียกใช้ method method SwitchOn พอจะเปิดไฟต้องเรียกใช้ method LightOn ไรงี้ ทั้งๆที่ทั้งหมดมันก็เป็นการสั่งให้เปิดเหมือนกันแท้ๆ แต่ทำไมแต่ละคลาสต้องตั้งชื่อต่างกันด้วยยยย แค่คิดก็หงุดหงิดแล้วที่ต้องไปคอยจำชื่อ method แต่ละตัว ดังนั้นในรอบนี้เราจะมาทำการสร้างมาตรฐานการเขียนโค้ดด้วยสิ่งที่ชื่อว่า Interface กันนะฮ๊าฟฟฟฟ \(อารมณ์ดีละพอได้ยินชื่อนี้\)

{% embed url="https://www.youtube.com/watch?v=2y\_RMaR5as4&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=43" caption="" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 Interface คือ

แบบอย่างหรือมาตรฐานของคลาส โดยมันจะบังคับว่าคลาสที่ implement interface จะต้องมีทุกอย่างที่ interface นั้นๆมีด้วย

{% hint style="danger" %}
**Microsoft standard**  
โดยปรกติเวลาที่ทาง Microsoft สร้าง interface ขึ้นมา เขาจะใส่ตัว**ไอใหญ่ I นำหน้าไว้เสมอ** เช่น ICloneable, IComparable, IDisposable บลาๆ ซึ่งเราจะไม่ทำตามก็ทำงานได้นะ และภาษาอื่นๆก็ไม่ได้ทำแบบนี้ด้วย แต่**ถ้าเราเขียนภาษา C\# เป็นชีวิตจิตใจอยู่แล้วผมนะนำว่าเราควรจะทำตามมาตรฐานนี้ไว้ครับเพราะมันเป็นมาตรฐานสากลของ developer สาย .NET นั่นเอง**
{% endhint %}

{% hint style="warning" %}
**Interface**  
1.ไม่ได้เอาไว้สร้าง object แต่สามารถเก็บ object ของ class ที่ implement interface นั้นๆได้  
2.ของที่อยู่ใน interface จะไม่สามารถมี body หรือ implementation ได้ \(ยกเว้น C\# version 8.0 ขึ้นไป\)
{% endhint %}

{% hint style="success" %}
**C\# version 8.0**  
ใน C\# version 8.0 หรือใน .NET Core 3 นั้นตัว interface จะสามารถมีสิ่งที่เรียกว่า **Default interface members** หรือพูดง่ายๆคือของใน interface สามารถมี body ได้แล้วนั่นเอง
{% endhint %}

ตัวอย่างการทำ Default interface members

```csharp
interface ILogger
{
    void Log(LogLevel level, string message);
    void Log(Exception ex) => Log(LogLevel.Error, ex.ToString());
}
```

{% hint style="success" %}
**C\# 8.0 มีอะไรใหม่บ้าง**  
1.อ่านเอาได้จาก [Microsoft: What's new in C\# 8.0](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-8)  
2.การใช้ default interface members [Microsoft: Default interface members versions](https://docs.microsoft.com/en-us/dotnet/csharp/tutorials/default-interface-members-versions)
{% endhint %}

