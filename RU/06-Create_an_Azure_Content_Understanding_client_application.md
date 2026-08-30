# Создайте клиентское приложение для работы с сервисом "Понимание контента" в Azure.

# Введение.

Azure Content Understanding – это многофункциональный сервис, который упрощает создание интеллектуальных инструментов анализа данных, способных извлекать информацию из различных форматов контента, включая документы, изображения, аудиофайлы и видео.

Совет.

Чтобы узнать, как создавать анализаторы Azure Content Understanding, ознакомьтесь с [далее следует ссылка или указание на ресурс].[Create a multimodal analysis solution with Azure Content Understanding](/en-us/training/modules/analyze-content-ai/)Модуль.

Вы можете разрабатывать клиентские приложения, использующие инструменты анализа данных Azure Content Understanding, с помощью SDK для Python или через REST API, что является основной темой этого модуля.

![Схема работы сервиса Azure Content Understanding и клиентского приложения.](https://learn.microsoft.com/en-us/training/wwl-data-ai/analyze-content-ai-api/media/content-understanding.png)

В этом модуле вы узнаете, как писать код, который использует Python SDK и REST API для отправки файла с данными в систему анализа и обработки результатов.

---

# Подготовьтесь к использованию API для анализа и понимания контента с помощью искусственного интеллекта.

Прежде чем вы сможете использовать API понимания контента Azure, вам необходимо иметь ресурс Microsoft Foundry в вашей подписке Azure. Вы можете создать этот ресурс следующими способами:

- Создайте ресурс **Microsoft Foundry** в портале Azure.
- Создайте проект **Microsoft Foundry**, который по умолчанию включает в себя ресурс Microsoft Foundry.

Совет.

Создание проекта в Microsoft Foundry позволяет использовать визуальные инструменты для создания и управления схемами и аналитическими компонентами Azure Content Understanding.

После того как вы настроили ресурс Microsoft Foundry, вам потребуется следующая информация для подключения к API Azure Content Understanding из клиентского приложения:

- Ресурс "endpoint" в платформе Microsoft Foundry.
- Один из ключей API, связанных с этим конечным пунктом (endpoint).

Эти значения можно получить в портале Azure, как показано на следующем изображении:

![Скриншот настроек Microsoft Foundry в портале Azure.](https://learn.microsoft.com/en-us/training/wwl-data-ai/analyze-content-ai-api/media/azure-portal.png)

Если вы работаете в рамках проекта Microsoft Foundry, вы можете найти конечную точку (endpoint) и ключ для соответствующего ресурса Foundry на главной странице портала проекта Foundry.

При работе над проектом Microsoft Foundry вы также можете писать код, который использует SDK Microsoft Foundry для подключения к проекту с использованием аутентификации Microsoft Entra ID и получения информации о подключении к ресурсу Microsoft Foundry.

## Установка пакета разработки (SDK) для Python.

Для использования Python SDK для анализа контента необходимо установить... `azure-ai-contentunderstanding` пакет; упаковка.

```
pip install azure-ai-contentunderstanding
```

Примечание.

Для использования Python SDK требуется версия Python 3.9 или более поздняя. Вы также можете использовать REST API напрямую из любого языка программирования, поддерживающего HTTP-запросы.

Важно.

Перед использованием API для анализа контента необходимо настроить стандартные модели развертывания для вашего ресурса Microsoft Foundry. Для работы API анализа контента требуются... `GPT-4.1`, `GPT-4.1-mini`и. `text-embedding-3-large` развертывание моделей. Вы можете настроить эти параметры в портале Azure или с помощью API. Для получения дополнительной информации, см. раздел "**".[Set up model deployments](/en-us/azure/ai-services/content-understanding/how-to/migration-preview-to-ga#prerequisites)**.

Совет.

Чтобы узнать больше о программировании с использованием SDK Microsoft Foundry, ознакомьтесь со следующим материалом:[Develop an AI app with the Microsoft Foundry SDK](/en-us/training/modules/ai-foundry-sdk/)Модуль.

---

# Создайте инструмент для анализа понимания контента.

В большинстве случаев рекомендуется создавать и тестировать анализаторы с помощью визуального интерфейса в среде Content Understanding Studio. Однако, в некоторых случаях вам может потребоваться создать анализатор, отправив в API определение схемы в формате JSON для нужных полей контента.

## Определение схемы для анализатора.

Анализаторы основаны на схемах, которые определяют поля, которые вы хотите извлечь или создать из файла с данными. В своей основе схема представляет собой набор полей, который может быть задан в документе формата JSON, как показано в этом примере определения анализатора:

```
{
    "description": "Simple business card",
    "baseAnalyzerId": "prebuilt-document",
    "config": {
        "returnDetails": true
    },
    "fieldSchema": {
        "fields": {
            "ContactName": {
                "type": "string",
                "method": "extract",
                "description": "Name on business card"
            },
            "EmailAddress": {
                "type": "string",
                "method": "extract",
                "description": "Email address on business card"
            }
        }
    },
    "models": {
        "completion": "gpt-4.1",
        "embedding": "text-embedding-3-large"
    }
}
```

Этот пример схемы пользовательского анализатора основан на предварительно настроенном анализаторе документов и описывает два поля, которые обычно можно найти на визитной карточке: *ContactName* (Имя контакта) и *EmailAddress* (Адрес электронной почты). Оба поля определены как строковые типы данных, и предполагается, что они будут *извлечены* из документа (то есть, ожидается, что строковые значения будут присутствовать в документе, чтобы их можно было "прочитать", а не быть полями, которые могут быть *сгенерированы* путем анализа информации о документе). `models` Этот параметр указывает генеративные модели, которые анализатор использует для обработки данных.

Примечание.

Этот пример намеренно упрощен и содержит минимальный объем информации, необходимый для создания работающего анализатора. В реальности схема, скорее всего, будет включать больше полей различных типов, а определение анализатора – больше настроек. Более того, в JSON-файле может даже содержаться пример документа. Подробнее см. [ссылка]. [Azure Content Understanding API documentation](/en-us/rest/api/contentunderstanding/content-analyzers/create-or-replace) для получения более подробной информации.

## Использование Python SDK для создания анализатора.

После того как вы определили параметры анализатора, вы можете использовать SDK для Python, чтобы создать этот анализатор. `ContentUnderstandingClient` Этот класс предоставляет... `begin_create_analyzer` метод, который автоматизирует процесс асинхронного создания объектов.

```
from azure.ai.contentunderstanding import ContentUnderstandingClient
from azure.core.credentials import AzureKeyCredential

# Authenticate the client
endpoint = "<YOUR_ENDPOINT>"
credential = AzureKeyCredential("<YOUR_API_KEY>")
client = ContentUnderstandingClient(endpoint=endpoint, credential=credential)

# Define the analyzer
analyzer_name = "business_card_analyser"
analyzer_definition = {
    "description": "Simple business card",
    "baseAnalyzerId": "prebuilt-document",
    "config": {"returnDetails": True},
    "fieldSchema": {
        "fields": {
            "ContactName": {
                "type": "string",
                "method": "extract",
                "description": "Name on business card"
            },
            "EmailAddress": {
                "type": "string",
                "method": "extract",
                "description": "Email address on business card"
            }
        }
    },
    "models": {
        "completion": "gpt-4.1",
        "embedding": "text-embedding-3-large"
    }
}

# Create the analyzer and wait for completion
poller = client.begin_create_analyzer(analyzer_name, body=analyzer_definition)
result = poller.result()
print(f"Analyzer created: {result.analyzer_id}")
```

## Использование REST API для создания анализатора.

В качестве альтернативы, вы можете использовать REST API напрямую. Данные в формате JSON передаются следующим образом: `PUT` Отправьте запрос к указанному адресу с использованием ключа API в заголовке запроса, чтобы инициировать процесс создания анализатора.

Ответ от... `PUT` Запрос содержит в заголовке информацию об **операции и местоположении**, которая предоставляет URL-адрес для обратного вызова (*callback*), который можно использовать для проверки статуса запроса путем отправки. `GET` запрос.

Следующий код на языке Python отправляет запрос для создания анализатора, основываясь на содержимом файла с именем *card.json* (который, как предполагается, содержит определение в формате JSON, описанное ранее).

```
import json
import requests

# Get the business card schema
with open("card.json", "r") as file:
    schema_json = json.load(file)

# Use a PUT request to submit the schema for a new analyzer
analyzer_name = "business_card_analyser"

headers = {
    "Ocp-Apim-Subscription-Key": "<YOUR_API_KEY>",
    "Content-Type": "application/json"}

url = f"{<YOUR_ENDPOINT>}/contentunderstanding/analyzers/{analyzer_name}?api-version=2025-11-01"

response = requests.put(url, headers=headers, data=json.dumps(schema_json))

# Get the response and extract the ID assigned to the operation
callback_url = response.headers["Operation-Location"]

# Use a GET request to check the status of the operation
result_response = requests.get(callback_url, headers=headers)

# Keep polling until the operation is complete
status = result_response.json().get("status")
while status == "Running":
    result_response = requests.get(callback_url, headers=headers)
    status = result_response.json().get("status")

print("Done!")
```

---

# Проанализировать контент.
Или:
Анализ контента.
(В зависимости от контекста)

Для анализа содержимого файла можно использовать API Azure Content Understanding, чтобы отправить его на сервер обработки. Вы можете указать содержимое в виде URL-адреса (для файлов, размещенных в доступном по сети месте) или загрузить двоичные данные файла напрямую (например, документ .pdf, изображение .png, аудиофайл .mp3 или видеофайл .mp4). Запрос на анализ включает в себя указание используемого анализатора.

Анализ – это асинхронная операция. После отправки запроса вы получаете идентификатор операции, который можно использовать для проверки ее статуса и получения результатов после завершения работы.

Например, предположим, что вы хотите использовать анализатор визиток, о котором мы говорили ранее, чтобы извлечь имя и адрес электронной почты из следующего отсканированного изображения визитки:

![Фотография визитной карточки Джона Смита.](https://learn.microsoft.com/en-us/training/wwl-data-ai/analyze-content-ai-api/media/business-card.png)

## Использование SDK для Python.

SDK для Python, предназначенный для анализа контента.`azure-ai-contentunderstanding`(предлагает) предоставляет. `ContentUnderstandingClient` Этот класс упрощает взаимодействие с сервисом. SDK (Software Development Kit) обрабатывает аутентификацию, форматирование запросов и автоматическую проверку статуса асинхронных операций.

Следующий код на языке Python использует SDK для отправки данных визитной карточки на анализ и получения результатов:

```
from azure.ai.contentunderstanding import ContentUnderstandingClient
from azure.ai.contentunderstanding.models import AnalysisInput
from azure.core.credentials import AzureKeyCredential

# Authenticate the client
endpoint = "<YOUR_ENDPOINT>"
credential = AzureKeyCredential("<YOUR_API_KEY>")
client = ContentUnderstandingClient(endpoint=endpoint, credential=credential)

# Analyze the business card using the custom analyzer
analyzer_name = "business_card_analyser"
poller = client.begin_analyze(
    analyzer_id=analyzer_name,
    inputs=[AnalysisInput(url="https://host.com/business-card.png")]
)

# Wait for the operation to complete and get the results
result = poller.result()

# Extract field values from the results
content = result.contents[0]
if content.fields:
    for field_name, field_data in content.fields.items():
        if field_data.type == "string":
            print(f"{field_name}: {field_data.value}")
```

Совет.

SDK (наборы средств разработки). `begin_analyze` Этот метод возвращает объект "построителя" (poller object). Вызов... `.result()` Модуль опроса автоматически выполняет процесс опроса до завершения операции, поэтому вам не нужно писать собственный цикл опроса.

## Использование REST API.

Вы также можете отправлять запросы на анализ напрямую, используя REST API. Ваше клиентское приложение отправляет HTTP-запросы к компоненту "Content Understanding" вашего ресурса Microsoft Foundry, передавая ключ API в заголовке запроса.

Следующий код на языке Python отправляет запрос на анализ, используя URL-адрес, а затем периодически проверяет состояние сервиса до тех пор, пока операция не будет завершена и результаты не будут возвращены.

```
import json
import requests

## Use a POST request to submit the file URL to the analyzer
analyzer_name = "business_card_analyser"

headers = {
        "Ocp-Apim-Subscription-Key": "<YOUR_API_KEY>",
        "Content-Type": "application/json"}

url = f"{<YOUR_ENDPOINT>}/contentunderstanding/analyzers/{analyzer_name}:analyze?api-version=2025-11-01"

request_body = {
    "inputs": [
        {
            "url": "https://host.com/business-card.png"
        }
    ]
}

response = requests.post(url, headers=headers, json=request_body)

# Get the response and extract the ID assigned to the analysis operation
response_json = response.json()
id_value = response_json.get("id")

# Use a GET request to check the status of the analysis operation
result_url = f"{<YOUR_ENDPOINT>}/contentunderstanding/analyzerResults/{id_value}?api-version=2025-11-01"

result_response = requests.get(result_url, headers=headers)

# Keep polling until the analysis is complete
status = result_response.json().get("status")
while status == "Running":
        result_response = requests.get(result_url, headers=headers)
        status = result_response.json().get("status")

# Get the analysis results
if status == "Succeeded":
    result_json = result_response.json()
```

Примечание.

Вы можете указать URL-адрес для расположения файла с содержимым, как показано здесь. Чтобы отправить данные двоичного файла напрямую, используйте... `analyzeBinary` вместо этого была проведена операция.

## Обработка результатов анализа.

Результаты зависят от следующих факторов:

- Тип контента, предназначенного для анализа данным инструментом (например, документ, видео, изображение или аудиозапись).
- Схема анализатора.
- Содержимое файла, который был проанализирован.

Например, результаты работы анализатора визиток, основанного на работе с документами, при анализе визитки, описанной ранее, могут включать в себя:

- Извлеченные данные.
- Структура документа, полученная в результате оптического распознавания символов (OCR), включающая информацию о расположении строк текста, отдельных слов и абзацев на каждой странице.

### Использование SDK для Python.

При использовании SDK... `AnalysisResult` Этот объект обеспечивает доступ к результатам с указанием их типов данных. `contents` Объект данных содержит список элементов контента, каждый из которых имеет поля, текст в формате Markdown и метаданные. Следующий код демонстрирует, как извлечь значения строковых полей:

```
# (continued from previous SDK code example)

content = result.contents[0]
if content.fields:
    for field_name, field_data in content.fields.items():
        if field_data.type == "string":
            print(f"{field_name}: {field_data.value}")
```

### Использование REST API.

При использовании REST API ответ представляет собой данные в формате JSON, которые ваше приложение должно обработать. Вот полный пример ответа в формате JSON для анализа визитной карточки:

```
{
    "id": "00000000-0000-0000-0000-a00000000000",
    "status": "Succeeded",
    "result": {
        "analyzerId": "biz_card_analyser_2",
        "apiVersion": "2025-11-01",
        "createdAt": "2025-05-16T03:51:46Z",
        "warnings": [],
        "contents": [
            {
                "markdown": "John Smith\nEmail: john@contoso.com\n",
                "fields": {
                    "ContactName": {
                        "type": "string",
                        "valueString": "John Smith",
                        "spans": [
                            {
                                "offset": 0,
                                "length": 10
                            }
                        ],
                        "confidence": 0.994,
                        "source": "D(1,69,234,333,234,333,283,69,283)"
                    },
                    "EmailAddress": {
                        "type": "string",
                        "valueString": "john@contoso.com",
                        "spans": [
                            {
                                "offset": 18,
                                "length": 16
                            }
                        ],
                        "confidence": 0.998,
                        "source": "D(1,179,309,458,309,458,341,179,341)"
                    }
                },
                "kind": "document",
                "startPageNumber": 1,
                "endPageNumber": 1,
                "unit": "pixel",
                "pages": [
                    {
                        "pageNumber": 1,
                        "angle": 0.03410444,
                        "width": 1000,
                        "height": 620,
                        "spans": [
                            {
                                "offset": 0,
                                "length": 35
                            }
                        ],
                        "words": [
                            {
                                "content": "John",
                                "span": {
                                    "offset": 0,
                                    "length": 4
                                },
                                "confidence": 0.992,
                                "source": "D(1,69,234,181,234,180,283,69,283)"
                            },
                            {
                                "content": "Smith",
                                "span": {
                                    "offset": 5,
                                    "length": 5
                                },
                                "confidence": 0.998,
                                "source": "D(1,200,234,333,234,333,282,200,283)"
                            },
                            {
                                "content": "Email:",
                                "span": {
                                    "offset": 11,
                                    "length": 6
                                },
                                "confidence": 0.995,
                                "source": "D(1,75,310,165,309,165,340,75,340)"
                            },
                            {
                                "content": "john@contoso.com",
                                "span": {
                                    "offset": 18,
                                    "length": 16
                                },
                                "confidence": 0.977,
                                "source": "D(1,179,309,458,311,458,340,179,341)"
                            }
                        ],
                        "lines": [
                            {
                                "content": "John Smith",
                                "source": "D(1,69,234,333,233,333,282,69,282)",
                                "span": {
                                    "offset": 0,
                                    "length": 10
                                }
                            },
                            {
                                "content": "Email: john@contoso.com",
                                "source": "D(1,75,309,458,309,458,340,75,340)",
                                "span": {
                                    "offset": 11,
                                    "length": 23
                                }
                            }
                        ]
                    }
                ],
                "paragraphs": [
                    {
                        "content": "John Smith Email: john@contoso.com",
                        "source": "D(1,69,233,458,233,458,340,69,340)",
                        "span": {
                            "offset": 0,
                            "length": 34
                        }
                    }
                ],
                "sections": [
                    {
                        "span": {
                            "offset": 0,
                            "length": 34
                        },
                        "elements": [
                            "/paragraphs/0"
                        ]
                    }
                ]
            }
        ]
    }
}
```

Ваше приложение обычно должно обрабатывать данные в формате JSON для получения значений полей. Например, следующий код на языке Python извлекает все строковые значения:

```
# (continued from previous code example)

# Iterate through the fields and extract the names and type-specific values
contents = result_json["result"]["contents"]
for content in contents:
    if "fields" in content:
        fields = content["fields"]
        for field_name, field_data in fields.items():
            if field_data['type'] == "string":
                print(f"{field_name}: {field_data['valueString']}")
```

Результат работы этого кода представлен здесь:

```
ContactName: John Smith
EmailAddress: john@contoso.com
```

---

# Краткое изложение.
Или: Резюме.
(В зависимости от контекста)

Сервис Azure Content Understanding – это многомодальный ИИ-сервис, который позволяет извлекать информацию из различных типов контента.  SDK для Python и REST API этого сервиса позволяют создавать клиентские приложения, которые анализируют контент для извлечения и генерации значений полей данных.

Примечание.

Для получения дополнительной информации о сервисе Azure Content Understanding, пожалуйста, обратитесь к разделу: [здесь должна быть ссылка или указание на источник информации].[Azure Content Understanding documentation](/en-us/azure/ai-services/content-understanding/)**.
