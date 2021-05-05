---

---
## Generating concatenate and surrogate keys

Surrogate keys are the central point of any Data Warehouse connecting fact and dimension tables. I usually utilize hash functions of source PK columns, which comes from Data Vault 2.0 and gives us consistent results coming from various sources.

But sometimes in case of a compound key you might want to retain readability of the whole concatenated values. This is especially applicable for marketing analytics, where the row key might contain utm_source, utm_medium, utm_campaign, ads_group_id, keyword_id.

Human-readable values help debug, while hashed surrogate keys are still used for merging data. See the example: