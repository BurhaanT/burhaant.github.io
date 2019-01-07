---
layout: post
title: Implementing ASP.NET Web API Versioning using Custom Header and testing it in an Angular.js application
date: 2015-10-01 05:31
author: burhaan
comments: true
categories: [Uncategorized]
---
Implementing ASP.NET Web API Versioning using Custom Header and testing it in an Angular.js application
Posted by: Mahesh Sabnis , on 9/22/2015, in Category ASP.NET 
Views: 4507  Abstract: Manage ASP.NET Web API versioning using Custom request header and testing the versioning using an Angular.js application
APIs evolve. With every change, data gets modified which clients may or may not be able to handle appropriately. One solution is to keep our APIs backward compatible by providing different URIs for different versions of our application. So /products which was pointing to /v1/products now starts pointing to /v2/products, but users who want to, can still access the previous version using /v1/products or products?ver=1 depending on how your url redirection is setup. Another solution is to keep the URL constant and use custom HTTP headers to tell users of the version. There are pros and cons to both the approaches.

In ASP.NET Web API, Versioning is needed in cases when we change the server-side logic in methods of our service. In such a case, if we change the hosting URL for the WEB API, then existing clients will suffer and the service provider will face lots of criticism. So how do we manage the WEB API versioning and changes by keeping the same URL as far as possible? The WEB API Controller versioning is possible using the following methods:
<ul>
Using Query String
Custom Request Header
Adding new controller</ul>
In this article, we will implement ASP.NET Web API versioning using Custom Request Header.
<br/>
Read the article: <a href='http://www.dotnetcurry.com/aspnet/1185/aspnet-web-api-versioning-angularjs-app' target='_blank'>Implementing ASP.NET Web API Versioning using Custom Header and testing it in an Angular.js application</a>.
