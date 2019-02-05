---
layout: post
title: An Introduction to Azure Active Directory (AAD)
date: 2019-01-21 10:04
author: burhaan
comments: true
categories: [Azure]
---

I've been creating applications for a long time and more often than not, there's been a requirements for some level of security.
This ranges from data in transit to data at rest as well as relevant authorisation.
In most cases, this functionality has been provided out the box and when it came to authentication, anything more than the implementation of a simple membership provider has been someone else's issue.

I've done a bit of playing around with Azure Active Directory (AAD) but nothing significant enough for me to classify it as a skill. In other words, most of the time I'd be guessing...

However, having a service that does the heavy lifting for me is alluring. When I start thinking about multifactor authentication, oAuth and oAuth Delegation, policies (and the list goes on), it just seems the AAD is an easier option. When you've been trying to get an application out the door as quickly as possible and with least amount of effort while maintaining it's integrity, easy (and battle tested) is good.

So, on to it. Hopefully, my learnings can make your life a little easier and I can save you the effort of trawling through all the documents and videos out there to get you up and running as quickly as possible.

**Azure Active Directory vs. Microsoft Identity Platform**  
I start with "What is Azure Active Directory?", and almost the first paragraph on Microsoft docs brings up [Microsoft Identity Platform (aka. Azure Active Directory for Developers)](https://docs.microsoft.com/en-us/azure/active-directory/develop/index){:target="\_blank"}.

"Sheesh!" you say. "I've only just started and it's changed already!" (I hear ya!...)

Keep calm and carry on! This seems to be nothing more than marketing that's meant to make AAD more appealing to developers. As far as I can tell, there's nothing new insofar as AAD is concerned but it does provide some handy SDK's and libraries that help leverage AAD within your application (for a number of plaftforms and languages)

![keep calm](/img/Keep-Calm-and-Carry-On-Navy-Blue-Poster.jpg)

Moving on...

## What is Azure Active Directory in a nutshell

The first thing you'll want to do, before you go any further is skim over some of the terminology related to AAD - take a look through [this table](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis#terminology){:target="\_blank"}

AAD is Microsoft's cloud-based identity and access management service. This post focuses on the applicability of AAD to bespoke software access - in the forms of Authentication and Authorisation.
However, AAD can handle a number of other things including Office365, external and internal resources, etc. You can see more [here](https://docs.microsoft.com/en-us/office365/enterprise/microsoft-cloud-it-architecture-resources#identity){:target="\_blank"}.

Okay, so using AAD I can esure that only specific _people_ can access a resoure (Authentication) and can only perform actions they are allowed to (Authorisation).

## Why would I use Azure Active Directory

AAD is not your only option when thinking about Authentication and Authorisation. Azure hasn't always been around and there are on-prem options available as well as options available from other cloud providers such as [Amazon](https://aws.amazon.com/iam/?nc2=type_a){:target="\_blank"} and [Google Cloud](https://cloud.google.com/identity/){:target="\_blank"}.

Identity and access management can be a complex domain and while I haven't setup something like this from scratch myself, I can't say I'd want to. I enjoy building apps; I enjoy writing code. Sometimes my apps need to be secure and that security needs to be reliable, always available and compliant. I want to be able to incorporate these elements into my application as quickly as possible with the least amount of friction.

I'm sure other cloud providers are equally as capable but I am more familiar with the Microsoft stack so I'll just go with that - justification enough for me.

I need to specifically mention [Yaser's blog](https://mehraban.com.au){:taget="\_blank"} here - An excellent resource and I've probably used it a few times when I've got stuck. In particular, he's got a great post on [using Azure B2C as your identity manager](https://mehraban.com.au/2017/08/16/using-azure-b2c-identity-manager-part-1/){:target="\_blank"}.

## The Outcome

I'd like to start here. A contrived example will help me get to an end goal that makes sense.
As such, my purpose is to create a simple API that requires Authentication. I'd like my API consumer to be able to provide a username and password that's authenticated against AAD. AAD should then return a token and that token is passed to my API in the header (i.e. Bearer Authentication) and verifiable by my API. If the token in valid, allow the consumer to interact with my API otherwise, deny!

## Setting up an AAD Tenant

Before we even get going, I need to ensure I have an AAD tenant that I can work against (wondering what this is? - Take a look at the aforementioned table ;) ).  
Then, follow the [steps here](https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-tenant){:target="\_blank"} to setup the tenant.

I've setup my tenant with the following details:

`Organisation: burhaantargett`  
`Initial domain name: burhaantargett.onmicrosoft.com`

## Testing out Azure Active Directory Authentication using Postman

Following the instructions on setting up AAD is a simple process. However, before moving on I'd like to make sure that everything so far works as expected so I dont (later) start hunting down phantom authentication errors in code.
As such, at this stage I want to test, independant of code, and [Postman](https://www.getpostman.com/){:target="\_blank"} is a useful otion.  
All I want to do at this stage it successfully make a call to AAD with a username and password and receive a token back.

### Registering Postman

For any application to be able to communicate with AAD, it needs to be registered.  
_Why do application need to be registered you ask? Take a read of [this](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-how-applications-are-added#why-do-applications-integrate-with-azure-ad){:target="\_blank"}_

Registering your application (Postman in this case) is a simple process:

1. Navigate to `Azue Active Directory` within the Azure Portal
2. Click on `App registrations`
3. Click on `New application registration`

![Azure AD Registration Steps](/img/content/AzureAD-App-Registration-Steps.png)

## Setting up a Web API Project

I want to get an API project up and running quickly so I'll run `dotnet new webapi`. This gives me a base from which to start. Done and dusted...ready to go!

## Authenticating an API

### Additional Resources

If you're looking for specific developer tutorials, take a look at [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/index){:target="\_blank"}
