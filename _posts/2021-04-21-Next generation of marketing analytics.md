---

---
# Next generation of marketing analytics

Has to be reliable and interpretable yet simple.

The market is fed up with one-off solutions and hours of UI clicking to build something worthwhile. Being unstable they require a tremendous amount of effort to evolve.

On contrary, being flexible means you could easily adapt your analytics in ever-changing business environments:

* Express everything as code and version-control it
* Abstract reusable pieces of code into importable modules
* Cover with tests comprehensively

At the same time you want to maintain transparency and predictability to end users:

* Comprehensive documentation on how every number is calculated
* Visual Data Warehouse graph explorer (+ data lineage)
* Metadata access: are we up-to date with sources, which ones bring most of the data

An outstanding solution has to include Software Engineering best practices and core principles: modular structure, code + data testing, continuous integration, self-documenting code.

For these purposes I want to leverage the whole power of cloud services and modern data stack: Azure SQL, dbt, Github Actions, Metabase.

![](https://habrastorage.org/webt/tr/rg/fx/trrgfx7o121cp7zs26cbqskyif0.png)

In the next post I will cover the building blocks of my solution.