---

---
# dbt supercharges SQL with macros

With dbt you can leverage the full power of Jinja templating language in your SQL scripts. These code blocks are compiled into valid statements before being executed on a target database. Let me show you some useful tricks to use with the CORE DWH module.

## Flexible row filtering

You have already seen _filter_rows()_ macro invocation in place of WHERE expression. It is used in every staging model. It helps fetch only relevant account rows and limit the number of rows for development and testing purposes if a table is large enough (especially useful for fact tables – sessions, hits, conversions).

See the [stg_ym_sessions_facts](https://github.com/kzzzr/mybi-dbt-core/blob/master/models/staging/metrika/stg_ym_sessions_facts.sql) code: