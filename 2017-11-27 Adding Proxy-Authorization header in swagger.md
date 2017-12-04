---
title: Adding Proxy-Authorization header in swagger
published: true
description: 
tags: #discuss #swagger #swashbuckle
---

Hi all,
I already posted this question on [stack overflow](https://stackoverflow.com/questions/47509258/add-proxy-authorization-header-in-swagger), but I think, that here I can find interested people faster.

I need to add specific header on swagger ui using .net core. Is there any way to include header like this?

Already tried:
* implementing IOperationFilter:
```c#
public void Apply(Operation operation, OperationFilterContext context)
{
    if (operation.Parameters == null)
        operation.Parameters = new List<IParameter>();

    if (operation.Parameters.All(p => p.Name != "Proxy-Authorization"))
    {
        operation.Parameters.Add(new NonBodyParameter
        {
            Name = "Proxy-Authorization",
            In = "header",
            Description = "Proxy-Authorization token",
            Required = true,
            Type = "string"
        });
    }
}
```

* adding security definition:

```c#
options.AddSecurityDefinition("Authorization", new ApiKeyScheme()
{
    In = "header",
    Description = "Please insert Proxy Authorization Secret into field",
    Name = "Proxy-Authorization",
    Type = "apiKey"
});
```

Both didn't work. When I change header name everything works fine (`HttpContext.Request.Headers` contains desirable header), but this specific ( `Proxy-Authorization`) header is wiped out from a call.

Did you ever meet this issue? How to solve it?

EDIT:

I confirmed that this is not the issue in .net core only. Full .net framework has also this bug.

Furthermore it is only affected on header with `proxy-` prefix.