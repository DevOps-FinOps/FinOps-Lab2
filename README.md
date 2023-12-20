# FinOps-Lab2
1. Microsoft Dedicated SQL Pool (ранее Microsoft SQL Warehouse) - сервис аналитики больших данных, предоставляющий загрузку, хранение данных, обучение моделей и анализ данных с их помощью. Интегрирован с СУБД Microsoft SQL Server. Используются технологии Apache Hadoop и Spark (фреймворки распределенных вычислений), PolyBase (позволяет запрашивать данные из БД и S3-хранилищ), Transact-SQL (процедурное расширение SQL).   
https://learn.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png

Аналог: Платформа обработки данных Selectel. Используются те же Apache Hadoop и Spark, для загрузки данных Apache Kafka, в качестве реляционной СУБД Greenplum. Минус платформы Selectel - у сервиса Microsoft значительно более подробная документация.
https://423.selcdn.ru/kb/abdp.ml_scheme_5rg8u8.png

2. Azure Functions - serverless платформа (с динамическим выделением ресурсов) для запуска написанных клиентом приложений, основанный на сценариях, позволяющих запускать код по расписанию, при загрузке или изменении файлов в хранилище, HTTP-триггере и так далее.

Аналог: Yandex Functions. Поддерживает те же функции, даже набор языков больше (нет PowerShell, зато есть Bash, R, Go, PHP, которых нет у Microsoft, и общие Python, C#, Java, JavaScript). Документация такая же подробная, как у Microsoft.
