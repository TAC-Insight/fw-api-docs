---
label: Using Parameters
title: Using Parameters
icon: ":grey_question:"
description: Using parameters in your custom Fast-Weigh Data Queries
order: 5
---

# :grey_question: Using Parameters

Parameters allow you to pass data in the Query URL to be used within your GraphQL statement.

Parameters are appended to the URL using "query string parameters". 

These follow a standard key=value format and are passed in the URL following a "?" character.

Multiple paramters are separated by the "&" character.

Ex: 
```
https://data.fast-weigh.dev/query/run?x-api-key=your-api-key&queryId=your-query-id&fromDate=2021-01-01&toDate=2021-02-01
```

## Required parameters

- **queryId**
- **x-api-key**
Note: This can also be passed as an HTTP header. If it is, it can be omitted from the query string params.

## Updating a query to use parameters


