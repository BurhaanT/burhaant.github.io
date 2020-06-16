---
layout: post
title: Implementing context resolutions in GraphQL 
date: 2020-06-05 10:00
author: burhaan
comments: true
categories: [GraphQL]
share-img: /img/content/graphQL/graphQL.png
---

[_GraphQL is an open-source data query and manipulation language for APIs, and a runtime for fulfilling queries with existing data_](https://en.wikipedia.org/wiki/GraphQL)

Graph provides the flexibility to concurrently resolve queries (and sequentially resolve mutations) by executing a field resolution that returns some data. 
What this means is that you can have an example query that looks like this:

```
query {
    Person {
        Firstname
        Lastname
        AddressDetails {
            HouseNumber
            StreetName
        }
    }
}
```
Which gets executed on the backend through this sort of implementation:

```
    Field<Person>("Person")
        _resolve_: async context => {
            //execute some code to return the relevant object
            //...
        } 
```

The actual resolution can span multiple lines and may include things like making a call to an api or retrieving some information from your repository, and may grow from a few lines to many hundreds of lines. Further, there may be multiple resolutions in a single file that resolves different queries.
Bear in mind that if you're making some sort of call, there's a good chance you've injected some interface implementation into your class via the constructor:

```
public PersonQuery(IUserRepo usrRepo, IUserApiRequestor usrApiRequestor, IAuthentication auth....)
```

Depending on the number of interfaces required and the number of queries resolved, this too may grow beyond easy management.

In addition to this, how exactly do you unit test this resolution? - I mean there may be a way but I haven't found it yet.

Given this, I've found that a simpler and cleaner implmentation is to inject what I call a _Resolver_ that effectovely executes the requisite resolution.

Your constructor then changes to look like:

```
public PersonQuery(IUserResolver usrResolver)
```

The implmentation of the resolver may look as follows:
```
public class UserResolver: IUserResolver
{
    public UserResolver(IUserRepo usrRepo, IUserApiRequestor usrApiRequestor, IAuthentication auth...)
    {}

    public async Task<object> Resolve(ResolveFieldContext<object> context)
    {
        //Execute some code to:
        //Call your repository, a local function or an API
    }
}
```

In turn, the actual GraphQL field resolution now becomes:
```
    Field<Person>("Person")
        _resolve_: async context => await usrResolver.Resolve(context)
```

The _resolver_ implmentation can now be mocked and unit tested too. The context resolution is trimmed to a single, easy to understand line of code and the implmentation contructor handles all the required interfaces it needs versus the resolutions constructor taking in all interfaces for every query.

Everyone's happy and it's as easy as that.

