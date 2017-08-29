---
layout: post
title: Настройка httpErrors 
---
```    
<httpErrors errorMode="DetailedLocalOnly" existingResponse="Auto">
      <remove statusCode="404" />
      <remove statusCode="500" />
      <error statusCode="404" path="/business/page_404" responseMode="ExecuteURL" />
      <error statusCode="500" path="Front\html\500.html" responseMode="File" />
</httpErrors>
```

```
protected void Application_Error(object sender, EventArgs e)
{
    var exception = Server.GetLastError();
    Server.ClearError();

    var httpException = exception as HttpException;
    if (httpException == null)
        _logger.Fatal(exception, "Application_Error");

    Response.StatusCode = httpException?.GetHttpCode() ?? (int) HttpStatusCode.InternalServerError;
}
```

```
    context.HttpContext.Response.TrySkipIisCustomErrors = true;
```