---

---
The whole solution consists of several integral parts.

## Cloud ELT service

To gather data from various sources: CRM, Google Analytics, Ads services, Sheets.

Key thing here is you can easily adapt any service provider or even plug your own self-managed connectors if you wish.

![](https://habrastorage.org/webt/iv/dr/cr/ivdrcrig483ptsxo9b7d3uiqlgs.png)

## CORE DWH module

Which serves as a medium between tables created by a cloud connector and Data Marts

It also covers every source table with tests comprehensively (right now it is roughly 600% code coverage)

    packages:
     - git: "https://github.com/kzzzr/mybi-dbt-core.git"
       revision: 0.2.0

![](https://habrastorage.org/webt/sy/ak/fm/syakfm_hro46kitc6pztoydhc_q.png)

## Client DWH deployment

Where the main business logic resides.

Most commonly it is about calculating ROI, building sales funnels, clustering customers, conducting clickstream and web analytics.

Every client project incorporates specific data sources by importing the CORE DWH module.

![](https://habrastorage.org/webt/8i/ri/0j/8iri0j1_t-bwewedwivizo0wwto.png)

## Data access layer

Could be anything from well established visualizations and dashboards to modern self-service BI as well as reverse ETL tools to serve data back to operational systems (CRM and communication tools).

![](https://habrastorage.org/webt/ba/lr/rt/balrrtnspyflg0dsi27kiylkt7s.gif)