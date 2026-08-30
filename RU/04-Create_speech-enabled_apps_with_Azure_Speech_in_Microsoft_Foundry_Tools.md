# Создавайте приложения с поддержкой голосового управления, используя сервис Azure Speech в составе инструментов Microsoft Foundry.

# Введение.

Сервис Azure Speech в составе инструментов Foundry предоставляет API, которые можно использовать для создания приложений с поддержкой голосового управления, включая:

- **Преобразование речи в текст:** API, который обеспечивает распознавание речи, позволяя вашему приложению принимать голосовые команды.
- **Преобразование текста в речь:** API, который позволяет осуществлять синтез речи, благодаря чему ваше приложение может воспроизводить текст в виде звука.
- **Перевод речи:** API, который позволяет переводить устную речь на несколько языков.
- **Voice Live:** API, который позволяет создавать искусственные интеллект-агенты, способные вести диалоги в режиме реального времени.

Этот модуль посвящен распознаванию и синтезу речи, которые являются ключевыми функциями любого приложения, работающего с голосовым управлением.

Примеры кода, представленные в этом разделе, написаны на языке Python, но вы можете использовать любой из доступных пакетов Azure Speech SDK для разработки приложений с поддержкой голосового управления на предпочитаемом вами языке. Доступные пакеты SDK включают в себя:

