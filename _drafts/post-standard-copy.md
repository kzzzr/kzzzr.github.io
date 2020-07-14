---
title: Compressing Redshift columnar data even further with proper encodings
excerpt_separator: "<!--more-->"
categories:
- Blog
tags:
- Post Formats
- readability
- standard

---
# Basics

Amazon Redshift is database aimed primarily on analytics and OLAP queries.

One if its key features is storing data in columnar format, in other words keeping one column's data adjacent on disk.

That enables storing higher volumes of data compared to row formats due to encoding algorithms and one column's homogenous data nature (it compresses very well).

By default when you initially create and populate a table, Redshift chooses compression algorithms for every column by default.

See more about [Redshift compression encodings](https://docs.aws.amazon.com/redshift/latest/dg/c_Compression_encodings.html).

# Question default compression options

I wanted to know if it was possible to apply better encoding algorithms and compress data even further.

Luckily we have [ANALYZE COMPRESSION](https://docs.aws.amazon.com/redshift/latest/dg/r_ANALYZE_COMPRESSION.html) command:

> Performs compression analysis and produces a report with the suggested compression encoding for the tables analyzed. For each column, the report includes an estimate of the potential reduction in disk space compared to the current encoding.

```sql
ANALYZE COMPRESSION hevo.wheely_prod_orders__backup COMPROWS 1000000 ;
```

Here is the sample output:

|Table|Column|Encoding|Est_reduction_pct|
|-----|------|--------|-----------------|
|wheely_prod_orders|_id|raw|0.00|
|wheely_prod_orders|status|zstd|49.93|
|wheely_prod_orders|new_client|zstd|39.04|
|wheely_prod_orders|markup|zstd|99.66|
|wheely_prod_orders|country_code|zstd|60.67|
|wheely_prod_orders|tips|zstd|83.34|
|wheely_prod_orders|tags|zstd|35.80|

Your next steps would be:

1. CREATE a new table with proposed encodings (possibly new dist/sort keys)
2. INSERT data to a new table (perform a deep copy)
3. Perform tests and benchmarks
4. Interchange tables, delete old table

# Wrap it up into automatic macro

Now when it comes to automating routine operations or performing it on a number of tables frameworks such as [dbt](https://docs.getdbt.com/) come very handy.

It is possible to automate the whole cycle of operations and even put it on regular basis with dbt macro as simple as this:

```
  {{ redshift.compress_table('hevo',
                              'wheely_prod_orders',
                              drop_backup=False,
                              comprows=1000000) }}
```

See the description of [dbt Redshift package](https://github.com/fishtown-analytics/redshift) as well as [compression macro source code](https://github.com/fishtown-analytics/redshift/blob/master/macros/compression.sql).

Let us examine what is does underneath:

```sql
    -- ensure new table does not exist yet
    drop table if exists "hevo"."wheely_prod_orders__compressed";
    -- CREATE new table with new encodings
    create table "hevo"."wheely_prod_orders__compressed" (
        -- COLUMNS
        "_id" VARCHAR(512) encode raw  not null , 
        "status" VARCHAR(512) encode zstd , 
        "new_client" VARCHAR(512) encode zstd , 
        "sorted_at" TIMESTAMP WITHOUT TIME ZONE encode az64 ,         
        "transfer_other_zone_id" VARCHAR(512) encode lzo ,
        "ts" VARCHAR(512) encode lzo ,
        ...
        -- CONSTRAINTS
        , PRIMARY KEY (_id)        
    )
    --KEYS
        -- DIST
         diststyle key 
         distkey("_id") 
        -- SORT
         compound sortkey("_id")     
    ;

  -- perform deep copy  
  insert into "hevo"."wheely_prod_orders__compressed" (
        select * from "hevo"."wheely_prod_orders"
    );

  -- perform atomic table interchange  
  begin;
    -- drop table if exists "hevo"."wheely_prod_orders__backup" cascade;
    alter table "hevo"."wheely_prod_orders" rename to "wheely_prod_orders__backup";
    alter table "hevo"."wheely_prod_orders__compressed" rename to "wheely_prod_orders";
  commit;
```

# Assess the results

Let us dive into the results.

Compression (before, after, manual)

\+ Visualization

# Key outputs

1. Thoroughly assess the result and queries performance. The main goal is still retain query speed and performance while improve disk usage and IO.
2. Consider [ZSTD](https://github.com/facebook/zstd) compression algorithm. According to [Redshift doc page](https://docs.aws.amazon.com/redshift/latest/dg/zstd-encoding.html):

> Zstandard (ZSTD) encoding provides a high compression ratio with very good performance across diverse datasets. ZSTD works especially well with CHAR and VARCHAR columns that store a wide range of long and short strings, such as product descriptions, user comments, logs, and JSON strings. Where some algorithms, such as [Delta](https://docs.aws.amazon.com/redshift/latest/dg/c_Delta_encoding.html) encoding or [Mostly](https://docs.aws.amazon.com/redshift/latest/dg/c_MostlyN_encoding.html) encoding, can potentially use more storage space than no compression, ZSTD is very unlikely to increase disk usage.

1. Wrap it up into a macro that could be run on a regular basis

Feel free to leave any comments or questions.