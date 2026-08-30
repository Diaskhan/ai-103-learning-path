# Анализ текста с помощью сервиса Azure Language в инструментах Foundry.

# Введение.

Каждый день в мире генерируется огромный объем данных, большая часть из которых представлена в текстовом формате: электронные письма, публикации в социальных сетях, отзывы в интернете, деловые документы и многое другое. Технологии искусственного интеллекта, использующие статистические и семантические модели, позволяют создавать приложения, которые извлекают смысл и ценную информацию из этих текстовых данных.

Язык Azure, используемый в инструментах Foundry, предоставляет API для распространенных методов анализа текста, которые вы можете легко интегрировать в свои собственные приложения и системы.

В этом модуле вы узнаете, как использовать сервис Azure Language в инструментах Foundry Tools в своих собственных приложениях, с примерами на языке Python. Вы можете разрабатывать приложения для анализа текста, используя различные специализированные SDK, включая:

- [Microsoft Azure Text Analytics Client Library for Python](https://pypi.org/project/azure-ai-textanalytics/)
- [Microsoft Azure Text Analytics Client Library for .NET](https://www.nuget.org/packages/Azure.AI.TextAnalytics)
- [Microsoft Azure Text Analytics Client Library for JavaScript](https://www.npmjs.com/package/@azure/ai-text-analytics)

Примечание.

Мы понимаем, что разные люди предпочитают учиться разными способами. Вы можете пройти этот модуль в формате видео или изучить материалы в виде текста и изображений. Текст содержит более подробную информацию, чем видео, поэтому в некоторых случаях вам может быть полезно использовать его как дополнительный материал к видеолекции.

---

# Язык Azure в инструментах Microsoft Foundry.

Язык Azure, используемый в инструментах Foundry, предназначен для извлечения информации из текстовых данных. Он предоставляет функциональные возможности, которые можно использовать для выполнения таких задач, как:

- *Определение языка* – процесс установления того, на каком языке написан текст.
- *Распознавание именованных сущностей* — это процесс выявления упоминаний конкретных объектов, включая людей, места, временные периоды, организации и другие объекты.
- *Извлечение персональных данных (PII)* – выявление и удаление личной информации из текста.

  ![Диаграмма, иллюстрирующая работу ресурса Azure Language, который выполняет определение языка текста, распознавание именованных сущностей и извлечение персональных данных.](https://learn.microsoft.com/en-us/training/wwl-data-ai/analyze-text-ai-language/media/text-analytics-resource.png)

Примечание.

Сервис Azure Language также предоставляет функциональность для анализа тональности текста, автоматического создания кратких изложений, выделения ключевых фраз и выполнения других распространенных задач, связанных с обработкой естественного языка. Эти устаревшие возможности предоставляются для поддержки существующих приложений.

## Использование ресурса Microsoft Foundry для анализа текста.

Для использования сервиса Azure Language в инструментах Foundry для анализа текста необходимо создать ресурс Microsoft Foundry в вашей подписке Azure.

После того как вы предоставили ресурс Foundry в своей подписке Azure, вы можете использовать его **адрес (endpoint)** для вызова API сервиса Azure Language из вашего кода. Для аутентификации запросов можно либо указать **ключ**, связанный с вашим ресурсом, либо использовать идентификатор Microsoft Entra ID. Вы можете вызывать API сервиса Azure Language, отправляя запросы в формате JSON к REST-интерфейсу, или используя любые доступные SDK, разработанные для конкретных языков программирования.

Примечание.

Примеры кода, представленные в этом разделе, написаны на языке Python и используют... [далее следует указание того, что именно используется]. [Python SDK for Azure Language in Foundry Tools](https://pypi.org/project/azure-ai-textanalytics/)SDK для других распространенных языков программирования (таких как Microsoft C#, JavaScript и другие) имеют схожую структуру.

### Аутентификация.

Для аутентификации с использованием метода, основанного на ключах, используйте ключ, связанный с вашим ресурсом Foundry. Эту информацию можно найти в портале Foundry.

Совет.

На главной странице портала Foundry по умолчанию отображаются конечная точка (endpoint) и ключ для вашего *проекта*. Чтобы просмотреть ключ и конечную точку для вашего *ресурса*, вы можете перейти на вкладку **Admin** в разделе **Operate** портала и посмотреть информацию о родительском ресурсе вашего проекта. Ключ проекта и ключи ресурсов Foundry совпадают, а конечная точка проекта представляет собой конечную точку ресурса с добавлением */api/projects/{project\_name}* – следовательно, если конечная точка проекта является... `https://my-ai-app-foundry.services.ai.azure.com/api/projects/my-ai-app`...то конечная точка ресурса является... `https://my-ai-app-foundry.services.ai.azure.com`.

Например, следующий код на языке Python создает объект **TextAnalyticsClient**, который можно использовать для отправки запросов к API сервиса Azure Language в ресурсе Foundry.

```
# run "pip install azure-ai-textanalytics" first to install the package 
from azure.core.credentials import AzureKeyCredential
from azure.ai.textanalytics import TextAnalyticsClient

# Create client using endpoint and key
credential = AzureKeyCredential("YOUR_FOUNDRY_RESOURCE_KEY")
client = TextAnalyticsClient(endpoint="YOUR_FOUNDRY_RESOURCE_ENDPOINT", 
                             credential=credential)
```

Для повышения безопасности в производственных решениях компания Microsoft рекомендует использовать аутентификацию через Microsoft Entra ID. Например, следующий код на языке Python использует стандартную идентификацию Azure, применимую к контексту, в котором работает клиентское приложение.

```
# run "pip install azure-identity azure-ai-textanalytics" first to install the packages 
from azure.identity import DefaultAzureCredential
from azure.ai.textanalytics import TextAnalyticsClient

# Create client using endpoint and default Azure identity
credential = DefaultAzureCredential()
client = TextAnalyticsClient(endpoint="YOUR_FOUNDRY_RESOURCE_ENDPOINT", 
                             credential=credential)
```

---

# Определить язык.

API определения языка Azure анализирует текстовый ввод и для каждого предоставленного документа возвращает идентификаторы языков вместе с оценкой, указывающей на достоверность анализа.

Эта функция полезна для систем хранения контента, которые собирают произвольный текст, когда язык не известен. Другой сценарий может включать в себя приложение для чата. Если пользователь начинает сеанс работы с приложением, определение языка может быть использовано для того, чтобы определить, на каком языке он общается, и позволить вам настроить ответы приложения на этом языке.

Вы можете проанализировать результаты этой обработки данных, чтобы определить, на каком языке написан исходный документ. Ответ также содержит оценку, которая отражает уверенность модели (значение от 0 до 1).

Определение языка может применяться как к целым документам, так и к отдельным фразам. Важно отметить, что размер документа не должен превышать 5120 символов. Ограничение по размеру распространяется на каждый документ, а каждая коллекция ограничена 1000 элементами (идентификаторами). Ниже приведен пример правильно отформатированного JSON-файла, который вы можете отправить в теле запроса к сервису, включающий коллекцию **документов**, каждый из которых содержит уникальный **идентификатор** и **текст** для анализа.

Например, следующий код на языке Python анализирует два (небольших) документа для определения языка, на котором они написаны.

```
# Assumes code to create TextAnalyticsClient is above...

# Example text to analyze
documents = ["Hello World!", "Bonjour le monde!"]

# Detect language
response = client.detect_language(documents=documents)
for doc in response:
    print(f"Document: {doc.id}")
    print(f"\tPrimary Language: {doc.primary_language.name}")
    print(f"\tISO6391 Name: {doc.primary_language.iso6391_name}")
    print(f"\tConfidence Score: {doc.primary_language.confidence_score}")
```

Ответ содержит результат для каждого **документа** в запросе, включая определенный язык и значение, указывающее на уровень достоверности определения языка. Уровень достоверности – это число от 0 до 1, где значения, близкие к 1, соответствуют более высокой степени уверенности. Вот пример ответа, полученного из предыдущего кода.

```
Document: 0
        Primary Language: English
        ISO6391 Name: en
        Confidence Score: 0.9
Document: 1
        Primary Language: French
        ISO6391 Name: fr
        Confidence Score: 0.98
```

В нашей выборке оба языка демонстрируют высокий уровень уверенности в определении, главным образом потому, что текст относительно прост и язык в нем легко распознать.

Если вы пытаетесь определить язык документа, содержащего текст на нескольких языках, например... `I know a cool AI developer. He has a certain je ne sais quoi!`В некоторых случаях ответ может содержать некоторую неоднозначность. Если в одном документе содержится текст на разных языках, определяется язык, который наиболее часто встречается в тексте, но оценка его достоверности будет ниже, что отражает относительную уверенность в этом определении.

Последнее условие, которое следует учитывать, возникает в случае неоднозначности относительно языка содержимого. Такая ситуация может возникнуть, если вы отправляете текстовый контент, который анализатор не может обработать, например, из-за проблем с кодировкой символов при преобразовании текста в строковую переменную. В результате, ответ о названии языка и ISO-коде будет возвращен как... `(unknown)` и значение оценки будет возвращено в виде... `0`.

---

# Извлечь сущности.

Распознавание именованных сущностей (Named Entity Recognition) позволяет выявлять объекты, упоминаемые в тексте. Эти объекты группируются по категориям и подкатегориям, например:

- Человек.
- Местоположение.
- Дата и время.
- Организация.
- Адрес.
- Электронная почта.
- URL (Uniform Resource Locator) - Унифицированный указатель ресурсов.

Примечание.

Для получения полного списка категорий, пожалуйста, обратитесь к [укажите источник]. [documentation](/en-us/azure/ai-services/language-service/named-entity-recognition/concepts/named-entity-categories?tabs=ga-api).

Входные данные для распознавания сущностей аналогичны входным данным для других функций API языка Azure:

```
# Example text to analyze
documents = ["Microsoft was founded on April 4, 1975 by Bill Gates and Paul Allen in Albuquerque, New Mexico.",
             "Satya Nadella became CEO of Microsoft on February 4, 2014."]

# Extract named entities
response = client.recognize_entities(documents=documents)
for doc in response:
    print(f"Entities in document {doc.id}:")
    for entity in doc.entities:
        print(f" - {entity.text} ({entity.category})")
```

В ответе представлен список категорий сущностей, обнаруженных в каждом документе:

```
Entities in document 0:
 - Microsoft (Organization)
 - April 4, 1975 (DateTime)
 - Bill Gates (Person)
 - Paul Allen (Person)
 - Albuquerque (Location)
 - New Mexico (Location)
Entities in document 1:
 - Satya Nadella (Person)
 - CEO (PersonType)
 - Microsoft (Organization)
 - February 4, 2014. (DateTime)
```

---

# Извлечь персональные данные, позволяющие идентифицировать личность.

Во многих случаях необходимо выявлять и защищать конфиденциальную личную информацию, содержащуюся в документах. Например, вам может потребоваться удалить персональные данные из отзывов клиентов, медицинских записей или юридических документов перед их передачей другим лицам.

Сервис Azure Language предоставляет возможности для обнаружения и сокрытия персональных данных (PII). Он позволяет выявлять конфиденциальную информацию, такую как имена, адреса, номера телефонов, электронные почтовые адреса, идентификационные номера и номера кредитных карт. Вы можете извлекать эти данные для анализа или скрывать их (маскировать), чтобы защитить частную жизнь пользователей.

Как и в случае со всеми функциями сервиса Azure Language, вы можете отправить один или несколько документов для анализа.

```
# Example text to analyze
documents = ["John Smith works at Contoso Ltd. His email is john.smith@contoso.com and his phone number is 555-012-456.",
             "Patient Sarah Johnson, SSN 123-45-6789, was admitted on 03/15/2024."]

# Extract PII entities
response = client.recognize_pii_entities(documents=documents, language="en")
for doc in response:
    print(f"\nPII entities in document {doc.id}:")
    for entity in doc.entities:
        print(f" - {entity.text}: {entity.category} (confidence: {entity.confidence_score:.2f})")
```

В ответе представлены идентифицированные в тексте персональные данные (PII), а также их категории и показатели достоверности.

```
PII entities in document 0:
 - John Smith: Person (confidence: 0.99)
 - Contoso Ltd: Organization (confidence: 0.85)
 - john.smith@contoso.com: Email (confidence: 1.00)
 - 555-012-456: PhoneNumber (confidence: 0.80)
PII entities in document 1:
 - Sarah Johnson: Person (confidence: 0.99)
 - 123-45-6789: USSocialSecurityNumber (confidence: 0.99)
 - 03/15/2024: DateTime (confidence: 0.80)
```

Вы также можете замаскировать конфиденциальные данные (персональную информацию) для защиты чувствительной информации. Сервис возвращает отредактированную версию текста, в которой персональная информация заменена звездочками или указанным символом.

```
# Redact PII entities
response = client.recognize_pii_entities(documents=documents, language="en")
for doc in response:
    print(f"\nDocument {doc.id} (redacted):")
    print(f" {doc.redacted_text}")
```

Это приводит к тому, что вывод содержит замаскированную конфиденциальную информацию:

```
Document 0 (redacted):
 ********** works at ************. His email is ************************ and his phone number is ********.
Document 1 (redacted):
 Patient *************, SSN ***********, was admitted on **********.
```

---

# Краткое изложение.
Или: Резюме.
(В зависимости от контекста)

В этом модуле вы узнали, как использовать сервис Azure Language в инструментах Foundry для:

- Определить язык текста.
- Распознавайте именованные сущности в тексте.
- Извлеките из текста персональные данные, позволяющие идентифицировать личность.

Совет.

Для получения дополнительной информации об использовании сервиса Azure Language в инструментах Foundry и о некоторых концепциях, рассматриваемых в этом модуле, обратитесь к [ссылке/разделу].[Azure Language in Foundry Tools documentation](/en-us/azure/ai-services/language-service?azure-portal=true)**.
