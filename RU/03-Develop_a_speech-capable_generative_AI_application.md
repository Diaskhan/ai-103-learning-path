# Разработайте приложение на основе генеративного искусственного интеллекта, способное генерировать речь.

# Введение.

Распознавание и синтез речи – это полезные функции, которые могут быть применены во многих ситуациях, в том числе:

- Запись устных разговоров во время телефонных звонков и совещаний.
- Создание текстовых описаний (подписей) для видео или презентаций.
- Создание звуковых пользовательских интерфейсов для повышения доступности приложений.
- Разработка голосовых помощников на основе искусственного интеллекта, которые могут читать текстовые сообщения или электронные письма вслух.

В этом модуле мы рассмотрим, как использовать генеративные модели искусственного интеллекта с возможностью распознавания и синтеза речи в платформе Microsoft Foundry для преобразования речи в текст и текста в речь.

Примечание.

Мы понимаем, что разные люди предпочитают учиться разными способами. Вы можете пройти этот модуль в формате видео или изучить материалы в виде текста и изображений. Текст содержит более подробную информацию, чем видео, поэтому в некоторых случаях вам может быть полезно использовать его как дополнительный материал к видеолекции.

---

# Выберите модель с поддержкой голосового управления.

Microsoft Foundry Models – это каталог моделей искусственного интеллекта, включающий генеративные модели от различных поставщиков. Различные модели обладают разными возможностями и оптимизированы для решения различных задач.

Чтобы найти подходящую модель, вы можете воспользоваться фильтрами и функцией поиска в портале Microsoft Foundry.

![Скриншот каталога моделей в портале Foundry.](https://learn.microsoft.com/en-us/training/wwl-data-ai/develop-generative-ai-audio-apps/media/model-catalog.png)

Когда речь идет о моделях, способных к генерации речи, стоит рассмотреть два наиболее распространенных варианта использования:

- Модели генеративного искусственного интеллекта, способные преобразовывать речь в текст.
- Модели генеративного искусственного интеллекта, способные преобразовывать текст в речь.

Платформа Microsoft Foundry предлагает модели, которые поддерживают оба этих варианта использования, включая специализированные модели с возможностью распознавания и синтеза речи из семейства моделей OpenAI **gpt-4o**.

Совет.

Чтобы узнать больше о доступных моделях в Microsoft Foundry, обратитесь к разделу "**".[Microsoft Foundry Models overview](/en-us/azure/foundry/concepts/foundry-models-overview?azure-portal=true)Статья в документации Microsoft Foundry.

---

# Синтезировать речь.

Синтез речи, или преобразование текста в речь, является обратным процессом по отношению к преобразованию речи в текст. Он заключается в передаче текстовых данных модели, которая, в свою очередь, генерирует аудиопоток, содержащий произнесенный текст.

Модели, поддерживающие функции преобразования текста в речь, включают в себя:

- **gpt-4o-синтез речи**
- gpt-4o-мини-TTS.

Примечание.

Доступность конкретных моделей может отличаться в зависимости от региона. Ознакомьтесь с информацией о...[model regional availability](/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure?pivots=azure-openai#model-summary-table-and-region-availability&azure-portal=true)Таблица в документации Microsoft Foundry.

## Использование модели преобразования текста в речь.

Аналогично моделям преобразования речи в текст, вы можете использовать клиент **AzureOpenAI** в SDK OpenAI для подключения к ресурсу Microsoft Foundry и загружать текстовые данные в модель преобразования текста в речь для синтеза речи.

```
from openai import AzureOpenAI
from pathlib import Path

# Create an AzureOpenAI client
client = AzureOpenAI(
    azure_endpoint=YOUR_FOUNDRY_ENDPOINT,
    api_key=YOUR_FOUNDRY_KEY,
    api_version="2025-03-01-preview"
)

# Path for audio output file
speech_file_path = Path("output_speech.wav")

# Generate speech and save to file
with client.audio.speech.with_streaming_response.create(
            model=YOUR_MODEL_DEPLOYMENT,
            voice="alloy",
            input="This speech was AI-generated!",
            instructions="Speak in an upbeat, excited tone.",
    ) as response:
    response.stream_to_file(speech_file_path)

print(f"Speech generated and saved to {speech_file_path}")
```

---

# Преобразовать речь в текст.
Или:
Распознать речь и преобразовать ее в текст.
Или:
Транскрибировать речь.

Транскрибация речи, или преобразование речи в текст, предполагает передачу аудиозаписи модели, которая затем выдает текстовую расшифровку этой записи.

Модели, поддерживающие функции преобразования речи в текст, включают в себя:

- **gpt-4o-транскрибация**
- gpt-4o-мини-транскрибатор.
- gpt-4o-транскрибирование и разделение по говорящим.

Примечание.

Доступность конкретных моделей может отличаться в зависимости от региона. Ознакомьтесь с информацией о...[model regional availability](/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure?pivots=azure-openai#model-summary-table-and-region-availability&azure-portal=true)Таблица в документации Microsoft Foundry.

## Использование модели преобразования речи в текст.

Чтобы использовать модель преобразования речи в текст в своем приложении, вы можете воспользоваться клиентом **AzureOpenAI** в SDK OpenAI для подключения к конечной точке вашего ресурса Microsoft Foundry и загрузить содержимое аудиофайла в модель для транскрипции.

```
from openai import AzureOpenAI
from pathlib import Path

# Create an AzureOpenAI client
client = AzureOpenAI(
    azure_endpoint=YOUR_FOUNDRY_ENDPOINT,
    api_key=YOUR_FOUNDRY_KEY,
    api_version="2025-03-01-preview"
)

# Get the audio file
file_path = Path("speech.mp3")
audio_file = open(file_path, "rb")

# Use the model to transcribe the audio file
transcription = client.audio.transcriptions.create(
    model=YOUR_MODEL_DEPLOYMENT,
    file=audio_file,
    response_format="text"
)

print(transcription)
```

---

# Краткое изложение.
Или: Резюме.
(В зависимости от контекста)

В этом модуле вы узнали об искусственных интеллектах, способных к обработке и генерации речи, а также о том, как можно использовать платформу Microsoft Foundry для создания решений на основе генеративного ИИ, которые:

- Преобразовать речь в текст.
- Преобразовать текст в речь.

Совет.

Для получения дополнительной информации о моделях, поддерживающих распознавание и синтез речи, в платформе Microsoft Foundry, пожалуйста, обратитесь к разделу: [ссылка].[Audio models](/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure?pivots=azure-openai#audio-models&azure-portal=true)в документации Microsoft Foundry.
