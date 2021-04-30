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
     - "1433:1433"
 
 dbt:
   build: .
   entrypoint: /bin/bash
   tty: true
   volumes:
    - .:/usr/app/mybi-dbt-core
    
{% endraw %}
{% endhighlight %}    