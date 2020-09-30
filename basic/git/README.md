# 👶 Git พื้นฐาน

## 😢 ปัญหา

เวลาที่เราเขียนโค้ดเรามักจะเจอปัญหาว่า จะแชร์งานเพื่อทำงานร่วมกับเพื่อนๆยังไง จะเอา **Thumbdrive** ไปจิ้มเพื่อแชร์ให้กันก็กลัวติดไวรัส หรือจะตั้ง **NAT** ขึ้นมาก็ดูจะไม่สะดวก สุดท้ายก็เลยเอางานขึ้น **OneDrive, Google Drive, Dropbox** ก็พอจะใช้งานได้ระดับนึง แต่มันก็ยังไม่สะดวกอยู่ดี

แล้วปรกติบริษัทต่างๆที่เขาทำงานร่วมกับกับคนเป็นร้อยๆคน มีไฟล์เป็นหมื่นๆไฟล์ เขาทำงานร่วมกันได้ยังไงกันนะ ?

## 😄 วิธีแก้ปัญหา

เข้าใช้โปรแกรมพวก **Version control** เข้ามาช่วยในการจัดการแชร์ไฟล์เพื่อทำงานร่วมกับเป็นทีมยังไงล่ะ ดังนั้นในคอร์สนี้เราจะมาพูดถึงตัวที่ทั่วโลกนิยมใช้กันนั่นก็คือ **Git** นั่นเองขอรับบบ ดังนั้นไปดูกันเลยว่ามันทำอะไรได้บ้าง และมันจะช่วยลดงานของเหล่า developer อย่างเราๆได้ยังไงกันบ้าง

## 🤔 Git คือไย & ลงไรบ้าง ?

{% embed url="https://www.youtube.com/watch?v=g-qzBp3MOQQ&list=PLUjAn8nwWniizz7zcVDpuvgFtKFR2wzeb&index=1" %}

### โปรแกรมที่ต้องติดตั้ง

* [Git SCM](https://git-scm.com/)
* [Git for Windows](https://gitforwindows.org/)
* [Tortoise Git](https://tortoisegit.org/) \(สำหรับคนที่ต้องการ GUI\)

## 🤔 เริ่มใช้งาน Clone + Commit + Push

{% embed url="https://www.youtube.com/watch?v=nmZyEOlP38U&list=PLUjAn8nwWniizz7zcVDpuvgFtKFR2wzeb&index=2" %}

### เว็บไซต์

* [GitHub](https://github.com/)

### คำสั่งที่จำเป็นต้องรู้

**Clone** - เป็นการดึง source code ตัวล่าสุดจากเซิฟเวอร์มาไว้ในเครื่องของเรา เพื่อที่เราจะได้สามารถทำงานต่อกับเพื่อนๆในทีมได้

```text
git clone <GIT_REPOSITORY_URL>
```

**Add** - เป็นการนำไฟล์ที่ถูกแก้ไขเข้าไปยัง git state เพื่อเป็นการระบุว่าไฟล์ไหนบ้างที่เราสนใจ เพื่อเตรียมไปใช้ในการ commit งานขั้นต่อไป ซึ่งในวีดีโอจะเป็นขั้นตอนที่เราจะ commit แล้วเลือกไฟล์ที่เราสนใจ เพราะใน Git GUI มันทำ 2 คำสั่งเลยตอนที่ commit

```bash
// เลือกทุกไฟล์
git add .

// หรือเลือกทีละไฟล์
git add <FILE_NAME>
```

**Commit** - เป็นการสั่งบันทึกสถานะ source code ที่อยู่ใน state ของเครื่องเราว่าเราพอใจในจุดนี้แล้ว ทำให้เราสามารถย้อนกลับมาที่จุดที่เรา commit นี้ในภายหลังได้

```text
git commit -am "YOUR_COMMIT_MESSAGE"
```

**Push** - เป็นการนำ source code ที่อยู่ในเครื่องเรา ส่งกลับไปยังเซิฟเวอร์ เพื่อทำการอัพเดทเซิฟเวอร์ว่ามี source code ใหม่มานะ \(ถ้า commit ตรงกันอยู่แล้วมันจะไม่เกิดไรขึ้น\)

```text
git push
```

## 🤔 ทำงานร่วมกันคนอื่นยังไง ?

{% embed url="https://www.youtube.com/watch?v=T7yM2HYBeHs&list=PLUjAn8nwWniizz7zcVDpuvgFtKFR2wzeb&index=3" %}

### คำสั่งที่จำเป็นต้องรู้

**Pull** - เป็นการดึงข้อมูลจากเซิฟเวอร์เข้ามาอัพเดทที่เครื่องของเรา เพื่อให้เราได้ source code ที่เป็นตัวล่าสุดจากฝั่งเซิฟเวอร์

```text
git pull
```

## 🤔 แก้ไฟล์เดียวกันแล้วพังทำไงดี ?

{% embed url="https://www.youtube.com/watch?v=78moLog1QcE&list=PLUjAn8nwWniizz7zcVDpuvgFtKFR2wzeb&index=4" %}

## 🤔 อยากย้อนสถานะของงานทำไงดี ?

{% embed url="https://www.youtube.com/watch?v=KzCqrWYu3JY&list=PLUjAn8nwWniizz7zcVDpuvgFtKFR2wzeb&index=5" %}

## 🤔 มีอย่างอื่นอีกไหม ?

เพียบเลยในการทำงานกับ Git เช่นการทำ **Branching** เพื่อแยกงานออกไปทำไม่ให้ไปชนกับใคร การทำ **Cherry-pick** เพื่อเลือกเอาเฉพาะส่วนที่เราสนใจออกมา การทำ **Sub module, Merging, Re-base** โอ้ยขี้เกียจไล่ เดี๋ยวถ้าว่างจะกลับมาพูดในเรื่องนี้อีกทีละกัน แต่ง่ายๆคือเพื่อนๆสามารถเข้าไปเรียนรู้ได้ด้วยตัวเองจากคอร์สอันนี้เลย ฟรีแถมคุณภาพดีเยี่ยมด้วย กด้ได้เลยจากลิงค์ด้านล่างนี้

{% embed url="https://learngitbranching.js.org" %}

{% hint style="success" %}
**แนะนำให้อ่าน**  
ส่วนสำหรับเพื่อนๆที่สนใจอยากจะใช้งาน Git Repository แบบอื่นๆนอกจาก GitHub ก็สามารถไปลองศึกษาต่อได้จากลิงค์ด้านล่างตัวนี้ ซึ่งเป็นส่วนหนึ่งของคอร์ส DevOps ครัช  
[Azure DevOps - เล่นกับ Repository](https://saladpuk.gitbook.io/learn/cloud/azure-devops/repository)
{% endhint %}

