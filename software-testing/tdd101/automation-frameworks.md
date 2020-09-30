# 9.Automation Frameworks in .NET

💬 ถ้าถามว่าตัวที่ใช้เขียน automation testing ในฝั่ง .NET นั้นมีแค่ xUnit หรือเปล่า? คำตอบไม่ครับมีหลายตัวเลย ดังนั้นในรอบนี้ผมจะแนะนำ framework ที่ใช้ในการเขียน automation ที่นิยมๆของ .NET กันนะครับ

{% embed url="https://www.youtube.com/watch?v=mLPSpGP8GwU&list=PLUjAn8nwWniiL3ToFK8PfmAo8U6IoGAkg&index=11" caption="" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 Framework ที่นิยมใช้ในฝั่ง .NET มี 3 ตัวเน่อ

1. [MSTest](https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-with-mstest)
2. [NUnit](https://nunit.org/)
3. [xUnit](https://xunit.net/)

### 👨‍🚀 รูปแบบในการเขียน test \(AAA\)

1. **Arrange** อธิบายว่าสิ่งที่เราจะเทสคืออะไร
2. **Act** เขียนโค้ดให้มันไปเรียกส่วนที่เราจะทดสอบ
3. **Assert** ตรวจสอบผลลัพท์ที่ได้ว่ามันถูกต้องตามที่เราอยากให้มันเป็นหรือเปล่า

### 👨‍🚀 Code coverage

เป็นตัวชี้ว่าโค้ดที่เราเขียนนั้นมี test cases เข้าไปตรวจสอบทั้งหมดกี่ % แล้ว

{% hint style="warning" %}
การที่โค้ดของเราทุกส่วนมี test cases เข้าไปตรวจทั้งหมด 100% แล้วไม่ได้หมายความว่ามันจะไม่มี bug นะขอรับ
{% endhint %}

