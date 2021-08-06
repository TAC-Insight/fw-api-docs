---
label: For Developers
title: For Developers
icon: ":male-technologist:"
description: GraphQL crash course
order: 1
---

# :male-technologist: Info for devs

GraphQL queries are handled via standard HTTP.

## All GraphQL queries are POST requests

The first thing to understand is the GraphQL query itself is sent along in the body of the request. Since HTTP GET calls cannot include a request body, every call sent to the GraphQL server must be a POST request.

## The request body is JSON encoded

The request body itself should be JSON encoded. So the query you write will be transformed into something like this:

```json
{
    "query": "query get_customers {Customer {CustomerID CustomerName}}"
}
```

## Use libraries

Unless you are programming in a very obscure language, [there are probably client libraries that can abstract away a lot of this for you](https://graphql.org/code/). 

These clients typically let you write your query in standard GraphQL syntax, and they handle the rest.

Also, thanks to GraphQL's build-in introspection, many libraries can generate a fully-typed API client for you to use!

