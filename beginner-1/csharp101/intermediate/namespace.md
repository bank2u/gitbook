# 26.Namespace

💬 วันนึงพอเราเขียนโปรแกรมไปเยอะๆมีคลาสต่างๆมากมาย เราจะแบ่งแยกยังไงว่าคลาสพวกนั้นมันทำงานอยู่ในหมวดหมู่ไหนบ้าง เช่นหมวดคำนวณ หมวดตรวจสอบความถูกต้อง หรือคลาสนั้นๆเป็นแค่ model \(คืออะไรหว่า?\) ซึ่งจากปัญหาที่ว่ามา พระเอกที่จะมาช่วยเราในตอนนี้ก็คือสิ่งที่เรียกว่า **Namespace** นั่นเอง

{% embed url="https://www.youtube.com/watch?v=O1mjLqdHvuc&list=PLUjAn8nwWnijERZ3HpzBk7NfSrau74\_lQ&index=44" caption="" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 Namespace คือ

เป็นแค่วิธีการแบ่งแยกกลุ่มของโค้ดของเรา โดยเราจะใช้แบ่งหมวดการทำงานของโค้ดแต่ละเรื่องว่ามันใช้สำหรับทำอะไร เช่นจากตัวอย่างด้านล่างผมแยกโค้ดออกเป็น 2 กลุ่มคือ 1.ของที่เป็น data model กับ 2.ของที่ใช้ต่อฐานข้อมูล

```csharp
namespace ProjectName.Models
{
    public class Teacher
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
    public class Student
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public Teacher TeacherAssistance { get; set; }
    }
}

namespace ProjectName.DAC
{
    public interface SQLConnector
    {
        void Connect();
        void Disconnect();
    }
    public interface IMongoDbConnector
    {
        void Connect();
        void Disconnect();
    }
}
```

{% hint style="info" %}
**Sub namespace**  
เราสามารถสร้าง namespace ย่อยๆลงไปได้เรื่อยๆ โดยใช้เครื่องหมายจุด . นะครับ
{% endhint %}

{% hint style="warning" %}
**Nested namespace**  
เราสามารถสร้าง namespace ภายใน namespace ได้เหมือนกัน และมันก็คือการทำ sub namespace ธรรมดานั่นแหละ **ซึ่งปรกติเขานิยมใช้จุดแทนธรรมดาแทนการสร้าง nested namespace ครับ**
{% endhint %}

### 👨‍🚀 using keyword

เวลาที่เราจะเรียกใช้ namespace อะไร เราจะต้องใช้คำสั่ง **using** keyword ไว้ด้านบนสุดของไฟล์ เพื่อบอกว่าโค้ดที่เรากำลังเขียนอยู่นี้มันสามารถใช้ namespace อะไรได้บ้าง ซึ่งถ้าผมอยากจะใช้คลาส **Teacher** จากตัวอย่างด้านบนผมจะต้องเขียนโค้ดไว้ตามตัวอย่างด้านล่างนี้เบย

```csharp
using System;
using ProjectName.Models;

namespace DemoConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Teacher t01 = new Teacher();
        }
    }
}
```

### 👨‍🚀 Aliases

ในบางครั้งเราไม่อยากนำ namespace เข้ามาใช้ทั้งหมด เราก็สามารถสร้าง **aliases** ให้กับ namespace ที่จะใช้ได้ด้วยนะ ตามโค้ดด้านล่างเลย ผมใช้ aliases ที่บรรทัดที่ 1 และเรียกใช้งานมันบรรทัดที่ 9

```csharp
using sys = System;

namespace DemoConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            sys.Console.WriteLine("Hello World!");
        }
    }
}
```

