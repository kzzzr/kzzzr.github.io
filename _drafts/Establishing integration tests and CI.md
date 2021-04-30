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

Secondly, I would like to populate source tables with a small portion of data to test on. I just include it into my repository under `/integration_tests/data/` subfolder in _.csv_ format. These files are loaded by `dbt seed` command.

The most important thing here is data in csv files should reflect production source data fully. If there is a relationship between tables then there should be a linked row with the same identifier, otherwise tests will fail.

Example [integration_tests/data/metrika/metrika_sessions_facts.csv](https://github.com/kzzzr/mybi-dbt-core/blob/master/integration_tests/data/metrika/metrika_sessions_facts.csv):

| account_id | clientids_id | dates_id | traffic_id | locations_id | devices_id | sessions | bounces | pageviews | duration |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 21600 | 1 | 159250 | 15645 | 165 | 35 | 1 | 0 | 11 | 240.00 |
| 21600 | 1 | 159250 | 726276 | 1 | 35 | 1 | 0 | 3 | 101.00 |
| 21600 | 1 | 159250 | 4821954 | 211 | 34 | 1 | 0 | 6 | 137.00 |
| 21600 | 1 | 159250 | 4821952 | 1 | 34 | 1 | 0 | 1 | 22.00 |
| 21600 | 1 | 159250 | 823634 | 169 | 34 | 1 | 0 | 1 | 15.00 |
| 21600 | 1 | 159250 | 4821936 | 1 | 35 | 1 | 1 | 1 | 0.00 |
| 21600 | 1 | 159250 | 724226 | 346 | 35 | 1 | 0 | 8 | 303.00 |

Things are a bit more complicated, since I have to create a separate dbt project in a `/integration_tests/` subdirectory and configure it with module importing from parent directory. But overall it looks nice and neat, the imported module is created as a symlink.

[packages.yml](https://github.com/kzzzr/mybi-dbt-core/blob/master/integration_tests/packages.yml):

{% highlight yaml %}
{% raw %}

packages:
   - local: ../
   
{% endraw %}
{% endhighlight %}   