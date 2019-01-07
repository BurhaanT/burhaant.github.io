---
layout: post
title: Unusual Ways of Boosting Up App Performance. Lambdas and LINQs 
date: 2017-01-11 16:59
author: burhaan
comments: true
categories: [Uncategorized]
---
<blockquote>Lambda expressions are a very powerful .NET feature that can significantly simplify your code in particular cases. Unfortunately, convenience has its price. Wrong usage of lambdas can significantly impact app performance. Let’s look at what exactly can go wrong.The trick is in how lambdas work. To implement a lambda (which is a sort of a local function), the compiler has to create a delegate. Obviously, each time a lambda is called, a delegate is created as well. This means that if the lambda stays on a hot path (is called frequently), it will generate huge memory traffic.Is there anything we can do? Fortunately, .NET developers have already thought about this and implemented a caching mechanism for delegates.</blockquote><p>Source: <em><a href="https://blog.jetbrains.com/dotnet/2014/07/24/unusual-ways-of-boosting-up-app-performance-lambdas-and-linqs/" target="_blank">Unusual Ways of Boosting Up App Performance. Lambdas and LINQs | ReSharper Ultimate Blog</a></em></p>
