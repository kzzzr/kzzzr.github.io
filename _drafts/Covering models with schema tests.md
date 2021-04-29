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

* name: stg_ym_sessions_facts
  tests:
  * not_empty:
    severity: warn
  * unique:
    column_name: id
  * not_null:
    column_name: account_id
  * not_null:
    column_name: clientids_id
  * not_null:
    column_name: dates_id
  * not_null:
    column_name: traffic_id
  * not_null:
    column_name: locations_id
  * not_null:
    column_name: devices_id
  * relationships:
    to: ref('stg_general_accounts')
    column_name: account_id
    field: account_id
  * relationships:
    to: ref('stg_general_clientids')
    column_name: clientids_id
    field: id
  * relationships:
    to: ref('stg_general_dates')
    column_name: dates_id
    field: id
  * relationships:
    to: ref('stg_general_traffic')
    column_name: traffic_id
    field: id
  * relationships:
    to: ref('stg_general_locations')
    column_name: locations_id
    field: id
  * relationships:
    to: ref('stg_ym_devices')
    column_name: devices_id
    field: id

{% endraw %}
{% endhighlight %}

Comprehensive enough to find any discrepancy, bug or undesired condition in your data right on the staging area. Remember these tests will be imported along with the CORE DWH module and applied to any project, regardless of customer deployment. What matters is the source schema and its tests list.

  
**‘_not_empty_’** test severity level is set to _warn_ simply to highlight the lack of data avoiding termination with error code.

There are precisely **335** tests for **56** models which stands for around **600%** test coverage (click to expand):

[![](https://habrastorage.org/webt/vr/z1/f5/vrz1f5ckzywcr7lsroumvr_urfo.gif)](https://habrastorage.org/webt/vr/z1/f5/vrz1f5ckzywcr7lsroumvr_urfo.gif)