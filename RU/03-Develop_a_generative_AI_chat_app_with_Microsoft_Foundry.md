# Разработайте приложение для чата с использованием генеративного искусственного интеллекта совместно с платформой Microsoft Foundry.

# Введение.

Разработчики, создающие решения на основе искусственного интеллекта с использованием платформы Microsoft Foundry, должны работать с комбинацией сервисов и программных фреймворков. При разработке приложения для чат-бота с использованием генеративного ИИ вы можете выбирать из различных вариантов, чтобы написать код, необходимый для создания эффективных приложений ИИ в Azure.

В этом модуле вы узнаете, как выбирать подходящие параметры конечной точки, SDK, методы аутентификации и API для чатов, чтобы создать приложение для общения с использованием генеративного искусственного интеллекта.

Примечание.

Мы понимаем, что разные люди предпочитают учиться разными способами. Вы можете пройти этот модуль в формате видео или изучить материалы в виде текста и изображений. Текст содержит более подробную информацию, чем видео, поэтому в некоторых случаях вам может быть полезно использовать его как дополнительный материал к видеолекции.

---

# Исследуйте площадку с помощью этой модели.

Прежде чем приступать к написанию кода для создания приложения чат-бота на основе генеративного искусственного интеллекта, полезно изучить возможности вашего проекта через портал Foundry. Этот портал предоставляет интерактивные инструменты для тестирования моделей и генерации образцов кода, которые можно использовать в качестве отправной точки для разработки ваших приложений.

