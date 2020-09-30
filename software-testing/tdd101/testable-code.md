# 5.โค้ดที่ทดสอบได้

💬 คราวนี้เราจะลองเอา test cases ที่เราเขียนไว้มาแปลงเป็น**โค้ดที่ทดสอบได้**กันดูนะครับ

{% embed url="https://www.youtube.com/watch?v=6OQKq\_xO-LA&list=PLUjAn8nwWniiL3ToFK8PfmAo8U6IoGAkg&index=5" caption="" %}

{% embed url="https://www.youtube.com/watch?v=Rc77D2TQqF8&list=PLUjAn8nwWniiL3ToFK8PfmAo8U6IoGAkg&index=6" caption="" %}

{% embed url="https://www.youtube.com/watch?v=Fi5mzcuGmVI&list=PLUjAn8nwWniiL3ToFK8PfmAo8U6IoGAkg&index=7" caption="" %}

{% hint style="info" %}
**System under test \(SUT\)**  
คือโค้ดที่ถูกเขียน test cases เอาไว้ทดสอบแล้ว เพื่อให้ developer มีความมั่นใจในงานที่ตัวเองเขียนว่านั้นถูกเทสแล้ว และมันสามารถเพิ่ม test cases สำหรับทดสอบ หรือ refactor มันได้ในภายหลังอีกด้วย
{% endhint %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 หลักการเขียน TDD

1. นำ test cases แปลงเป้นโค้ด แล้ว run ให้มัน fail เสียก่อน
2. แก้ไขโค้ดของเราจากที่มัน fail ให้มันผ่าน โดยออกแรงให้น้อยที่สุด หรือไม่จำเป็นต้องเขียนโค้ดให้ดีมากก็ได้
3. หลังจากที่ผ่านแล้ว เราก็จะแก้โค้ดทำมันเป็นโค้ดที่ดีขึ้นกว่าเดิม โดยที่หลังแก้แล้วมันจะต้อง run แล้วผ่านเหมือนเดิม

