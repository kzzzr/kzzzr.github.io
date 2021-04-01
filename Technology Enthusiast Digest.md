---
published: false

---
# Technology Enthusiast Digest

Here I will structure materials and talks chronologically ordered.

At the end you will find **TBD** heading with posts to be done. Please comment on which topics you are interested in to prioritize them.

## 2021 Apr

(TBD)

## 2021 Mar

(RU) – [Мультитул для управления Хранилищем Данных — кейс Wheely + dbt](https://habr.com/ru/company/wheely/blog/549614/)

* 2+ года dbt в продакшн в управлении Хранилищем Данных.
* Структура хранилища + бизнес-вертикали
* Оптимизация физического хранения в Redshift
* SQL + Jinja = Flexibility
* Macro: UDF, currency exchange
* Importing modules: calendar, external data, logging
* Deployment with dbt Cloud: Schedule + Webhooks, Slack notifications

(RU) – [ОБЗОР LOOKER - БОЛЬШЕ ЧЕМ ПРОСТО BI](https://youtu.be/-YMCafO_cZk)

Сегодня приглашаю на вебинар по **Looker** - топовый BI-инструмент на рынке!

* Моделирование данных: структура проекта и блоки LookML
* Исследование данных: pivot, drill-down, table calculations
* Визуализация: типы графиков, дашбординг, кросс-фильтры, sharing
* Продвинутые фишки: Marketplace, API, Persistent Derived Tables

## 2021 Feb

(RU) – [SQL для аналитики — рейтинг прикладных задач с решениями](https://habr.com/ru/company/otus/blog/541882/)

Собрал аналитические задачи на SQL с интерактивными примерами / решениями на sqlfiddle.com. От простого к сложному:

* Конкатенация значений из нескольких строк
* CASE в агрегирующих функциях
* **Дедупликация** данных
* Анализ временных рядов
* NULL & IF-THEN-ELSE в SQL
* Аналитические функции
* Историзация со Slowly Changing Dimensions (**SCD**)
* **FULL JOIN** для соединений без потерь
* Разбиение пользовательских событий на **сессии**

(RU) – [Путь Инженера Аналитики: Решение для Маркетинга / Артемий Козырь](https://youtu.be/SoOcvYPSm7o)

* Расскажу про ниндзя-проект по сбору **Сквозной Аналитики** для Performance-маркетинга
* Бизнес-цели проекта: консолидация трат, метрики возврата на вложения
* **ELT** и пылесосинг данных из источников
* Организация Хранилища на **dbt**
* Open Source BI на **Metabase**
* Нюансы **Dev & Ops**
* Отвечу на вопросы: облака, выбор инструментов, обучение, карьера, пути развития

## 2021 Jan

(RU) – [Аналитический движок Amazon Redshift + преимущества Облака](https://habr.com/ru/company/wheely/blog/539154/)

* Новый пост по тематике аналитических Хранилищ на примере **Amazon Redshift.**
* Основы гибких кластерных вычислений
* **Колоночное** хранение и компрессия данных
* Вместо индексов: ключи сегментации и сортировки
* Управление доступами, правами, ресурсами
* Интеграция с **S3** или **Даталейк** на ровном месте

(RU) – [Сквозная Аналитика на Azure SQL + dbt + Github Actions + Metabase](https://habr.com/ru/post/538106/)

* Задача: собрать траты и результаты в единую картинку
* Интеграция сервисов через **myBI Connect**
* Организация DWH с помощью **dbt**
* Дашбординг с **Metabase**
* Рейтинг проблем: разметка, CRM, матчинг и атрибуция, хеш-ключи

## 2020

(RU) – [Дата-инжиниринг в превосходных условиях](https://habr.com/ru/company/wheely/blog/535476/)

* В публикации раскрываю вопросы **Modern Data Stack** и фундамента Data Driven организации на примере **Wheely**
* **MongoDB**: Специфика документ-ориентированного хранения (semi-structured & nested data)
* Пылесосинг данных: облачный ETL **Hevo** Data
* **Airflow** для оркестрации прикладных задач: бизнес-планирование в **Notion.so**

(ENG) – [Compressing Redshift columnar data even further with proper encodings](https://kzzzr.github.io/blog/compressing-redshift-columnar-data-even-further-with-proper-encodings/)

* Basics of compression encodings
* Questioning default compression options
* ANALYZE COMPRESSION
* Wrap it up into automatic macro
* Assessing results

(RU) – [Кто ответит за качество аналитики: QA для Хранилища Данных](https://habr.com/ru/company/otus/blog/526174/)

## TBD

* How to refactor code
  * Why do it: easily extendible and develop in future
  * Before and after
  * DRY: using macros 
  * Ex:  Separate window functions from large calculations
  * Ex: journeys pipeline
  * Ex: financial metrics (gather and calculate them all in one place)


* Bulk load from MongoDB
  * mongoexport cli
  * Arrays, query, time filter by ObjectId
  * field list
  * handle ObjectId to string
  * upload to S3
  * copy from s3 to Redshift


* Online Transformations in ELT pipeline (streaming transformations)
  * Removing PII: Masking and hashing data
  * Get country code from phone numbers
  * Flattening data
  * Hevo capabilites & showcase
* How to choose ELT provider: points to consider + benchmarking
  * Fivetran vs Hevo vs Stitch vs Alooma
  * available sources
  * user interface
  * historical load
  * online replication + log reading
  * transformations on the fly
  * destinations
  * handling nested data and json
  * flexibility: reload data
  * mapping: choose tables and fields to load
* What is bitmask and how to use it
  * Why use bit mask
  * How to optimally encode several dimensions in a single field
  * How to work with bit mask in Redshift
* **Redshift** optimizations techniques:
  * refactoring SQL (requests, zones, quotes)
  * dist keys + sort keys: compound & interleaved
  * pre-calculating: materialized views, Persistent Derived Tables
  * compression
  * vacuum+analyze hooks
* Capturing user Events with **Snowplow**
  * Cross-platform / App / Web events
  * Sending payload on the webhook
  * Parsing & analyzing data
  * Capabilities & limitations


* Обзор Open Source BI **Metabase**
  * Open source BI vs Proprietary
  * Плюсы: быстро, просто, красиво, функционально
  * Минусы: недостаточно визуализаций (каскадная таблица), недоступны сложные вычисления, иногда требуется конфигурация типов полей, drill-down слабоват
* Monitoring dbt deployments
  * logging module: fork + schema configuration
  * hook on each action
  * models: deployments + model deployments
  * tiles + dashboard + filters
  * key results and observes
* Reload data from MongoDB to DWH
  * Specific rows (ObjectIds)
  * mongoshell & mongoexport utilities
  * arrays, query, time filter by ObjectId
  * examples of Shell + sql scripts
* Deep dive to Data Quality:
  * Schema test: not null, unique, reference, accepted values
  * Freshness tests. Is data up to date?
  * mismatch: replication not synced
  * missing in source: file headers
  * missing in target: replay (ETL pipeline failure)
  * alerts on warnings
* Configuring **Slim CI** for your Data Warehouse
  * Only test models have been modified since last time tested
  * Ensure code quality
  * Make it really fast
* RFM analytics
  * Assessing customer retention
  * Training and deploying ML model
  * Recency, frequency, monetary value
* Orchestrating your pipelines. dbt vs Airflow?
  * What is the difference
  * Prefect / Dagster
* Examples of how to avoid complexity (refactoring)
  * KISS: as simple as possible. 
  * Simplifying complex domain: Journeys
  * Currency conversion macro
  * Geospatial: hexagons, cities
* Advanced Data Quality
  * Comparing Source <-> Target continuously
  * Using shell scripts and S3 to sync data
  * Capturing discrepancies with Looker dashboards
  * Reloading data (fixing)
* Pandas exclusive lock on a table
  * Don’t forget to close connection, or ETL pipelines will get stuck


* [myBI connect](http://connect.mybi.ru/) overview
  * multiple connectors
  * advanced features: cube.js, webhook, custom reports
  * monitoring and data quality


* Delivering reports to Notion
  * Notion API for Python (unofficial)
  * Looker API: fetching dashboards
  * Posting to Notion page (defined by YAML configuration)
* Redshift maintenance operations
  * Vacuum + analyze
  * Using External tables with Spectrum
  * Unloading data to S3
* Marketing Data Pipeline at Wheely
  * merging offline + online, sheets and automated data exports
  * refactoring sql: easy, readable, maintainable
  * connecting sources: gs, adv
  * sheets structure
  * partnerships logic
  * dbt models + merge logic
* Data Vault 2.0 
  * Code generation basics
  * dbt + Data Vault = dbtvault
  * Examples


* Data Warehouse / Cluster definition code
  * Schemas + Groups + Users
  * Ensure policies and access segregation
  * Workload Management (Divide into Queues)


* [Pandas Data Analytics Showcase – Kiva.org](https://github.com/kzzzr/skillbox-kiva)
  * Basics and advanced of pandas and visualizations
  * Exploratory Data Analysis
  * Summarizing and presenting results


* PostgreSQL production load troubleshooting
  * Identifying root problems with queries
  * Turning on query logging
  * Index & table bloat
  * VACUUM ANALYZE & REINDEX CONCURRENTLY
  * Summarizing and presenting results