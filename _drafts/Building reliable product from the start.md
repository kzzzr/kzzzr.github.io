---

---
The whole solution consists of several integral parts.

## Cloud ELT service

To gather data from various sources: CRM, Google Analytics, Ads services, Sheets.

Key thing here is you can easily adapt any service provider or even plug your own self-managed connectors if you wish.

**_![](https://lh4.googleusercontent.com/QRrSBVEcMMpsUUDtQSbBtgPRSBXhyLcfi8ywfDCNFkn9Xn_sqTvDOQSKv1p0WCGjoEXDpgr9BxQuMlrcRGPm0LimaTPDyCY-IIJbxarsOuijt5n2CVA3G-OCip_weADO9RdWbz8H =624x231)_**

## CORE DWH module

Which serves as a medium between tables created by a cloud connector and Data Marts

It also covers every source table with tests comprehensively (right now it is roughly 600% code coverage)

packages:

\- git: "[https://github.com/kzzzr/mybi-dbt-core.git](https://github.com/kzzzr/mybi-dbt-core.git "https://github.com/kzzzr/mybi-dbt-core.git")"

revision: 0.2.0

![](https://lh6.googleusercontent.com/a7518SpBiGvCDp9HN8PlQjBln6VzsMIHIyXQYzeWUZiqBBycy4ZjqdgDrDQXn53j2EQV1xnVTZgGI5oeerkej0-_DlPPR867dFhskeX8EcZIegPc2h0x-9QQCgzsTffJx3W9TpWi =624x55)

## Client DWH deployment

Where the main business logic resides.

Most commonly it is about calculating ROI, building sales funnels, clustering customers, conducting clickstream and web analytics.

Every client project incorporates specific data sources by importing the CORE DWH module.

**_![](https://lh6.googleusercontent.com/JGV1G_6oRb8zd8KQsznGYe2h4k9pRzmyb3UuAk-cW70dugTQPxfwmIyaChmb0iiIwMs4YLEUW3tF-iFZk5EIsgK5F5QJtvF_uQEMyjeeuNVty1YmK3Y4EUywo1PfCuV7CoG7P9F7 =624x245)_**

## Data access layer

Could be anything from well established visualizations and dashboards to modern self-service BI as well as reverse ETL tools to serve data back to operational systems.

**_![](https://lh5.googleusercontent.com/G7huFLuhb2mQpQa-d2IW7yZHCOXZWaLEHT06F0Ac5VW2TzfKPVCY6S5Kqrhq5rZ43hfDGD3hGAHUH5-bCK4aKgCxE2yJ8Kp7YPX8PyVBkem7gJH0YxyfBbYPVtINHRFX-jW4ngv4 =624x223)_**