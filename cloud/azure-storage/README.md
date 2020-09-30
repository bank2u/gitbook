---
description: เก็บไฟล์ 1GB  = 0.61 บาท รับโหลดหลัก TB ชิวๆ ทำ BigData สบายๆ
---

# 👶 Azure Storage

ในคอร์สนี้เราจะลงลึงในตัว **Azure Storage** กัน ซึ่งเจ้าตัวนี้มันมีหน้าที่เอาไว้เก็บข้อมูลทุกๆอย่าง และข้อดีที่ติดตัวมันมากตั้งแต่ต้นเลยคือ สามารถรับโหลดหนักๆได้ชิวๆ มี security ที่หนานแน่น รองรับการเก็บแบบ Structural และ Nonstructural อีกด้วย บลาๆ \(คือถ้าไล่หมดนี่ผมคงต้องไปขอค่าโฆษณาจาก Microsoft แล้วล่ะ ฮ่าๆ\) ดังนั้นใครที่กำลังมองหาที่เก็บไฟล์แบบเมพๆอยู่ล่ะก็ไม่ควรพลาด Azure Storage

## 🤔 แล้วมันเก็บอะไรได้บ้าง ?

หลักๆตัว Azure Storage เขาแบ่งการเก็บข้อมูลออกเป็น 4 กลุ่มตามนี้

### 1.Blob storage <a id="blob-storage"></a>

ที่เก็บไฟล์ทั่วไป เช่น ข้อความ, รูป, เสียง, วีดีโอ, เอกสาร บลาๆ หรือเราเรียกของพวกนี้อีกอย่างว่า **Unstructured data** นั่นเอง ซึ่งเราสามารถกำหนดสิทธิ์ในการเข้าถึงได้ หรือจะให้มันทำการเข้ารหัสไฟล์ให้เรา **Encrypted data** เพื่อป้องกันไม่ให้คนอื่นมาอ่านก็ยังได้ อีกทั้งเรายังสามารถเลือกเปิดแชร์ให้ใครเข้ามาโหลดไฟล์ก็ได้อีกด้วย

### 2.Azure Files <a id="azure-files"></a>

กลุ่มนี้จะเป็นการเก็บ Network drive หรือพูดง่ายๆคือเก็บไดรฟ์ไว้บนคลาว์ได้เลย แล้วเครื่องไหนที่เชื่อมเข้ามาก็สามารถแชร์ไดรฟ์กันได้ด้วย ให้เห็นภาพง่ายๆคือพวก Dropbox, Google Drive, OneDrive นั่นเอง หรือจะเอาไว้เก็บ capture OS image ก็ยังได้เลยนะ

### 3.Queue storage <a id="queue-storage"></a>

เป็นที่เก็บ **message** ที่มีการทำงานเป็น **คิว** ซึ่งพฤติกรรมเจ้าตัวนี้มันจะเก็บข้อมูลแบบมีลำดับใครมาก่อนมาหลังนั่นเอง ส่วนการใช้งานจะออกไปในเชิงการส่งข้อมูลจากที่หนึ่งไปยังอีกที่หนึ่ง แล้วเราต้องการการันตีว่าข้อข้อมูลพวกนั้นถูกส่งไปแน่ๆ เพราะคิวมันจะมีตัวช่วยในการตรวจสอบงานให้เราด้วย

### 4.Table storage <a id="table-storage"></a>

เป็นที่เก็บข้อมูลที่อยู่ในรูปแบบของตาราง และสามารถเก็บข้อมูลที่เป็น NoSQL ได้ จินตนาการง่ายๆว่ามันคือฐานข้อมูลประเภทนึงที่ยืดหยุ่นมากๆก็ได้

## 🤔 มันต่างกับที่ฝากไฟล์ทั่วไปยังไง ?

หลายเรื่องเลยจะไม่รู้จะพูดเรื่องไหนก่อนดี เลยขอเอาเรื่องที่ทุกคนเห็นภาพได้ง่ายๆละกัน

### ราคา

ถูกม๊วกกกกกก คือถ้าเลือกเก็บไฟล์เฉยๆเลยนะ เขาคิด **GB ละ $0.02 USD ราว 0.61บาท/เดือน อ่ะ!!** คือแค่เห็นราคานี้ผมจบเลย ถูกกว่าไปซื้อ Harddisk มาใช้เองซะอีก

