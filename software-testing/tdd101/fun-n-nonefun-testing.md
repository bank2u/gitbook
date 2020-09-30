# 7.Functional & None-Functional testing

💬 เวลาเขียนเทสมันจะมีของที่เราทดหลัก 2 อย่างคือ **Functional testing** และ **None-Functional testing** ด้วย ในรอบนี้เราจะมาดูกันว่า None-functional testing มันประกอบไปด้วยอะไรบ้าง

{% hint style="info" %}
**None-Functional testing**  
คืองานที่เราต้องเทสแต่บางทีลูกค้าไม่ได้ขอมาตรงๆ เช่น Scalable, Security, Privacy, Readability
{% endhint %}

{% hint style="info" %}
**Functional testing**  
การเทสงานความถูกต้องของโค้ดที่เราเขียนว่ามันทำงานได้จริงๆหรือเปล่า เช่น Unit testing, Integration testing, N2N บลาๆเยอะม๊วก
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=QAESRjmHG-U&list=PLUjAn8nwWniiL3ToFK8PfmAo8U6IoGAkg&index=9" caption="" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 การเขียน Unit test

คือการทดสอบว่าโค้ดที่เราเขียนขึ้นมาสามารถทำงานได้ถูกต้องหรือเปล่า โดยจะเป็นการทดสอบในระดับที่เล็กที่สุดของโค้ดนั่นคือในระดับ method นั่นเอง ส่วนในการเขียน testing แบบอื่นๆจะเริ่มมองในมุมที่กว่าขึ้นเรื่อยๆละเช่นในระดับ module, feature, system บลาๆ

