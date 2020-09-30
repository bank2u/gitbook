---
description: "\U0001F914 การเก็บรหัสลับบนคลาว์ เขาทำกันยังไงนะ ?"
---

# การป้องกันความลับหลุดตอนที่ 3

## 😘 ทวนปัญหา

จาก 2 บทความที่ผ่านมาเราก็จะเห็นวิธีการเก็บ **`ความลับ`** ของแอพเราอย่างง่ายๆไปละ แถมยังสามารถจัดการแยก **`Development Environment`** ออกจาก **`Production Environment`** ได้ด้วย แต่ว่ามันก็ยังไม่สามารถเก็บความลับได้ถึงขีดสุด ดังนั้นเรามาไล่รายการที่ค้างกันก่อนดีก่า ตามด้านล่างเบย

### 💔 รายการที่ต้องแก้

* ~~การผัง Secret ไว้เป็น Hard code ทำให้เราต้อง Build & Deploy โปรเจคใหม่เท่านั้น~~
* ~~ใน Source code ของมี Secret ติดเข้าไปใน commit เสมอ~~
* ~~ถ้าเราไม่ให้ access เข้าถึง service ตัวนั้นๆ เราก็จะเอาแอพขึ้นคลาว์ไม่ได้~~
* Developer บางคนอาจจะรู้ Secret ซึ่งเขาอาจจะเป็นคนร้ายในอนาคต หรือ เป็นจุดที่อันตรายถ้าเขาเก็บมันไว้ไม่ดี
* ถ้ามีคนที่มี access เข้าถึง service บนคลาว์ตัวนั้นๆได้ เขาก็สามารถเข้ามาดู secret เหล่านั้นได้เหมือนเดิม

