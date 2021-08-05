---
label: Calling Queries
title: Calling Queries
icon: ":telephone_receiver:"
description: Calling your custom Fast-Weigh data queries
order: 1
---

# :telephone_receiver: Calling Queries

Calling the Data Queries is as simple as calling a URL.

It can even be called directly in a web browser.

## Query Parameters

Parameters are appended to the URL using "query string parameters". 

These follow a standard key=value format and are passed in the URL following a "?" character.

### Required parameters

- **queryId**
- **x-api-key**
Note: This can also be passed as an HTTP header. If it is, it can be omitted from the query string params.

### Other parameters

Add other parameters as required by your GraphQL query to the URL with the "&" symbol.

Ex: fromDate and toDate params
```
https://data.fast-weigh.dev/query/run?x-api-key=your-api-key&queryId=your-query-id&fromDate=2021-01-01&toDate=2021-02-01
```

## Calling our example query
<div style="position: relative; padding-bottom: 66.66666666666666%; height: 0;"><iframe src="https://www.loom.com/embed/d1eaecec47da430b9302aa558c8a0daa" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe></div>