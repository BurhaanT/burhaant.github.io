---
layout: post
title: Using Azure Active Directory to Provide Authentication for an API (Part 2)
date: 2019-11-01 10:00
author: burhaan
comments: true
categories: [Azure]
---

> The source code for this post can be found over at [this GitHub repo](https://github.com/BurhaanT/spike-azure-active-directory).

Progressing from [Part 1](/2019-02-05-using-azure-active-directory-to-provide-authentication-for-an-api-part-1), we're at the stage where we have a Azure Active Directory (AAD) tenant set up. 
We can make calls to AAD using postman and receive an access token which we could now use to call the API.

However, how do we get the API to validate that access token, ensuring it is valid and that a call which presents that access token should actually be allowed to transact. This is the focus of this post.

## Creating an API
Feel free to use whatever you like to do this. In my case, I'll use .Net Core (v 3.0) and use the default `ValuesController` to demonstrate the point.

For now, I'll have the entire controller requiring authentication (by using the `[Authorize]` attribute). For the purpose of this post, we'll just use the `Get` action.

```
    [Authorize]
    [Route("api/[controller]")]
    [ApiController]
    public class ValuesController : ControllerBase
    {
        [HttpGet]
        public ActionResult<IEnumerable<string>> Get()
        {
            return new string[] { "value1", "value2" };
        }
    }
```

In the `settings.json` file add the following:
```
    "AzAdAuth": {
        "Domain": "https://login.microsoftonline.com/_{tenantID}_/",
        "Secret": "_{clientSecret}_"
    }
```

In the `StartUp` class within `ConfigureServices` add the following:
```
    services.AddAuthentication(options =>
    {
        options.DefaultScheme = JwtBearerDefaults.AuthenticationScheme;
    })
    .AddJwtBearer(options =>
    {
        options.Authority = $"{Configuration["AzAdAuth:Domain"]}";
        options.Audience = Configuration["Auth0:Audience"];
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.ASCII.GetBytes($"{Configuration["AzAdAuth:Secret"]}")),
            ValidateIssuer = true,
            ValidateAudience = false
        };
    }); 
```

Lastly, within `Configure` add:
```
    app.UseAuthentication();
    app.UseAuthorization();
``` 

If you ran the application now and tried calling this endpoint, you'd get a `connection refused` response. :-)

Let's setup a request in Postman and get a valid access token. 
a) Create a GET request that makes a call to `{apiHost}/api/values`
b) Under `Authorization` select `OAuth 2.0` as the Type
c) Click on `Get New Access Token` and fill out the details as below

```
Token Name: _whatever you like_
Grant Type: `Authorization Code`
Callback URL: `https://getpostman.com/oauth2/callback`
Auth URL: `https://login.microsoft.com/{tenantID}/oauth2/v2.0/authorize`
Access Token URL: `https://login.microsoft.com/{tenantID}/oauth2/v2.0/token`
Client ID: `_{clientId}_`
Client Secret: `_{clientSecret}_`
Scope: `_{Application (client) ID}_/all`
Client Authentication: `Send as Basic Auth header`
```

Click *Request Token* then *Use Token* on the following dialog and you now have a bearer token that can be used to make a request to our API. 

If you now execute the request, the values controller with return a 200 with the body of the the response presenting the return values from the controller.

Easy as that, we now have an API that's secured by Azure Active Directory and requires a Bearer token to execute.

### Additional Resources
- <https://developer.okta.com/blog/2018/03/23/token-authentication-aspnetcore-complete-guide>
- <https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-protocols-oidc>
- <https://mehmetkut.com/2017/05/protect-aspnetcore-webapi-resources-with-azure-active-directory-en/>
- <https://blogs.msdn.microsoft.com/gianlucb/2017/12/05/azure-ad-scope-based-authorization/>
- <https://stackoverflow.com/questions/47275079/request-access-token-in-postman-for-azure-ad-b2c>
- <https://docs.microsoft.com/en-us/aspnet/core/security/authentication/azure-ad-b2c-webapi?view=aspnetcore-2.2#use-postman-to-get-a-token-and-test-the-api>
- <https://tools.ietf.org/html/rfc6749#page-8>
- <https://dasith.me/2018/12/31/client-credentials-flow-with-azuread/>
- <https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/aspnetcore2-2>
- <https://oidcdebugger.com/>
- <https://docs.microsoft.com/en-us/azure/active-directory/develop/consent-framework>
- [An Explanation on different Authentication Schemes](https://github.com/aspnet/announcements/issues/262)
- <https://medium.com/developer-diary/net-core-3-0-preview-4-web-api-authentication-from-scratch-part-3-token-authentication-2d8af41b0045>