{% hint style="success" %}
**แนะนำให้อ่าน**  
บทความนี้เป็นส่วนหนึ่งของคอร์ส [🤠 **Cloud Playground**](https://www.saladpuk.com/cloud/cloud-playground) ที่จะพาเพื่อนๆแมวน้ำทั้งหลายได้ลองมาดูว่า การสร้างโปรเจคเพื่อทำงานบนคลาว์ โดยใช้มาตรฐานสากลจริงๆแล้วเขาทำกันยังไง ส่วนถ้าสนใจอยากอ่านต่อก็กดไปที่ลิงค์สีฟ้าๆได้เลย

ส่วนถ้าอยากอ่านบทความก่อนหน้าให้อ่านได้จากลิงค์นี้เบย **\*\*\[**การป้องกันความลับหลุดตอนที่ 2**\]\(**[https://www.saladpuk.com/cloud/cloud-playground/app-config-02](https://www.saladpuk.com/cloud/cloud-playground/app-config-02)**\) \*\***ซึ่งในบทนี้ผมอธิบายเรื่อง **Development Environment vs Production Environment** ไว้ในระดับนึงละ
{% endhint %}

ในตอนนี้ปัญหาของเราก็จะเหลือแค่ ป้องกันไม่ให้คนในทีมเป็นคนร้ายเสียเอง เนื่องจากบางคนอาจจะมีสิทธิ์ในการเข้าถึง `ความลับ` เหล่านั้นได้นั่นเอง

## 🤠 เก็บ Secret ด้วย App Configuration Service

คราวก่อนเรามีการเก็บความลับต่างๆไว้ภายใน **`Configuration`** ของตัว **Web App Service** ตามรูปด้านล่าง

![](../../.gitbook/assets/image%20%28581%29.png)

> ซึ่งการทำแบบนี้ก็ไม่ได้ผิดอะไรนะ เพียงแค่เราต้องไว้ใจ Developer คนที่สามารถเข้าถึง Web App Service ตัวนี้เป็นอย่างมากว่าเขาจะไม่แอบเข้ามาเอา `ความลับ` ที่เปิดโชว์อยู่ตรงนี้ไปทำมิตรดีมิตรร้ายอะไร \(หุหุ\)

สำหรับโปรเจคบางตัวที่มีระดับความซีเรียสของความลับสูงๆ เช่น การเงิน, ข้อมูลส่วนตัว บลาๆ มันก็อาจจะบังคับให้เราไม่สามารถทำในรูปแบบนี้ได้ ดังนั้นในรอบนี้เราจะ **แยกความลับออกจากคลาว์เซอร์วิส** เพื่อให้ความลับเรายิ่งเข้าถึงยากขึ้นไปอีก โดยในตัวอย่างนี้จะใช้วิธีแบบง่ายไปก่อนจะได้เข้าใจ concept พื้นฐานนั่นเอง ดังนั้นไปดูตัวช่วยในเรื่องนี้ของเรากันเลยดีกั่ว

### 🔥 สร้าง App Configuration Service

![](../../.gitbook/assets/image%20%28306%29.png)

เจ้าตัวนี้จะช่วยให้เราบริหารจัดการ configuration ต่างๆของเราได้ง่ายขึ้น โดยที่เวลาเราจะแก้ configuration ต่างๆ เราก็ไม่จำเป็นต้อง rebuild & re-deploy ตัวแอพของเราเลย แถมยังเข้าถึงได้จากหลายๆแอพอีกด้วย ดังนั้นก็ทำการสร้างแล้วใส่ข้อมูลต่างๆตามชอบใจเลย

![&#xE40;&#xE08;&#xE49;&#xE32;&#xE15;&#xE31;&#xE27;&#xE19;&#xE35;&#xE49;&#xE21;&#xE35; Free tier &#xE43;&#xE2B;&#xE49;&#xE40;&#xE23;&#xE32;&#xE43;&#xE0A;&#xE49;&#xE44;&#xE14;&#xE49;&#xE19;&#xE30; &#xE44;&#xE21;&#xE48;&#xE40;&#xE2A;&#xE35;&#xE22;&#xE01;&#xE30;&#xE15;&#xE31;&#xE07;&#xE40;&#xE1A;&#xE22;](../../.gitbook/assets/image%20%28205%29.png)

หลังจากที่สร้างมันเสร็จแล้ว เราก็จะไปดูที่เมนู `Configuration explorer` เพื่อเข้าไปจัดการเก็บความลับของเราไว้ในเจ้า service นี้กัน ตามรูปด้านล่างเรย

![](../../.gitbook/assets/image%20%28166%29.png)

ซึ่งในเมนูนี้เขาจะโชว์รายการ configuration ทั้งหมดของเราขึ้นมา ซึ่งเจ้าตัวนี้ผมพึ่งสร้างขึ้นมามันก็เลยจะยังไม่มีรายการอะไรอยู่ในนี้ ดังนั้นเราก็จะลองสร้าง configuration ตัวแรกของเรากันดีกว่า โดยการกดปุ่ม **`+ Create`** ตามรูปด้านล่างเรยครัช

![&#xE02;&#xE2D;&#xE2A;&#xE23;&#xE49;&#xE32;&#xE07;&#xE40;&#xE1B;&#xE47;&#xE19; Key-value &#xE41;&#xE1A;&#xE1A;&#xE18;&#xE23;&#xE23;&#xE21;&#xE14;&#xE32;&#xE44;&#xE1B;&#xE01;&#xE48;&#xE2D;&#xE19;&#xE19;&#xE30; &#xE08;&#xE30;&#xE44;&#xE14;&#xE49;&#xE44;&#xE21;&#xE48; &#xE07;&#xE07;](../../.gitbook/assets/image%20%28118%29.png)

ถัดไปก็ทำการกำหนดค่าต่างๆ ซึ่งผมจะใช้ **Key** เดิมนั่นก็คือเจ้า **DbConnectionString** นั่นเอง ส่วนค่าของมันผมใช้เป็น Yeeha! from App Configuration ละกัน ส่วนที่เหลือจะใส่หรือไม่ใส่ก็ได้ แล้วก็กดกด **`Apply`** โลด

![](../../.gitbook/assets/image%20%28768%29.png)

เสร็จเรียบร้อยละ เจ้า **DbConnectionStrign** ก็จะถูกเก็บเข้าไปอยู่ในตัว service ตัวนี้เรียบร้อยแล้ว

![](../../.gitbook/assets/image-951.png)

### 🔥 ทำ Web App ให้รองรับ App Configuration Service

เนื่องจากเราไม่อยากให้ความลับของเราไปโชว์หลาอยู่ที่ Web App Service ดังนั้นเราก็จะต้องมากำหนดให้เจ้าตัวเว็บของเราไปอ่านค่า configuration ต่างๆจาก App Configuration Service ที่เราพึ่งสร้างไปนั่นเอง ดังนั้นเราก็จะติดตั้ง package ด้านล่างนี้เข้าไปให้กับตัวเว็บของเรานั่นเอง

```text
dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore --version 3.0.0-preview-011100002-1192
```

สุดท้ายก็ไปที่ไฟล์ `Program.cs` เพื่อไปบอกให้มันไปอ่านค่าจาก App Configuration Service นั่นเอง \(โค้ดของ dotnet core 2 กับ dotnet core 3 ไม่เหมือนกันนะดูดีๆ

{% tabs %}
{% tab title=".NET Core 2.x" %}
```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config
            .AddAzureAppConfiguration("AppConfigConnectionStrings");
        })
        .UseStartup<Startup>();
```
{% endtab %}

{% tab title=".NET Core 3.x" %}
```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
    {
        var settings = config.Build();
        config
        .AddAzureAppConfiguration("AppConfigConnectionStrings");
    })
    .UseStartup<Startup>());
```
{% endtab %}
{% endtabs %}

ซึ่งในโค้ดด้านบนเราจะเห็น **AppConfigConnectionStrings** ซึ่งตรงจุดนี้เราจะต้องเอา **`Connection String`** ของตัว App Configuration Service มาใส่ไว้ เพื่อให้ตัวเว็บของเราสามารถเข้าไปอ่านความลับต่างๆมาได้นั่นเอง โดยเราจะเข้าไปเอาค่านี้ได้จากเมนู **`Access keys`** ของเจ้า App Configuration Service นั่นเอง ตามรูปด้านล่างเลย แล้วกดปุ่ม Copy ที่ Connection string เอานะ

![](../../.gitbook/assets/image%20%28815%29.png)

หลังจากที่เอาที่ copy ไปวางใส่ละ สุดท้ายเราก็จะลองเปิดเว็บในเครื่องตัวเองดู ซึ่งผลลัพท์ก็คือ

![Localhost](../../.gitbook/assets/image-967.png)

ตัวเว็บที่อยู่ใน Localhost ของเรา มันจะไปใช้ Configuration ที่อยู่บน **App Configuration** แทนนั่นเองครับ ดังนั้นถัดไปผมก็จะลอง Deploy ตัวเว็บนี้ขึ้นไปลองบนคลาว์ แล้วลองเปิดดูบ้างนะ ซึ่งผลลัพท์ที่ได้ก็คือ

![Cloud](../../.gitbook/assets/image%20%28249%29.png)

ตัวเว็บที่ที่อยู่บนคลาว์ของเรา มันก็ไปใช้ Configuration ที่อยู่บน **App Configuration** เหมือนกับที่อยู่ใน Localhost นั่นเอง ซึ่งการใช้วิธีนี้มันจะช่วยให้เราไม่ต้องเก็บ Configuration ไว้บน Web App Service แล้วนั่นเอง เพียงแค่เราแยก Subscription ที่ทำการเก็บ App Configuration ออกจาก Web App Service เพียงเท่านี้จะทำให้ Developer ถูกจำกัดสิทธิ์ในการเข้าถึงได้แทบจะเป็นแบบที่เราอยากได้ละ

## 😡 อย่ามาหลอกกันนะเฟร้ย!!

จากที่ทำมาทั้งหมดเหมือนมันจะดูดี แต่ถ้าดูดีๆเราจะพบว่าเจ้า **`ความลับ`** มันกลับมาอยู่ในรูปแบบของ Hard Code ที่ฝังไว้กับ source code เหมือนบทความแรกเลย และ ต่อให้เราให้มันไปอ่าน Environment Variable หรืออะไรก็ตาม มันก็ไม่ได้เป็นการซ่อนความลับอะไรเลยนั่นเอง

คำตอบของผมคือ **ใช่ครับ** มันเป็นการย้อนวนกลับมาที่ปัญหาแรกๆของเรา แต่ผมถือว่า **มันเป็นการถอยกลับที่มีความหมาย** เพราะในบทความนี้เราได้เตรียม **infrastructure** เพื่อให้ระบบของเราทำการแยก ความลับ ออกจากคนที่ไม่ประสงค์ดีได้แล้ว ดังนั้นเดี๋ยวเราไปดูบทความที่ 4 ตัวถัดไปว่าเราจะจัดการเก็บซ่อนของพวกนี้ทุกเม็ดได้ยังไงกันบ้างดีกั่ว

## 🎯 สรุปคร่าวๆก่อนไปต่อ

ตอนนี้ปัญหาบางตัวถูกแก้ไป แต่ตัวเก่าบางตัวก็กลับมาทักทายเราเช่นเคยแล้วนะ ดังนั้นมาอัพเดทกันหน่อยดีกั่ว

* การผัง Secret ไว้เป็น Hard code ทำให้เราต้อง Build & Deploy โปรเจคใหม่เท่านั้น
* ใน Source code ของมี Secret ติดเข้าไปใน commit เสมอ
* Developer บางคนอาจจะรู้ Secret ซึ่งเขาอาจจะเป็นคนร้ายในอนาคต หรือ เป็นจุดที่อันตรายถ้าเขาเก็บมันไว้ไม่ดี
* ถ้ามีคนที่มี access เข้าถึง service บนคลาว์ตัวนั้นๆได้ เขาก็สามารถเข้ามาดู secret เหล่านั้นได้เหมือนเดิม
* ~~ถ้าเราไม่ให้ access เข้าถึง service ตัวนั้นๆ เราก็จะเอาแอพขึ้นคลาว์ไม่ได้~~