- [Azure Speech for Python](https://pypi.org/project/azure-cognitiveservices-speech?azure-portal=true)
- [Azure Speech for Microsoft .NET](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech?azure-portal=true)
- [Azure Speech for JavaScript](https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk?azure-portal=true)
- [Azure Speech For Java](https://mvnrepository.com/artifact/com.microsoft.cognitiveservices.speech/client-sdk?azure-portal=true)

Примечание.

Мы понимаем, что разные люди предпочитают учиться разными способами. Вы можете пройти этот модуль в формате видео или изучить материалы в виде текста и изображений. Текст содержит более подробную информацию, чем видео, поэтому в некоторых случаях вам может быть полезно использовать его как дополнительный материал к видеолекции.

---

# Функции распознавания речи Azure в инструментах Foundry.

Функциональность Azure Speech в инструментах Foundry представляет собой набор возможностей, связанных с распознаванием и синтезом речи, которые предоставляются ресурсом Foundry. Вы можете использовать эти возможности для добавления поддержки работы с речью в приложения и агенты, разработанные в проектах Microsoft Foundry. Например:

- Создание приложения для транскрибирования записанных телефонных разговоров или собраний.
- Создание искусственного интеллекта (ИИ), который может читать текстовые сообщения или электронные письма вслух.

![Схема, иллюстрирующая работу ресурса Azure Speech, выполняющего функции преобразования речи в текст и текста в речь.](https://learn.microsoft.com/en-us/training/wwl-data-ai/create-speech-enabled-apps/media/azure-speech.png)

## Использование сервиса Azure Speech в ресурсе Microsoft Foundry.

Для использования сервиса Azure Speech в программах Foundry Tools необходимо создать ресурс Microsoft Foundry в вашей подписке Azure.

После того как вы предоставили ресурс Foundry в вашей подписке Azure, вы можете использовать его **адрес (endpoint)** для вызова API сервиса Azure Language из вашего кода, аутентифицируя запросы с помощью **ключа**, связанного с вашим ресурсом. Вы можете вызывать API сервиса Azure Language, отправляя запросы в формате JSON к REST-интерфейсу, или используя любые доступные SDK, разработанные для конкретных языков программирования.

Примечание.

Примеры кода, представленные в этом разделе, написаны на языке Python и используют... [далее следует указание того, что именно используется]. [Python SDK for Azure Speech in Foundry Tools](https://pypi.org/project/azure-cognitiveservices-speech/)SDK для других распространенных языков программирования (таких как Microsoft C#, JavaScript и другие) имеют схожую структуру.

### Создание объекта конфигурации для работы с речью.

Для обеспечения доступа к сервису Azure Speech в среде Foundry Tools необходимо создать объект **SpeechConfig**, который содержит информацию о подключении к этому сервису в вашем ресурсе Foundry.

Совет.

На главной странице портала Foundry по умолчанию отображаются конечная точка (endpoint) и ключ для вашего *проекта*. Чтобы просмотреть ключ и конечную точку для вашего *ресурса*, вы можете перейти на вкладку **Admin** в разделе **Operate** портала и посмотреть информацию о родительском ресурсе вашего проекта. Ключ проекта и ключи ресурсов Foundry совпадают, а конечная точка проекта представляет собой конечную точку ресурса с добавлением */api/projects/{project\_name}* – следовательно, если конечная точка проекта является... `https://my-ai-app-foundry.services.ai.azure.com/api/projects/my-ai-app`...то конечная точка ресурса является... `https://my-ai-app-foundry.services.ai.azure.com`.

Например, следующий код на языке Python создает объект **SpeechConfig**, который можно использовать для отправки запросов к API сервиса распознавания речи Azure в ресурсе Foundry.

```
# run "pip install azure-cognitiveservices-speech" first to install the package 
import azure.cognitiveservices.speech as speech_sdk

# Create SpeechConfig using endpoint and key
speech_config = speech_sdk.SpeechConfig(subscription="YOUR_FOUNDRY_KEY",
                                        endpoint="YOUR_FOUNDRY_ENDPOINT")
```

Примечание.

В версиях SDK для Python, выпущенных до **1.48.2**, требовалось указывать *регион*, в котором размещен ваш ресурс, вместо указания конечной точки (endpoint). В последней версии вы можете использовать либо конечную точку ресурса Foundry, либо регион.

---

# Используйте API преобразования речи в текст.

Функция Azure Speech в инструментах Foundry поддерживает распознавание речи с помощью API "Преобразование речи в текст". Хотя конкретные детали могут различаться в зависимости от используемого SDK (Python, C# и т.д.), существует единый принцип использования API "Преобразование речи в текст":

![Схема, демонстрирующая процесс создания объекта `SpeechRecognizer` на основе объектов `SpeechConfig` и `AudioConfig`, а также использование метода `RecognizeOnceAsync` для вызова API преобразования речи в текст.](https://learn.microsoft.com/en-us/training/wwl-data-ai/create-speech-enabled-apps/media/speech-to-text.png)

1. Используйте объект `SpeechConfig` для хранения информации, необходимой для подключения к вашему ресурсу Foundry. В частности, укажите его **адрес сервера** (или **регион**) и **ключ доступа**.
2. При необходимости можно использовать параметр **AudioConfig** для указания источника звука, который будет транскрибироваться. По умолчанию используется встроенный микрофон системы, но также можно указать аудиофайл.
3. Используйте объекты `SpeechConfig` и `AudioConfig` для создания объекта `SpeechRecognizer`. Этот объект является клиентским прокси-сервером для API преобразования речи в текст.
4. Используйте методы объекта **SpeechRecognizer**, чтобы вызывать базовые функции API. Например, метод **RecognizeOnceAsync()** использует службу Azure Speech для асинхронной транскрипции одной произнесенной фразы.
5. Обработайте полученный ответ. В случае использования метода **RecognizeOnceAsync()**, результатом является объект **SpeechRecognitionResult**, который содержит следующие свойства:
   - Продолжительность.
   - Смещение в тиках.
   - Свойства.
   - Причина.
Разум.
Обоснование.
Мотив.
Логика.
Смысл.
Понимание.
Объяснение.
Аргумент.
Пример (в значении "почему").
   - Идентификатор результата.
   - Пожалуйста, предоставьте текст, который вам нужно перевести. Я готов выполнить перевод на русский язык.

Если операция прошла успешно, свойство **Reason** принимает одно из предопределенных значений: **RecognizedSpeech** (распознана речь), а свойство **Text** содержит расшифровку. Другие возможные значения для свойства **Result** включают **NoMatch** (указывает на то, что аудиофайл был успешно обработан, но речь не была распознана) или **Canceled** (указывает на возникновение ошибки). В этом случае вы можете проверить коллекцию **Properties** и свойство **CancellationReason**, чтобы определить причину ошибки.

## Пример: Транскрибирование аудиофайла.

Следующий пример кода на языке Python использует сервис Azure Speech в системе Foundry Tools для транскрибирования речи из аудиофайла.

```
import azure.cognitiveservices.speech as speech_sdk

# Speech config encapsulates the connection to the resource
speech_config = speech_sdk.SpeechConfig(subscription="YOUR_FOUNDRY_KEY",
                                       endpoint="YOUR_FOUNDRY_ENDPOINT")

# Audio config determines the audio stream source (defaults to system mic)
file_path = "audio.wav"
audio_config = speech_sdk.audio.AudioConfig(filename=file_path)

# Use a speech recognizer to transcribe the audio
speech_recognizer = speech_sdk.SpeechRecognizer(speech_config=speech_config,
                                               audio_config=audio_config)

result = speech_recognizer.recognize_once_async().get()

# Did it succeeed
if result.reason == speech_sdk.ResultReason.RecognizedSpeech:
    # Yes!
    print(f"Transcription:\n{result.text}")
else:
    # No. Try to determine why.
    print("Error transcribing message: {}".format(result.reason))
```

---

# Используйте API преобразования текста в речь.

Аналогично своим API для преобразования речи в текст, платформа Azure Speech в составе инструментов Foundry предлагает API для синтеза речи, то есть для преобразования текста в речь.

Как и в случае с распознаванием речи, на практике большинство интерактивных приложений, использующих голосовое управление, создаются с использованием SDK для работы с речью от Microsoft Azure.

Принцип реализации синтеза речи во многом схож с принципом распознавания речи:

![Схема, демонстрирующая процесс создания объекта `SpeechSynthesizer` на основе объектов `SpeechConfig` и `AudioConfig`, а также использование метода `SpeakTextAsync` для вызова API распознавания речи.](https://learn.microsoft.com/en-us/training/wwl-data-ai/create-speech-enabled-apps/media/text-to-speech.png)

1. Используйте объект `SpeechConfig` для хранения информации, необходимой для подключения к вашему ресурсу Azure Speech. В частности, это его расположение (`location`) и ключ доступа (`key`).
2. При необходимости можно использовать объект **AudioConfig** для указания устройства вывода синтезированной речи. По умолчанию используется динамик системы, но также можно указать аудиофайл или, установив это значение в "null", получить прямой доступ к объекту потока аудиоданных.
3. Используйте объекты `SpeechConfig` и `AudioConfig` для создания объекта `SpeechSynthesizer`. Этот объект является клиентским прокси-сервером для API преобразования текста в речь.
4. Используйте методы объекта **SpeechSynthesizer** для вызова базовых функций API. Например, метод **SpeakTextAsync()** использует службу Azure Speech для преобразования текста в речь.
5. Обработайте ответ от сервиса Azure Speech. В случае использования метода **SpeakTextAsync**, результатом является объект **SpeechSynthesisResult**, который содержит следующие свойства:
   - Аудиоданные.
   - Свойства.
   - Причина.
Разум.
Обоснование.
Мотив.
Логика.
Смысл.
Понимание.
Объяснение.
Аргумент.
Пример (в значении "почему").
   - Идентификатор результата.

Когда синтез речи успешно завершен, свойство **Reason** устанавливается в значение перечисления **SynthesizingAudioCompleted**, а свойство **AudioData** содержит аудиопоток (который, в зависимости от настроек **AudioConfig**, мог быть автоматически отправлен на динамик или сохранен в файл).

## Пример – преобразование текста в речь.

Следующий пример на языке Python использует сервис Azure Speech в системе Foundry Tools для преобразования текста в речь.

```
import azure.cognitiveservices.speech as speechsdk

# Speech config encapsulates the connection to the resource
speech_config = speechsdk.SpeechConfig(subscription=KEY, endpoint=ENDPOINT)

# Audio output config determines where to send the audio stream (defaults to speaker)
audio_config = speechsdk.audio.AudioOutputConfig(use_default_speaker=True)

# Use speech synthesizer to synthesize text as speech
speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config,
                                                 audio_config=audio_config)
text = "My voice is my password!"
speech_synthesis_result = speech_synthesizer.speak_text_async(text).get()

# Did it succeeed?
if speech_synthesis_result.reason == speechsdk.ResultReason.SynthesizingAudioCompleted:
    # Yes!
    print("Speech synthesized for text [{}]".format(text))
elif speech_synthesis_result.reason == speechsdk.ResultReason.Canceled:
    # No - Ty to find out why not
    cancellation_details = speech_synthesis_result.cancellation_details
    print("Speech synthesis canceled: {}".format(cancellation_details.reason))
    if cancellation_details.reason == speechsdk.CancellationReason.Error:
        if cancellation_details.error_details:
            print("Error details: {}".format(cancellation_details.error_details))
```

---

# Настройте формат аудио и голоса.

При синтезе речи вы можете использовать объект `SpeechConfig` для настройки аудио, которое возвращает сервис Azure Speech в инструментах Foundry.

## Аудиоформат.

Сервис Azure Speech поддерживает различные форматы вывода для аудиопотока, генерируемого при синтезе речи. В зависимости от ваших конкретных потребностей, вы можете выбрать формат, исходя из требуемых параметров:

- Тип аудиофайла.
- Частота дискретизации.
- Глубина битового представления.

Например, следующий код на языке Python задает формат вывода речи для ранее определенного объекта `SpeechConfig` с именем *speech_config*:

```
speech_config.set_speech_synthesis_output_format(SpeechSynthesisOutputFormat.Riff24Khz16BitMonoPcm)
```

Для получения полного списка поддерживаемых форматов и соответствующих им числовых значений, обратитесь к [укажите источник информации]. [Azure Speech SDK documentation](/en-us/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesisoutputformat).

## Голоса.

Сервис Azure Speech предоставляет различные голоса, которые можно использовать для персонализации приложений с поддержкой голосового управления. Голоса идентифицируются по названиям, указывающим на регион, имя человека и другие детали – например... `en-US-Brian:DragonHDLatestNeural`.

Следующий пример кода на языке Python устанавливает используемый голос.

```
speech_config.speech_synthesis_voice_name='en-US-Brian:DragonHDLatestNeural'
```

Для получения информации о голосах, пожалуйста, обратитесь к разделу... [Azure Speech SDK documentation](/en-us/azure/ai-services/speech-service/language-support?tabs=tts).

---

# Используйте язык разметки для синтеза речи.

Хотя SDK Azure Speech позволяет отправлять простой текст для преобразования в речь, сервис также поддерживает XML-формат для описания характеристик речи, которую вы хотите сгенерировать. Этот язык разметки синтеза речи (Speech Synthesis Markup Language, SSML) предоставляет больше возможностей для управления звучанием речевого вывода, позволяя вам:

- При использовании нейросети для синтеза речи укажите желаемый стиль произношения, например, "восторженный" или "веселый".
- Вставляйте паузы или моменты тишины.
- Укажите *фонемы* (фонетические произношения), например, чтобы произносить текст "SQL" как "сиквел".
- Настройте *просодию* голоса (влияя на высоту тона, тембр и скорость речи).
- Используйте стандартные правила преобразования текста, например, для указания того, что определенная строка должна быть представлена в виде даты, времени, номера телефона или другого формата.
- Вставьте записанную речь или аудиозапись, например, для включения стандартного голосового сообщения или имитации фонового шума.

Например, рассмотрим следующий пример использования языка SSML:

```
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" 
                     xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US"> 
    <voice name="en-US-AriaNeural"> 
        <mstts:express-as style="cheerful"> 
          I say tomato 
        </mstts:express-as> 
    </voice> 
    <voice name="en-US-GuyNeural"> 
        I say <phoneme alphabet="sapi" ph="t ao m ae t ow"> tomato </phoneme>. 
        <break strength="weak"/>Lets call the whole thing off! 
    </voice> 
</speak>
```

Этот текст в формате SSML описывает диалог между двумя разными синтезированными голосами, например, следующим образом:

- Ариана: (весело) "Я говорю 'помидор':"
- **Парень:** "Я говорю "помидор" (произносится как "том-а-до"),... Давайте все отменить!"

Чтобы отправить описание в формате SSML (Speech Synthesis Markup Language) в службу распознавания речи, можно использовать соответствующий метод объекта `SpeechSynthesizer`, например, следующим образом:

```
speech_synthesis_result = speech_synthesizer.speak_ssml_async('<speak>...').get()
```

Для получения дополнительной информации о языке SSML, пожалуйста, обратитесь к [ссылке/разделу]. [Azure Speech SDK documentation](/en-us/azure/ai-services/speech-service/speech-synthesis-markup).

---

# Краткое изложение.
Или: Резюме.
(В зависимости от контекста)

В этом модуле вы узнали, как:

- Подключите службу Azure Speech в инструментах Foundry, расположенных в ресурсе Foundry.
- Используйте API преобразования речи в текст для реализации функции распознавания речи.
- Используйте API преобразования текста в речь для реализации синтеза речи.
- Настройте формат аудио и голоса.
- Используйте язык разметки для синтеза речи (SSML).

Для получения дополнительной информации об Azure Speech обратитесь к документации по этой технологии. [Azure Speech in Foundry Tools documentation](/en-us/azure/ai-services/speech-service/).
