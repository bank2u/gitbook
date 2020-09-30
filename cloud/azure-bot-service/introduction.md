---
description: มาดูเบื้องหลังการทำงานของ Chat Bot กับ LUIS กันดีกว่า
---

# Bot เข้าใจเราได้ยังไงกันนะ

หลังจากที่เราได้เลองเขียน **Chat Bot** แบบง่ายๆไปแล้ว หลายๆคนก็น่าจะมีคำถามว่า **บอทมันรู้ได้ไงว่าต้องตอบอะไรเรา?** หรือ **อยากให้มันฉลาดขึ้นต้องทำยังไง?** ดังนั้นในบทนี้เราจะมาดูเบื้องหลังการทำงานของตัว Bot ของเรากันนะครับ

{% hint style="info" %}
**Azure Bot Service**  
สำหรับใครที่ยังไม่ได้ดูวิธีการสร้าง Chat Bot ผมแนะนำให้ไปดูบทความด้านล่างนี้ก่อนนะครับ สอนการสร้าง Bot Service ตั้งแต่เริ่มต้นเลย [**บทความสร้าง Bot Service จิ้มลงมาเบย**](https://saladpuk.gitbook.io/learn/cloud/azure-bot-service)
{% endhint %}

## 🤔 เบื้องหลัง Bot มันทำงานยังไง ?

การที่จะรู้ว่าตัว Bot มันทำงานจริงๆยังไงได้ เราก็ต้องไปไล่ดู Source code ของเรากัน เพื่อจะได้เข้าใจมันได้อย่างถ่องแท้ก่อน ดังนั้นเปิดตัวโปรเจคขึ้นมาได้เลยครับ

สำหรับคนที่ข้ามมาอ่านบทนี้แล้วอยากรู้เรื่องสามารถดาวโหลด source code ด้านล่างได้เลยนะ

{% file src="../../.gitbook/assets/saladpuk-demo-src.zip" caption="Download: Chat Bot source code" %}

{% hint style="info" %}
**วิธีเปิดโปรเจค**  
ในตัวอย่างนี้ผมจะใช้ Visual Studio Code เปิดโปรเจคนะครับ หากใครยังไม่มีให้ไปดาวโหลดได้จากลิงค์ด้านล่างนี้ และมันต้องใช้ .NET Core SDK ด้วย ดังนั้นติดตั้งให้เรียบร้อยเลยครับ

* [Visual Studio Code](https://code.visualstudio.com/)
* [.NET Core SDK](https://dotnet.microsoft.com/download)
{% endhint %}

หลังจากที่เปิดตัวโปรเจคมาแล้วให้เปิดไฟล์ **`FlightBookingRecognizer.cs`** ขึ้นมาเลยครับ ซึ่งเจ้าตัวไฟล์นี้แหละคือจุดที่ Bot จะทำความเข้าใจว่าผู้ใช้ต้องการจะสื่ออะไร แล้วจะตอบโต้กลับยังไง

![](../../.gitbook/assets/image%20%28485%29.png)

ซึ่งเจ้าโค้ดตัวนั้นมันเขียนไว้ตามด้านล่างนี้

{% code title="FlightBookingRecognizer.cs" %}
```csharp
public class FlightBookingRecognizer : IRecognizer
{
    private readonly LuisRecognizer _recognizer;

    public FlightBookingRecognizer(IConfiguration configuration)
    {
        var luisIsConfigured = !string.IsNullOrEmpty(configuration["LuisAppId"]) 
                && !string.IsNullOrEmpty(configuration["LuisAPIKey"]) 
                && !string.IsNullOrEmpty(configuration["LuisAPIHostName"]);
        if (luisIsConfigured)
        {
            var luisApplication = new LuisApplication(
                configuration["LuisAppId"],
                configuration["LuisAPIKey"],
                "https://" + configuration["LuisAPIHostName"]);
            _recognizer = new LuisRecognizer(luisApplication);
        }
    }

    // Returns true if luis is configured in the appsettings.json and initialized.
    public virtual bool IsConfigured => _recognizer != null;

    public virtual async Task<RecognizerResult> RecognizeAsync(ITurnContext turnContext, CancellationToken cancellationToken)
        => await _recognizer.RecognizeAsync(turnContext, cancellationToken);

    public virtual async Task<T> RecognizeAsync<T>(ITurnContext turnContext, CancellationToken cancellationToken)
        where T : IRecognizerConvert, new()
        => await _recognizer.RecognizeAsync<T>(turnContext, cancellationToken);
}
```
{% endcode %}

โดยสรุปหลักการทำงานคือ เมื่อผู้ใช้ทำการป้อนข้อมูลเข้ามาแล้วตัว Bot จะทำการวิเคราะห์ความหมายว่าผู้ใช้จะสื่อว่าอะไร โดยใช้ตัววิเคราะห์ที่ชื่อว่า **LUIS** นั่นเอง \(จำได้ไหมตอนสร้างมันจะให้เราสร้าง LUIS ด้วยไง\) โดยผลลัพท์ที่ LUIS วิเคราะห์เสร็จจะส่งกลับมาจาก method **RecognizeAsync** นั่นเอง

{% hint style="info" %}
**LUIS - Language Understanding**  
คือ **AI** ที่คอยวิเคราะห์ภาษาว่า input ที่รับเข้ามานั้นเขาต้องการจะสื่อว่าอะไร เช่น **`แพงจัง`** อาจจะหมายความว่า**`ขอลดราคา`**อะไรแบบนี้ ซึ่งอีกซักหน่อยเราจะได้ไปลองเล่นเจ้า LUIS กัน ติดตามอ่านได้จาก side menu หรือถ้าขี้เกียจรอก็กระโดดไปอ่าน document หลักของ Microsoft ได้เลยครับ  
[Microsoft Language Understanding \(LUIS\) Document](https://azure.microsoft.com/en-in/services/cognitive-services/language-understanding-intelligent-service/)
{% endhint %}

โดยสรุปว่า Bot มันเข้าใจความหมายที่ผู้ใช้ส่งเข้ามาได้ยังไงก็คือ การส่งต่อให้ **LUIS** ไปตีความหมายแล้วส่งผลลัพท์กลับมา เพื่อให้ Bot เลือกตัดสินใจต่อว่าจะทำอะไรต่อดีนั่นเอง

## 🤔 แล้วจะให้ Bot โต้ตอบกับผู้ใช้ หรือตัดสินใจยัง ?

ในส่วนของการโต้ตอบหรือตัดสินใจต่างๆของ Bot นั้นสามารถดูตัวอย่างได้จากไฟล์ **`Dialogs/MainDialog.cs`** ครับ ซึ่งตัวโค้ดจุดแรก \(Constructor\) เขาเขียนไว้แบบนี้

```csharp
public MainDialog(FlightBookingRecognizer luisRecognizer, BookingDialog bookingDialog, ILogger<MainDialog> logger)
    : base(nameof(MainDialog))
{
    _luisRecognizer = luisRecognizer;
    Logger = logger;

    AddDialog(new TextPrompt(nameof(TextPrompt)));
    AddDialog(bookingDialog);
    AddDialog(new WaterfallDialog(nameof(WaterfallDialog), new WaterfallStep[]
    {
        IntroStepAsync,
        ActStepAsync,
        FinalStepAsync,
    }));

    // The initial child Dialog to run.
    InitialDialogId = nameof(WaterfallDialog);
}
```

> **อธิบายโค้ดด้านบน**  
> จะเห็นว่าใน **บรรทัดที่ 1** ตัว Bot จะถูกส่งคลาส `FlightBookingRecognizer` เข้ามานั่นหมายความว่าตัว Bot ก็จะสามารถใช้ LUIS ได้เมื่อต้องการ  
> ถัดมาบรรทัดที่ 9~14 เป็นการกำหนดขั้นตอนการทำงานแต่ละ step ของ Bot เช่น เริ่มต้นมาก็จะเรียกขั้นตอน `IntroStepAsync` ต่อมาก็จะเรียก `ActStepAsync` และสุดท้ายก็ `FinalStepAsync` ตามลำดับ

คราวนี้เราลองไปดูตัวอย่าง IntroStepAsync กัน เราก็จะเห็นโค้ดประมาณนี้

{% code title="MainDialog.cs" %}
```csharp
private async Task<DialogTurnResult> IntroStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
{
    ...

    // Use the text provided in FinalStepAsync or the default if it is the first time.
    var messageText = stepContext.Options?.ToString() ?? "What can I help you with today?\nSay something like \"Book a flight from Paris to Berlin on March 22, 2020\"";
    var promptMessage = MessageFactory.Text(messageText, messageText, InputHints.ExpectingInput);
    return await stepContext.PromptAsync(nameof(TextPrompt), new PromptOptions { Prompt = promptMessage }, cancellationToken);
}
```
{% endcode %}

> **อธิบายโค้ดด้านบน**  
> จะเห็นว่าพอเราเข้าไปคุยกับ Bot ปุ๊ป คำพูดแรกที่คุยกับเราคือตามบรรทัดที่ 6 ใช่ไหมครับ`What can I help you with today?    
> Say something like "Book a flight from Paris to Berlin on March 22, 2020"`  
> ซึ่งนี่แหละคือสิ่งที่ Bot ทำใน **IntroStepAsync** นั่นเอง

หลังจากผ่าน IntroStepAsync ไปแล้วมันก็จะเข้ามาทำงานที่ method ActStepAsync ต่อซึ่งเขาก็เขียนไว้ประมาณนี้

{% code title="MainDialog.cs" %}
```csharp
private async Task<DialogTurnResult> ActStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
{
    ...

    var luisResult = await _luisRecognizer.RecognizeAsync<FlightBooking>(stepContext.Context, cancellationToken);
    switch (luisResult.TopIntent().intent)
    {
        case FlightBooking.Intent.BookFlight:
            ...

        case FlightBooking.Intent.GetWeather:
            ...

        default:
            var didntUnderstandMessageText = $"Sorry, I didn't get that. Please try asking in a different way (intent was {luisResult.TopIntent().intent})";
            ...
    }

    return await stepContext.NextAsync(null, cancellationToken);
}
```
{% endcode %}

> **อธิบายโค้ดด้านบน**  
> ในขั้นตอนนี้ตัว Bot จะให้ LUIS วิเคราะห์สิ่งที่ผู้ใช้พิมพ์เข้ามา ในบรรทัดที่ 5 แล้ว Bot ก็จะดูว่าผู้ใช้อยากจะทำอะไร ที่เขียนไว้ใน switch case ของบรรทัดที่ 6~17 เช่น ขอจองเที่ยวบิน หรือ เช็คสภาพอากาศ และสุดท้ายถ้าไม่ใช่ทั้ง 2 กรณีก็จะบอกว่าไม่เข้าใจสิ่งที่พูด

จากตัวอย่างนี้เราก็พอจะเห็นแล้วนะครับว่า การสร้างบทโต้ตอบของ Bot กับผู้ใช้สามารถลองแก้ไขหรือเพิ่มลดได้ที่พวก Dialog ต่างๆ ซึ่งถ้าดูดีๆในตัว MainDialog ก็จะมีการเรียกใช้ **`BookingDialog`** ด้วยเหมือนกันตอนที่จะจองเที่ยวบินจริงๆครับ

## 🎯 บทสรุป

จากทั้งหมดที่ไล่มาจริงๆตัว Bot นั้นก็มีหน้าที่แค่ สร้างชุดคำถามคำตอบให้ผู้ใช้หลายๆแบบนั่นเอง ส่วนขั้นตอนวิเคราะห์ว่าผู้ใช้ป้อนเข้ามานั่นจะสื่อถึงเรื่องไหนก็ใช้ตัว AI ที่ชื่อว่า LUIS เป็นตัวช่วยเพื่อจับคู่การทำงานของ กลุ่มคำถามคำตอบที่ตัวเองมี หรือ ใช้ในการตัดสินใจต่างๆต่อนั่นเอง

สุดท้ายตัว Bot จะเก่งขึ้นเรื่องได้ โดยเราจะต้องใช้ Bot ไปเชื่อมต่อกับ Services อื่นๆ เช่น Google Calendar เพื่อลงบันทึก event หรือ Line เพื่อช่วยโต้ตอบลูกค้าเวลาที่มีปัญหา และที่ขาดไม่ได้คือการเชื่อมเข้ากับตัว AI ต่างๆเช่น Speech services หรือ Vision service เพื่อให้ Bot สามารถพูดออกมา หรือ มองเห็นสิ่งของต่างๆได้นั่นเองครับ

