---

---
# Establishing integration tests and CI

I find it pretty hard to build on top of the CORE DWH module if it is not stable enough. Testing of the whole module during every patch and PR and before you can use it ensures quality and reliability. That is where Integration tests and CI come essential.

First I will spin up a MS SQL Server instance in a docker container to perform real queries on it.

[docker-compose.yml](https://github.com/kzzzr/mybi-dbt-core/blob/master/docker-compose.yml):

{% highlight yaml %}
{% raw %}

version: '2'
services:

mssql:
image: mcr.microsoft.com/mssql/server:2019-latest
environment:
ACCEPT_EULA: "Y"
SA_PASSWORD: "P@ssword"
ports:
\- "1433:1433"

dbt:
build: .
entrypoint: /bin/bash
tty: true
volumes:
\- .:/usr/app/mybi-dbt-core

{% endraw %}
{% endhighlight %}

Secondly, I would like to populate source tables with a small portion of data to test on. I just include it into my repository under `/integration_tests/data/` subfolder in .csv format. These files are loaded by _`dbt seed`_ command.

For each source table used in CORE DWH there is a number of rows which reflect production source data. What is more important, if tables are linked via foreign key then csv files should

The most important thing here is data in csv files should reflect production source data fully. If there is a relationship between tables then there should be a linked row with the same identifier, otherwise tests will fail.

Example [integration_tests/data/metrika/metrika_sessions_facts.csv](https://github.com/kzzzr/mybi-dbt-core/blob/master/integration_tests/data/metrika/metrika_sessions_facts.csv):