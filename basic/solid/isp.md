# Interface Segregation Principle

## 👑 หัวใจหลักของ Interface Segregation Principle \(ISP\)

> Clients **should not** be forced to depend on methods they do not use.

**"โค้ดไม่ควรจะถูกบังคับให้มี method ที่ไม่ได้ใช้"** ฟังแล้วก็เหมือนจะง่ายดีนะ ก็แค่อย่าเอา method ที่ไม่ได้ใช่ไปใส่ในโค้ดก็พอแล้วใช่ไหม? \(ไม่เห็นต้องบอกเลยใครมันจะไปเขียน method ลงไปในโค้ดมั่วๆกันแฟร๊\)

แต่เพื่อนๆเคยเจอไหมว่าในบางทีโค้ดเราถูกบังคับให้เติม method ที่ไม่ได้ใช้ลงไป เพราะ **Interface** หรือ **Base Class** กำหนดว่า**มันต้องมี**ยังไงล่ะ 😱 !! ซึ่งเหตุการณ์แบบนี้เราเรียกว่า **Interface Pollution** หรือ **Fat Interface** นั่นเอง แบ๊วเราจะจัดการยังไงกันดีล่าาาาา

{% hint style="info" %}
**Interface Pollution**  
คือคำที่เรียกอาการของ Interface ที่โดนรวมกับ interface อื่นๆจนมันบวมฉ่ำ \(fat\) ลองคิดดูว่า Class ที่จะไป implement interface นั้นจะต้องเตรียมรับแรงกระแทกของ method ปริมาณมากยังไงดี และ ทุก method ที่หลั่งไหลเข้ามาคลาสนั้นจำเป็นต้องใช้จริงๆหรือเปล่า ?
{% endhint %}

## ❓ ทำไมต้องห้ามมี method ที่ไม่ได้ใช้ในโค้ด ?

ถ้าคลาสถูกบังคับจาก Interface ว่าจะต้องมี method ต่างๆ โดยที่ตัวมันเองไม่ได้ใช้ นั่นหมายความว่าคลาสพวกนั้นถูกผูกติด \(coupling\) กับ Interface ที่มี method ขยะปนมาด้วย และถ้า Interface พวกนั้นไปเพิ่ม method ขยะเข้าไปอีก นั่นก็หมายความว่า เราก็ต้องไปไล่ implement method ขยะด้วยทุกๆครั้งด้วยนะซิ และคลาสนั้นก็จะมีความเป็น Cohesion ลดลงไปด้วยนั่นเอง _\(Cohesion คือไรไปอ่านเอาบนแรกๆที่ side menu นู่นเลย\)_ และที่แย่ที่สุดของปัญหานี้คือโค้ดเรามี **Rigidity** และ **Viscosity** สูงมาก

{% hint style="info" %}
**Code Smells**  
Rigidity - โค้ดแก้ไขเพิ่มเติมยาก  
Viscosity - มีโอกาสที่แก้ไขปัญหาแบบผิดวิธี ออกนอกลู่นอกทาง  
_\* เรื่องของ Code Smells แบบเต็มๆไปอ่านเอาได้จาก side menu เด้อ_
{% endhint %}

## 🥶 ตัวอย่างการเกิด Interface Pollution

สมมุติว่าเราต้องออกแบบโค้ดให้กับ **หลอดไฟ** และ **ทีวี** ให้มันสามารถสั่ง เปิด/ปิด หรือดูว่ามันเปิดอยู่หรือเปล่าได้ เราก็จะออกแบบมันออกมาเป็นตามรูปนี้ชิมิ

![](../../.gitbook/assets/image%20%28798%29.png)

```csharp
public interface IElectronic
{
    void TurnOn();
    void TurnOff();
    bool IsTurnedOn();
}

public class Television : IElectronic { ... }
public class LightBulb : IElectronic { ... }
```

แล้วอยู่มาวันนึง เราก็อยากให้หลอดไฟและทีวี**สามารถตั้งเวลาปิดได้เองอัตโนมัติ** ดังนั้นเราก็เลยเพิ่มโค้ดเข้าไปอีกนิสนุง ตามรูปด้านล่าง

![](../../.gitbook/assets/image%20%28672%29.png)

ดูผิวเผินก็เหมือนไม่มีอะไร แค่ต้องไปเพิ่ม method ให้กับ **LightBulb** กับ **Television** เข้าไปหน่อยให้มันสามารถตั้งเวลาปิดได้ จนกระทั่งได้อ่านข้อความบรรด้านล่าง