![&#xE2D;&#xE38;&#xE15;&#xE4A;&#xE30;! 1GB &#xE04;&#xE34;&#xE14;&#xE44;&#xE21;&#xE48;&#xE16;&#xE36;&#xE07;&#xE1A;&#xE32;&#xE17;](../../.gitbook/assets/image%20%28821%29.png)

### ไฟล์ไม่หาย

ถ้าเราเก็บไฟล์บนคลาว์เนี่ยบอกเลยว่าอยู่คงทนถาวร ไม่ใช่เหมือนที่ฝากไฟล์ที่วันดีคืนไฟล์หายซะงั้น เพราะเขามีระบบ backup ไฟล์ให้เราไปเก็บไว้ 3 ที่เลย ถ้าไฟล์ที่นึงหายมันก็จะใช้ตัว backup เอากลับมาให้ทันที แถมตัว Azure Storage ยังสามารถให้เราเลือก Backup ไปหลายๆที่ทั่วโลกได้ด้วย!! คิดภาพง่ายๆว่าเก็บที่เดียวกลัวไม่ชัวร์เขาเลยมีระบบ backup ไปทั่วโลกให้เลย ส่วนใครจะเห็นได้บ้างอันนี้ขึ้นกับเรากำหนดครับ \(default คือเราคนเดียวเท่านั้น\)

![&#xE40;&#xE25;&#xE37;&#xE2D;&#xE01;&#xE40;&#xE25;&#xE22;&#xE2D;&#xE22;&#xE32;&#xE01;&#xE01;&#xE23;&#xE30;&#xE08;&#xE32;&#xE22;&#xE40;&#xE01;&#xE47;&#xE1A;&#xE02;&#xE49;&#xE2D;&#xE21;&#xE39;&#xE25;&#xE44;&#xE27;&#xE49;&#xE17;&#xE35;&#xE48;&#xE44;&#xE2B;&#xE19;&#xE1A;&#xE49;&#xE32;&#xE07;](../../.gitbook/assets/image%20%28629%29.png)

### ความปลอดภัย

ที่เก็บข้อมูลอันนี้สุดยอดมาก เราสามารถไปตั้งให้เขาเข้ารหัสไฟล์ให้เราได้ด้วย!! เผื่อเราเผลอเปิดทิ้งไว้ไรงี้ และระดับความปลอดภัยนี่เป็นมาตรฐานระดับโลก ยังไม่เคยได้ยินข่าวว่าถูกเจาะได้เลยนะ \(มีแต่บริษัทตั้งค่าไม่ดีแล้วรั่วหลุดมาเอง อุ๊ปปปปปส์\)

### เข้าถึงได้หลากหลาย

เพียงแค่มี internet เราก็สามารถเข้าใช้งาน Azure Storage ได้เลย เพราะมันทำงานผ่าน REST API นั่นเอง อีกทั้งภาษาหลักๆบนโลกนี้เขาก็มี Library รองรับให้เราไปเขียนโปรแกรมเพื่อจัดการไฟล์ได้เลย

ลิงค์สำหรับ Library แต่ละภาษาและรวมถือ REST API ด้วย

* [Azure Storage REST API](https://docs.microsoft.com/rest/api/storageservices/)
* [Azure Storage client library for .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
* [Azure Storage client library for Java/Android](https://docs.microsoft.com/java/api/overview/azure/storage)
* [Azure Storage client library for Node.js](https://docs.microsoft.com/javascript/api/azure-storage)
* [Azure Storage client library for Python](https://github.com/Azure/azure-storage-python)
* [Azure Storage client library for PHP](https://github.com/Azure/azure-storage-php)
* [Azure Storage client library for Ruby](https://github.com/Azure/azure-storage-ruby)
* [Azure Storage client library for C++](https://github.com/Azure/azure-storage-cpp)

นอกจากจะเขียนโปรแกรมทำงานตรงๆแล้ว เขายังมี Tools ต่างๆให้เราทำงานร่วมได้เลย เช่นพวก command line หรือ โปรแกรมจัดการ

* [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)
* [Azure PowerShell Cmdlets for Storage](https://docs.microsoft.com/powershell/module/az.storage)
* [Azure CLI Cmdlets for Storage](https://docs.microsoft.com/cli/azure/storage)
* [AzCopy Command-Line Utility](https://aka.ms/downloadazcopy)
* [Azure Storage Client Tools](https://docs.microsoft.com/en-us/azure/storage/storage-explorers)
* [Azure Developer Tools](https://azure.microsoft.com/tools/)

## 🤔 เท่านี้เองเหรอ ?

คือนี่กะจะให้ผมเขียนหนังสือเลยเหรอ ? จริงๆที่ว่ามานี่เป็นแค่ส่วนนึงของเขาเท่านั้นที่สามารถทำได้ และแต่ละตัวที่เกริ่นมาก็มีรายละเอียดเยอะม๊วกๆๆๆๆ เพื่อตอบโจทย์ทุกสถานะการณ์ของการเก็บไฟล์เลย ดังนั้นเตรียมอ่านต่อในรายละเอียดแต่ละเรื่องเอาละกัน

ยกตัวอย่างนิดนึงที่ไม่ได้พูดถึงก็ได้ เราสามารถไปตั้งให้เวลามีคนอัพโหลดรูปเข้ามาแล้วเขาจะเอารูปไปสร้างเป็น thumbnail ให้เหมาะกับหน้าจอหลายๆ size ก็มี หรือ อัพโหลดวีดีโอแล้วทำการแปลงฟอร์แมตให้อ่านได้หลากหลายก็มี แถมยังมี .... พอเต๊าะ

## 🧭 เนื้อหาของคอร์สนี้ทั้งหมด

### Blob Storage

{% page-ref page="blobs/create.md" %}

{% page-ref page="blobs/detail.md" %}

{% page-ref page="blobs/blob-code-01.md" %}

{% page-ref page="blobs/staticweb.md" %}

{% hint style="warning" %}
เนื้อหาของคอร์สนี้จะค่อยๆเอามาเติมเรื่อยๆ คอยติดตามได้จากหน้านี้ หรือไม่ก็หน้าอัพเดทข่าวสารที่อยู่ตรง side menu ครับ
{% endhint %}

