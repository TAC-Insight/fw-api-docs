---
label: Overview
title: Overview
icon: ":bar_chart:"
description: Write custom Fast-Weigh data queries using GraphQL
order: 100
---

# :bar_chart: Data Queries

Fast-Weigh Data Queries are custom endpoints you can use for all sorts of things. They can be the data source for a custom [embedded report](/custom-reporting/telerik), an Excel data source, or an integration endpoint.

Data Queries are mapped to a unique URL endpoint. That means any platform capable of getting data from the web can call these queries. An example Data Query endpoint looks like:

```
https://data.fast-weigh.dev/query/csv?x-api-key=your-api-key&queryId=your-queryId
```

## Getting started

You'll need an API Key. If you don't already have one, you can follow the [authentication guide](/authentication) to get one set up.

You'll also need to know your GraphQL server name. This is the first part of the URL listed on your [API Info](https://portal.fast-weigh.com/APIInfo) page under the settings gear.

#### GraphQL server name example:
GraphQL Endpoint: https://**fwt**.fast-weigh.dev/v1/graphql
Server name: **fwt**

With your key, you can log into the Data Queries site at: [https://data.fast-weigh.dev](https://data.fast-weigh.dev)

![](/static/dq-login.png)
