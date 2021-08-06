---
label: Crash Course
title: Crash Course
icon: ":school:"
description: GraphQL crash course
order: 50
---

# :school: GraphQL Crash Course

If you are not familiar with GraphQL, no worries. It's easy to learn. If you have experience working with SQL you'll feel right at home. The syntax also looks similar to a JSON document if you are used to working with responses from REST endpoints. In fact, the structure of the query is simply the format of the JSON response you'd like to receive back.

Another nice feature of GraphQL is schema introspection. That's a fancy way of saying that the fields are self documenting. This allows us to build things like the [Fast-Weigh Data Query browser based editor](https://data.fast-weigh.dev) that provides autocompletion and field documentation that is always up to date with the latest API changes. (:beers:cheers to no documentation lag!)

## GraphQL basics

With GraphQL, you're still issuing a request to an HTTP endpoint just like you would for a REST API. The big difference is there is a single endpoint that services all requests.

For Fast-Weigh, the endpoint looks like this:

```
https://<your-servername-here>.fast-weigh.dev/v1/graphql
```

#### Calling the endpoint directly

If you calling the GraphQL endpoint directly, all requests must be POST requests with a JSON encoded query as the body of the request (Content-Type: application/json)

#### Using Fast-Weigh Data Queries

If you are using GraphQL to build Fast-Weigh Data Queries, instructions on how to call those queries can be found [here](/data-queries/calling-queries).

## Writing a simple query

Here's a simple query that requests a few fields from the Customer GraphQL type:

```gql
query customers {
  Customer {
    CustomerID
    CustomerName
    ContactName
    ContactPhone
  }
}
```

Let's break that syntax into it's parts:

```
<ActionType> <QueryName> {
  <Type> {
    <TypeField1>
    <TypeField2>
  }
}
```

- **ActionType**: GraphQL endpoints support the following "ActionTypes". Note that Fast-Weigh only supports the "query" type at this time.
    - query: :white_check_mark: Similar to a SQL SELECT statement
    - subscription: :x: A websocket based live data stream
    - mutation: :x: Similar to a SQL UPDATE or INSERT statement
- **QueryName**: This is the identifier for the query. You can give it any name you'd like. In the GET CUSTOMERS example the QueryName could have been "getAllCustomers", "get-customers-list", or anything else you could dream up. I went with "customers" for brevity.
- **Type**: If you are familiar with SQL, you can think of this as the corresponding SQL table.
- **TypeField**: Again, if you are familiar with SQL, you can think of this as the columns within the corresponding SQL table.

The response back from this query will be a JSON response matching the format requested.

**_If a requested field has no value, it will be omitted from the response. This compresses the payload size and increases performance._**

## Finding the available fields