[![Screenshot of the Model playground in Microsoft Foundry portal.](https://learn.microsoft.com/en-us/training/wwl-data-ai/foundry-sdk/media/foundry-playground.png)](../../wwl-data-ai/foundry-sdk/media/foundry-playground.png#lightbox)

## Изучение возможностей платформы "Model".

В портале Foundry раздел "**Интерактивная среда для работы с моделями**" предоставляет интерактивную платформу для тестирования моделей, прежде чем вы начнете писать код. Вы можете получить к нему доступ, выбрав пункт "**Интерактивная среда для работы с моделями**" в левом меню навигации.

Эта игровая площадка позволяет вам:

- Отправляйте запросы развернутым моделям и получайте ответы в режиме реального времени.
- Настройте параметры, такие как температура и максимальное количество токенов.
- Добавьте системные сообщения для настройки поведения модели.
- Попробуйте различные модели и конфигурации.

Эта среда разработки без использования кода помогает вам понять, как модели реагируют на различные входные данные и настройки, что упрощает проектирование вашего приложения.

## Генерация примеров кода.

Одной из самых полезных функций среды разработки "Model" является кнопка "**Код**", расположенная в панели чата. В любой момент во время работы вы можете нажать эту кнопку, чтобы увидеть примеры кода, которые позволят вам воспроизвести сеанс общения в вашем приложении.

Представленные примеры кода включают в себя варианты для:

- **API** – Использование API ответов (Responses API) или другого API, например, ChatCompletions.
- **Язык программирования:** Выберите предпочитаемый язык программирования.
- **SDK (Software Development Kit):** Выберите SDK, для которого вы хотите увидеть пример использования.

Эти образцы уже содержат информацию о вашем проекте, имени развернутой модели и текущих настройках. Они служат готовой отправной точкой для разработки вашего приложения.

Вы можете скопировать этот код непосредственно в свою среду разработки и изменить его в соответствии с вашими потребностями.

## От детской площадки до программирования.

Обычно процесс разработки приложения с использованием искусственного интеллекта на платформе Microsoft Foundry выглядит следующим образом:

1. **Поэкспериментируйте в тестовой среде:** Протестируйте различные запросы, настройте параметры и найдите оптимальные решения.
2. **Создание примеров кода:** Используйте вкладку "Код", чтобы получить примеры использования SDK.
3. **Разработайте свое приложение:** Возьмите сгенерированный код и адаптируйте его под свои конкретные нужды.
4. **Повторяйте и совершенствуйте:** Возвращайтесь к тестовой среде, чтобы проверить новые идеи, а затем обновляйте свой код.

Этот подход позволяет быстро создавать прототипы и проверять свои идеи, прежде чем тратить время на их разработку.

В следующем разделе вы узнаете об доступных конечных точках и SDK, которые можно использовать для разработки клиентского приложения.

---

# Выберите конечную точку и SDK.

Платформа Microsoft Foundry предоставляет гибкие возможности для разработки чат-приложений на основе генеративного искусственного интеллекта. Прежде чем начать разработку, важно понимать доступные варианты и то, как выбрать наиболее подходящий из них. При разработке приложения следует учитывать следующее:

- **Точки доступа:** Проекты Microsoft Foundry предоставляют две точки доступа, которые можно использовать для подключения к проектам и получения доступа к их ресурсам, таким как развернутые модели, из клиентских приложений. Каждый проект имеет как *точку доступа к проекту*, так и *точку доступа к Azure OpenAI*.
- **Клиентская библиотека разработки (SDK)**: В зависимости от выбранного вами интерфейса, вы можете использовать либо *Microsoft Foundry SDK*, либо *OpenAI SDK* для разработки приложения чат-бота с использованием генеративного искусственного интеллекта. Обе библиотеки поддерживают клиентский объект, совместимый с API OpenAI, который может отправлять запросы к моделям, но существуют некоторые различия в конкретных функциональных возможностях, доступных в каждой библиотеке.
- **Аутентификация:** В зависимости от выбранного компонента и SDK, существует несколько способов, с помощью которых клиентское приложение может быть аутентифицировано системой Foundry для получения доступа к ресурсам. Как правило, производственные приложения должны использовать аутентификацию через *Microsoft Entra ID*, которая требует, чтобы приложение работало в контексте определенной учетной записи; однако, в некоторых случаях можно также использовать аутентификацию на основе *ключей* или *токенов*.
- **API для чатов:** Клиентская библиотека OpenAI поддерживает два API для работы с чатами: *ChatCompletions* и *Responses*. Хотя API *Responses* рекомендуется использовать в большинстве новых проектов, API *ChatCompletions* хорошо зарекомендовал себя и совместим со многими моделями и платформами генеративного искусственного интеллекта.

Давайте начнем с рассмотрения доступных конечных точек (endpoints), клиентских библиотек (SDK) и методов аутентификации. Позже мы более подробно изучим API для работы с ответами (Responses) и завершением чатов (ChatCompletions).

## Использование SDK Foundry с указанием конечной точки проекта.

SDK Microsoft Foundry предоставляет программный доступ к ресурсам в ваших проектах через REST API и клиентские библиотеки, специфичные для различных языков программирования, включая:

- [Azure AI Projects for Python](https://pypi.org/project/azure-ai-projects?azure-portal=true)
- [Azure AI Projects for Microsoft .NET](https://www.nuget.org/packages/Azure.AI.Projects?azure-portal=true)
- [Azure AI Projects for JavaScript](https://www.npmjs.com/package/@azure/ai-projects?azure-portal=true)

Примечание.

Этот модуль содержит примеры кода на языке Python для решения распространенных задач. Вы можете обратиться к документации SDK, специфичной для вашего языка программирования, чтобы найти аналогичный код на предпочитаемом вами языке. Каждый SDK разрабатывается и поддерживается независимо, поэтому некоторые функции могут находиться на разных стадиях реализации.

### Установка SDK (программного разработчика).

Чтобы использовать библиотеку Azure AI Projects в Python, установите пакет **azure-ai-projects** из PyPI, а также необходимые дополнительные пакеты:

```
pip install azure-ai-projects azure-identity openai
```

Примечание.

При разработке приложения для чата с использованием SDK Foundry необходимо также импортировать пакет SDK OpenAI, поскольку функциональность клиентской части чата в SDK Foundry основана на SDK OpenAI.

### Подключение к конечной точке проекта.

Каждый проект в Foundry имеет уникальный адрес завершения, который можно найти на странице "Обзор" проекта в портале Foundry. [https://ai.azure.com](https://ai.azure.com?azure-portal=true).

Адрес конечной точки проекта имеет следующий формат:

```
https://{resource-name}.services.ai.azure.com/api/projects/<project-name>
```

Используйте этот интерфейс для создания объекта **AIProjectClient**:

```
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

project_endpoint = "https://{resource-name}.services.ai.azure.com/api/projects/<project-name>"
project_client = AIProjectClient(
    credential=DefaultAzureCredential(),
    endpoint=project_endpoint
)
```

Примечание.

Код использует стандартные учетные данные Azure для аутентификации. Для включения этой функции необходимо установить пакет **azure-identity** (который был указан в предыдущей команде установки).

Совет.

Для успешного доступа к проекту код должен выполняться в аутентифицированной сессии Azure. Например, вы можете использовать интерфейс командной строки Azure (Azure CLI). `az login` команда, требующая авторизации перед выполнением кода.

Заказчик проекта:`AIProjectClient`Этот инструмент предоставляет доступ к функциям платформы Foundry, которые не имеют аналогов в OpenAI. Используйте клиент проекта для:

- Получить информацию о подключениях к ресурсам.
- Получить доступ к настройкам проекта.
- Включить отслеживание.
- Управление наборами данных и индексами.

### Создание клиента для чата.

Чтобы взаимодействовать с моделью в вашем проекте Foundry, вам потребуется объект клиента, совместимый с OpenAI. Вы можете получить такой объект, используя метод `get_openai_client()` объекта клиента проекта, например, следующим образом:

```
openai_client = project_client.get_openai_client(api_version="2024-10-21")
```

Затем вы можете использовать этот объект клиента чата для отправки запросов к моделям и получения ответов.

## Использование SDK OpenAI с конечной точкой Azure OpenAI.

SDK от OpenAI – это официальная клиентская библиотека для работы с API OpenAI. Она обрабатывает HTTP-запросы, аутентификацию, повторные попытки и разбор ответов. SDK работает с моделями, размещенными на платформе OpenAI, с развертываниями Azure OpenAI и с моделями Foundry, используя единые принципы.

### Установка SDK (программного разработчика).

Чтобы использовать библиотеку OpenAI в Python, установите пакет **openai** из репозитория PyPI, а также необходимые дополнительные пакеты.

```
pip install openai azure-identity
```

Примечание.

Пакет *azure-identity* необходим, если вы планируете использовать аутентификацию на основе токенов для подключения к сервису, используя учетные данные Microsoft Entra ID.

### Подключение к конечной точке Azure OpenAI.

Каждый проект Foundry включает в себя конечную точку Azure OpenAI, которую вы можете найти на странице "Обзор" проекта в портале Foundry. [https://ai.azure.com](https://ai.azure.com?azure-portal=true).

Адрес конечной точки Azure OpenAI имеет следующий формат:

```
https://{resource-name}.openai.azure.com/openai/v1
```

Создайте клиент OpenAI, указав ваш конечный пункт (endpoint) и учетные данные Azure.

```
from openai import OpenAI
from azure.identity import DefaultAzureCredential, get_bearer_token_provider

token_provider = get_bearer_token_provider(
    DefaultAzureCredential(), "https://ai.azure.com/.default"
)

openai_client = OpenAI(  
  base_url = "https://{resource-name}.openai.azure.com/openai/v1/",  
  api_key=token_provider,
)
```

В дополнение к использованию Microsoft Entra ID (рекомендуемый способ), вы можете использовать ключ API или переменные окружения для аутентификации.

**Аутентификация с использованием ключа API:**

```
import os
from openai import OpenAI

openai_client = OpenAI(
    api_key=os.getenv("AZURE_OPENAI_API_KEY"),
    base_url="https://{resource-name}.openai.azure.com/openai/v1/"
)
```

Важно.

Используйте ключи API с осторожностью. Храните их в безопасном месте, например, в хранилище ключей Azure, и никогда не включайте их напрямую в свой код.

**Переменные окружения:**

Если вы установите... `OPENAI_BASE_URL` и `OPENAI_API_KEY` переменные окружения: клиент использует их автоматически.

```
from openai import OpenAI

openai_client = OpenAI()  # Uses environment variables
```

Независимо от выбранного вами способа аутентификации, клиент OpenAI выполняет операции вывода данных из моделей. Используйте его для:

- Создание ответов с помощью API для работы с текстовыми данными.
- Генерация текста и изображений.
- Получение доступа к моделям Foundry напрямую (модели, отличные от моделей Azure OpenAI).

Использование объекта клиента *AzureOpenAI*.

В общем случае для общения с моделями через конечную точку Azure OpenAI v1 следует использовать объект клиента **OpenAI**. Однако, у вас также есть возможность создать объект клиента **AzureOpenAI**, если вам необходимо использовать функциональность конкретной версии API Azure OpenAI. Для создания объекта клиента **AzureOpenAI** необходимо указать версию API и конечную точку Azure, например, следующим образом:

```
import os
from openai import AzureOpenAI

openai_client = AzureOpenAI(
    azure_endpoint = "https://{resource-name}.openai.azure.com"
    api_key=os.getenv("AZURE_OPENAI_KEY"),  
    api_version="2024-10-21",
)
```

## Выбор между SDK от Foundry и SDK от OpenAI.

Платформа Microsoft Foundry поддерживает два подхода к разработке приложений на основе искусственного интеллекта. Каждый из них предназначен для решения различных задач, и понимание того, когда следует использовать тот или иной подход, поможет вам создать оптимальное решение.

### Когда следует использовать SDK Foundry?

Используйте SDK Foundry, когда вашему приложению требуются функции, специфичные для платформы Foundry.

- **Сервис Foundry Agent:** платформа для создания и управления искусственными интеллектуальными агентами.
- Рабочие процессы вызова и утверждения использования инструментов.
- Оценка работы облачных сервисов для тестирования и проверки ответов, генерируемых системами искусственного интеллекта.
- **Отслеживание и мониторинг** для анализа поведения приложений.
- **Прямые подключения к моделям Foundry** (модели, доступные через каталог моделей и не являющиеся частью Azure OpenAI).
- Функции, связанные с метаданными проекта, связями между элементами и управлением проектом.

Компания Microsoft рекомендует использовать SDK Foundry при разработке приложений, использующих агентов, системы оценки или функции, специфичные для платформы Foundry.

### Когда следует использовать SDK от OpenAI?

Используйте SDK от OpenAI, когда вам требуется максимальная совместимость с API OpenAI.

- Полная совместимость с API OpenAI для существующего кода и инструментов.
- Возможность переноса между развертываниями OpenAI и Azure OpenAI.
- API для работы с текстовыми запросами, ответами и изображениями в чате.
- Минимальная зависимость от концепций, специфичных для конкретной платформы разработки.

SDK от OpenAI идеально подходит для задач обработки данных с использованием моделей, когда вам необходимо, чтобы существующий код OpenAI работал с минимальными изменениями. Однако, этот подход не предоставляет функции, специфичные для платформы Foundry, такие как агенты или системы оценки.

Microsoft Foundry предоставляет вам гибкость в разработке приложений на основе искусственного интеллекта. Используйте SDK Foundry вместе с... `AIProjectClient` Когда вам требуются функции на уровне проекта, такие как агенты, оценки, отслеживание и связи, используйте SDK от OpenAI. Если вам нужно простое использование моделей с максимальной совместимостью с OpenAI, используйте SDK OpenAI. Оба SDK работают с вашим проектом Foundry, поэтому вы можете комбинировать их по мере необходимости в своих приложениях. Вы также можете использовать оба SDK вместе в одном приложении: SDK Foundry для функций проекта и SDK OpenAI для работы с моделями.

---

# Создавайте ответы с помощью API для работы с ответами.

API *Responses* от OpenAI объединяет возможности двух ранее отдельных API (*ChatCompletions* и *Assistants*) в единый интерфейс. Он обеспечивает генерацию ответов с учетом контекста и нескольких этапов взаимодействия, что делает его идеальным для приложений искусственного интеллекта, предназначенных для ведения диалогов. Вы можете получить доступ к API *Responses* через клиент, совместимый с OpenAI, используя либо SDK Foundry, либо SDK OpenAI.

## Понимание API для работы с ответами.

API "Ответы" предлагает ряд преимуществ по сравнению с традиционными методами генерации текста в чатах:

- **Разговоры с сохранением контекста:** Поддерживает контекст беседы на протяжении нескольких этапов взаимодействия.
- **Единый опыт взаимодействия:** Объединяет возможности чат-ботов и интерфейса Assistants API.
- **Прямая работа с моделями Foundry:** Поддерживает работу с моделями, размещенными непосредственно в платформе Microsoft Foundry, а не только с моделями Azure OpenAI.
- **Простая интеграция:** Доступ через клиент, совместимый с OpenAI.

Примечание.

API *Responses* является рекомендуемым способом для генерации ответов искусственного интеллекта в приложениях Microsoft Foundry. Он заменяет более старый API *ChatCompletions* в большинстве случаев.

## Генерация простого ответа.

С помощью клиента, совместимого с OpenAI, вы можете генерировать ответы, используя метод **responses.create()**:

```
# Generate a response using the OpenAI-compatible client
response = openai_client.responses.create(
    model="gpt-4.1",  # Your model deployment name
    input="What is Microsoft Foundry?"
)

# Display the response
print(response.output_text)
```

Параметр **входных данных** принимает текстовую строку, содержащую ваш запрос. Модель генерирует ответ на основе этих входных данных.

## Понимание структуры ответов.

Объект ответа содержит несколько полезных свойств:

- **output_text**: Сгенерированный текстовый ответ.
- **id:** Уникальный идентификатор для этого ответа.
- **статус:** Статус ответа (например, "выполнено").
- **использование:** Информация об использовании токенов (входные, выходные и общее количество).
- **модель:** Модель, используемая для генерации ответа.

Вы можете получить доступ к этим свойствам, чтобы эффективно обрабатывать ответы:

```
response = openai_client.responses.create(
    model="gpt-4.1",
    input="Explain machine learning in simple terms."
)

print(f"Response: {response.output_text}")
print(f"Response ID: {response.id}")
print(f"Tokens used: {response.usage.total_tokens}")
print(f"Status: {response.status}")
```

### Добавление инструкций.

В дополнение к данным, которые вы предоставляете, можно также задавать *инструкции* (часто называемые *системными подсказками*), чтобы направлять работу модели:

```
response = client.responses.create(
    model="gpt-4.1",
    instructions="You are a helpful AI assistant that answers questions clearly and concisely.",
    input="Explain neural networks."
)

print(response.output_text)
```

## Управление процессом генерации ответов.

Вы можете управлять процессом генерации ответов с помощью дополнительных параметров:

```
response = openai_client.responses.create(
    model="gpt-4.1",
    instructions="You are a helpful AI assistant that answers questions clearly and concisely.",
    input="Write a creative story about AI.",
    temperature=0.8,  # Higher temperature for more creativity
    max_output_tokens=200  # Limit response length
)

print(response.output_text)
```

- **Температура:** Управляет степенью случайности (от 0.0 до 2.0). Более высокие значения делают вывод более творческим и разнообразным.
- **max_output_tokens**: Ограничивает максимальное количество токенов в ответе.
- `top_p`: Альтернативный параметр для управления случайностью, используемый вместо параметра "температура".

## Работа с моделями, созданными непосредственно в Foundry.

При использовании SDK Foundry или клиента Azure OpenAI для подключения к определенному адресу (*endpoint*) проекта, API Responses работает как с моделями Azure OpenAI, так и с моделями, предоставляемыми напрямую компанией Foundry (например, Microsoft Phi, DeepSeek или другие модели, размещенные непосредственно в Microsoft Foundry).

```
# Using a Foundry direct model
response = openai_client.responses.create(
    model="microsoft-phi-4",  # Example Foundry direct model
    instructions="You are a helpful AI assistant that answers questions clearly and concisely.",
    input="What are the benefits of small language models?"
)

print(response.output_text)
```

## Создание интерактивных диалоговых интерфейсов.

Для более сложных сценариев диалога вы можете предоставлять системные инструкции и создавать многоступенчатые разговоры:

```
# First turn in the conversation
response1 = openai_client.responses.create(
    model="gpt-4.1",
    instructions="You are a helpful AI assistant that explains technology concepts clearly.",
    input="What is machine learning?"
)

print("Assistant:", response1.output_text)

# Continue the conversation
response2 = openai_client.responses.create(
    model="gpt-4.1",
    instructions="You are a helpful AI assistant that explains technology concepts clearly.",
    input="Can you give me an example?",
    previous_response_id=response1.id
)

print("Assistant:", response2.output_text)
```

В реальности, реализация, скорее всего, будет построена в виде цикла, в котором пользователь сможет интерактивно вводить сообщения, основываясь на каждом полученном ответе от модели.

```
# Track responses
last_response_id = None

# Loop until the user wants to quit
print("Assistant: Enter a prompt (or type 'quit' to exit)")
while True:
    input_text = input('\nYou: ')
    if input_text.lower() == "quit":
        print("Assistant: Goodbye!")
        break

    # Get a response
    response = openai_client.responses.create(
                model=model_name,
                instructions="You are a helpful AI assistant that explains technology concepts clearly.",
                input=input_text,
                previous_response_id=last_response_id
    )
    assistant_text = response.output_text
    print("\nAssistant:", assistant_text)
    last_response_id = response.id
```

Результат работы этого примера выглядит примерно так:

```
Assistant: Enter a prompt (or type 'quit' to exit)

You: What is machine learning?

Assistant: Machine learning is a type of artificial intelligence (AI) that enables computers to learn from data and improve their performance over time without being explicitly programmed. It involves training algorithms on large datasets to recognize patterns, make predictions, or take actions based on those patterns. This allows machines to become more accurate and efficient in their tasks as they are exposed to more data.

You: Can you give me an example?

Assistant: Certainly! Let's look at a simple example of supervised learning—predicting house prices based on features like size, location, and number of rooms.
Imagine you want to build a machine learning model that can predict the price of a house based on various factors.
...
    { the example provided in the model response may be extensive}
...

You: quit

Assistant: Goodbye!
```

При каждом новом вводе данных пользователем, информация, отправляемая модели, включает в себя системное сообщение "*Инструкции*", ввод пользователя и *предыдущий* ответ, полученный от модели. Таким образом, новый ввод данных основывается на контексте, предоставленном ответом, сгенерированным моделью для предыдущего ввода.

### Альтернатива: Ручное объединение диалогов.

Вы можете управлять диалогами вручную, самостоятельно формируя историю сообщений. Такой подход дает вам больше контроля над тем, какая информация будет включена в контекст:

```
try:
    # Start with initial message
    conversation_history = [
        {
            "type": "message",
            "role": "user",
            "content": "What is machine learning?"
        }
    ]

    # First response
    response1 = openai_client.responses.create(
        model="gpt-4.1",
        input=conversation_history
    )

    print("Assistant:", response1.output_text)

    # Add assistant response to history
    conversation_history += response1.output

    # Add new user message
    conversation_history.append({
        "type": "message",
        "role": "user", 
        "content": "Can you give me an example?"
    })

    # Second response with full history
    response2 = openai_client.responses.create(
        model="gpt-4.1",
        input=conversation_history
    )

    print("Assistant:", response2.output_text)

except Exception as ex:
    print(f"Error: {ex}")
```

Этот ручной метод полезен в тех случаях, когда вам необходимо:

- Настройте, какие сообщения будут включены в контекст.
- Реализуйте механизм сокращения количества сообщений в диалоге для управления лимитом токенов.
- Сохраняйте и восстанавливайте историю переписки из базы данных.

### Получение конкретных предыдущих ответов.

API для работы с ответами сохраняет историю ответов, что позволяет вам получать доступ к предыдущим ответам.

```
try:   

    # Retrieve a previous response
    response_id = "resp_67cb61fa3a448190bcf2c42d96f0d1a8"  # Example ID
    previous_response = openai_client.responses.retrieve(response_id)

    print(f"Previous response: {previous_response.output_text}")

except Exception as ex:
    print(f"Error: {ex}")
```

### Соображения, касающиеся размера контекстного окна.

Параметр **previous_response_id** связывает ответы друг с другом, обеспечивая сохранение контекста разговора при выполнении нескольких запросов к API.

Важно отметить, что сохранение истории разговоров может увеличить расход токенов. В рамках одного сеанса активное контекстное окно может включать в себя:

- Инструкции по эксплуатации (инструкции, правила безопасности).
- Ваш текущий запрос.
- История переписки (предыдущие сообщения пользователя и ответы ассистента).
- Схемы инструментов (функции, спецификации OpenAPI, инструменты управления кластерами и т.д.).
- Результаты работы инструмента (результаты поиска, вывод интерпретатора кода, файлы).
- Извлеченная информация или документы (из памяти системы, из базы знаний с использованием технологии RAG, путем поиска в файлах).

Все эти данные объединяются, разбиваются на отдельные элементы (токены) и отправляются в модель вместе при каждом запросе. SDK помогает вам управлять состоянием, но он не делает использование токенов автоматически более экономичным.

## Создание адаптивных приложений для чата.

Время генерации ответов может варьироваться в зависимости от различных факторов, таких как используемая модель, размер контекстного окна и объем запроса. Пользователи могут испытывать раздражение, если приложение кажется "зависшим" во время ожидания ответа, поэтому важно учитывать скорость работы приложения при его разработке.

### Потоковая передача ответов.

Для длинных ответов можно использовать потоковую передачу, чтобы получать результат постепенно – таким образом, пользователь видит частично сформированные ответы по мере их поступления.

```
stream = openai_client.responses.create(
    model="gpt-4.1",
    input="Write a short story about a robot learning to paint.",
    stream=True
)

for event in stream:
    print(event, end="", flush=True)
```

Если вы отслеживаете историю разговора во время потоковой передачи данных, вы можете получить идентификатор ответа в конце сеанса, например, следующим образом:

```
stream = openai_client.responses.create(
    model="gpt-4.1",
    input="Write a short story about a robot learning to paint.",
    stream=True
)
for event in stream:
                if event.type == "response.output_text.delta":
                    print(event.delta, end="")
                elif event.type == "response.completed":
                    response_id = event.response.id
```

### Асинхронное использование.

Для приложений, требующих высокой производительности, можно использовать асинхронный клиент, который позволяет выполнять вызовы API без блокировки. Асинхронное использование идеально подходит для длительных запросов или когда необходимо обрабатывать несколько запросов одновременно, не блокируя работу вашего приложения. Для его использования необходимо импортировать... `AsyncOpenAI` вместо того чтобы; вместо. `OpenAI` и использовать. `await` при каждом вызове API:

```
import asyncio
from openai import AsyncOpenAI

client = AsyncOpenAI(
    base_url="https://<resource-name>.openai.azure.com/openai/v1/",
    api_key=token_provider,
)

async def main():
    response = await client.responses.create(
        model="gpt-4.1",
        input="Explain quantum computing briefly."
    )
    print(response.output_text)

asyncio.run(main())
```

Асинхронная потоковая передача работает аналогичным образом:

```
async def stream_response():
    stream = await client.responses.create(
        model="gpt-4.1",
        input="Write a haiku about coding.",
        stream=True
    )

    async for event in stream:
        print(event, end="", flush=True)

asyncio.run(stream_response())
```

Используя API "Ответы" через SDK Microsoft Foundry, вы можете создавать сложные приложения с использованием технологий искусственного интеллекта, которые поддерживают контекст, работают с различными моделями и обеспечивают удобный пользовательский интерфейс.

---

# Создавайте ответы с помощью API Chat Completions.

API *ChatCompletions* от OpenAI широко используется в различных моделях и платформах генеративного искусственного интеллекта. Хотя для разработки новых проектов рекомендуется использовать API *Responses*, вероятно, вам придется столкнуться со случаями, когда API *ChatCompletions* будет полезен для поддержки кода или обеспечения совместимости между различными платформами.

## Отправка запроса.

API *ChatCompletions* использует коллекции объектов *сообщений* в формате JSON для представления запросов:

```
completion = openai_client.chat.completions.create(
    model="gpt-4o",  # Your model deployment name
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "When was Microsoft founded?"}
    ]
)

print(completion.choices[0].message.content)
```

## Сохранение контекста разговора.

В отличие от API *Responses*, API *ChatCompletions* не предоставляет функцию отслеживания состояния диалога. Для сохранения контекста разговора необходимо самостоятельно реализовать код, который будет фиксировать предыдущие запросы и ответы.

```
# Initial messages
conversation_messages=[
    {
        "role": "system",
        "content": "You are a helpful AI assistant that answers questions and provides information."
    }
]

# Add the first user message
conversation_messages.append(
    {"role": "user",
    "content": "When was Microsoft founded?"}
)

# Get a completion
completion = openai_client.chat.completions.create(
    model="gpt-4o",
    messages=conversation_messages
)
assistant_message = completion.choices[0].message.content
print("Assistant:", assistant_text)

# Append the response to the conversation
conversation_messages.append(
    {"role": "assistant", "content": assistant_text}
)

# Add the next user message
conversation_messages.append(
    {"role": "user",
    "content": "Who founded it?"}
)

# Get a completion
completion = openai_client.chat.completions.create(
    model="gpt-4o",
    messages=conversation_messages
)
assistant_message = completion.choices[0].message.content
print("Assistant:", assistant_text)

# and so on...
```

В реальном приложении взаимодействие, скорее всего, будет реализовано в цикле, например, следующим образом:

```
# Initial messages
conversation_messages=[
    {
        "role": "system",
        "content": "You are a helpful AI assistant that answers questions and provides information."
    }
]

# Loop until the user wants to quit
print("Assistant: Enter a prompt (or type 'quit' to exit)")
while True:
    input_text = input('\nYou: ')
    if input_text.lower() == "quit":
        print("Assistant: Goodbye!")
        break

    # Add the user message
    conversation_messages.append(
        {"role": "user",
        "content": input_text}
    )

    # Get a completion
    completion = openai_client.chat.completions.create(
        model="gpt-4o",
        messages=conversation_messages
    )
    assistant_message = completion.choices[0].message.content
    print("\nAssistant:", assistant_message)

    # Append the response to the conversation
    conversation_messages.append(
        {"role": "assistant", "content": assistant_message}
    )
```

Результат работы этого примера выглядит примерно так:

```
Assistant: Enter a prompt (or type 'quit' to exit)

You: When was Microsoft founded?

Assistant: Microsoft was founded on April 4, 1975 in Albuquerque, New Mexico, USA.

You: Who founded it?

Assistant: Microsoft was founded by Bill Gates and Paul Allen.

You: quit

Assistant: Goodbye!
```

Каждый новый запрос пользователя и полученный ответ добавляются в ход беседы, и вся история разговора передается при каждом следующем взаимодействии.

Хотя API *ChatCompletions* не обладает таким же широким набором функций, как API *Responses*, он хорошо зарекомендовал себя в экосистеме моделей генеративного искусственного интеллекта, поэтому полезно с ним ознакомиться.

---

# Краткое изложение.
Или: Резюме.
(В зависимости от контекста)

Платформа Microsoft Foundry предоставляет два комплекта средств разработки (SDK) для создания приложений с использованием искусственного интеллекта. SDK Foundry обеспечивает доступ к функциям, работающим на уровне проекта, таким как агенты, оценки, отслеживание и соединения. SDK OpenAI позволяет выполнять вывод данных моделей с полной совместимостью с API OpenAI.

## Основные выводы.
Или:
Ключевые моменты.
Или:
Главное, что следует запомнить.
(The best option depends on the context.)

В этом модуле вы узнали, как:

- Используйте SDK Foundry вместе с конечной точкой проекта Foundry для доступа к настройкам проекта, подключениям, отслеживанию и наборам данных.
- Используйте SDK OpenAI с проектом Foundry и конечными точками Azure OpenAI для выполнения вычислений моделей.
- Используйте API *Responses* и *ChatCompletions* для **генерации ответов** и управления диалогами.

## Дополнительная информация.
Для дальнейшего изучения.
Рекомендуемая литература.
Более подробная информация.
(В зависимости от контекста)

Для получения дополнительной информации по темам, рассматриваемым в этом модуле, ознакомьтесь со следующими ресурсами:

- [Microsoft Foundry SDK overview](/en-us/azure/ai-foundry/how-to/develop/sdk-overview)
- [Responses API documentation](/en-us/azure/ai-foundry/openai/how-to/responses)
- [Microsoft Foundry Discord](https://aka.ms/azureaifoundry/discord)
- [Microsoft Foundry Developer Forum](https://aka.ms/azureaifoundry/forum)
