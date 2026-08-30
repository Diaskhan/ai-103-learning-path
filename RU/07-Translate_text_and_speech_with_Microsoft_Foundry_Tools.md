# Переводите текст и речь с помощью инструментов Microsoft Foundry.

# Введение.

Во всем мире используется множество языков, и возможность обмена информацией между людьми, говорящими на разных языках, часто является важным условием для решения глобальных проблем.

Перевод с одного языка на другой – это специализированный навык, который часто требует много времени и может быть дорогостоящим. Автоматизированный перевод (иногда называемый "машинным переводом") часто используется для сокращения затрат времени и средств; однако он требует сложного программного обеспечения, которое понимает лингвистические правила и идиомы как исходного, так и целевого языков.

Модели искусственного интеллекта обычно являются основой автоматизированных решений для перевода, будь то перевод текстовых документов или устной речи. В этом модуле вы узнаете о различных способах разработки решений для перевода на основе искусственного интеллекта с использованием платформы Microsoft Foundry.

Примечание.

Мы понимаем, что разные люди предпочитают учиться разными способами. Вы можете пройти этот модуль в формате видео или изучить материалы в виде текста и изображений. Текст содержит более подробную информацию, чем видео, поэтому в некоторых случаях вам может быть полезно использовать его как дополнительный материал к видеолекции.

---

# Перевод в Microsoft Foundry.

Многие большие языковые модели (LLM) способны генерировать текст на нескольких языках и даже переводить отдельные фразы или целые документы. Однако для полноценных решений по многоязычному переводу, как правило, требуются специализированные модели; а Microsoft Foundry предоставляет поддержку для работы с переводами через инструменты Foundry. В частности:

- **Azure Translator в инструментах Foundry:** Комплексный сервис перевода текста, поддерживающий широкий спектр языков и позволяющий создавать собственные модели машинного перевода.
- **Функции распознавания и синтеза речи Azure в инструментах Foundry:** Набор инструментов, связанных с обработкой речи, включая преобразование речи в текст и перевод речи на другие языки одновременно.

