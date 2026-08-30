# Разработайте голосового агента Azure с использованием технологии "живого" взаимодействия в платформе Microsoft Foundry.

# Введение.

Приложения с голосовым управлением меняют то, как мы взаимодействуем с технологиями, и этот модуль поможет вам создать интерактивные голосовые решения в режиме реального времени, используя передовые API и инструменты. API "Voice Live" в сервисе Azure Speech, доступный в Foundry Tools, представляет собой решение, обеспечивающее взаимодействие между голосом и голосом с низкой задержкой и высоким качеством для голосовых агентов. Этот API предназначен для разработчиков, стремящихся создать масштабируемые и эффективные решения на основе голосового управления, поскольку он устраняет необходимость ручной настройки нескольких компонентов.

После завершения этого модуля вы сможете:

- Реализуйте API Azure Speech Voice Live для обеспечения взаимодействия в реальном времени и двусторонней связи.
- Настройте и запустите сеанс работы агента.
- Разрабатывайте и управляйте обработчиками событий для создания динамичного и интерактивного пользовательского интерфейса.
- Используйте программу Voice Live в сочетании с агентом Foundry.

Примечание.

Мы понимаем, что разные люди предпочитают учиться разными способами. Вы можете пройти этот модуль в формате видео или изучить материалы в виде текста и изображений. Текст содержит более подробную информацию, чем видео, поэтому в некоторых случаях вам может быть полезно использовать его как дополнительный материал к видеолекции.

---

# Изучите API Azure Voice Live.

API сервиса "Голос" позволяет разработчикам создавать приложения с голосовым управлением, обеспечивающие двустороннюю связь в режиме реального времени. Этот раздел посвящен архитектуре, настройке и реализации данного API.

## Основные особенности API сервиса Voice Live.

API сервиса "Voice" обеспечивает взаимодействие в режиме реального времени с использованием соединений WebSocket. Он поддерживает расширенные функции, такие как распознавание речи, синтез речи, потоковая передача аватаров и обработка аудио.

- События в формате JSON используются для управления диалогами, потоками аудио и ответами.
- События классифицируются на события, инициируемые клиентом (передаваемые от клиента к серверу), и события, инициируемые сервером (передаваемые от сервера к клиенту).

Основные особенности включают в себя:

- Обработка аудио в реальном времени с поддержкой различных форматов, таких как PCM16 и G.711.
- Расширенные возможности голосового сопровождения, включая голоса OpenAI и пользовательские голоса, созданные в сервисе Azure.
- Интеграция аватаров с использованием технологии WebRTC для видео и анимации.
- Встроенные функции шумоподавления и устранения эха.

Примечание.

API Voice Live оптимизирован для работы с ресурсами платформы Microsoft Foundry. Мы рекомендуем использовать ресурсы Microsoft Foundry, чтобы получить полный доступ ко всем функциям и обеспечить наилучшую интеграцию с этой платформой.

