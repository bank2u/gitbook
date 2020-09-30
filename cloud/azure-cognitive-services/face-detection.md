---
description: ใครว่า AI ยากมาลองดูกัน ภาษาอะไรก็เขียนได้ จบภายใน 10 นาที
---

# เขียนแอพ ทายอายุ บอกเพศ ง่ายจิ๊ดเดียว

![](../../.gitbook/assets/image%20%28908%29.png)

ในรอบนี้เราจะมาลองเขียนแอพตามหัวเรื่องกันเลย ซึ่งตัวแอพของเราจะส่งรูปไปให้ AI ประมวลผลว่า `มีกี่คนอยู่ในรูป` `แต่ละคนอายุเท่าไหร่` `ใครเขียนคิ้วทาปากบ้าง` `ใครใส่เครื่องประดับอะไรบ้าง` `อยู่ในอารมณ์ไหน` บลาๆ ซึ่งการที่จะทำแบบนี้ได้ผมจะใช้ AI สำเร็จรูปของ **Microsoft Azure** ที่ชื่อว่า **Cognitive Services** ครับ

{% hint style="success" %}
**แนะนำให้อ่าน**  
บทความนี้เป็นหนึ่งในซีรี่ AI ดังนั้นถ้าเพื่อนสนใจของสนุกๆ เช่น [**Login ด้วยใบหน้า**](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services/faceauth)**,** **ยืนยันตัวตนด้วยเสียง,** [**แปลงภาพเป็นข้อความ**](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services/ocr)**,** [**แยกแยะภาพต่างๆ**](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services/image-classification) และอื่นๆ สามารถดูเนื้อหาทั้งหมดได้จาก Side menu ในหมวดของ **Cognitive Services** ครับ ซึ่งถ้ามีบทความเกี่ยวกับ AI ก็จะมาลงในหมวดนี้เรื่อยๆ แต่ถ้าอยากรู้ว่า AI สำเร็จรูปตัวอื่นๆของ Microsoft Azure มีอะไรน่าเล่นบ้าง ไปอ่านกันได้จากลิงค์นี้เลยครัช [👶 Azure Cognitive Services](https://saladpuk.gitbook.io/learn/cloud/azure-cognitive-services) เชื่อผมเต๊อะ AI ไม่ได้ยากแบบที่คิด
{% endhint %}

## 😗 ทำความเข้าใจกันก่อน

ตัวอย่างนี้ผมจะใช้ภาษา C\# ด้วย .NET Core ดังนั้นใครที่จะทำตามด้วยภาษา C\# ให้ลง `Visual Studio Code` และ `.NET Core SDK` ในลิงค์ด้านล่างด้วยถ้ายังไม่มี **ส่วนคนที่ใช้ภาษาอื่นๆก็สามารถทำตามได้เหมือนกัน** เพราะขั้นตอนทั้งหมดเราเรียกใช้ **REST API** เพียงอย่างเดียวเลยครับ

* [Download Visual Studio Code](https://code.visualstudio.com/)
* [Download .NET Core SDK](https://dotnet.microsoft.com/download)

## 🤔 เริ่มยังไงดี ?

ก่อนที่จะเริ่มเขียนโค้ดผมอยากเคลียให้เข้าใจตรงกันก่อน ว่าในตัวอย่างนี้เราจะต้องทำอะไรกันบ้างตามนี้เลย

1. ขั้นตอนแรกเราต้องสร้าง **Cognitive Services** เสียก่อน เพื่อไปเอา **Key กับ Endpoint** มา เราถึงจะมีสิทธิ์ในการเรียกใช้ AI ต่อนั่นเอง
2. สร้าง C\# โปรเจคขึ้นมา แล้ว setup project ให้พร้อมทำงานกับ REST API และ Json
3. คำความรู้จักกับ **Face Detect API** กันก่อน
4. เขียนโค้ดอัพโหลดรูปขึ้นไปให้ AI ประมวลผล แล้วเอาผลลัพท์กลับมา
5. แปลง Json กลับมาเป็นข้อมูลที่เราเข้าใจได้ แล้วเอามาโชว์ออกที่หน้าจอ

## 🔥 \(1-2\) สร้าง Cognitive Services และ Project C\#

ขั้นตอนที่ 1 กับ 2 เป็นพื้นฐานในการทำ AI ของคอร์สนี้เลย ผมเลยแยกออกไปอีกลิงค์นึง ไม่งั้นทุกบทความผมจะต้องมาคอยนั่ง copy วางตลอดเวลา เลยขอรวมขั้นตอนที่ 1 กับ 2 ไว้ในลิงค์ด้านล่างนี้ครับ

{% page-ref page="create-cognitiveservices.md" %}

## 🔥 \(3\) ทำความรู้จักกับ Face Detect API กันก่อน

ก่อนที่เราจะไปเรียกใช้งานเจ้า Cognitive Services API เราลองมาดูกันก่อนว่า เจ้า API ตัวนี้มันมี interface เป็นยังไงบ้าง ซึ่งผมเอาตัวอย่างคร่าวๆมาให้แล้วตามด้านล่างเลย

{% api-method method="post" host="{endpoint}" path="/face/v1.0/detect" %}
{% api-method-summary %}
Face - Detect
{% endapi-method-summary %}

{% api-method-description %}
ตรวจหาใบหน้าจากรูปได้หลายใบหน้าในภาพเดียว และสามารถแยกแยะแต่ละใบหน้าได้
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="returnFaceAttributes" type="string" required=false %}
จะให้ส่งค่าอะไรกลับบ้าง โดยตัวเลือกที่สามารถเลือกได้คือ age, gender, headPose, smile, facialHair, glasses, emotion, hair, makeup, occlusion, accessories, blur, exposure, noise \(ถ้าเลือกหลายตัวให้ใส่ comma คั่น\) ยิ่งเลือกหลายตัวยิ่งใช้เวลาประมวลผลนานขึ้น
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="ocp-apim-subscription-key" type="string" required=true %}
Key ที่ได้จาก Cognitive Services ที่ทำในขั้นตอนที่ 1
{% endapi-method-parameter %}

{% api-method-parameter name="content-type" type="string" required=true %}
application/json
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="url" type="string" required=true %}
URL ของรูปที่จะส่งขึ้นไป
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
[
    {
        "faceId": "c5c24a82-6845-4031-9d5d-978df9175426",
        "recognitionModel": "recognition_02",
        "faceRectangle": {
            "width": 78,
            "height": 78,
            "left": 394,
            "top": 54
        },
        "faceLandmarks": {
            "pupilLeft": {
                "x": 412.7,
                "y": 78.4
            },
            "pupilRight": {
                "x": 446.8,
                "y": 74.2
            },
            "noseTip": {
                "x": 437.7,
                "y": 92.4
            },
            "mouthLeft": {
                "x": 417.8,
                "y": 114.4
            },
            "mouthRight": {
                "x": 451.3,
                "y": 109.3
            },
            "eyebrowLeftOuter": {
                "x": 397.9,
                "y": 78.5
            },
            "eyebrowLeftInner": {
                "x": 425.4,
                "y": 70.5
            },
            "eyeLeftOuter": {
                "x": 406.7,
                "y": 80.6
            },
            "eyeLeftTop": {
                "x": 412.2,
                "y": 76.2
            },
            "eyeLeftBottom": {
                "x": 413.0,
                "y": 80.1
            },
            "eyeLeftInner": {
                "x": 418.9,
                "y": 78.0
            },
            "eyebrowRightInner": {
                "x": 4.8,
                "y": 69.7
            },
            "eyebrowRightOuter": {
                "x": 5.5,
                "y": 68.5
            },
            "eyeRightInner": {
                "x": 441.5,
                "y": 75.0
            },
            "eyeRightTop": {
                "x": 446.4,
                "y": 71.7
            },
            "eyeRightBottom": {
                "x": 447.0,
                "y": 75.3
            },
            "eyeRightOuter": {
                "x": 451.7,
                "y": 73.4
            },
            "noseRootLeft": {
                "x": 428.0,
                "y": 77.1
            },
            "noseRootRight": {
                "x": 435.8,
                "y": 75.6
            },
            "noseLeftAlarTop": {
                "x": 428.3,
                "y": 89.7
            },
            "noseRightAlarTop": {
                "x": 442.2,
                "y": 87.0
            },
            "noseLeftAlarOutTip": {
                "x": 424.3,
                "y": 96.4
            },
            "noseRightAlarOutTip": {
                "x": 446.6,
                "y": 92.5
            },
            "upperLipTop": {
                "x": 437.6,
                "y": 105.9
            },
            "upperLipBottom": {
                "x": 437.6,
                "y": 108.2
            },
            "underLipTop": {
                "x": 436.8,
                "y": 111.4
            },
            "underLipBottom": {
                "x": 437.3,
                "y": 114.5
            }
        },
        "faceAttributes": {
            "age": 71.0,
            "gender": "male",
            "smile": 0.88,
            "facialHair": {
                "moustache": 0.8,
                "beard": 0.1,
                "sideburns": 0.02
            },
            "glasses": "sunglasses",
            "headPose": {
                "roll": 2.1,
                "yaw": 3,
                "pitch": 1.6
            },
            "emotion": {
                "anger": 0.575,
                "contempt": 0,
                "disgust": 0.006,
                "fear": 0.008,
                "happiness": 0.394,
                "neutral": 0.013,
                "sadness": 0,
                "surprise": 0.004
            },
            "hair": {
                "bald": 0.0,
                "invisible": false,
                "hairColor": [
                    {"color": "brown", "confidence": 1.0},
                    {"color": "blond", "confidence": 0.88},
                    {"color": "black", "confidence": 0.48},
                    {"color": "other", "confidence": 0.11},
                    {"color": "gray", "confidence": 0.07},
                    {"color": "red", "confidence": 0.03}
                ]
            },
            "makeup": {
                "eyeMakeup": true,
                "lipMakeup": false
            },
            "occlusion": {
                "foreheadOccluded": false,
                "eyeOccluded": false,
                "mouthOccluded": false
            },
            "accessories": [
                {"type": "headWear", "confidence": 0.99},
                {"type": "glasses", "confidence": 1.0},
                {"type": "mask"," confidence": 0.87}
            ],
            "blur": {
                "blurLevel": "Medium",
                "value": 0.51
            },
            "exposure": {
                "exposureLevel": "GoodExposure",
                "value": 0.55
            },
            "noise": {
                "noiseLevel": "Low",
                "value": 0.12
            }
        }
    }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% hint style="success" %}
