---
layout: post
title: Implementating swagger on .NET Core WebApi using Swashbuckle
date: 2019-01-24 09:24
author: burhaan
comments: true
categories: [API, Swagger, Swashbuckle, .NET Core]
---

I quite like starting at the basics, and this entails the what's and why's of a particular technology as this helps me set the context and understand when to use a particular technology.
I'm not going to rewrite the prethora on information that relates to swagger and swashbuckle but below, you'll find useful links that will help answer these questions.

That being said, I'd like to summarise a few things...

## What is Swagger?

In simple terms, Swagger is a specification that describes the functionality provided by your API. Specifically, it's intended to address the following:

- What operations are supported by the API
- What parameters are available
- What's returned by the endpoints
- Whether authorisation is required / or not.
- Licensing and contact information

There are a number of approaches to adding this to your API.

- **Design first:** This works by designing your API endpoints using the Swagger Editor and then using [Swagger Codegen](https://swagger.io/tools/swagger-codegen/){:target="\_blank"} to generate the relevant code. It won't codegen the actual functionality but you will end up with a skeleton that can be built upon.
- **Code first:** That is, write your code and apply the relevant attributes, associated middleware, etc. **_This is the approach this post will focus on_**

## Why would I want to use Swagger?

To answer this question, we need to consider the purpose of and API first.
In most cases, an API is does not operate in isolation. It needs to connect to other applications or interfaces. In many cases, the developer(s) that consume an API are not the same developers that write the API and where it is, that developer is unlikely to remeber every operation and how the endpoints were intended to be consumed and/or what the endpoint actually does.

In a nusthell, API's are intended to be used by something or someone else.
This effort becomes really difficult when the consumer doesn't know _how_ to use it as intended - enter API documentation. Clear, up-to-date documentation makes integration tasks like this simpler and faster.

### Resources

- [An overview of Swagger and OpenAPI (MS Docs)](https://docs.microsoft.com/en-us/aspnet/core/tutorials/web-api-help-pages-using-swagger?view=aspnetcore-2.2){:target="\_blank"}
- [What is Swagger?](https://swagger.io/docs/specification/2-0/what-is-swagger/){:target="\_blank"}
- [Getting started with Swagger](https://swagger.io/tools/open-source/getting-started/){:target="\_blank"}
