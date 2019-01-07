---
layout: post
title: Error Handling in Service Broker procedures
date: 2015-12-02 02:50
author: burhaan
comments: true
categories: [Uncategorized]
---
Error handling in T-SQL traditionally has been a sort of misterious voo-doo for most developers, with it’s mixture of error severities, SET settings like XACT_ABORT, ARITHABORT or ARITHIGNORE and the options to handle the error on the server or at the client. For a long time now the best resource I know on this subject was, and perhaps still is, Erland Sommarskog set of articles at http://www.sommarskog.se/error-handling-I.html and http://www.sommarskog.se/error-handling-II.html. But the introduction of Service Broker activated procedures adds some new issues to consider when designing your application and this post is about what these issues are these and how to best cope with them.

via <a href='http://rusanu.com/2007/10/31/error-handling-in-service-broker-procedures/' target='_blank'>Error Handling in Service Broker procedures</a>.
