---

---
[sources.yml](https://github.com/kzzzr/mybi-dbt-core/blob/master/models/sources/sources.yml) describes how to reach tables created by ELT service

{% highlight yaml %}
{% raw %}
   - name: metrika
     database: "{{ env_var('DBT_MSSQL_DATABASE') }}"
     schema: "{{ env_var('DBT_MSSQL_SCHEMA') }}"
     description: Yandex.Metrika
     tags: ['metrika']
     tables:  
       - name: sessions_facts
         identifier: metrika_sessions_facts
       - name: goals_facts
         identifier: metrika_goals_facts
       - name: purchases_facts
         identifier: metrika_purchases_facts
       - name: devices
         identifier: metrika_devices
       - name: goals
         identifier: metrika_goals
       - name: purchases
         identifier: metrika_purchases
    {% endraw %}  
    {% endhighlight %}

Mind the **_database_** and **_schema_** keys. They might vary between projects and deployments so they are configured as environment variables. I will show you where they get actual values a little bit later.

**_Identifier_** key is the full name to reference a source table in a database, while _name_ key works like an alias for referencing a table in dbt code. It comes pretty handy if you have long table names.

By labeling sources with _tags_ you can select, run or test certain parts of your DWH, for example you might want to rebuild models depending on a particular source after fixing a bug.

Listing particular sources with `dbt ls` command looks like this (click to expand):

[![](https://habrastorage.org/webt/ft/wg/zq/ftwgzq9mcxfshm0vjdibiw44ciq.gif)](https://habrastorage.org/webt/ft/wg/zq/ftwgzq9mcxfshm0vjdibiw44ciq.gif)