# เข้าใจ Blob storage ให้มากขึ้น

## 🤔 ของต่างๆใน Blob storage มันมีอะไรบ้างนะ ?

ในตัว Azure Storage นั้นได้มีการเรียกของต่างๆที่เก็บไว้ใน Blob storage ตามรูปด้านล่างนี้

![](../../../.gitbook/assets/image%20%28477%29.png)

ซึ่งในตัวอย่างที่แล้วผมได้อัพโหลดไฟล์ลงใน Blob storage ไป เราก็จะได้ของออกมาเป็นภาพนี้

![](../../../.gitbook/assets/image%20%28133%29.png)

> **อธิบายรูป**  
> 1.ผมได้สร้าง **Storage account** ขึ้นมา 1 ตัวชื่อ saladpukstorage สีเขียวบนสุด  
> 2.ผมทำการสร้าง **Container** หรือโฟเดอร์ขึ้นมา 1 ตัวชื่อ saladpuk-image  
> 3.ผมอัพโหลดรูปลงใน Container saladpuk-image ชื่อไฟล์ saladpakLogo-03.png

ดังนั้นเมื่อเราเอารูปของที่ผมทำไปเทียบกับในความหมายของ Blob storage ก็จะได้ออกมาว่า

> Account = saladpukstorage  
> Container = saladpuk-image  
> Blob = saladparkLogo-03.png

## 🤔 URL ของ Storage เราจะเป็นแบบไหน ?

ตัวไฟล์ทุกตัวจะมี url แยกของใครของมันเลย ซึ่งจะมี base url เดียวกันตามชื่อของ account ครับ เช่นของผมตั้งชื่อว่า **saladpukstorage** ผมก็จะมี base url เป็น

`http://saladpukstorage.blob.core.windows.net`

ถัดมาไฟล์รูป `saladparkLogo-03.png` ที่อัพโหลดไปมันอยู่ใน container ชื่อ `saladpuk-image` มันก็จะมี url เป็น

`https://saladpukstorage.blob.core.windows.net/saladpuk-image/saladpakLogo-03.png`

## 🤔 มีข้อจำกัดอะไรไหม ?

### Storage Account

ใน Azure เราจะสร้าง storage account กี่ตัวก็ได้ตามสบาย ดังนั้นอุ่นใจได้

### Container

สร้างไม่มีข้อจำกัดใดๆเลย ภายใต้ storage account จะมี container เป็นแสนๆตัวก็ทำไปเต๊อะ

### Blob

ของที่อยู่ใน container จริงๆแล้วมันจะโดนแยกออกมาเป็น 3 ชนิด ตามนี้ครับ

* **Block blobs** เก็บข้อมูลไฟล์ทั่วไป เช่น text, รูป, เสียง บลาๆ ซึ่งสามารถเก็บได้สูงสุดไม่เกิน **4.7 TB ต่อไฟล์**
* **Append blobs** เหมือน block blobs แต่จะสามารถเขียนไฟล์ต่อเนื่องได้ เช่นเอาไว้เก็บ log ว่ามีใครมาทำอะไรกับเซิฟเวอร์บ้าง
* **Page blobs** เก็บไฟล์ที่เป็น virtual hard drive \(VHD\) จากพวก VM ซึ่งสามารถเก็บได้สูงสุดไม่เกิน **8 TB**

