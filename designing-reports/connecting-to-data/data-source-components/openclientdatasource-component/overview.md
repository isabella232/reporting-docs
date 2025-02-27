---
title: OpenClientDataSource Component Overview
page_title: OpenClientDataSource Component Overview
description: OpenClientDataSource Component Overview
slug: telerikreporting/designing-reports/connecting-to-data/data-source-components/openclientdatasource-component/overview
tags: overview
published: True
position: 0
previous_url: /OpenClientDataSource
---

# OpenClientDataSource Component Overview

OpenClient data source is a component dedicated to feed report data items from OpenEdge AppServer ABL procedures. In order to communicate with the AppServer the data source component uses Open Client.NET proxy class library that is generated using the OpenEdge.NET Proxy Generator tool: 

* [Preparing to generate proxies for a .NET client using ProxyGen or Batch ProxyGen](https://docs.progress.com/bundle/openedge-gui-for-dotnet-introduction-development-117/page/Preparing-to-generate-proxies-for-a-.NET-client-using-ProxyGen-or-Batch-ProxyGen.html)

* [Generating Proxies and Web Service Definitions](https://docs.progress.com/bundle/openedge-open-client-toolkit-introduction-117/page/Generating-Proxies-and-Web-Service-Definitions.html)

## Supported ABL Procedures

In order to be suitable for reporting purpose the ABL procedure has to have the following properties:

* The procedure should not be nested

* The data should be returned as OUTPUT or INPUT-OUTPUT parameter of type DATASET or DATATABLE

* In order to have data schema while designing reports it is recommended to specify schema for the DATASET/DATATABLE

## Supported developer platforms

* .NET Framework 4.0 and above 

## See Also

* [Telerik.Reporting.OpenClientDataSource](/reporting/api/Telerik.Reporting.OpenClientDataSource)

* [Preparing to generate proxies for a .NET client using ProxyGen or Batch ProxyGen](https://docs.progress.com/bundle/openedge-gui-for-dotnet-introduction-development-117/page/Preparing-to-generate-proxies-for-a-.NET-client-using-ProxyGen-or-Batch-ProxyGen.html)

* [Generating Proxies and Web Service Definitions](https://docs.progress.com/bundle/openedge-open-client-toolkit-introduction-117/page/Generating-Proxies-and-Web-Service-Definitions.html)
