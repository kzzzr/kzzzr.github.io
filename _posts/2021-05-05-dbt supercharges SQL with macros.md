---

---
# dbt supercharges SQL with macros

With dbt you can leverage the full power of Jinja templating language in your SQL scripts. These code blocks are compiled into valid statements before being executed on a target database. Let me show you some useful tricks to use with the CORE DWH module.

## Flexible row filtering

You have already seen _`filter_rows()`_ macro invocation in place of WHERE expression. It is used in every staging model. It helps fetch only relevant account rows and limit the number of rows for development and testing purposes if a table is large enough (especially useful for fact tables – sessions, hits, conversions).

See the [stg_ym_sessions_facts](https://github.com/kzzzr/mybi-dbt-core/blob/master/models/staging/metrika/stg_ym_sessions_facts.sql) code:

{% highlight sql linenos %}
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

It compiles to a target script as follows:

{% highlight sql linenos %}
{% raw %}

with source as (

select

HASHBYTES(

    'SHA2_256',
    concat(
        coalesce(cast(account_id as NVARCHAR), ''),
        '-',
        coalesce(cast(clientids_id as NVARCHAR), ''),
        '-',
        coalesce(cast(dates_id as NVARCHAR), ''),
        '-',
        coalesce(cast(traffic_id as NVARCHAR), ''),
        '-',
        coalesce(cast(locations_id as NVARCHAR), ''),
        '-',
        coalesce(cast(devices_id as NVARCHAR), '')
    )
) as id
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

from "master"."core_ci"."metrika_sessions_facts" as f
left join "master"."core_ci_staging"."stg_general_dates" as gd
on gd.id = f.dates_id

where 1 = 1
and account_id in (21600)
and 1 = 1

)

select * from source

{% endraw %}
{% endhighlight %}

Here is the source code of macro [filter_rows](https://github.com/kzzzr/mybi-dbt-core/blob/master/macros/filter_rows.sql) itself:

{% highlight sql linenos %}
{% raw %}

-- filter data for selected accounts, resize for dev, ci pipelines
{% macro filter_rows(
   account_id=none,
   last_number_of_days=none,
   ts_field='dt'
) -%}
  
   {#- prepare expression to filter on according account_id -#}
   {% if account_id -%}
       {%- set filter_account_id = 'account_id in (' ~ account_id ~ ')' -%}
   {% else -%}
       {%- set filter_account_id = '1 = 1' -%}
   {%- endif -%}
 
   {#- prepare expression to filter only last N days of data (e.g. last 3 days) -#}
   {%- if target.name in ['dev', 'ci'] and last_number_of_days -%}
       {%- set limit_rows = ts_field ~ ' >= dateadd(day, ' ~ -var('filter_days_of_data') ~ ', convert(date, getdate()))' -%}
   {%- else -%}
       {%- set limit_rows = '1 = 1' -%}
   {%- endif -%}
 
   {#- prepare final filter expression -#}
   where 1 = 1
       and {{ filter_account_id }}
       and {{ limit_rows }}
 
{%- endmacro -%}

{% endraw %}
{% endhighlight %}

As a result WHERE expression consists of several parts:

* Filtering on account id if one is passed (or using 1 = 1 if none is passed) 
* Limit the number of rows to 3 last days for `dev` and `ci` environments where turned on (fact tables)

Source systems account identifiers are supplied at project level in [dbt_project.yml](https://github.com/kzzzr/mybi-dbt-core/blob/master/dbt_project.yml) as variables:

{% highlight yaml linenos %}
{% raw %}
vars:
   filter_days_of_data: 3
   mybi_dbt_core:
       account_id_metrika: null
       account_id_b24: null
       account_id_direct: null
       account_id_facebook: null
       account_id_adwords: null
       account_id_mytarget: null
       account_id_amocrm: null       
       account_id_ga: null
{% endraw %}
{% endhighlight %}

When you import the CORE DWH module you simply override these variables with desired values.