![Схема инструментов литейного производства для справки.](https://learn.microsoft.com/en-us/training/wwl-data-ai/translate-text-speech/media/translation.png)

Сервисы Azure Translator и Azure Speech доступны через ресурс Microsoft Foundry, а также предоставляют обширные API и специализированные SDK для различных языков, которые можно использовать для разработки комплексных решений в области перевода.

---

# Переведите текст.

Сервис Azure Translator, интегрированный в инструменты Foundry, предоставляет API для перевода текста между более чем 90 поддерживаемыми языками. С помощью Azure Translator вы можете:

- Переводите или транслитерируйте текст, используя стандартную модель перевода или большую языковую модель (LLM).
- Переводите документы синхронно или асинхронно, сохраняя при этом их структуру.
- Используйте специализированные модели машинного перевода для перевода терминов, относящихся к конкретной области знаний.

В этом разделе мы сосредоточимся на API для *перевода текста*. Более подробную информацию о всех возможностях сервиса Azure Translator вы можете найти в... [Azure Translator in Foundry Tools documentation](/en-us/azure/ai-services/translator?azure-portal=true).

## Используйте сервис Azure Translator в портале Microsoft Foundry.

Вы можете ознакомиться с сервисом Azure Translator в портале Microsoft Foundry, где доступны инструменты для тестирования перевода текста и документов.

![Скриншот интерфейса для тестирования текстового переводчика в портале Foundry.](https://learn.microsoft.com/en-us/training/wwl-data-ai/translate-text-speech/media/translator-playground.png)

Портал Foundry – это отличный способ поработать с сервисом Azure Translator, сравнить результаты работы стандартной модели с результатами, полученными при использовании больших языковых моделей (LLM), а также ознакомиться с примерами кода для использования переводчика в ваших собственных приложениях.

## Используйте сервис Azure Translator в коде вашего приложения.

Вы можете использовать... [REST API](/en-us/azure/ai-services/translator/text-translation/reference/rest-api-guide?azure-portal=true) чтобы вызывать функции сервиса Azure Translator, или вы можете написать код на предпочитаемом вами языке программирования, используя один из поддерживаемых SDK (наборов средств разработки), которые включают в себя:

- [Azure Translator Text Translation Client for Python](https://pypi.org/project/azure-ai-translation-text/1.0.1/?azure-portal=true)
- [Azure Translator Text Translation Client for Microsoft .NET](https://www.nuget.org/packages/Azure.AI.Translation.Text/1.0.0?azure-portal=true)
- [Azure Translator Text Translation Client for Java](https://mvnrepository.com/artifact/com.azure/azure-ai-translation-text/1.0.0?azure-portal=true)
- [Azure Translator Text Translation Client for JavaScript](https://www.npmjs.com/package/@azure-rest/ai-translation-text/v/1.0.0?azure-portal=true)

### Подключитесь к ресурсу Azure Translator.

API-интерфейсы Azure Translator предоставляются через REST-*точки доступа*, к которым ваше клиентское приложение должно установить аутентифицированное соединение. Точка доступа может быть:

- Глобальный конечный пункт сервиса Azure Translator: `api.cognitive.microsofttranslator.com`
- Региональные конечные точки сервиса Azure Translator: Эти конечные точки включают в себя... `api-nam.cognitive.microsofttranslator.com`, `api-apc.cognitive.microsofttranslator.com`и. `api-eur.cognitive.microsofttranslator.com`
- Точки доступа к ресурсам Foundry: `{foundry-resource-name}.cognitiveservices.azure.com/`

Вы можете подключить клиент к определенному узлу (endpoint), либо вы можете установить соединение, указав регион, в котором размещены ваши ресурсы. Например, для подключения к сервису Azure Translator с использованием вашего ключа API Foundry для аутентификации, можно использовать любой из методов, показанных в следующем примере кода:

```
from azure.core.credentials import AzureKeyCredential
from azure.ai.translation.text import *

key_credential = AzureKeyCredential("FOUNDRY_KEY")

# Connect to a Foundry resource endpoint
client = TextTranslationClient(credential=key_credential, endpoint="FOUNDRY_ENDPOINT")

# Or connect using a region
client = TextTranslationClient(credential=key_credential, region="FOUNDRY_REGION")
```

Совет.

Для получения дополнительной информации о конструкторе класса **TextTranslationClient**, обратитесь к документации. [Azure Translator Python SDK documentation](/en-us/python/api/azure-ai-translation-text/azure.ai.translation.text.texttranslationclient?azure-portal=true#constructor).

### Определите доступные языки.

Сервис Azure Translator поддерживает более 90 языков. В некоторых случаях вам может потребоваться предоставить пользователям список доступных языков для перевода; пример кода, демонстрирующий это, приведен ниже:

```
languages = client.get_supported_languages(scope="translation")
print("{} languages supported:".format(len(languages.translation)))
for language in languages.translation.keys():
    print(languages.translation[language].name + " (" + language + ")")
```

Результаты включают в себя *название* и *код ISO* для каждого языка:

```
137 languages supported:
Afrikaans (af)
Amharic (am)
Arabic (ar)
Assamese (as)
Azerbaijani (az)
Bashkir (ba)
Belarusian (be)
Bulgarian (bg)
...
```

Совет.

Для получения дополнительной информации о методе **get_supported_language**, обратитесь к документации. [Azure Translator Python SDK documentation](/en-us/python/api/azure-ai-translation-text/azure.ai.translation.text.texttranslationclient?azure-portal=true#azure-ai-translation-text-texttranslationclient-get-supported-languages).

### Переведите текст.

Для перевода текста с языка оригинала (*исходного языка*) на один или несколько языков назначения, используйте метод **translate**.

- Исходный текст передается в метод в виде списка объектов **InputTextItem**, каждый из которых содержит текстовую строку, подлежащую переводу.
- Вы можете опционально указать параметр **from_language**, используя код языка-источника в соответствии со стандартом ISO (например, "en"). Либо вы можете опустить этот параметр, чтобы сервис Azure Translator автоматически определил язык-исходник.
- Языки перевода указываются в параметре **to_language** в виде списка кодов языков. Сервис Azure Translator вернет перевод для каждого указанного, корректного кода языка.

Следующий пример демонстрирует перевод двух текстовых фрагментов, написанных на разных, не указанных языках, на французский (*fr*) и английский (*en*).

```
input_text_elements = [InputTextItem(text="Hola"), InputTextItem(text="こんにちは")]
translation_results = client.translate(body=input_text_elements, to_language=["fr", "en"])
idx = 0
for translation in translation_results:
    input_text = input_text_elements[idx].text
    idx += 1
    sourceLanguage = translation.detected_language
    for translated_text in translation.translations:
        print(f"'{input_text}' was translated from {sourceLanguage.language} to {translated_text.to} as '{translated_text.text}'.")
```

Вывод этого кода показывает определенные языки оригинала как испанский (*es*) и японский (*ja*).

```
'Hola' was translated from es to fr as 'Bonjour'.
'Hola' was translated from es to en as 'Hello'.
'こんにちは' was translated from ja to fr as 'Bonjour'.
'こんにちは' was translated from ja to en as 'Hello'.
```

Совет.

Для получения дополнительной информации о методе **translate**, обратитесь к разделу... [Azure Translator Python SDK documentation](/en-us/python/api/azure-ai-translation-text/azure.ai.translation.text.texttranslationclient?azure-portal=true#azure-ai-translation-text-texttranslationclient-translate).

### Транслитерировать текст.

Японский текст в предыдущем примере написан с использованием слогового письма хирагана. Поэтому, вместо того чтобы переводить его на другой язык, вы можете рассмотреть возможность транслитерации – то есть передачи японских слов латинскими буквами (как это делается в английском тексте).

Для этого мы можем передать японский текст в метод транслитерации, указав параметр `from_script` со значением `Jpan` и параметр `to_script` со значением `Latn`, как показано ниже:

```
source_text = "こんにちは"
input_text_elements = [InputTextItem(text=source_text)]
transliteration_results = client.transliterate(body=input_text_elements, language="ja",
                                               from_script="Jpan", to_script="Latn")
for transliteration in transliteration_results:
    sourceScript = transliteration.script
    targetScript = transliteration.text
    print(f"'{source_text}' was transliterated into {sourceScript} as {targetScript}.")
```

Этот пример кода выдает следующий результат:

```
'こんにちは' was transliterated into Latn as Kon'nichiwa​.
```

Совет.

Для получения дополнительной информации о методе транслитерации, обратитесь к разделу... [Azure Translator Python SDK documentation](/en-us/python/api/azure-ai-translation-text/azure.ai.translation.text.texttranslationclient?azure-portal=true#azure-ai-translation-text-texttranslationclient-transliterate).

---

# Перевести речь.

Функция Azure Speech в составе инструментов Foundry предоставляет несколько API, которые можно использовать для создания приложений и сервисов с поддержкой голосового управления. API перевода речи позволяет создавать решения, которые преобразуют устную речь в текст или воспроизводят ее в виде аудиоперевода.

## Используйте функцию перевода речи Azure в коде вашего приложения.

Для поддержки перевода речи, сервис Azure Speech предоставляет API, который вы можете использовать в своем приложении через объект **TranslationRecognizer**. Этот объект создается путем подключения к вашему ресурсу Azure Speech с использованием объекта **SpeechTranslationConfig**.

### Подключитесь к ресурсу Azure Speech.

Для использования API перевода речи Azure в клиентском коде необходимо использовать объект **SpeechTranslationConfig** для подключения к ресурсу Azure Speech. Microsoft Foundry предоставляет Azure Speech как инструмент внутри ресурса Foundry, и вы можете подключиться, указав соответствующий *адрес сервера* или *регион* для вашего ресурса, как показано в следующем примере кода:

```
import azure.cognitiveservices.speech as speech_sdk

# Connect to a Foundry resource endpoint
translation_cfg = speech_sdk.translation.SpeechTranslationConfig(
                    subscription="FOUNDRY_KEY", endpoint="FOUNDRY_ENDPOINT")

# Or connect using a region
translation_cfg = speech_sdk.translation.SpeechTranslationConfig(
                    subscription="FOUNDRY_KEY", region="FOUNDRY_REGION")
```

Совет.

Для получения дополнительной информации о конструкторе `SpeechTranslationConfig`, обратитесь к документации по [ссылке]. [Azure Speech Python SDK documentation](/en-us/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.translation.speechtranslationconfig?azure-portal=true#constructor).

### Настройте языки перевода и параметры ввода.

Сервис Azure Speech может переводить аудиозаписи с одного или нескольких языков. Для настройки исходного и целевого языков используется объект `SpeechTranslationConfig`, а для указания источника звукового потока – объект `AudioConfig`.

Например, следующий код настраивает объект `SpeechTranslationConfig` для перевода с американского английского языка (*en-US*) на французский (*fr*) и японский (*ja*), а также использует объект `AudioConfig` для указания источника звука как встроенного микрофона системы.

```
# Configure languages
translation_cfg.speech_recognition_language = 'en-US'
translation_cfg.add_target_language('fr')
translation_cfg.add_target_language('ja')
print('Ready to translate from',translation_cfg.speech_recognition_language)

# Configure audio source
audio_cfg = speech_sdk.AudioConfig(use_default_microphone=True)
```

Совет.

Для получения дополнительной информации об классе **AudioConfig**, обратитесь к документации по [ссылке]. [Azure Speech Python SDK documentation](/en-us/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audio.audioconfig?azure-portal=true).

### Преобразовать речь в текст.

Теперь вы можете использовать объект `TranslationRecognizer` для перевода устной речи, например, следующим образом:

```
# Get a TranslationRecognizr object
translator = speech_sdk.translation.TranslationRecognizer(translation_config=translation_cfg,
                                                          audio_config=audio_cfg)
# Get input from mic and translate
print("Speak now...")
translation_results = translator.recognize_once_async().get()
print(f"Translating '{translation_results.text}'")

# Print each translation
translations = translation_results.translations
for translation_language in translations:
    print(f"{translation_language}: '{translations[translation_language]}'")
```

Запуск этого кода и произнесение слова "привет" в микрофон приводит к следующему результату:

```
Speak now...
Translating 'Hello.'
fr: 'Bonjour.'
ja: 'こんにちは。'
```

Совет.

Для получения дополнительной информации о классе **TranslationRecognizer**, обратитесь к документации [здесь]. [Azure Speech Python SDK documentation](/en-us/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.translation.translationrecognizer?azure-portal=true).

## Синтезируйте переводы в виде речи.

Если вам необходимо реализовать систему автоматического перевода речи в речь, существует два основных подхода, которые вы можете выбрать:

### Ручной синтез.

Ручной синтез – это простой способ объединить результаты машинного перевода речи. С помощью ручного синтеза можно создавать аудиофайлы на основе текстовых переводов. По сути, это комбинация двух отдельных этапов, в ходе которых вы:

1. Используйте компонент **TranslationRecognizer** для преобразования устной речи в текстовые расшифровки на одном или нескольких целевых языках.
2. Перебирайте результаты перевода, содержащиеся в списке "Переводы", используя компонент "Синтезатор речи" (SpeechSynthesizer) для преобразования текста каждого языка в аудиопоток.

Например, мы можем расширить предыдущий пример и использовать службу Azure Speech для синтеза речи каждого перевода, который возвращается, следующим образом:

```
import azure.cognitiveservices.speech as speech_sdk

# Configure translation
translation_cfg = speech_sdk.translation.SpeechTranslationConfig(subscription="FOUNDRY_KEY",
                                                                 endpoint="FOUNDRY_ENDPOINT")
translation_cfg.speech_recognition_language = 'en-US'
translation_cfg.add_target_language('fr')
translation_cfg.add_target_language('ja')
audio_cfg = speech_sdk.AudioConfig(use_default_microphone=True)

# Configure speech synthesis
speech_cfg = speech_sdk.SpeechConfig(subscription="FOUNDRY_KEY", 
                                     endpoint="FOUNDRY_ENDPOINT")
audio_out_cfg = speech_sdk.audio.AudioOutputConfig(use_default_speaker=True)
voices = {
        "fr": "fr-FR-HenriNeural",
        "ja": "ja-JP-NanamiNeural"
}

# Get trsnslations
translator = speech_sdk.translation.TranslationRecognizer(translation_config=translation_cfg,
                                                          audio_config=audio_cfg)
print("Speak now...")
translation_results = translator.recognize_once_async().get()
print(f"Translating '{translation_results.text}'")

# process the translation results
translations = translation_results.translations
for translation_language in translations:

    # Print ressults
    print(f"{translation_language}: '{translations[translation_language]}'")

    # Speak results
    speech_cfg.speech_synthesis_voice_name = voices.get(translation_language)
    speech_synthesizer = speech_sdk.SpeechSynthesizer(speech_cfg, audio_out_cfg)
    speak = speech_synthesizer.speak_text_async(translations[translation_language]).get()

    # CHeck for speech failure
    if speak.reason != speech_sdk.ResultReason.SynthesizingAudioCompleted:
        print(speak.reason)
```

Обратите внимание, что для использования API синтеза речи необходимо создать объект **SpeechConfig**, а также отдельный объект **AudioConfig** для направления звукового вывода на динамик по умолчанию. Кроме того, можно указать языковые модели голоса, чтобы оптимизировать произношение переведенной речи.

### Синтез, основанный на событиях.

Когда вам необходимо выполнить прямой перевод (перевод с одного исходного языка на один целевой язык), вы можете использовать синтез, основанный на событиях, чтобы преобразовать текст перевода в аудиопоток. Для этого вам нужно:

- Укажите желаемый голос для синтезированной речи в настройках перевода (**TranslationConfig**).
- Создайте обработчик событий для события **Synthesizing**, которое генерируется объектом **TranslationRecognizer**.
- В обработчике события используйте метод **GetAudio()** параметра **Result**, чтобы получить поток байтов, содержащий переведенный аудиофайл.

Примечание.

Вы не можете использовать синтез на основе событий для многоязычного перевода.

Например, следующий код на языке Python использует встроенный обработчик событий для захвата преобразованного аудиопотока и сохранения его в файл. Затем этот код воспроизводит файл с помощью библиотеки **playsound**.

```
import azure.cognitiveservices.speech as speech_sdk
from playsound3 import playsound

# Configure translation
source_language, target_language = "en-US", "fr"
output_file = "translation.wav"
translation_cfg = speech_sdk.translation.SpeechTranslationConfig(subscription="FOUNDRY_KEY",
                                                                 endpoint="FOUNDRY_ENDPOINT")
translation_cfg.speech_recognition_language = source_language
translation_cfg.add_target_language(target_language)
translation_cfg.voice_name = "fr-FR-HenriNeural"
audio_cfg = speech_sdk.AudioConfig(use_default_microphone=True)
translator = speech_sdk.translation.TranslationRecognizer(translation_config=translation_cfg,
                                                          audio_config=audio_cfg)

# Event handler function to save the synthesized audio to a file
def synthesis_callback(evt):
    size = len(evt.result.audio)
    print(f'Audio synthesized: {size} byte(s) {"(COMPLETED)" if size == 0 else ""}')

    if size > 0:
        file = open(output_file, 'wb+')
        file.write(evt.result.audio)
        file.close()

# Connect the event handler function
translator.synthesizing.connect(synthesis_callback)

# Get input from mic and translate it
print(f"Speak now (in {source_language})...")
translation_results = translator.recognize_once()
print(f"Translating '{translation_results.text}'")

# Print and play the translation results
print(translation_results.translations[target_language])
playsound(output_file)
```

Совет.

Для получения дополнительной информации о создании машинных переводов, пожалуйста, обратитесь к разделу... [Azure Speech documentation](/en-us/azure/ai-services/speech-service/how-to-translate-speech?tabs=terminal&pivots=programming-language-python&azure-portal=true#synthesize-translations).

---

# Краткое изложение.
Или: Резюме.
(В зависимости от контекста)

Этот модуль рассматривал возможности внедрения решения для машинного перевода на базе искусственного интеллекта с использованием платформы Microsoft Foundry. В частности, в нем описывалось, как это сделать:

- Используйте сервис Azure Translator в инструментах Foundry для перевода текста.
- Используйте службу Azure Speech в инструментах Foundry для перевода речи.

Для получения дополнительной информации об инструментах, рассмотренных в этом разделе, обратитесь к следующей документации:

- [Azure Translator in Foundry Tools documentation](/en-us/azure/ai-services/translator?azure-portal=true)
- [Azure Speech in Foundry Tools translation documentation](/en-us/azure/ai-services/speech-service/speech-translation?azure-portal=true)