Для получения списка поддерживаемых моделей и регионов, посетите сайт: [ссылка на сайт]. [Voice Live API overview](/en-us/azure/ai-services/speech-service/voice-live#supported-models-and-regions).

## Подключитесь к API сервиса Voice Live.

API для работы с сервисом "Голос" поддерживает два метода аутентификации: Microsoft Entra (без использования ключей) и ключ API. Для ресурса Microsoft Foundry используется токено-ориентированная аутентификация в рамках системы Microsoft Entra. Полученный токен аутентификации применяется с помощью... `Bearer` токен с... `Authorization` Заголовок.

Для использования рекомендуемого метода аутентификации без ключа с использованием Microsoft Entra ID необходимо назначить роль **"Пользователь служб Cognitive Services"** вашей учетной записи пользователя или управляемому удостоверению. Для этого вам нужно сгенерировать токен, используя Azure CLI или SDK Azure. Токен должен быть сгенерирован с помощью... `https://ai.azure.com/.default` объем (чего-либо), или наследие. `https://cognitiveservices.azure.com/.default` область применения. Используйте этот токен в... `Authorization` заголовок запроса на установление соединения WebSocket, имеющий следующий формат: `Bearer <token>`.

Для получения доступа к ресурсам, ключ API можно предоставить одним из двух способов. Вы можете использовать... `api-key` заголовок "Connection" в предварительном соединении. Эта опция недоступна в среде веб-браузера. Или вы можете использовать... `api-key` Параметры строки запроса передаются в URI запроса. Параметры строки запроса шифруются при использовании протоколов HTTPS/WSS.

Примечание.

The. `api-key` Заголовок "Connection" в предварительном соединении (перед установлением полноценного соединения) недоступен в среде веб-браузера.

### Точка доступа WebSocket.

Конечная точка, которую следует использовать, зависит от того, как вы хотите получить доступ к своим ресурсам. Вы можете получать доступ к ресурсам через подключение к проекту Foundry при развертывании агента, или напрямую через подключение к модели.

- **Связь с проектом:** Конечная точка – это... `wss://<your-ai-foundry-resource-name>.services.ai.azure.com/voice-live/realtime?api-version=2025-10-01`
- **Подключение модели:** Конечная точка - это... `wss://<your-ai-foundry-resource-name>.cognitiveservices.azure.com/voice-live/realtime?api-version=2025-10-01`.

Конечная точка одинакова для всех моделей. Единственное отличие заключается в том, что требуется... `model` параметр запроса, или, при использовании сервиса "Agent", параметр, используемый этим сервисом. `agent_id` и `project_id` параметры.

## События API сервиса Voice Live.

События, генерируемые клиентом и сервером, обеспечивают взаимодействие и контроль в рамках API для работы с голосовыми потоками в реальном времени. К основным событиям, генерируемым клиентом, относятся:

- `session.update`Изменить параметры сессии.
- `input_audio_buffer.append`Добавить аудиоданные в буфер.
- `response.create`Генерировать ответы с использованием модели машинного обучения.

События, происходящие на сервере, предоставляют информацию об отзывах и текущем состоянии системы.

- `session.updated`Подтвердите изменения в настройках сессии.
- `response.done`Укажите, что процесс генерации ответа завершен.
- `conversation.item.created`Уведомлять о добавлении нового элемента в диалог.

Для получения полного списка событий, связанных с клиент-серверным взаимодействием, посетите сайт: [адрес сайта]. [Voice live API Reference](/en-us/azure/ai-services/speech-service/voice-live-api-reference).

Примечание.

Правильная обработка событий обеспечивает бесперебойное взаимодействие между клиентом и сервером.

### Настройте параметры сессии для API голосовой связи в режиме реального времени.

Часто, первое событие, отправляемое вызывающей стороной в рамках нового сеанса работы с API голосовой связи, это... `session.update` Это событие управляет широким спектром входных и выходных операций. Параметры сессии можно динамически обновлять с помощью... `session.update` Событие. Разработчики могут настраивать типы голоса, режимы работы, определение начала и окончания речи, а также форматы аудио.

Пример конфигурации:

```
{
  "type": "session.update",
  "session": {
    "modalities": ["text", "audio"],
    "voice": {
      "type": "openai",
      "name": "alloy"
    },
    "instructions": "You are a helpful assistant. Be concise and friendly.",
    "input_audio_format": "pcm16",
    "output_audio_format": "pcm16",
    "input_audio_sampling_rate": 24000,
    "turn_detection": {
      "type": "azure_semantic_vad",
      "threshold": 0.5,
      "prefix_padding_ms": 300,
      "silence_duration_ms": 500
    },
    "temperature": 0.8,
    "max_response_output_tokens": "inf"
  }
}
```

Совет.

Используйте семантический анализ речи (VAD) от Azure для интеллектуального определения начала и окончания реплик и улучшения хода беседы.

### Реализуйте обработку звука в режиме реального времени с помощью API Voice Live.

Обработка звука в реальном времени является ключевой функцией API для работы с голосовыми данными. Разработчики могут добавлять, сохранять и очищать буферы аудиоданных, используя специальные события клиентской стороны.

- **Добавить аудио:** Добавьте аудиоданные во входной буфер.
- **Обработка аудио:** Обработать буфер звука для транскрибации или генерации ответа.
- **Очистка аудиопотока:** Удалить данные аудио из буфера.

Функции подавления шума и устранения эха можно настроить для улучшения качества звука. Например:

```
{
  "type": "session.update",
  "session": {
    "input_audio_noise_reduction": {
      "type": "azure_deep_noise_suppression"
    },
    "input_audio_echo_cancellation": {
      "type": "server_echo_cancellation"
    }
  }
}
```

Примечание.

Подавление шума повышает точность определения начала и окончания речи (VAD) и улучшает производительность модели за счет фильтрации входного аудиосигнала.

### Интегрируйте потоковое вещание аватаров с использованием API сервиса Voice Live.

API сервиса Voice поддерживает потоковую передачу аватаров на основе технологии WebRTC для интерактивных приложений. Разработчики могут настроить параметры видео, анимации и деформации формы объектов.

- Используйте... `session.avatar.connect` мероприятие, предназначенное для представления клиенту предложения по разработке стратегии развития (SDP).
- Настройте разрешение видео, битрейт и параметры кодека.
- Определите параметры анимации, такие как морфологии (blend shapes) и артикуляционные элементы (виземы).

Пример конфигурации:

```
{
  "type": "session.avatar.connect",
  "client_sdp": "<client_sdp>"
}
```

Совет.

Используйте настройки видео высокого разрешения для улучшения качества изображения во время взаимодействия с аватаром.

---

# Создайте агента Voice Live.

Вы можете создать и запустить приложение для использования технологии Voice Live с помощью агента Microsoft Foundry. Использование агентов с поддержкой Voice Live предоставляет следующие преимущества по сравнению с прямым подключением к модели:

- Агенты включают в себя инструкции и настройки непосредственно в самом агенте, а не указывают их в коде сессии.
- Агенты поддерживают сложную логику и поведение, что упрощает управление и обновление сценариев диалогов без необходимости изменения клиентского кода.
- Подход с использованием агентов упрощает процесс интеграции. Идентификатор агента используется для установления соединения, а все необходимые настройки обрабатываются автоматически, что снижает необходимость ручной конфигурации в коде.
- Разделение логики работы от реализации голосового интерфейса обеспечивает лучшую поддерживаемость и масштабируемость в тех случаях, когда требуется несколько вариантов взаимодействия с пользователем или различные варианты бизнес-логики.

## Создайте голосового агента в тестовой среде для разработки агентов.

При разработке агента в портале Microsoft Foundry вы можете активировать режим голосового взаимодействия, чтобы легко интегрировать функцию "Voice Live" в своего агента и протестировать ее в тестовой среде.

![Скриншот интерфейса для тестирования агента с включенным голосовым режимом.](https://learn.microsoft.com/en-us/training/wwl-data-ai/develop-voice-live-agent/media/voice-mode.png)

После включения голосового режима вы можете использовать панель "Настройки" для активации функций работы с голосом в реальном времени, включая:

- **Язык:** Язык, на котором агент общается и понимает речь.
- **Расширенные настройки:**
  - Настройки обнаружения голосовой активности (VAD) для определения моментов прерываний и окончания речи.
  - Улучшение качества звука для подавления фонового шума и повышения общей четкости звучания.
- **Голос:** Конкретный голос, используемый агентом, а также расширенные настройки голоса для управления тоном и скоростью речи.
- **Временный ответ:** Агент может автоматически генерировать речь в процессе ожидания ответа от модели.
- **Аватар:** Использование визуального представления (аватара) для отображения пользователя или системы.

## Создайте голосового агента с помощью кода.

Если вы предпочитаете создавать своего агента с помощью кода, вы можете использовать соответствующий SDK для разработки агентов Foundry (например, SDK для Python), чтобы создать агента и добавить метаданные Voice Live в его описание.

```
import os
import json
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
from azure.ai.projects.models import PromptAgentDefinition

load_dotenv()

# Setup client
project_client = AIProjectClient(
    "PROJECT_ENDPOINT",
    credential=DefaultAzureCredential(),
)

# Define Voice Live session settings
voice_live_config = {
    "session": {
        "voice": {
            "name": "en-US-Ava:DragonHDLatestNeural",
            "type": "azure-standard",
            "temperature": 0.8
        },
        "input_audio_transcription": {
            "model": "azure-speech"
        },
        "turn_detection": {
            "type": "azure_semantic_vad",
            "end_of_utterance_detection": {
                "model": "semantic_detection_v1_multilingual"
            }
        },
        "input_audio_noise_reduction": {"type": "azure_deep_noise_suppression"},
        "input_audio_echo_cancellation": {"type": "server_echo_cancellation"}
    }
}

# Create agent with Voice Live configuration in metadata
agent = project_client.agents.create_version(
    agent_name="AGENT_NAME",
    definition=PromptAgentDefinition(
        model="MODEL_DEPLOYMENT_NAME",
        instructions="You are a helpful assistant.",
    ),
    metadata=chunk_config(json.dumps(voice_live_config))
)
print(f"Agent created: {agent.name} (version {agent.version})")

# Helper function for Voice Live configuration chunking (to handle 512-char metadata limit)
def chunk_config(config_json: str, limit: int = 512) -> dict:
    """Split config into chunked metadata entries."""
    metadata = {"microsoft.voice-live.configuration": config_json[:limit]}
    remaining = config_json[limit:]
    chunk_num = 1
    while remaining:
        metadata[f"microsoft.voice-live.configuration.{chunk_num}"] = remaining[:limit]
        remaining = remaining[limit:]
        chunk_num += 1
    return metadata
```

## Используйте своего агента в клиентском приложении.

Для использования вашего агента необходимо создать клиентское приложение, которое:

1. Подключается к агенту.
2. Настраивает параметры ввода и вывода звука.
3. Инициирует сеанс работы с сервисом Voice в режиме реального времени.
4. Контролирует работу аудиосистем и отслеживает их активность.
5. Обрабатывает события, такие как ввод речи пользователем и ответы от системы.

Хотя вы можете реализовать эти задачи, используя любую из функций, доступных в API, рекомендуемый подход для клиентских приложений Voice Live заключается в следующем:

- Используйте аутентификацию через Microsoft Entra ID для подключения к агенту в проекте Microsoft Foundry.
- Реализуйте собственный класс **VoiceAssistant**, который содержит строго типизированную конфигурацию агента, определяет функции для настройки и запуска голосовой сессии в реальном времени, а также обрабатывает голосовые события.
- Реализуйте собственный класс `AudioProcessor`, который будет обрабатывать ввод и вывод звука через аудиоустройства.

Следующий пример демонстрирует минимальную реализацию этой схемы на языке Python (с использованием библиотеки *PyAudio* для работы со звуком).

```
import os
import asyncio
import base64
import queue
from dotenv import load_dotenv
import pyaudio
from azure.identity.aio import AzureCliCredential
from azure.ai.voicelive.aio import connect
from azure.ai.voicelive.models import (
    InputAudioFormat,
    Modality,
    OutputAudioFormat,
    RequestSession,
    ServerEventType,
    AudioNoiseReduction,
    AudioEchoCancellation,
    AzureSemanticVadMultilingual
) 

# Main program entry point
def main():
    """Main entry point."""

    try:
        # Get required configuration from environment variables
        load_dotenv()
        endpoint = os.environ.get("AZURE_VOICELIVE_ENDPOINT")
        agent_name = os.environ.get("AZURE_VOICELIVE_AGENT_ID")
        project_name = os.environ.get("AZURE_VOICELIVE_PROJECT_NAME")

        # Create credential for authentication
        credential = AzureCliCredential()

        # Create and start the voice assistant
        assistant = VoiceAssistant(
            endpoint=endpoint,
            credential=credential,
            agent_name=agent_name,
            project_name=project_name
        )

        # Run the assistant
        try:
            asyncio.run(assistant.start())
        except KeyboardInterrupt:
            # Exit if the user enters CTRL+C
            print("\nGoodbye!")

    except Exception as e:
        print(f"An error occurred: {e}")

# VoiceAssistant class
class VoiceAssistant:
    """Main voice assistant - coordinates the conversation"""

    def __init__(self, endpoint, credential, agent_name, project_name):
        self.endpoint = endpoint
        self.credential = credential

        # Agent configuration
        self.agent_config = {
            "agent_name": agent_name,
            "project_name": project_name
        }

    async def start(self):
        """Start the voice assistant."""

        try:
            # Connect the agent
            async with connect(
                endpoint=self.endpoint,
                credential=self.credential,
                api_version="2026-01-01-preview",
                agent_config=self.agent_config
            ) as connection:
                self.connection = connection

                # Initialize audio processor
                self.audio_processor = AudioProcessor(connection)

                # Configure the session
                await self.setup_session()

                # Start audio I/O 
                self.audio_processor.start_playback()
                print("\nVoice session started...")
                print("Press Ctrl+C to exit\n")

                # Process events
                await self.process_events()

        finally:
            if hasattr(self, 'audio_processor'):
                self.audio_processor.shutdown()

    async def setup_session(self):
        """Configure the session with audio settings."""

        session_config = RequestSession(
            # Enable both text and audio
            modalities=[Modality.TEXT, Modality.AUDIO],

            # Audio format (16-bit PCM at 24kHz)
            input_audio_format=InputAudioFormat.PCM16,
            output_audio_format=OutputAudioFormat.PCM16,

            # Voice activity detection (when to detect speech)
            turn_detection=AzureSemanticVadMultilingual(),

            # Prevent echo from speaker feedback
            input_audio_echo_cancellation=AudioEchoCancellation(),

            # Reduce background noise
            input_audio_noise_reduction=AudioNoiseReduction(type="azure_deep_noise_suppression")
        )

        await self.connection.session.update(session=session_config)
        print("Session configured")

    async def process_events(self):
        """Process events from the VoiceLive service."""

        # Listen for events from the service
        async for event in self.connection:
            await self.handle_event(event)

    async def handle_event(self, event):
        """Handle different event types from the service."""

        # Session is ready - start capturing audio
        if event.type == ServerEventType.SESSION_UPDATED:
            print(f"Connected to agent: {event.session.agent.name}")
            self.audio_processor.start_capture()

        # User speech was transcribed
        elif event.type == ServerEventType.CONVERSATION_ITEM_INPUT_AUDIO_TRANSCRIPTION_COMPLETED:
            print(f'You: {event.get("transcript", "")}')

        # Agent is responding with audio transcript
        elif event.type == ServerEventType.RESPONSE_AUDIO_TRANSCRIPT_DONE:
            print(f'Agent: {event.get("transcript", "")}')

        # User started speaking (interrupt any playing audio)
        elif event.type == ServerEventType.INPUT_AUDIO_BUFFER_SPEECH_STARTED:
            self.audio_processor.clear_playback_queue()
            print("Listening...")

        # User stopped speaking
        elif event.type == ServerEventType.INPUT_AUDIO_BUFFER_SPEECH_STOPPED:
            print("Thinking...")

        # Receiving audio response chunks
        elif event.type == ServerEventType.RESPONSE_AUDIO_DELTA:
            self.audio_processor.queue_audio(event.delta)

        # Audio response complete
        elif event.type == ServerEventType.RESPONSE_AUDIO_DONE:
            print("Response complete\n")

        # Handle errors
        elif event.type == ServerEventType.ERROR:
            print(f"Error: {event.error.message}")

# AudioProcessor class
class AudioProcessor:
    """Handles audio input (microphone) and output (speakers) """

    def __init__(self, connection):
        self.connection = connection
        self.audio = pyaudio.PyAudio()

        # Audio settings: 24kHz, 16-bit PCM, mono
        self.format = pyaudio.paInt16
        self.channels = 1
        self.rate = 24000
        self.chunk_size = 1200  # 50ms chunks

        # Streams for input and output
        self.input_stream = None
        self.output_stream = None
        self.playback_queue = queue.Queue()

    def start_capture(self):
        """Start capturing audio from the microphone."""

        def capture_callback(in_data, frame_count, time_info, status):
            # Convert audio to base64 and send to VoiceLive
            audio_base64 = base64.b64encode(in_data).decode("utf-8")
            asyncio.run_coroutine_threadsafe(
                self.connection.input_audio_buffer.append(audio=audio_base64),
                self.loop
            )
            return (None, pyaudio.paContinue)

        # Store event loop for use in callback thread
        self.loop = asyncio.get_event_loop()

        self.input_stream = self.audio.open(
            format=self.format,
            channels=self.channels,
            rate=self.rate,
            input=True,
            frames_per_buffer=self.chunk_size,
            stream_callback=capture_callback
        )
        print("Microphone started")

    def start_playback(self):
        """Start audio playback system."""

        remaining = bytes()

        def playback_callback(in_data, frame_count, time_info, status):
            nonlocal remaining

            # Calculate bytes needed
            bytes_needed = frame_count * pyaudio.get_sample_size(pyaudio.paInt16)
            output = remaining[:bytes_needed]
            remaining = remaining[bytes_needed:]

            # Get more audio from queue if needed
            while len(output) < bytes_needed:
                try:
                    audio_data = self.playback_queue.get_nowait()
                    if audio_data is None:  # End signal
                        break
                    output += audio_data
                except queue.Empty:
                    # Pad with silence if no audio available
                    output += bytes(bytes_needed - len(output))
                    break

            # Keep any extra for next callback
            if len(output) > bytes_needed:
                remaining = output[bytes_needed:]
                output = output[:bytes_needed]

            return (output, pyaudio.paContinue)

        self.output_stream = self.audio.open(
            format=self.format,
            channels=self.channels,
            rate=self.rate,
            output=True,
            frames_per_buffer=self.chunk_size,
            stream_callback=playback_callback
        )
        print("Speakers ready")

    def queue_audio(self, audio_data):
        """Add audio data to the playback queue."""
        self.playback_queue.put(audio_data)

    def clear_playback_queue(self):
        """Clear any pending audio (used when user interrupts)."""
        while not self.playback_queue.empty():
            try:
                self.playback_queue.get_nowait()
            except queue.Empty:
                break

    def shutdown(self):
        """Clean up audio resources."""
        if self.input_stream:
            self.input_stream.stop_stream()
            self.input_stream.close()

        if self.output_stream:
            self.playback_queue.put(None)  # Signal end
            self.output_stream.stop_stream()
            self.output_stream.close()

        self.audio.terminate()
        print("Audio stopped")

if __name__ == "__main__":
    main()
```

---

# Изучите библиотеку клиентских приложений для работы с технологией синтеза речи на основе искусственного интеллекта (AI Voice) в языке программирования Python.

Клиентская библиотека Azure AI Voice Live для языка Python предоставляет инструмент для работы в режиме реального времени с сервисом Azure AI Voice Live, позволяющий осуществлять преобразование речи в речь. Она устанавливает сессию WebSocket для передачи аудиосигнала с микрофона на сервер и получения от сервера данных, необходимых для интерактивного взаимодействия.

Важно.

Начиная с версии 1.0.0, этот SDK поддерживает только асинхронные операции. Синхронный интерфейс устарел, и разработка сосредоточена исключительно на асинхронных подходах. Все примеры и образцы кода используют синтаксис async/await.

В этом разделе вы узнаете, как использовать SDK для реализации аутентификации и обработки событий. Вы также увидите простой пример создания сессии. Для получения полной информации о пакете Voice Live посетите сайт: [адрес сайта]. [voice live Package reference](/en-us/python/api/azure-ai-voicelive/azure.ai.voicelive?view=azure-python).

## Реализуйте аутентификацию.

Вы можете реализовать аутентификацию с использованием ключа API или токена Microsoft Entra ID. Следующий пример кода демонстрирует реализацию с использованием ключа API. Предполагается, что переменные окружения настроены следующим образом: `.env` или непосредственно в вашей рабочей среде.

```
import asyncio
from azure.core.credentials import AzureKeyCredential
from azure.ai.voicelive import connect

async def main():
    async with connect(
        endpoint="your-endpoint",
        credential=AzureKeyCredential("your-api-key"),
        model="gpt-4o"
    ) as connection:
        # Your async code here
        pass

asyncio.run(main())
```

Для производственных сред рекомендуется использовать аутентификацию Microsoft Entra. Следующий пример кода демонстрирует реализацию... `DefaultAzureCredential` для аутентификации:

```
import asyncio
from azure.identity.aio import DefaultAzureCredential
from azure.ai.voicelive import connect

async def main():
    credential = DefaultAzureCredential()

    async with connect(
        endpoint="your-endpoint",
        credential=credential,
        model="gpt-4o"
    ) as connection:
        # Your async code here
        pass

asyncio.run(main())
```

## Обработка событий.

Правильная обработка событий обеспечивает более плавное взаимодействие между клиентом и агентом. Например, при возникновении ситуации, когда пользователь прерывает речь агента, необходимо немедленно прекратить воспроизведение звука агентом на стороне клиента. В противном случае клиент продолжит воспроизводить последний ответ агента до тех пор, пока прерывание не будет обработано в API, что приведет к тому, что агент будет "перебивать" пользователя.

Следующий пример кода демонстрирует некоторые базовые методы обработки событий:

```
async for event in connection:
    if event.type == ServerEventType.SESSION_UPDATED:
        print(f"Session ready: {event.session.id}")
        # Start audio capture

    elif event.type == ServerEventType.INPUT_AUDIO_BUFFER_SPEECH_STARTED:
        print("User started speaking")
        # Stop playback and cancel any current response

    elif event.type == ServerEventType.RESPONSE_AUDIO_DELTA:
        # Play the audio chunk
        audio_bytes = event.delta

    elif event.type == ServerEventType.ERROR:
        print(f"Error: {event.error.message}")
```

## Минимальный пример.

Следующий пример кода демонстрирует процесс аутентификации к API и настройки сессии.

```
import asyncio
from azure.core.credentials import AzureKeyCredential
from azure.ai.voicelive.aio import connect
from azure.ai.voicelive.models import (
    RequestSession, Modality, InputAudioFormat, OutputAudioFormat, ServerVad, ServerEventType
)

API_KEY = "your-api-key"
ENDPOINT = "your-endpoint"
MODEL = "gpt-4o"

async def main():
    async with connect(
        endpoint=ENDPOINT,
        credential=AzureKeyCredential(API_KEY),
        model=MODEL,
    ) as conn:
        session = RequestSession(
            modalities=[Modality.TEXT, Modality.AUDIO],
            instructions="You are a helpful assistant.",
            input_audio_format=InputAudioFormat.PCM16,
            output_audio_format=OutputAudioFormat.PCM16,
            turn_detection=ServerVad(
                threshold=0.5, 
                prefix_padding_ms=300, 
                silence_duration_ms=500
            ),
        )
        await conn.session.update(session=session)

        # Process events
        async for evt in conn:
            print(f"Event: {evt.type}")
            if evt.type == ServerEventType.RESPONSE_DONE:
                break

asyncio.run(main())
```

---

# Краткое изложение.
Или: Резюме.
(В зависимости от контекста)

В этом модуле вы узнали о возможностях API Voice Live, включая установление соединений через WebSocket, распознавание речи, синтез речи и потоковую передачу аватаров. Вы также изучили сервис Azure AI Voice Live для создания приложений, обеспечивающих взаимодействие в режиме реального времени с использованием Python, включая настройку клиентской библиотеки и управление сеансами. Кроме того, вы узнали, как реализовать обработчики событий на Python для динамической обработки данных и обработки аудио в реальном времени. Наконец, вы разработали веб-приложение на основе Python с использованием фреймворка Flask, интегрировали его с ресурсами Azure и протестировали приложение.

## Дополнительная информация для ознакомления.
Или:
Для более подробного изучения.
Или:
Рекомендуемая литература.
(The best option depends on the context.)

- [What is the Speech service?](/en-us/azure/cognitive-services/speech-service/)
- [How to customize voice live input and output](/en-us/azure/ai-services/speech-service/voice-live-how-to-customize)
