---

---
[sources.yml](https://github.com/kzzzr/mybi-dbt-core/blob/master/models/sources/sources.yml) describes how to reach tables created by ELT service

{% highlight yaml %}
   - name: metrika
     database: "'{{ env_var('DBT_MSSQL_DATABASE') '}}"
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
{% endhighlight %}