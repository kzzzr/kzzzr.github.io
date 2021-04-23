---

---
# CORE DWH – building a common foundation

Plugging source data is pretty simple. You just subscribe to a plan, authorize your accounts, configure pipelines and it just works by filling tables with new data continuously.

**_![](https://lh4.googleusercontent.com/updr0D4uK4BQjF7GIhRT71i6-WIyIrrO-hsuYPmdw8d4CDDE7dmYu-7UeToHOEpFW1LiVYOBOcMSsJKoGk2w6k3UNZdjK64cSnPzrDkWCG3YJ5CVR4RcQJ6_91v73xph6U4ejhg3 =624x240)_**

I want to use the best analytics engineering tool on the market – [dbt](https://www.getdbt.com/) from Fishtown Analytics. Whole DWH CORE module could be maintained as a single repo with dbt project.

myBI uses Azure SQL as a backend to store collected data in. Azure SQL descends from MS SQL which is a dbt’s [community-supported adapter](https://docs.getdbt.com/docs/available-adapters#community-supported). Hence it requires additional actions and steps to perform compared to [Fishtown’s supprted adapters](https://docs.getdbt.com/docs/available-adapters#fishtown-supported) (Snowflake, PostgreSQL, BigQuery, Databricks, Presto), but eventually it works like a charm.

See the [Dockerfile](https://github.com/kzzzr/mybi-dbt-core/blob/master/Dockerfile):

    ARG DBT_VERSION=0.19.1
    FROM fishtownanalytics/dbt:${DBT_VERSION}
     
    RUN set -ex \
       && buildDeps=' \
           unixodbc-dev \
           gnupg \
           curl \
       ' \
       && apt-get update -yqq \
       && apt-get upgrade -yqq \
       && apt-get install -yqq --no-install-recommends $buildDeps \
       && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
       && curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list \
       && apt-get update \
       && ACCEPT_EULA=Y apt-get install -yqq msodbcsql17 \
       && pip install dbt-sqlserver==0.19.0.2
     
    ENV DBT_PROFILES_DIR=.
     
    # ENTRYPOINT ["tail", "-f", "/dev/null"]
    ENTRYPOINT [ "/bin/bash" ]
    

Here I do some steps:

1. Use official image from Fishtown Analytics with latest dbt version 0.19.1
2. Install dependencies: MS SQL utilities as well as [dbt-sqlserver](https://github.com/dbt-msft/dbt-sqlserver) package
3. Configure dbt to use profiles file (connection strings) in the project root directory