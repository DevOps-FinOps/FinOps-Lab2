# Аналитическая лабораторная работа 2 - миграция из Azure в отечественные сервисы
Команда: Париш Павел (капитан), Клопов Михаил, Буров Глеб

Была дана таблица Excel, из которой нам нужны первая колонка, в ней было название сервиса Azure, которому нужно подобрать отечественный аналог, и вторая с подкатегорией, присутствующей у некоторых сервисов. Вот какие сервисы в ней были:

1. Microsoft Dedicated SQL Pool (ранее Microsoft SQL Warehouse) - сервис аналитики больших данных, предоставляющий загрузку, хранение данных, обучение моделей и анализ данных с их помощью. Интегрирован с СУБД Microsoft SQL Server. Используются технологии Apache Hadoop и Spark (фреймворки распределенных вычислений), PolyBase (позволяет запрашивать данные из БД и S3-хранилищ), Transact-SQL (процедурное расширение SQL).   
https://learn.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png

Аналог: Платформа обработки данных Selectel. Используются те же Apache Hadoop и Spark, для загрузки данных Apache Kafka, в качестве реляционной СУБД Greenplum. Минус платформы Selectel - у сервиса Microsoft значительно более подробная документация.
https://423.selcdn.ru/kb/abdp.ml_scheme_5rg8u8.png

2. Azure Functions - serverless платформа (с динамическим выделением ресурсов) для запуска написанных клиентом приложений, основанный на сценариях, позволяющих запускать код по расписанию, при загрузке или изменении файлов в хранилище, HTTP-триггере и так далее.