GraphQL introspection powers autocomplete within our [recommended tooling](/tooling) and our [browser based Data Queries editor](https://data.fast-weigh.dev).

These tools also allow you to browser the schema, as explained [here](/graphql/schema).

## Joining via relationships

Relationships in GraphQL can be of 2 types:
- Object: 1 to 1
- Array: 1 to many

#### Object relationships

Object relationships will embed a single subdocument in the response.

For example, the Customer type has an Object Relationship with a TaxCode.

So a query like this:

```gql
query customers_and_orders {
  Customer {
    CustomerID
    CustomerName
    TaxCode {
      Code
    }
  }
}
```

Will result in a response that looks like this:

```json
{
  "data": {
    "Customer": [
      {
        "CustomerID": "",
        "CustomerName": "",
        "TaxCode: {
          Code: ""
        }
      }
    ]
  }
}
```

#### Array relationships

Array relationships will nest an list of objects in the response.

As an example, a Customer may have many Orders.

So the Customer type has an array relationship with orders. And querying for the Orders would look some like this:

```gql
query customers_and_orders {
  Customer {
    CustomerID
    CustomerName
    Orders {
      OrderNumber
      Description
    }
  }
}
```

And result in:

```json
{
  "data": {
    "Customer": [
      {
        "CustomerID": "",
        "CustomerName": "",
        "Orders: [
          {
            OrderNumber: 1,
            Description: "",
          },
          {
            OrderNumber: 2,
            Description: "",
          }
        ]
      }
    ]
  }
}

```

## Filtering data

Until now, the queries we've written would have returned all results from the Customer table.

To filter the results, GraphQL supplies us with a few syntax options.

#### Limit & offset

This one is pretty easy. Within the Type your are querying, supply the limit argument with an integer specifying the number of results you'd like returned.

```gql
query customers_and_orders {
  Customer(limit: 10) {
    CustomerID
    CustomerName
    Orders {
      OrderNumber
      Description
      PONumber
    }
  }
}
```

Note that this only limits the Customer type to 10 results. So the nested Orders array may contain many more. You can limit on the nested Orders type in the same way.

```gql
query customers_and_orders {
  Customer(limit: 10) {
    CustomerID
    CustomerName
    Orders(limit: 10) {
      OrderNumber
      Description
      PONumber
    }
  }
}
```

If you want to limit each response but still cycle through to retrieve all of the filtered data, you can use an **offset** loop to go through the next set of 10 records.

In practice, you'd want to increment this number on each call by whatever your limit is. So the first call would be offset of 0, second call an offset of 10, then 20, 30, and so on. Once you get a payload with fewer records than your limit, you'll know there are no more records to retrieve and can stop issuing queries.

```gql
query customers_and_orders {
  Customer(limit: 10, offset: 10) {
    CustomerID
    CustomerName
    Orders {
      OrderNumber
      Description
      PONumber
    }
  }
}
```

#### Order by

Building on the query above, you can order the results by adding an "order_by" field to your arguments.

```gql
query customers_and_orders {
  Customer(
    limit: 10,
    order_by: {CustomerID: asc}
  ) {
    CustomerID
    CustomerName
    Orders {
      OrderNumber
      Description
      PONumber
    }
  }
}
```

Notice you must pass an object to the "order_by" argument with the field you'd like to order, as well as the sort direction.

The following sort directions are permited:
- asc
- asc_nulls_first
- asc_nulls_last
- desc
- desc_nulls_first
- desc_nulls_last

#### Where clauses

Let's modify the query once again and supply a "where" argument.

The "where" argument can apply a number of different filters to a given field.

- **_eq**: equal to
- **_neq**: not equal to
- **_gt**: greater than
- **_gte**: great than or equal to
- **_lt**: less than
- **_lte**: less than or equal to
- **_in**: exists in array of values
- **_nin**: does not exist in array of values
- **_like**: matches some part of the given string
- **_nlike**: does not match some part of the - given string
- **_is_null**: ...is null :laughing:

Here's an example of querying for just Customers in the State of Tennessee:

```gql
query customers_and_orders {
  Customer(
    where: {State: {_eq: "TN"}}
  ) {
    CustomerID
    CustomerName
    Orders {
      OrderNumber
      Description
      PONumber
    }
  }
}
```

And here's an example of passing an array of States to the filter using the "_in" clause:

```gql
query customers_and_orders {
  Customer(
    where: {State: {_in: ["TN","KY","GA"]}}
  ) {
    CustomerID
    CustomerName
    Orders {
      OrderNumber
      Description
      PONumber
    }
  }
}
```

Remember that filters can also be used on nested Types as well. So if we wanted to filter for just Active orders, we could write:

```gql
query customers_and_orders {
  Customer(
    where: {State: {_in: ["TN","KY","GA"]}}
  ) {
    CustomerID
    CustomerName
    Orders(
      where: {Status: {_eq: "A"}}
    ) {
      OrderNumber
      Description
      PONumber
    }
  }
}
```

## Compound queries

Completely separate requests can also be merged within a single query. This is useful if you need to bring in data from multiple tables and only want to make a single HTTP call to do so. 

Typically compound queries would only be used to stitch together the responses of smaller queries. You would not want to combine multiple large payload queries in the same request/response as this can be quite taxing to fetch and parse due to query complexity and payload size.

Here's an example request for some base resources within Fast-Weigh. These lists are generally quite short and are good candidates for compound queries if applicable to your use case.

```gql
query sample_compound_query {
  Region {
    RegionName
    RegionDescription
  }
  Salesperson {
    Name
    Email
  }
  TaxCode {
    Code
    MaterialPercent
    FreightPercent
    SurchargePercent
  }
  Product {
    ProductID
    ProductDescription
  }
}
```

Results in:

```json
{
  "data": {
    "Region": [
      {
        "RegionName": "",
        "RegionDescription": ""
      },
    ],
    "Salesperson": [
      {
        "Name": "",
        "Email": ""
      },
    ],
    "TaxCode": [
      {
        "Code": "",
        "MaterialPercent": 0.0000000,
        "FreightPercent": 0.0000000,
        "SurchargePercent": 0.0000000
      },
    ],
    "Product": [
      {
        "ProductID": "",
        "ProductDescription": ""
      },
    ]
  }
}
```

## Putting queries into practice

Once you've got the query basics down, it's time to start experimenting and using them for your own use case.

You can do this easily using our [Fast-Weigh Data Queries](/data-queries/overview).

Or start [quering via HTTP and code](/graphql/for-developers) in your programming language of choice.