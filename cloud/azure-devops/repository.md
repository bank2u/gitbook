# เล่นกับ Repository

{% hint style="success" %}
**แนะนำให้อ่าน**  
บทความนี้เป็นส่วนหนึ่งของคอร์ส [**👶 Azure DevOps**](https://saladpuk.gitbook.io/learn/cloud/azure-devops) ที่จะมาสอนการใช้งานทุกสิ่งที่ DevOps ควรจะต้องมี เพื่อให้เราสามารถทำ Feedback loop ได้ไวขึ้น ดังนั้นถ้าเพื่อนๆสนใจอยากดูว่ามันทำอะไรได้ก็กดไปอ่านที่บทความหลักได้เลยครัช
{% endhint %}

{% hint style="warning" %}
**คำเตือน**  
ถ้าเพื่อนๆต้องการทำตามบทความนี้ จะต้องมีโปรเจคใน **Azure DevOps** เสียก่อน แต่ถ้าใครยังไม่มีก็สามารถไปทำตามได้จากบทตัวนี้ก่อน [**เล่น Azure DevOps กัน**](https://saladpuk.gitbook.io/learn/cloud/azure-devops/azure-devops) แล้วค่อยกลับมาทำตามบทความนี้ก็ได้ครัช
{% endhint %}

## 🔥 ทำงานกับ Repository

ที่โปรเจคที่เราได้มามันจะมีเมนูอยู่ด้านซ้ายมือเอาไว้เข้าไปจัดการตามแต่ละเรื่องต่างๆ ซึ่งในรอบนี้เราจะมาลองใช้ **Repository** เพื่อเอาไว้ให้ทีมทำงานร่วมกันจัดการ source code ดังนั้นเราก็จะจิ้มมันลงไปหนึ่งจึ๊ก

![](../../.gitbook/assets/image%20%28772%29.png)

พอเรากดเข้ามาแล้ว เขาก็จะแตกเมนูเป็นตัวย่อยๆให้เราทำงานด้วย \(เดี๋ยวไปอธิบายแต่ละเมนูแยกเป็นแต่ละบทความอีกที\) แต่ที่ด้านขวามือเราจะเห็นว่าหน้าตามันเปลี่ยนไป **ซึ่งถ้าเรายังไม่เคยทำอะไรกับโปรเจคนี้เลย**เขาจะให้เราเลือกว่าเราจะทำงานกับ **Repository** ของโปรเจคนี้ยังไง ซึ่งเขามีตัวเลือกให้เราลองเล่นได้ทั้งหมด 4 อย่าง

### 1️⃣ ใช้ Repository ใหม่จาก Azure DevOps เลย

ในกรณีที่เราอยากได้ Repository ที่ไม่มีอะไรเลย เราก็สามารถเริ่มต้นทำงานกับเจ้า repository ตัวนีได้เลย โดยการ **Clone** จาก URL ที่เขาโชว์ไว้ให้นั่นเอง ซึ่งจากตัวนี้ถ้าเราอยากได้ **Credentials** ก็สามารถกดสร้างได้จากหน้านี้เลย

![](../../.gitbook/assets/image%20%2886%29.png)

### 2️⃣ เอางานเก่าขึ้นมาใช้แทน

ในกรณีที่เพื่อนๆมี source code อยู่ใน Repository เก่าของตัวเองอยู่แล้ว ก็สามารถเอางานที่อยู่ในนั้น Push ขึ้นมาได้เลยโดยใช้ Repository ตัวใหม่ที่ Azure DevOps สร้างมาให้ตัวนี้

![](../../.gitbook/assets/image%20%28284%29.png)

### 3️⃣ ใช้ Repository เดิม

ในกรณีที่เพื่อนๆมีงาน source code อยู่ใน **GitHub** หรือใน **Team Foundation Version Control \(TFCV\)** อยู่ก่อนหน้าแล้ว ก็สามารถเลือกใช้ตัวเลือกนี้เพื่อให้มันลิงค์ไปเอางานจาก repository ที่เลือกไว้ได้เลย

![](../../.gitbook/assets/image%20%28209%29.png)

### 4️⃣ สร้าง.gitignore

สำหรับเพื่อนๆที่อยากให้มันสร้าง **`.gitignore`** ก็สามารถกดสร้างจากเมนูตรงนี้ได้เลย โดยที่มันก็จะมี default gitignore ให้เราเลือกเหมือน GitHub อยู่ละ

![](../../.gitbook/assets/image%20%28851%29.png)

## 🔥 ลองเล่นกันเรย

### Git Clone

สำหรับตัวอย่างตัวนี้ผมจะเล่นจากตัวพื้นฐานสุดๆก่อนเลยละกัน นั่นก็คือผมจะเลือกใช้วิธีที่ 1️⃣ เราจะได้เห็นทุกอย่างแบบคลีนๆเลยนั่นเอง ดังนั้นผมก็จะ **Clone project** ตัวนี้ลงมาที่เครื่องเลยละกัน โดยการ copy URL ที่เขาเตรียมไว้ให้นั่นเอง

```text
https://sakuljaru@dev.azure.com/sakuljaru/saladpuk/_git/saladpuk
```

ดังนั้นเราก็เปิด **Command prompt** หรือ **Terminal** ไปยังตำแหน่งที่เราจะทำงานด้วย \(ในตัวอย่างของผมคือ d:\temp นะ\) แล้วก็สั่ง **Clone** ลงไปโลด

```bash
git clone https://sakuljaru@dev.azure.com/sakuljaru/saladpuk/_git/saladpuk
```

> **ผลลัพท์**  
> Cloning into 'saladpuk'...  
> warning: You appear to have cloned an empty repository.

สิ่งที่ผมได้คือโฟร์เดอร์ว่างๆออกมาครับ \(คลีนจริงๆ\)

![](../../.gitbook/assets/image%20%28368%29.png)

### Git Pull

จากตรงนี้ผมจะขอลองสร้าง `.gitignore` ของตัวโปรเจค **Visual Studio** หน่อยละกัน โดยการกลับไปที่ตัวเว็บในตัวเลือกข้อ 4️⃣ โดยเลือก **VisualStudio** ลงไป แล้วกดปุ่ม **`Initialize`** นั่นเอง

![](../../.gitbook/assets/image%20%28931%29.png)

หลังจากทำเสร็จหน้าตามันจะเปลี่ยนไปนิดหน่อย เพื่อแสดงรายการไฟล์ที่อยู่ใน Repository ของเรานั่นเอง ซึ่งจากจุดนี้ก็จะคล้ายๆกับ GitHub ละ \(เดี๋ยวรายละเอียดมาอธิบายต่ออีกที\)

![](../../.gitbook/assets/image%20%28567%29.png)

จากนั้นกลับมาที่ **Command prompt** หรือ **Terminal** ที่เราเปิดค้างไว้ เราก็จะเข้าไปใน directory ที่เราพึ่งได้มา \(ในตัวอย่างจะได้ directory ชื่อ **saladpuk** มา\) ดังนั้นเราก็จะเข้าไปทำงานกับ directory นั้นด้วยคำสั่ง change directory หรือ **cd** นั่นเอง

```bash
cd saladpuk
```

> **ผลลัพท์**  
> D:\temp\saladpuk&gt;

หลังจากนั้นเราก็จะลอง Pull project ลงมาใหม่ด้วยคำสั่ง **git pull** นั่นเอง

```bash
git pull
```

ถ้าทำเสร็จเราจะเห็นว่าใน folder ของเรามีไฟล์ใหม่เพิ่มเข้ามา 2 ไฟล์ตามที่ใน repository มีแล้วนั่นเอง

![](../../.gitbook/assets/image%20%28549%29.png)

### Git Add & Commit

คราวนี้เราก็ลองสร้างโปรเจคด้วยภาษาอะไรก็ได้ตามที่แต่ละคนถนัด ซึ่งในตัวอย่างผมจะสร้าง เว็บไซต์ ด้วย ASP.NET Core MVC ผ่าน Command line ครับ

{% hint style="info" %}
**แนะนำให้อ่าน**  
สำหรับใครที่อยากทำตามตัวอย่าง แต่ยังไม่ได้ติดตั้ง .NET Core ก็สามารถไปอ่านวิธีการติดตั้งได้จากลิงค์นี้เบย [https://youtu.be/57b7SbYwgrE?t=151](https://youtu.be/57b7SbYwgrE?t=151)
{% endhint %}

ดังนั้นผมก็จะเปิด Command prompt ตัวเดิมกลับมาแล้วใช้คำสั่งด้านล่างนี้ เพื่อสร้างตัวเว็บไซต์นะครับ

```bash
dotnet new mvc
```

พอมันทำงานเสร็จแล้ว เราก็ลองใช้คำสั่งด้านล่าง เพื่อตรวจสอบว่าตัวเว็บของเรามันสร้างเสร็จสมบูรณ์ไม่มีข้อผิดพลาดหรือเปล่าด้วย

```bash
 dotnet build
```

> **ผลลัพท์**  
> Microsoft \(R\) Build Engine version 16.3.0+0f4c62fea for .NET Core Copyright \(C\) Microsoft Corporation. All rights reserved.
>
> Restore completed in 34.59 ms for D:\temp\saladpuk\saladpuk.csproj. saladpuk -&gt; D:\temp\saladpuk\bin\Debug\netcoreapp3.0\saladpuk.dll saladpuk -&gt; D:\temp\saladpuk\bin\Debug\netcoreapp3.0\saladpuk.Views.dll
>
> **Build succeeded. 0 Warning\(s\) 0 Error\(s\)**
>
> Time Elapsed 00:00:18.61

ถ้าได้ทุกอย่างสมบูรณ์แล้วเราจะพบว่ามันสร้างไฟล์ที่จำเป็นในการสร้างเว็บไซต์ออกมาเต็มไปหมดเบย ตามรูปด้านล่าง

![](../../.gitbook/assets/image%20%28492%29.png)

ถัดไปเราก็จะเอาไฟล์เหล่านี้ไป commit เสียก่อนโดยใช้ 2 คำสั่งนี้

```bash
git add .
git commit -am "Created asp.net core MVC"
```

เรียบร้อยแล้ว ตอนนี้ไฟล์ของเราทั้งหมดก็พร้อมที่จะเอากลับไปที่ repository บน Azure DevOps แล้วนั่นเอง

![](../../.gitbook/assets/image%20%28181%29.png)

### Git Push

ถัดไปเราก็ใช้คำสั่งด้านล่างเพื่อส่งสิ่งที่เราทำขึ้นไปบน Azure DevOps นั่นเอง

```bash
git push
```

ถ้าเรายังไม่เคย Login เอาไว้เลยมันจะขึ้นมาถามให้เรา Login ดังนั้นก็จัดไปด้วย account ที่เราลงทะเบียนไว้ในบนความก่อนหน้านั่นเอง

![](../../.gitbook/assets/image%20%28186%29.png)

หลังจากที่เรา push งานเสร็จแล้วลองเปิดไปดูที่ Azure DevOps เราก็จะเห็นว่าไฟล์ที่เราทำไว้ทั้งหมดขึ้นมาอยู่บน repository เรียบร้อยแล้วนั่นเองขอรับ

![](../../.gitbook/assets/image%20%28884%29.png)

{% hint style="success" %}
**ลองเล่นดู**  
ในตัวอย่างนี้ผมตั้งโปรเจ็คเป็น **Public** เอาไว้ ดังนั้นเพื่อนๆก็สามารถเข้ามาดูตัว repository ผ่านลิงค์ตัวนี้ได้เบยครัช [https://dev.azure.com/sakuljaru/\_git/saladpuk](https://dev.azure.com/sakuljaru/_git/saladpuk)
{% endhint %}

จากตรงนี้ก็แสดงว่าเราสามารถใช้งานตัว Repository ของเราแบบง่ายๆได้ละ ดังนั้นถัดไปเราจะไปดูเรื่องการทำ **Build pipeline** เพื่อให้โปรเจคของเรากลายเป็น Automation ให้ได้มากที่สุดนั่นเอง ดังนั้นกดปุ่ม Next ด้านล่างเพื่ออ่านบทความการทำ **Continuous Integration** กันเบย

