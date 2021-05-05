---

---
## Performing custom ‘not empty’ tests

dbt is shipped with essential schema tests which I covered in one of my previous posts on CORE DWH test suite. There is plenty of additional tests and handy macros shipped with the [dbt-utils](https://github.com/fishtown-analytics/dbt-utils) package, including _equal_rowcount_, _expression_is_true_, _recency_, _at_least_one_, etc.

But still if you need something special you can easily add it yourself. For me this is signalling if one of my source tables is empty. I have added a simple macro which serves as a custom schema test [_test_not_empty()_](https://github.com/kzzzr/mybi-dbt-core/blob/master/macros/schema_tests.sql)_:_