---

---
Basic tests are:

1. Uniqueness of keys
2. Not null constraint
3. Foreign keys (relationships test)
4. Table not empty

See the example of tests configuration for [metrika.yml](https://github.com/kzzzr/mybi-dbt-core/blob/master/models/staging/metrika/metrika.yml):

  
{% highlight yaml %}
{% raw %}

   - name: stg_ym_sessions_facts
     tests:
       - not_empty:
           severity: warn
       - unique:
           column_name: id
       - not_null:
           column_name: account_id
       - not_null:
           column_name: clientids_id
       - not_null:
           column_name: dates_id
       - not_null:
           column_name: traffic_id
       - not_null:
           column_name: locations_id
       - not_null:
           column_name: devices_id
       - relationships:
           to: ref('stg_general_accounts')
           column_name: account_id
           field: account_id
       - relationships:
           to: ref('stg_general_clientids')
           column_name: clientids_id
           field: id
       - relationships:
           to: ref('stg_general_dates')
           column_name: dates_id
           field: id
       - relationships:
           to: ref('stg_general_traffic')
           column_name: traffic_id
           field: id
       - relationships:
           to: ref('stg_general_locations')
           column_name: locations_id
           field: id
       - relationships:
           to: ref('stg_ym_devices')
           column_name: devices_id
           field: id


{% endraw %}
{% endhighlight %}