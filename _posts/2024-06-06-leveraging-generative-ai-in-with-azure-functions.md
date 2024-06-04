---
layout: post
title: Leveraging Generative AI in with Azure Functions
date: 2024-06-06 10:00
author: burhaan
comments: true
categories: [AI]
share-img: /img/content/AI/AI.webp
---

As .NET developers, we're always on the lookout for ways to enhance our applications, making them more efficient, intelligent, and user-friendly. One of the most exciting advancements in recent years is the rise of generative AI. By integrating generative AI into our .NET Core applications, we can add powerful features like natural language processing, content generation, and more. In this blog post, we'll explore the benefits of using generative AI and walk through a simple .NET Core application that utilizes OpenAI's services via an Azure Function.

## Benefits of Generative AI for .NET Developers
- Enhanced User Experience: Generative AI can provide more interactive and personalized experiences. For instance, chatbots powered by AI can offer more human-like interactions.
- Automated Content Creation: From generating marketing content to drafting emails, generative AI can save time and ensure consistency.
- Improved Decision Making: AI can analyze vast amounts of data to provide insights and recommendations, helping users make better decisions.
- Cost Efficiency: Automating repetitive tasks with AI reduces the need for human intervention, lowering operational costs.
- Innovation: Integrating AI opens up new possibilities for innovative features and functionalities that can set your application apart.

## Building a .NET Core Application with OpenAI and Azure Functions

Let's create a simple .NET Core application that uses OpenAI to generate text based on user input via an Azure Function.

### Step 1: Setting Up the Azure Function Project

> Install .NET Core SDK: Ensure you have the latest .NET Core SDK installed. You can download it from the [official .NET website](https://dotnet.microsoft.com/download).

#### Create a New Azure Function Project:

'''
func init GenerativeAIApp --worker-runtime dotnet
cd GenerativeAIApp
func new --template "HTTP trigger" --name GenerateText
'''

#### Add OpenAI NuGet Package:

`dotnet add package OpenAI`

### Step 2: Configure OpenAI API

> Get API Key: Sign up on the OpenAI website and get your API key.

Add your API key to the local.settings.json file:

```
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "dotnet",
    "OpenAI_ApiKey": "your-api-key-here"
  }
}
```

### Step 3: Implementing the OpenAI Service

#### Create the Service Interface:

```
public interface IOpenAIService
{
    Task<string> GenerateText(string prompt);
}
```

#### Implement the Service:

```
public class OpenAIService : IOpenAIService
{
    private readonly HttpClient _client;
    private readonly string _apiKey;

    public OpenAIService(HttpClient client, string apiKey)
    {
        _client = client;
        _apiKey = apiKey;
    }

    public async Task<string> GenerateText(string prompt)
    {
        _client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", _apiKey);

        var requestData = new
        {
            prompt,
            max_tokens = 100
        };

        var response = await _client.PostAsJsonAsync("https://api.openai.com/v1/engines/davinci-codex/completions", requestData);
        response.EnsureSuccessStatusCode();

        var result = await response.Content.ReadAsAsync<dynamic>();
        return result.choices[0].text;
    }
}
```

#### Register the Service in Startup.cs:

```
public class Startup : FunctionsStartup
{
    public override void Configure(IFunctionsHostBuilder builder)
    {
        builder.Services.AddHttpClient<IOpenAIService, OpenAIService>((s, c) =>
        {
            var apiKey = Environment.GetEnvironmentVariable("OpenAI_ApiKey");
            c.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);
        });
    }
}
```

### Step 4: Creating the Azure Function

#### Modify the Function:

```
public class GenerateText
{
    private readonly IOpenAIService _openAIService;

    public GenerateText(IOpenAIService openAIService)
    {
        _openAIService = openAIService;
    }

    [FunctionName("GenerateText")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequest req,
        ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        var data = JsonConvert.DeserializeObject<dynamic>(requestBody);
        string prompt = data?.prompt;

        if (string.IsNullOrEmpty(prompt))
        {
            return new BadRequestObjectResult("Please pass a prompt in the request body");
        }

        var result = await _openAIService.GenerateText(prompt);
        return new OkObjectResult(result);
    }
}
```

### Step 5: Testing the Azure Function

#### Run the Azure Function:

`func start`

Test the Endpoint: Use a tool like Postman to send a POST request to http://localhost:7071/api/GenerateText with a JSON body containing the prompt.

```
{
  "prompt": "Once upon a time"
}
```

You should receive a generated text response from the OpenAI API.

### Conclusion
By following these steps, you've created a simple .NET Core application using Azure Functions that leverages the power of generative AI through OpenAI's API. This example demonstrates how easy it is to enhance your applications with AI capabilities, providing more value and functionality to your users. As you continue to explore generative AI, you'll discover even more ways to innovate and improve your .NET applications. Happy coding!