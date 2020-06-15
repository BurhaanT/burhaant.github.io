---
layout: post
title: Using enumerated types in GraphQL mutations 
date: 2020-06-15 10:00
author: burhaan
comments: true
categories: [GraphQL]
---

Mutations in GraphQL allow us to update data on our back-end. As with any other service, we want to ensure that any data we receive is in an expected and consistent format, constrained by rules we've implemented.

With reference types such as strings, that can be tricky. An empty string is still a string and that means we're need to consider additional code to validate those inputs. However in the case of strings that can be implemented as enumerated types, GraphQL can go some way in alleviating that additional validation semi-automatically.

Using the example of titles - i.e. Mr, Mrs, Ms, Dr, Proff, etc.... 

We can implement these as enumerated types in GraphQL as follows:

**1. Create an enumerated type**
```
public enum Title
{
    Mr,
    Mrs,
    Ms,
    Dr,
    Proff
} 
```

**2. Create an enum type that GraphQL understands based on the enum you've created**
```
public class TitleType: EnumerationGraphType<Title>
{
    public TitleType()
    {
        Name = nameof(Title);
    }
}
```

**3. Implement the type in your mutation**
```
.
.
.
Field<TitleType>(nameof(TitleType));
```

Now only valid enumeration values will be accepted with an appropriate message indicating such to the user if the wrong value is supplied.

