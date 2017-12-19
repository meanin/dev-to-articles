---
title: Forbidden Http headers
published: false
description: Workaround forbidden header
tags: #http #swagger #swashbuckle
---

#Documenting API with [Swagger](https://swagger.io/)

You all know swagger, right? It is used to document your API and provide first front end for testing your calls. As a .net developer I am using [swashbuckle](https://github.com/domaindrivendev/Swashbuckle/wiki) to generate [swagger-ui](https://github.com/swagger-api/swagger-ui). It's rather simple to configure and it provides necessary functionality. By 'necessary', this time I mean to give a user opportunity to insert parameters needed to make a proper call. Headers, path or body params. Last two are the simplest in that case. Basically, the only thing you need to do to make it work, is to create the self-explenatory xml comments connected with your API classes and/or methods. Nice, right?

Http headers, as they say, is where the hard part begins. Creating a field for http header parameter, in case of the .net swashbuckle, you have to implement IOperationFilter (in the simplest solution, there are more ways to achive that). Again, it is not so difficult, it has only one method:
```C#
public void Apply(Operation operation, OperationFilterContext context)
{
    if (operation.parameters == null)
        operation.parameters = new List<IParameter>();

    if (operation.parameters.All(p => p.name != "MyCustomHeader"))
    {
        operation.parameters.Add(new Parameter
        {
            name = "MyCustomHeader",
            @in = "header",
            description = "MyCustomHeader header",
            required = true,
            type = "string"
        });
    }
}
```
The real difficulty starts when you are trying to apply one of the forbidden headers. Do you know them? I wasn't aware of the fact, that some headers are forbidden. But what does it mean `forbidden`?

#My case
I was trying to validate API calls with `Proxy-Authorization` header. So, I made it in standard way - created a new operation filter. When it came to testing it, I ran application, navigated to swagger ui, filled up the fields and tested a call. 
![](https://raw.githubusercontent.com/meanin/dev-to-articles/master/img/2017-12-11-forbidden-http-headers/proxy-header.jpg)
Curl was generated properly, all the headers included, but it was not propagated in a call. Everything was described [here](https://dev.to/meanin/adding-proxy-authorization-header-in-swagger-4o2). Also, I rised this question on [stackoverflow](https://stackoverflow.com/questions/47509258/add-proxy-authorization-header-in-swagger). 
![](https://raw.githubusercontent.com/meanin/dev-to-articles/master/img/2017-12-11-forbidden-http-headers/no-proxy-header.jpg)

Fortunately, one of the swashbuckle contributors (thanks if you are reading), got involved and helped me. He asked me to create a [demo project](https://github.com/meanin/swashbuckle-proxy-authorization-header), which showed the whole issue in a very simple way. After creation completed, I've posted an answer on stack, and waited for reply. It went quickly, here [swagger-api/swagger-ui](https://github.com/swagger-api/swagger-ui) the [issue](https://github.com/swagger-api/swagger-ui/issues/3956) was created. There you can find a short explanation why there is no possibility to make a call from a browser with `proxy-authorization` header set. Basically, there are some [headers](https://developer.mozilla.org/en-US/docs/Glossary/Forbidden_header_name) that are impossible to create on a user-agent side. In that case user-agent is a browser. I wasn't aware of that. My aim for the nearest future is to learn more about http flow stuff. Please look at the list of forbidden headers below:

* Proxy-
* Sec-
* Accept-Charset
* Accept-Encoding
* Access-Control-Request-Headers
* Access-Control-Request-Method
* Connection
* Content-Length
* Cookie
* Cookie2
* Date
* DNT
* Expect
* Host
* Keep-Alive
* Origin
* Referer
* TE
* Trailer
* Transfer-Encoding
* Upgrade
* Via

#Workaround
We had a need to test an API method from swagger. We have been thinking about that for some time. After a few days, one of my colleagues, came up with the idea. Sure, there is no possibility to set `forbidden` header from the user-agent, but no one forbid us to set a custom one and rewrite it to proper one on a back-end side. 
```C#
public override void OnAuthorization(HttpActionContext context)
{
    var proxyAuthorizationHeader = context.Request.Headers.SingleOrDefault(
        h => h.Key == "ProxyAuthorization");

    if(context.Request.Headers.ProxyAuthorization == null 
        && proxyAuthorizationHeader.Value != null)
        context.Request.Headers.Add("Proxy-Authorization", 
            proxyAuthorizationHeader.Value);

    base.OnAuthorization(context);
}
```
Of course, this is not a permanent solution, but it works in the development environment. We can configure our app to support this behavior, it is 5 min or less. Couple of attributes to write, `#if DEBUG` compiler directive to set and everything works fine. We can test it, even with swagger.
![](https://raw.githubusercontent.com/meanin/dev-to-articles/master/img/2017-12-11-forbidden-http-headers/proxy-header-filled.jpg)

---

What do you think about it? Did you know about these `forbidden headers` earlier? What other pitfalls can I encounter on swagger?
