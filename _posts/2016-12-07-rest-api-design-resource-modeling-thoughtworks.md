---
layout: post
title: REST API Design - Resource Modeling | ThoughtWorks
date: 2016-12-07 12:36
author: burhaan
comments: true
categories: [Uncategorized]
---
“The key abstraction of information in REST is a resource. Any information that can be named can be a resource: a document or image, a temporal service (e.g. "today's weather in Los Angeles"), a collection of other resources, a non-virtual object (e.g. a person), and so on. In other words, any concept that might be the target of an author's hypertext reference must fit within the definition of a resource. A resource is a conceptual mapping to a set of entities, not the entity that corresponds to the mapping at any particular point in time.” - Roy Fielding’s dissertation.

Resources form the nucleus of any REST API design. Resource identifiers (URI), Resource representations, API operations (using various HTTP methods), etc. are all built around the concept of Resources. It is very important to select the right resources and model the resources at the right granularity while designing the REST API so that the API consumers get the desired functionality from the APIs, the APIs behave correctly and the APIs are maintainable.

Continue reading: <a href="https://www.thoughtworks.com/insights/blog/rest-api-design-resource-modeling" target="_blank">REST API Design - Resource Modeling | ThoughtWorks</a></em>