Аналог: Yandex Functions. Поддерживает те же функции, даже набор языков больше (нет PowerShell, зато есть Bash, R, Go, PHP, которых нет у Microsoft, и общие Python, C#, Java, JavaScript). Документация такая же подробная, как у Microsoft.

3. Azure Key Vault - менеджер секретов, позволяющий хранить и централизованно менять пароли, ключи, автоматически обновлять сертификаты, вести мониторинг за их использованием.

Аналог: Менеджер секретов Selectel. Функции те же, центры сертификации (certificate authority) другие.

4. Azure Logic Apps - платформа для запуска автоматизированных codeless процессов, таких как обработка заказов, электронной почты, передача файлов и т.д. при наступлении определённых условий.
https://learn.microsoft.com/ru-ru/azure/logic-apps/media/logic-apps-overview/example-enterprise-workflow.png

Аналог: здесь не всё так просто. для автоматизированной отправки электронной почты есть Yandex Cloud Postbox, для некоторых других функций Yandex Cloud Apps, для анализа данных Yandex Query. Но эти платформы не прямые аналоги, так что могут быть функции, доступные только на одной из них.

5. Azure Machine Learning - сервис для управления жизненным циклом (создания, обучения, мониторинга, визуализации) моделей машинного обучения по методологии MLOps с помощью пайплайнов.
https://learn.microsoft.com/en-us/azure/machine-learning/media/overview-what-is-azure-machine-learning/overview-ml-development-lifecycle.png

Аналог: ML-платформа Selectel. По функционалу они схожи, обе могут работать на базе Kubernetes-кластера как serverless, так и на выделенном сервере (в Azure это называется compute cluster). 
https://cdn.selectel.ru/site/img/ml-scheme.d2f6b9b.svg

5.1. Text Analytics - сервис, позволяющий делать сентиментный анализ, выделять ключевые слова и сущности. Для этого используется библиотека с открытым исходным кодом SynapseML, которую можно развернуть где угодно, например на вышеупомянутой платформе Selectel.

5.2. Machine Learning Studio - старый сервис для машиного обучения. Так как сами Microsoft закрывают его, предлагая мигрировать на вышеупомянутый, здесь не останавливаемся.

6. Visual Studio App Center - сервис для управления разработкой мобильных и десктопных приложений, в числе функций CI/CD, мониторинг производительности, аналитика использования, выпуск бета-версий для части пользователей.

Аналог: тут проблема. Что-то отдалённо напоминающее - Yandex Managed Service for GitLab, но там только CI/CD, да ещё и только с GitLab - если вы используете GitHub, BitBucket или Azure DevOps - придётся отдельно переносить. Встроенного мониторинга нет, канареечных релизов тоже. Так себе замена, в общем.

7. Azure VPN Gateway - служба отправки зашифрованного трафика в виртуальную сеть Azure извне или между виртуальными сетями.

Аналог: ГОСТ VPN от Selectel. Это вещь, конечно, более специализированная, предназначенная для работающих с государством и конфиденциальной информацией, поэтому там нет такого выбора протоколов, как у Azure, но что есть. Альтернатива - виртуальный межсетевой экран, про него будет говориться ниже. Ещё есть VPN от VK Cloud, но он интегрирован только с этим самым VK Cloud, у которого нет аналогов многим из вышеупомянутых сервисов Azure - в виртуальную сеть Selectel или Yandex Cloud через него не войти. Да и выбор протоколов тоже ограничен. Можно ещё попробовать арендовать виртуалку у нужного провайдера и самому захостить на ней VPN-шлюз, но не факт, что это решение будет соответствовать поставленным требованиям.

8. Azure Traffic Manager - балансировщик нагрузки, направляющий DNS-запросы на нужный сервис.

Аналог: Балансировщик нагрузки Selectel и Network Load Balancer Яндекса работают не на DNS-запросы, а на HTTP(S) и TCP/UDP, но результат должен быть по идее такой же.

9. Azure Firewall - фильтрует входящие и исходящие соединения и делает TLS inspection (дешифрует трафик, проверяет на наличие вредоносного содержимого и снова шифрует).

Аналог: У Selectel аналогичное решение - виртуальный сетевой экран UserGate VE с дополнительными опциями Stream Antivirus (AV) - проверка трафика на вредоносный код и/или Advanced Threat Protection (ATP) - фильтрация контента, но инфраструктура, на которой он хостится, оплачивается отдельно. Ещё есть базовый брандмауэр без анализа трафика, да ещё и работающий только в приватных подсетях, а в публичных документация предлагает использовать встроенный в ОС. В VK Cloud есть классный Web Application Firewall, который не только фильтрует и анализирует HTTP(S) трафик, но ещё и анализирует XML на предмет уязвимостей, но он работает только с сервисами VK Cloud, а там, как уже говорилось выше, есть не всё, что нам нужно. А у Яндекса вообще никакого файервола нет.

10. Azure Site Recovery - механизм переноса инфраструктуры в другой регион при сбое. Правда, только виртуалки и сервера - PaaS и SaaS в сделку не входят.
https://cdn-dynmedia-1.microsoft.com/is/image/microsoftcorp/site-recovery_prop1?resMode=sharp2&op_usm=1.5,0.65,15,0&wid=1810&qlt=100&fmt=png-alpha&fit=constrain

Аналог: здесь VK Cloud нам может помочь. У него есть аналогичное решение Disaster Recovery, которое позволяет работать с виртуалками из других облачных сервисов или даже selfhosted.

11. Azure Cognitive Services - набор прикладных ИИ-сервисов:

11.1. Azure QnA Maker - сервис, позволяющий создать чат-бота, отвечающего на вопросы с помощью базы знаний.
https://learn.microsoft.com/en-us/azure/ai-services/qnamaker/media/qnamaker-overview-learnabout/bot-chat-with-qnamaker.png

Аналог: прямого нет. Такой сервис можно без проблем написать и захостить самостоятельно, но такого красивого low-code решения, как в Azure, отечественные провайдеры не предлагают. Впрочем, и Azure этот сервис собирается в 2025 году свернуть.

Другие сервисы этого блока в задании не упомянуты, но кратко по ним пробегусь:

11.2. Vision - анализ изображений, распознавание текста и лиц.

Аналог: Yandex Vision и VK Cloud Voice, они всё это тоже могут.

11.3. Speech - перевод текста в голос и наоборот и распознавание голоса.

Аналог: Yandex SpeechKit и VK Cloud Voice, правда у них нет распознавания голоса. 

11.4. Language - электронный переводчик, способный переводить тексты с профессиональной терминологией.

Аналог: Yandex Translate с глоссариями.


12. Azure SignalR - служба, позволяющая в реальном времени обновлять содержимое веб-страниц на стороне пользователя, не дожидаясь запроса от него.

Аналог: отсутствует. Ну или по крайней мере мне не удалось найти хоть что-то подобное.

13. Azure Event Grid - обработчик событий, связывающий между собой приложения и службы.

Аналог: отсутствует. Ну или по крайней мере мне не удалось найти хоть что-то подобное.

14. Azure IoT Hub - платформа, позволяющая централизованно управлять устройствами и приложениями Интернерта вещей, интегрированный с Azure Event Grid.

Аналог: что-то вроде обработки событий предоставляет IoT Core из Yandex Cloud, а вот возможности автоматического обновления он не предоставляет, удобной настройки вроде тоже нет.

15. Виртуалки - ну это везде есть.

Вывод: миграция из Azure на российские облачные провайдеры - задача крайне проблемная. Каких-то сервисов нет вообще, другие разбросаны по разным провайдерам и их между собой никак не интегрировать.
