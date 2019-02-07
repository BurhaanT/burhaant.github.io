---
layout: post
title: Using Azure Active Directory to Provide Authentication for an API (Part 2)
date: 2019-02-05 10:04
author: burhaan
comments: true
categories: [Azure]
---

In [Part 1](/2019-02-11-using-azure-active-directory-to-provide-authentication-for-an-api-part-1) of this post, I started by setting up an Azure Active Directory (AAD) tenant and showing how to extract the relevant details from the portal so I can make a call to authenticate via an external application (i.e. [Postman](https://www.getpostman.com/){:target="\_blank"}). As part of that exercise I received a token back from AAD which I can use to call my API.

Now that I know my tenant works the way I anticipated, I want to ensure that I can Authenticate when calling an API endpoint. Again, I'll use Postman to call my endpoint. How do I ensure that my caller is authenticated? Because it will have an valid Bearer token in the header of the request.

To better illustrate this Abstract Protocol Flow, take a look at the diagram below ([reproduced from Digital Ocean](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2){:target="\_blank"}):

![Abstract protocal flow](/img/content/abstract_flow.png)

Here is a more detailed explanation of the steps in the diagram:

1. The application requests authorization to access service resources from the user
2. If the user authorized the request, the application receives an authorization grant
3. The application requests an access token from the authorization server (API) by presenting authentication of its own identity, and the authorization grant
4. If the application identity is authenticated and the authorization grant is valid, the authorization server (API) issues an access token to the application. Authorization is complete.
5. The application requests the resource from the resource server (API) and presents the access token for authentication
6. If the access token is valid, the resource server (API) serves the resource to the application

###################################
Remember that to be able to call AAD from Postman, I had to register Postman within AAD. But, how can I use the token for that application to call my API endpoint which is not associated or registered anywhere just yet?
###################################

**The repo containing the code for this post can be found [here](https://github.com/BurhaanT/spike-azure-active-directory){:target="\_blank"}**

## Securing an API endpoint (Authentication)
