---
layout: post
title: Azure Functions with Configuration and Dependency Injection
date: 2019-07-16 16:00
author: burhaan
comments: true
categories: [Azure]
---

In [my last post](/2019-06-13-dippping-into-azure-functions) I got going with a simple azure function that could be expanded on an used in just about any application.

Naturally, as you explore the possibilities you run into a few things that you've been doing for ages in your day-to-day code and wonder how to approach these with Azure Functions. 
In this post, I want to address two such interests:
1. Dependency Injection; and
2. Reading configuration values

# Overview
The code in this post fetches some stock price data and returns some of that data for the given symbol.

_Source code related to this post can be found [here](https://github.com/BurhaanT/Azure-Function-Share-Price-Checker)_

## Dependency Injection
My [previous example](/2019-06-13-dippping-into-azure-functions) made a call to retrieve weather information by instantiating an HttpClient then making a call to a REST endpoint, returning the result.


When we consider how we to test this functionality we immediately run into a problem in that we can't mock this HttpClient - which is not the end of the world in our little sample application, but not something you want to do in production quality code. Particularly if our little function ended up posting some information that changed the state of another system or data.

Assuming our Function class is named `CheckPrice`, lets start off my creating a constructor that accepts some interfaces to be passed in:
```
public CheckPrice(IHttpClientFactory httpClientFactory, IConfiguration config)
{
    _client = httpClientFactory.CreateClient();
    _config = config;
    _apiKey = _config["StockTickerApiKey"];
    _stockPriceUrl = $"https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol={symbol}&apiKey={_apiKey}";
}
```

In the method above, we accept an `IHttpClientFactory` and `IConfiguration`. The body of the method is unimportant at this stage but worth noting that it sets the values of a few variables.

But how are these injected and implemented? In a .Net or .Net Core application we'd usually have a `startup` class where dependencies are registered in a container and accessible when required.

In Azure Functions, we can do the same thing. Create a 'Startup.cs` class (technically it could be called whatever you like):
```
[assembly: FunctionsStartup(typeof(check_share_price.Startup))]
namespace check_share_price
{
    internal class Startup: FunctionsStartup
    {
        public Startup()
        {

        }

        public override void Configure(IFunctionsHostBuilder builder)
        {
            ...
        }
    }
}
```

Three things to notice here are:
- The first line which introduces this as a `FunctionStartup` class
- Implementation of the abstract `FunctionsStartup` which provides an abstract `Configure` method that we override; and
- The implementation `override void Configure(IFunctionsHostBuilder builder)` where IFunctionHostBuilder is automatically injected in by the framework

Registering the dependencies is now simple within the `Configure` method
```
builder.Services.AddHttpClient();
builder.Services.AddSingleton(configuration);
```

So, `CheckPrice(IHttpClientFactory httpClientFactory, IConfiguration config)` will now have the correct dependencies injected.

## Configuration
When you initially create your function, you're provided with a `local.settings.json` file which you can use to store whatever values you like. 
This is useful when running in a development machine where you may not want to create environment variables that means switching out of you IDE. You may also want to check some of these setting's into source control (outside this topic however, don't check any secrets into source control!)

In addition to this, you want to ensure that in a production environment these settings (especially secret information) is read out of environment variables.

In other words, when developing you'd like to read keys from some sort of local file but also environment variables when that exists.

```
var configBuilder = new ConfigurationBuilder()
                .AddEnvironmentVariables()
                .AddJsonFile("local.settings.json", optional: true, reloadOnChange: true);
IConfiguration configuration = configBuilder.Build();
```

Running this locally will fail with an error indicating that the settings (from the local file) cant be found. The reason for this is because it doesn't know where to look for it. Ordinarily, we'd provide this information to the `configBuilder` through the `ExecutionContext`:
```
public static async Task<IActionResult> Run([HttpTrigger(..., *ExecutionContext context*)
{
    var config = new ConfigurationBuilder()
        .SetBasePath(context.FunctionAppDirectory)
        ...
}
```

This `ExecutionContext` is available to our `Run` function and will be automatically injected however, the same is not true of the `Startup` class we've introduced and the framework will not inject it into our `Configure` method.

One workaround for this issue is to introduce:
```
var localRoot = Environment.GetEnvironmentVariable("AzureWebJobsScriptRoot");
var azureRoot = $"{Environment.GetEnvironmentVariable("HOME")}/site/wwwroot";

var actualRoot = localRoot ?? azureRoot;

var configBuilder = new ConfigurationBuilder()
    .SetBasePath(actualRoot)
```

The complete `Startup` class now looks like this:
```
internal class Startup: FunctionsStartup
{
    public Startup()
    {

    }


    public override void Configure(IFunctionsHostBuilder builder)
    {
        var localRoot = Environment.GetEnvironmentVariable("AzureWebJobsScriptRoot");
        var azureRoot = $"{Environment.GetEnvironmentVariable("HOME")}/site/wwwroot";

        var actualRoot = localRoot ?? azureRoot;

        var configBuilder = new ConfigurationBuilder()
            .SetBasePath(actualRoot)
            .AddEnvironmentVariables()
            .AddJsonFile("local.settings.json", optional: true, reloadOnChange: true);
        IConfiguration configuration = configBuilder.Build();


        builder.Services.AddHttpClient();
        builder.Services.AddSingleton(configuration);
    }
}
```

And our `CheckPrice` constructor will now work by accepting these _injected_ parameters:
```
public CheckPrice(IHttpClientFactory httpClientFactory, IConfiguration config)
{
    ...
}
```

There we have it, we have an more robust and testable Azure Function. 

### Additional Resources
- <https://docs.microsoft.com/en-us/azure/azure-functions/functions-dotnet-dependency-injection>