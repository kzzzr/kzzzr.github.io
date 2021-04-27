---

---
Models prefixed with **_stg_ _**comprise the so-called [_staging area_](https://github.com/kzzzr/mybi-dbt-core/tree/master/models/staging). Source tables are green-coloured, staging tables are blue on the picture below (click to expand):

[![](https://habrastorage.org/webt/ty/4y/eg/ty4yeg_ijmb3nemejt0cunglxh0.gif)](https://habrastorage.org/webt/ty/4y/eg/ty4yeg_ijmb3nemejt0cunglxh0.gif)

Since I want to use dbt to deploy my projects I would like to build a common ground. In other words that would be staging area pointing to source tables covered with data tests thoroughly.

Let us examine the source code for [stg_ym_sessions_facts](https://github.com/kzzzr/mybi-dbt-core/blob/master/models/staging/metrika/stg_ym_sessions_facts.sql):

{% highlight sql %}
{% raw %}

with source as (
 
select
 
     {{ surrogate_key(["account_id",
       "clientids_id",
       "dates_id",
       "traffic_id",
       "locations_id",
       "devices_id"])
       }} as id
   , f.account_id
   , f.clientids_id
   , f.dates_id
   , f.traffic_id
   , f.locations_id
   , f.devices_id
   , f.sessions
   , f.bounces
   , f.pageviews
   , f.duration
   , gd.dt
   , gd.ts
 
from {{ source('metrika', 'sessions_facts') }} as f
   left join {{ ref('stg_general_dates') }} as gd
       on gd.id = f.dates_id
 
{{ filter_rows(
   account_id=var('account_id_metrika'),
   last_number_of_days=true,
   ts_field='dt'
) }}
 
)
 
select * from source

{% endraw %}
{% endhighlight %}