**ลูกค้าอยากให้มีทีวีและหลอดไฟแบบโบราณซึ่งมันไม่สามารถตั้งเวลาปิดได้** เราจะออกแบบยังไง? ส่วนใหญ่ก็จะออกมาเป็นตามภาพด้านล่าง

![](../../.gitbook/assets/image%20%28715%29.png)

เจ้าคลาสใหม่ 2 ตัวที่เพิ่มเข้ามา จะต้องถูกยัดเยียดให้ implement **SetTurnOffSchedule\(\)** เข้าไปด้วย **ทั้งๆที่มันไม่ได้ใช้**

{% hint style="danger" %}
**ลองคิดดู**  
ถ้าในอนาคตมีหลอดไฟหรือทีวี แบบใหม่ๆเข้ามา เช่นสั่งงานด้วยเสียงได้ สั่งงานผ่าน bluetooth ได้ หรืออะไรก็ตามแต่เข้ามาเรื่อยๆ จะเกิดอะไรขึ้นกับคลาสที่ implement interface พวกนี้? นี่แหละปัญหาของ **Interface Pollution** 💀
{% endhint %}

## 😒 **ควรออกแบบยังไงดี**

### 😄 แยกการผูกกันโดยใช้ตัวแทน

แทนที่เราจะให้ IElectronic ไปผูกกับการตัวตั้งเวลา เราก็สร้างตัวแทนขึ้นมา **\(ElectronicScheduler\)** ซึ่งเจ้าตัวแทนมีหน้าที่เชื่อมการทำงานระหว่าง **การตั้งเวลา** และ **อุปกรณ์ที่ต้องทำงานด้วย** โดยเมื่อมันถึงเวลาที่ตั้งเมื่อไหร่ มันก็มีหน้าที่มาสั่งให้อุปกรณ์นั้นๆทำการปิดเครื่อง ตามภาพด้านล่าง

![](../../.gitbook/assets/image%20%284%29.png)

```csharp
public class ElectronicScheduler : ISwitchScheduler
{
    public IElectronic Electronic { get; set; }

    public ElectronicScheduler(IElectronic electronic)
    {
        Electronic = electronic;
    }

    public void SetTurnOffSchedule(int seconds)
    {
        var timer = new Timer(seconds * 1000);
        timer.Elapsed += (sndr, se) =>
        {
            timer.Stop();
            Electronic.TurnOff();
        };
        timer.Start();
    }
}
```

{% hint style="success" %}
👍 **ข้อดีในการออกแบบ**  
เมื่อยกหน้าที่การรับผิดชอบไปให้กับตัวแทนแล้ว ข้อดีที่ได้คือ ต่อให้ ISwitchScheduler มีการเปลี่ยนแปลงไปยังไงก็ตาม มันก็จะไม่มีผลกระทบกับตระกูล Electronic เลย กระทบแค่ ElectronicScheduler ตัวเดียวเท่านั้น และก็สมควรแล้วเพราะเป็นหน้าที่ดูแลของมัน

👎 **ข้อเสียในการออกแบบ**  
ทุกครั้งที่สร้าง object ของ Electronic ที่ตั้งเวลาได้ เราจะต้องสร้าง object ของ ElectronicScheduler ตามมาด้วยเสมอ
{% endhint %}

### 😄 แยกการผูกกันโดยการแยก Inheritance ตามความเหมาะสม

แทนที่เราจะเหมารวม Electronic ทุกประเภทจะต้องตั้งเวลาได้ เราก็แค่แยกการทำ Inheritance ตามความเหมาะสมของแต่ละคลาสก็เพียงพอแล้ว และแบบนี้ค่อนข้างที่จะเบาสบายกว่าด้วย

![](../../.gitbook/assets/image%20%2831%29.png)

## 🎯 บทสรุป

การละเมิดกฎของ **ISP** ทำให้โค้ดเกิด **Coupling**, **Rigidity** และ **Viscosity** ขึ้นสูงมาก เมื่อ fat interface มีการแก้ไข เราจะต้องไล่ตามเช็คทุกๆคลาสที่ใช้ interface นั้น วิธีการแก้ไขที่ง่ายที่สุดคือการแตก interface ออกเป็นเรื่องๆ แล้วให้เฉพาะคลาสที่ต้องใช้จริงๆไป implement interface เท่าที่ต้องใช้เท่านั้น

