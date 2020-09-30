# ลองเขียนโค้ดอัพโหลดไฟล์กันบ้าง

หลังจากที่เราได้ลองสร้าง Azure Storage และทำการอัพโหลดไฟล์ผ่านหน้าเว็บไปแล้ว ในรอบนี้เราจะลองเขียนโค้ด เพื่อทำการจัดการของต่างๆใน Azure Storage กันดูบ้างนะ ส่วนใครที่ยังไม่ได้อ่านการอัพโหลดผ่านเว็บลองไปอ่านได้จากลิงค์ด้านล่างนี้

{% page-ref page="create.md" %}

## 🤔 เขียนโค้ดติดต่อ Azure Storage ยังไง ?

ในรอบนี้ผมจะใช้ภาษา C\# .NET Core เขียนโค้ดนะครับ ส่วนภาษาอื่นๆไม่ต้องน้อยใจ ด้านล่างสุดผมทำลิงค์ของพวกภาษา Java, Node.js, Python, JavaScript, Go, PHP, Ruby ไว้เรียบร้อยแล้วลองดูได้

### สร้างโปรเจค

ในรอบนี้ผมจะใช้ .NET Core ในการสร้างโปรเจคนะครับ ดังนั้นเริ่มต้นก็เปิด command prompt หรือ terminal ขึ้นมาเลยแล้วใช้คำสั่งสร้างโปรเจคกันเลย ส่วนใครที่ยังไม่ได้ลง .NET Core ก็สามารไปลงได้จากลิงค์นี้ครับ [Download .NET Core ](https://dotnet.microsoft.com/download)

```text
dotnet new console -n blob-quickstart
```

> blob-quickstart คือชื่อโปรเจคนะครับ อยากได้ชื่ออื่นก็เปลี่ยนได้เลย

ตอนนี้เราก็จะได้โปรเจคมา 1 ตัวละ ถัดไปก็เข้าไปที่โปรเจคนั้นครับด้วยคำสั่งด้านล่างนี้ \(ใครที่เปลี่ยนชื่อโปรเจคก็ใส่ชื่อเป็นชื่อโปรเจคที่ตัวเองตั้งไว้นะ\)

```text
cd blob-quickstart
```

ทดสอบว่าโปรเจคไม่มีปัญหาอะไรด้วยคำสั่งด้านล่าง

```text
dotnet build
```

ซึ่งถ้าไม่มีปัญหาอะไรก็น่าจะได้ผลลัพท์ออกมาแบบนี้

```text
Build succeeded.
    0 Warning(s)
    0 Error(s)
```

### ติดตั้ง Library เพื่อทำงานกับ Azure Storage

ตัวโปรเจคที่เราสร้างขึ้นมามันจะไม่สามารถทำงานกับ Azure Storage ได้ ดังนั้นเราเลยต้องลง library เสริมโดยใช้คำสั่งด้านล่างก็เป็นอันเสร็จสิ้นพิธี

```text
dotnet add package Microsoft.Azure.Storage.Blob
```

### เขียนโค้ดติดต่อ Azure Storage

คราวนี้ให้เราเปิด Visual Studio Code ขึ้นมา โดยใช้คำสั่ง `code .` ภายใน command prompt/terminal ได้เลย หรือจะเปิด Visual Studio Code แล้วเปิด folder มาที่ตัวโปรเจคก็ได้ แล้วก็เปิดไฟล์ `Program.cs` ขึ้นมารอเลย

\(ใครที่ยังไม่มี Visual Studio Code สามารถดาวโหลดได้จากลิงค์นี้นะ [Download Visual Studio Code](https://code.visualstudio.com/)\)

![](../../../.gitbook/assets/image%20%28234%29.png)

{% hint style="info" %}
**Visual Studio Code: C\# Extension**  
ในตัวอย่างที่เป็นสีสวยงาม เพราะผมลง extension 2 ตัวให้กับ Visual Studio Code นะครับ ถ้าสนใจก็กดติดตั้งได้จากลิงค์ด้านล่างเลย

* [C\#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [C\# Extensions](https://marketplace.visualstudio.com/items?itemName=jchannon.csharpextensions)
{% endhint %}

ถัดมาผมก็จะเขียนโค้ดให้มันเตรียมทำงานกับ Azure Storage ไว้ประมาณนี้นะครับ

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Blob;

namespace blob_quickstart
{
    class Program
    {
        public static void Main()
        {
            Console.WriteLine("Processing.");

            ProcessAsync().GetAwaiter().GetResult();

            Console.WriteLine("All done.");
            Console.ReadLine();
        }

        private static async Task ProcessAsync()
        {
        }
    }
}
```

ตัวโค้ดจะเรียกใช้งาน method `ProcessAsync()` ซึ่งผมตั้งใจว่าจะให้มันเป็นตัวติดต่อไปทำงานกับ Blob storage ดังนั้นเราก็แก้เจ้า method นี้ตามด้านล่างเลย

```csharp
private static async Task ProcessAsync()
{
    var connectionString = "YOUR_CONNECTION_STRING_HERE";

    CloudStorageAccount storageAccount;
    if (CloudStorageAccount.TryParse(connectionString, out storageAccount))
    {
        // พร้อมทำงานกับ Blob Storage แล้ว
    }
    else
    {
        Console.WriteLine("The connection string isn't valid");
        Console.WriteLine("Press any key to exit the application.");
    }
}
```

ในโค้ดด้านบนเราจะทำการเชื่อมต่อไปยัง Blob Storage ซึ่งการเชื่อมต่อนั้นมันจะต้องการ Key บางอย่างเพื่อยืนยันว่าคนที่ต่อเข้ามามีสิทธิ์ที่ถูกต้องด้วย \(ในโค้ดบรรทัดที่ 3\) ดังนั้นในขั้นตอนถัดไปเราก็จะไปเอา Key มาครับ

#### เตรียม connection สำหรับต่อ Azure Storage

ตัวโค้ดที่เราจะเขียนนั้น มันจะต้องติดต่อไปที่ Azure Storage เพื่อทำการอัพโหลดของต่างๆเข้าไป ซึ่งการที่จะทำแบบนั้นได้เราจะต้องไปเอาสิทธิ์ในการอ่านเขียนไฟล์มาก่อน ดังนั้นเราก็จะไปเอาสิทธิ์ที่ว่านี้จาก Storage account ของเรา โดยเข้าไปที่ [Azure portal](https://portal.azure.com/) แล้วเปิด Storage account ของเราขึ้นมาแล้วเลือกไปที่เมนู **Access keys**

![](../../../.gitbook/assets/image%20%28293%29.png)

ภายใน Access keys เขาจะมี key ให้เรา 2 ตัว ส่วนเราจะใช้ key ตัวไหนก็ได้ ซึ่งในตัวอย่างผมจะใช้ key1 นะครับ ดังนั้นก็กด Copy Connection string มาเลย

![](../../../.gitbook/assets/image%20%28404%29.png)

คราวนี้เราก็จะเอา connection string ที่ copy มาไปวางใส่ในโค้ดของเรา ในบรรทีดที่ 3 ก็จะได้ประมาณนี้ครับ

```csharp
var connectionString = "DefaultEndpointsProtocol=https;AccountName=saladpukstorage;AccountKey=V84hggJN/t56SYwQHoMDUt5kFD2bOOtUdxwK5ndMdRCyBZ4kAo8WLz7pU/H09zfrdS+SmmC8aYJsrWwoYubm4Q==;EndpointSuffix=core.windows.net";
```

{% hint style="warning" %}
**ข้อควรระวัง**  
สำหรับงานที่เป็น production จริงๆไม่ควรทำแบบนี้นะครับ มันจะกลายเป็น hard code ซึ่งถ้าเราจะเปลี่ยน connection string เมื่อไหร่ก็ต้อง compile ใหม่ทุกครั้ง ของจริงควรเขียนเป็น configuration ที่แก้ไขได้อีกทีครับ และการไปเอา connection string ตัวจริงมาใช้ก็ไม่เหมาะครับ มันมีการเข้าถึง key ได้หลายแบบเลยเช่น **SAS** อะไรพวกนี้ ซึ่งเดี๋ยวในบทถัดๆไปจะกล่าวถึงอีกทีครับ
{% endhint %}

ตอนนี้โค้ดเราก็น่าจะสามารถเชื่อมต่อ Blob Storage ได้แล้วนะครับ

### เขียนโค้ดสร้าง Container

ถัดมาเราก็จะลองสร้าง **Container** ขึ้นมาเพื่อเอาไว้เก็บไฟล์ที่เราจะอัพโหลดขึ้นไปนะครับ ซึ่งในตัวอย่างผมจะสร้าง Container ชื่อ **saladpuk-upload-by-code** ซึ่งผมก็จะไปแก้ไขโค้ดที่อยู่ใน `if` กันนะให้เป็นแบบนี้

```csharp
var cloudBlobClient = storageAccount.CreateCloudBlobClient();
var cloudBlobContainer = cloudBlobClient.GetContainerReference("saladpuk-upload-by-code");
await cloudBlobContainer.CreateIfNotExistsAsync();
```

เจ้าตัว Container ที่ผมสร้างขึ้นมาใหม่นี้ โดยปรกติมันจะโดนกำหนดสิทธิ์ในการเข้าถึงเป็น **private** หรือพูดง่ายๆคือไม่เปิดให้คนอื่นดูเลยนอกจากคนที่มีสิทธิ์ แต่ในตัวอย่างนี้ผมจะเปิดสิทธิ์ให้คนอื่นเข้าถึงได้ เพราะจะทำเป็นคล้ายๆเว็บฝากรูป ดังนั้นผมก็จะเขียนโค้ดต่อใน `if` อีกนิดหน่อยเพื่อกำหนดสิทธิ์การเข้าถึงมันใหม่ตามนี้

```csharp
var permissions = new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
};
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

เพียงเท่านี้เราก็จะได้ Container ใหม่แล้วนะครับ

### เขียนโค้ดอัพโหลดไฟล์

ถัดมาผมก็จะเขียนโค้ดอัพโหลดรูปในเครื่องขึ้นไปที่ Blob Storage นะครับ ซึ่งรูปของผมอยู่ที่ `d:\saladpakLogo-05.png` และผมจะตั้งชื่อที่อัพโหลดขึ้นไปให้ชื่อว่า `img01.png` ด้วยดังนั้นผมเขียนโค้ดภายใน `if` ออกมาเป็นแบบนี้

```csharp
var file = new FileStream(@"d:\saladpakLogo-05.png", FileMode.Open);
var cloudBlockBlob = cloudBlobContainer.GetBlockBlobReference("img01.png");
cloudBlockBlob.Properties.ContentType = "image/png";
await cloudBlockBlob.UploadFromStreamAsync(file);
```

เป็นอันเรียบร้อยแล้ว ดังนั้นผมก็จะลองสั่งให้มันทำงานดู โดยการเข้าไปที่ command prompt/terminal โดยการใช้คำสั่งนี้

```text
dotnet run
```

ในหน้าจอ command prompt/terminal ก็น่าจะได้ผลลัพท์แบบนี้ \(ถ้าไม่ได้ลองไปตรวจดูนะว่าทำขั้นตอนไหนผิด\)

```text
Processing.
All done.
```

### ลองตรวจไฟล์ที่อัพไปดู

ไฟล์ที่เราอัพโหลดเข้าไปจะอยู่บนคลาว์ดังนั้นเราก็ต้องเข้าไปดูที่ตัว Blob Storage ของเราครับ

![](../../../.gitbook/assets/image-956.png)

เราก็จะเจอ Container ใหม่ที่เราสร้างขึ้นมาจากโค้ด ให้ลองกดมันเข้าไปเลย

![](../../../.gitbook/assets/image%20%28225%29.png)

แล้วเราก็จะเจอรูปของเรา ก็ให้กดมันเข้าไปอีกที

![](../../../.gitbook/assets/image%20%28370%29.png)

ถัดมาเราก็กด copy ลิงค์แล้วลองเอาไปเปิดดูใน web browser ดูครับ

![](../../../.gitbook/assets/image%20%28386%29.png)

![](../../../.gitbook/assets/image%20%28174%29.png)

เรียบร้อยแล้วครับ เราสามารถอัพโหลดรูปขึ้นไปบน Azure Storage ได้แล้ว และเรียกดูเหมือนเว็บฝากไฟล์ได้แบ้ว

## 📜 Source code

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Blob;

namespace blob_quickstart
{
    class Program
    {
        public static void Main()
        {
            Console.WriteLine("Processing.");

            ProcessAsync().GetAwaiter().GetResult();

            Console.WriteLine("All done.");
            Console.ReadLine();
        }

        private static async Task ProcessAsync()
        {
            var connectionString = "DefaultEndpointsProtocol=https;AccountName=saladpukstorage;AccountKey=V84hggJN/t56SYwQHoMDUt5kFD2bOOtUdxwK5ndMdRCyBZ4kAo8WLz7pU/H09zfrdS+SmmC8aYJsrWwoYubm4Q==;EndpointSuffix=core.windows.net";

            CloudStorageAccount storageAccount;
            if (CloudStorageAccount.TryParse(connectionString, out storageAccount))
            {
                var cloudBlobClient = storageAccount.CreateCloudBlobClient();
                var cloudBlobContainer = cloudBlobClient.GetContainerReference("saladpuk-upload-by-code");
                await cloudBlobContainer.CreateIfNotExistsAsync();

                var permissions = new BlobContainerPermissions
                {
                    PublicAccess = BlobContainerPublicAccessType.Blob
                };
                await cloudBlobContainer.SetPermissionsAsync(permissions);

                var file = new FileStream(@"d:\saladpakLogo-05.png", FileMode.Open);
                var cloudBlockBlob = cloudBlobContainer.GetBlockBlobReference("img01.png");
                cloudBlockBlob.Properties.ContentType = "image/png";
                await cloudBlockBlob.UploadFromStreamAsync(file);
            }
            else
            {
                Console.WriteLine("The connection string isn't valid");
                Console.WriteLine("Press any key to exit the application.");
            }
        }
    }
}
```

## 🔗 ตัวอย่างภาษาอื่นๆ

* [Node.js](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-nodejs-v10)
* [Python](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-python)
* [JavaScript - Azure SDK version 10](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-javascript-client-libraries-v10)
* [JavaScript - Azure SDK version 2](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-javascript-client-libraries)
* [Go](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-go?tabs=windows)
* [PHP](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-php?tabs=windows)
* [Ruby](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-ruby)

## 🔥 ทิ้งท้ายของน่าสนใจ!!

พอเขียนบทความนี้เสร็จปุ๊ป ทาง Microsoft ก็ส่งอีเมล์แจ้งเตือนมาทันทีเลยว่า ตรวจพบ security รั่วเนื่องจากผมเอา secret ของตัว Azure Storage ไปเปิดเผยต่อสาธารณะในบทความนี้ไงล่ะ \(ผมลิงค์บทความเข้ากับ GitHub\) นี่คือข้อดีอีกเรื่องของการที่เราใช้คลาว์ของ Microsoft ครับ ทำให้เราอุ่นใจขึ้นเยอะเลยว่าของหลายๆอย่างเราสามารถป้องกันก่อนที่มันจะร้ายแรงจนแก้ไม่ได้ครับ

![](../../../.gitbook/assets/image%20%28590%29.png)

ซึ่งเขาก็บอกรายละเอียดไว้ครบถ้วนเลย พร้อมวิธีแก้ปัญหา ดังนั้นผมก็เลยต้องไป reset key ทั้ง 2 ออกครับ \(แต่จริงๆผมไม่ต้องทำก็ได้ เพราะ Storage ตัวนี้เดี๋ยวก็ลบทิ้งละเพราะเขียนบทความเสร็จแล้ว\)

![](../../../.gitbook/assets/image%20%28746%29.png)