**Cognitive Services API**  
ใครอยากดูรายละเอียดเจ้า Face Detect ตัวเต็มๆก็สามารถเข้าไปดูได้จากลิงค์ด้านล่างนี้เลยนะครับ  
[Microsoft Cognitive Services API: Face Detect](https://southeastasia.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
{% endhint %}

## 🔥 \(4\) เขียนโค้ดอัพโหลดรูปให้ AI

คราวนี้เราก็จะมาเขียนโค้ดในการอัพโหลดรูปไปให้ AI กันบ้างละ ซึ่งเราก็จะใช้รูปด้านล่างนี้ลองอัพโหลดขึ้นไปละกัน

![&#xE1C;&#xE21;&#xE2D;&#xE22;&#xE32;&#xE01;&#xE01;&#xE25;&#xE31;&#xE1A;&#xE1A;&#xE49;&#xE32;&#xE19;](../../.gitbook/assets/image%20%28143%29.png)

ซึ่ง URL ของรูปนี้คือตามลิงค์นี้เลยครัช

```text
https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-Lm0_idNbY6k1lwp6hm4%2F-LoCfku1h93nvIQvqkaR%2F-LoCglDFUW0smoSUxSMs%2Fimage.png?alt=media&token=cd35a07f-4f22-4a00-b8ac-0274c4fc7791
```

คราวนี้กลับมาที่ส่วนของโค้ดของเราต่อ เราก็จะเขียนโค้ดให้มันไปเรียกใช้ API ตัวด้านบนกัน ซึ่งเราก็จะได้โค้ดออกมาประมาณนี้

{% code title="Program.cs" %}
```csharp
var returnFaceAttributes = "returnFaceAttributes=age,gender,glasses,emotion,makeup,accessories";
var faceDetectRequest = CreateRestRequest($"face/v1.0/detect?returnFaceId=true&{returnFaceAttributes}", new
{
    url = "https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-Lm0_idNbY6k1lwp6hm4%2F-LoCfku1h93nvIQvqkaR%2F-LoCglDFUW0smoSUxSMs%2Fimage.png?alt=media&token=cd35a07f-4f22-4a00-b8ac-0274c4fc7791"
});
var faceDetectResult = client.Execute(faceDetectRequest, Method.POST);
if (faceDetectResult.StatusCode == HttpStatusCode.OK)
{
    var faces = JArray.Parse(faceDetectResult.Content);
    foreach (var item in faces)
    {
        Console.WriteLine(item);
    }
}
else
{
    Console.WriteLine(faceDetectResult.Content);
}
```
{% endcode %}

**อธิบายโค้ด**

1. ในบรรทัด 1-6 ทำการเรียก API ไป โดยระบุว่าเราจะขอข้อมูลพวก `age` `gender` `glasses` `emotion` `accessories` ทั้งหลายมาด้วยในบรรทัดที่ 1
2. เมื่อได้ผลลัพท์กลับมา ก็ทำการตรวจสอบว่าที่ส่งไปมีอะไรผิดหรือเปล่า ในบรรทัดที่ 7
3. ถ้าไม่มีอะไรผิดพลาดเราก็จะเอาผลลัพท์ที่ได้มาอ่าน แล้วแสดงผลออกหน้าจอ ในบรรทัดที่ 9-13

จากทั้งหมดที่ทำมา เราก็จะทำการลอง Run กันนะครับ โดยกด `CTRL + F5` หรือจะใช้คำสั่ง `dotnet run` จากใน Command prompt หรือ Terminal ก็ได้ครับ ซึ่งก็จะได้ผลลัพท์ออกมาแบบนี้

```text
{
  "faceId": "c54ae586-b2b0-460a-99bf-c42565908df0",
  "faceRectangle": {
    "top": 97,
    "left": 122,
    "width": 165,
    "height": 165
  },
  "faceAttributes": {
    "gender": "male",
    "age": 49.0,
    "glasses": "NoGlasses",
    "emotion": {
      "anger": 0.0,
      "contempt": 0.002,
      "disgust": 0.0,
      "fear": 0.0,
      "happiness": 0.003,
      "neutral": 0.986,
      "sadness": 0.01,
      "surprise": 0.0
    },
    "makeup": {
      "eyeMakeup": false,
      "lipMakeup": false
    },
    "accessories": []
  }
}
```

ซึ่งผลลัพท์เขาบอกว่าในรูปเป็น `ผู้ชาย` `อายุ 49 ปี` `ไม่ได้ใส่แว่น` `อารมณ์ปรกติ` `ไม่ได้แต่งหน้า` `ไม่ได้สวมใส่เครื่องประดับ` เย่เหมือนจะตรงซะเกือบหมด แค่อายุยังคลาดเคลื่อนอยู่หน่อยจาก 70 เหลือ 49 ฮ่าๆ ไม่เป็นไรครับคนเอเชียหน้าเด็กก็งี้แหละ

## 🤔 ยาวจังของโค้ดทั้งหมดหน่อย

เพื่อไม่ให้เนื้อหายาวเพราะโค้ดจนเกินไป ดังนั้นผมขอ zip ตัวโปรเจคนี้แล้วไปดาวโหลดกันเอามาลองเล่นดูละกันนะ แต่ต้องไปเปลี่ยน `Key` กันเอาเองนะครัช

{% file src="../../.gitbook/assets/saladpuk-demo.zip" caption="Source Code: Face Detection" %}

## 🎯 บทสรุป

ในการทำงานกับ AI จริงๆไม่ใช่เรื่องยากเลยหัวใจหลักของมันจริงๆก็คือการเรียกใช้ REST API ให้ถูกตัวเท่านั้น ดังนั้นไม่ว่าเราจะเขียนภาษาไหนก็ตาม เราก็สามารถเรียกใช้ Face API เพื่อทำของประมาณนี้ได้เลย

{% hint style="success" %}
**Cognitive Services API**  
หากใครสนใจอยากดู API ทั้งหมดที่ Microsoft เตรียมไว้ให้เราเรียกใช้ AI สำเร็จรูป ก็สามารถเข้าไปดูได้จากลิงค์ด้านล่างนี้เลยครับ

* [Microsoft Cognitive Services API](https://southeastasia.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
{% endhint %}

{% hint style="success" %}
**Cognitive Services Library**  
สำหรับคนที่ต้องการเขียนทำงานกับ Cognitive Services จริงๆไม่ต้องไปนั่งเขียนเชื่อม API ทีละตัวก็ได้นะ เพราะทาง Microsoft นั้นได้มี Library ให้เราสามารถเรียกใช้ได้เลยครับ เช่นในฝั่ง .NET ก็จะมีตัวนี้ด้านล่างนี้ที่สามารถติดตั้งแล้วใช้งานได้เลย  
**dotnet add package Microsoft.Azure.CognitiveServices.Vision.Face --version 2.5.0-preview.1**
{% endhint %}

