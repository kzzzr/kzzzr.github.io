---

---
## Performing custom ‘not empty’ tests

dbt is shipped with essential schema tests which I covered in one of my previous posts on CORE DWH test suite. There is plenty of additional tests and handy macros shipped with the [dbt-utils](https://github.com/fishtown-analytics/dbt-utils) package, including _equal_rowcount_, _expression_is_true_, _recency_, _at_least_one_, etc.

But still if you need something special you can easily add it yourself. For me this is signalling if one of my source tables is empty. I have added a simple macro which serves as a custom schema test [_test_not_empty()_](https://github.com/kzzzr/mybi-dbt-core/blob/master/macros/schema_tests.sql)_:_

{% highlight sql linenos %}
{% raw %}
    -- NON_EMPTY
    {% macro test_not_empty(model, column_name) %}
     
    with validation as (
     
       select
           count(1) as row_count
     
       from {{ model }}
     
    ),
     
    validation_errors as (
     
       select
           row_count
     
       from validation
       where row_count = 0
     
    )
     
    select count(*)
    from validation_errors
     
    {% endmacro %}
{% endraw %}
{% endhighlight %}    

Since it is intended to be informational only, I configure it with severity level = _warn:_

{% highlight yaml linenos %}
{% raw %}
      - name: stg_ym_sessions_facts
         tests:
           - not_empty:
               severity: warn
{% endraw %}
{% endhighlight %}               