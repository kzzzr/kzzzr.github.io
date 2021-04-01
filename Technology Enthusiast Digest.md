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

## 2020 Dec

(RU) – [Дата-инжиниринг в превосходных условиях](https://habr.com/ru/company/wheely/blog/535476/)

* В публикации раскрываю вопросы **Modern Data Stack** и фундамента Data Driven организации на примере **Wheely**
* **MongoDB**: Специфика документ-ориентированного хранения (semi-structured & nested data)
* Пылесосинг данных: облачный ETL **Hevo** Data
* **Airflow** для оркестрации прикладных задач: бизнес-планирование в **Notion.so**

## TBD

* [myBI connect](http://connect.mybi.ru/) overview
  * multiple connectors
  * advanced features
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