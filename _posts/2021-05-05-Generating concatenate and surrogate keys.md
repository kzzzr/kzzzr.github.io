---

---
## Generating concatenate and surrogate keys

Surrogate keys are the central point of any Data Warehouse uniquely identifying rows and connecting facts and dimensions tables. I usually utilize hash functions of source PK columns, which comes from Data Vault 2.0 and gives us consistent results coming from various sources.

But sometimes in case of a compound key you might want to retain readability of the whole concatenated values. This is especially applicable for marketing analytics, where the row key might contain utm_source, utm_medium, utm_campaign, ads_group_id, keyword_id.

Human-readable values help debug, while hashed surrogate keys are still used for merging data. See the example:

[![](https://habrastorage.org/webt/h9/dt/vh/h9dtvhl9acv_v6mucjz0qzenmti.png)](https://habrastorage.org/webt/h9/dt/vh/h9dtvhl9acv_v6mucjz0qzenmti.png)

Generating these keys is performed via macros [hash(), concat(), surrogate_key()](https://github.com/kzzzr/mybi-dbt-core/blob/master/macros/surrogate_key.sql):


{% highlight sql linenos %}
{% raw %}

    ------------------------
    --- MSSQL hash macro ---
    ------------------------
    {% macro hash(field) -%}
       HASHBYTES('SHA2_256', {{field}})
    {%- endmacro %}
     
    --------------------------
    --- MSSQL concat macro ---
    --------------------------
    {% macro concat(fields) -%}
       concat({{ fields|join(', ') }})
    {%- endmacro %}
     
     
    --------------------------
    --- Surrogate hash key ---
    --------------------------
    {%- macro surrogate_key(field_list) -%}
     
       {%- if varargs|length >= 1 %}
     
       {%- do exceptions.warn("Warning: the `surrogate_key` macro now takes a single list argument instead of multiple string arguments. Support for multiple string arguments will be deprecated in a future release of dbt-utils.") -%}
     
       {# first argument is not included in varargs, so add first element to field_list_xf #}
       {%- set field_list_xf = [field_list] -%}
     
       {%- for field in varargs %}
       {%- set _ = field_list_xf.append(field) -%}
       {%- endfor -%}
     
       {%- else -%}
     
       {# if using list, just set field_list_xf as field_list #}
       {%- set field_list_xf = field_list -%}
     
       {%- endif -%}
     
     
       {%- set fields = [] -%}
     
       {%- for field in field_list_xf -%}
     
           {%- set _ = fields.append(
               "coalesce(cast(" ~ field ~ " as NVARCHAR " ~ "), '')"
           ) -%}
     
           {%- if not loop.last %}
               {%- set _ = fields.append("'-'") -%}
           {%- endif -%}
     
       {%- endfor -%}
     
       {# {{ concat(fields) }} #}
       {{ mybi_dbt_core.hash(mybi_dbt_core.concat(fields)) }}
     
    {%- endmacro -%}

{% endraw %}
{% endhighlight %}

You have to be careful with `null` values if there are any and be consistent when concatenating string values with separator (dash or underscore or any other).

After compilation step it becomes something like this:

{% highlight sql linenos %}
{% raw %}

    select 
        HASHBYTES(
            'SHA2_256',
            concat(
                coalesce(cast([dt] as NVARCHAR), ''),
                '-',
                coalesce(cast([campaign_id] as NVARCHAR), ''),
                '-',
                coalesce(cast([ads_id] as NVARCHAR), '')
            )
        ) as hash_key,
        concat(
            coalesce(cast([dt] as NVARCHAR), ''),
            '-',
            coalesce(cast([campaign_id] as NVARCHAR), ''),
            '-',
            coalesce(cast([ads_id] as NVARCHAR), '')
        ) as concat_key
        
{% endraw %}
{% endhighlight %}        