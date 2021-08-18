---
label: Using Parameters
title: Using Parameters
icon: ":grey_question:"
description: Using parameters in your custom Fast-Weigh Data Queries
order: 5
---

# :grey_question: Using Parameters

Parameters allow you to pass data in the Query URL to be used within your GraphQL statement.

## Common parameter types 
- Int
- String
- Boolean
- date (yes, this one is lowercase)

## Date formatting
It's important to note that dates must be formatted as YYYY-MM-DD when being passed as parameters.

## Parameter arrays

Parameter arrays can be used for filters like the "_in" filter and are defined with []'s.

- [String!]
- [Int!]

**IMPORTANT NAMING CONVENTION**

To use parameter arrays our query engine needs to know the parameter is intended as an array. As such, you must follow a naming convention for each type:

- [String!]: Must include "Array". Example: CustomerNameArray
- [Int!]: Must include "IntArray". Example: CustomerKeyIntArray

In the query variable editor you'll need to wrap the values in []'s as well.

```json
{
  "CustomerNameArray": ["ABC","XYZ"]
  "CustomerKeyIntArray": [1,2,3]
}
```

Please note the []'s are only a requirement for the query variable editor test values. You'll notice that the brackets are not included in the query URL sample at the top of your query screen. []'s are not supported in the actual URL call.

## Updating a query to use parameters

<div style="position: relative; padding-bottom: 56.25%; height: 0;"><iframe src="https://www.loom.com/embed/536a8a07360f44a7aa77da344bd84877" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe></div>


