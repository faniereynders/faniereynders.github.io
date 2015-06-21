---
layout: post
title: 'Creating a proxy for ASP.NET Web API using T4'
date: '2013-03-10T16:42:00.001+02:00'
author: Fanie Reynders
tags:
- rest
- webapi
- http services
- aspnet
modified_time: '2014-01-16T20:34:06.441+02:00'
thumbnail: http://2.bp.blogspot.com/-lwctvz0QsKw/UTyMIgHis0I/AAAAAAAAARY/fN3JaJYYAFU/s72-c/Untitled.png
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-4769518008738081137
blogger_orig_url: http://www.faniereynders.com/2013/03/aspnet-web-api.html
---

One of the coolest new features of ASP.NET is Web API, which allows you to build powerful RESTful services exposed over HTTP. HTTP is not just for serving up web pages, but it provides a platform for building APIs that expose services and data. It is simple, flexible, and ubiquitous.
	
ASP.NET Web API is all about ~~REST~~ HTTP Services. Unlike Web Services, it does not expose an end-point for meta-data exchange. When consuming a HTTP Service, you will have to create your own client-side code that must serve as a proxy for interfacing with the API.
	
Like any other WCF junkie out there, I've been working with Windows Communication Foundation for quite some time now implementing Web Services. Not only is it flexible and extensible, but it offers inter-operability, multiple message patterns, transactions and  meta-data exchange etc.

The nice thing about WCF is meta-data exchange, which allows you to generate the client-side proxy-code, contracts and classes used for talking to the service. On the other hand, ASP.NET Web API is more light-weight, operates only via HTTP and is used more for serving CRUD-based operations. The latter, however, does not have functionality (yet) for exposing service meta-data. Until now...

<!--more-->
	
> Update: Use [WebApiProxy]({% post_url 2014-01-16-introducing-webapiproxy-providing %})with your ASP.NET Web API 2 service for a metadata provider and generate JavaScript & C# proxy clients. [Read more here]({% post_url 2014-01-16-introducing-webapiproxy-providing %}).

###T4 to the rescue

As an API developer, I want to be able to use ASP.NET Web API for it's light-weight footprint as well as exposing some kind of meta-data exchange mechanism, which is not currently supported out of the box. This is where T4 templates come into play.

While playing around with the Text Template Transformation Toolkit (T4, hence the four T's), I realized that I can actually build generic T4 templates (that is delivered via a mechanism like Nuget) and then generate the API's proxy code (C# and JavaScript) based on certain settings.

The settings file contains a list of end-point URLs that the T4 template should crawl and download the meta-data for.

###The API
For the concept, I built an API that feeds basic people information. It needs to be called from any REST-based client and must be able to provide meta-data (like web services) to the consumers.

To have my API feed this type information, I've created the following classes:

- `ExposeProxyAttribute` - used to mark the endpoints for metadata generation.
- `TypeRetriever` - responsible for extracting all the types used in the API marked with the `ExposeProxyAttribute`. 
- `ContractInfo` - the model that houses the metadata which describes information like classes and methods.

####MexApiController
This is the base class for the MEX (Metadata Exchange) enabled API Controller. Every class (controller) that inherits from this, will receive an extra GET endpoint with a `?metadata` identifier, i.e. `/api/people?metadata`

```csharp
public abstract class MexApiController : ApiController
{
    private TypeRetriever tr;
 
    public MexApiController()
    {
        tr = new TypeRetriever();
    }
 
    //adds the mex endpoint as ?metadata
    public ContractInfo Get(string metadata)
    {
        return tr.GetTypeData(this.GetType());
    }
}
```

####PeopleController
Here is a normal Controller called `PeopleController`. It inherits the MexApiController which will enable the metadata endpoint for `/api/people`. 

```csharp
public class PeopleController : MexApiController
{
    // GET api/people
    [ExposeProxy]
    public IEnumerable<Person> Get()
    {
        return new Person[] { 
            new Person { Name = "Fanie" }, 
            new Person { Name = "Andrea" } 
        };
    }
 
    // GET api/people/5
    [ExposeProxy]
    public Person Get(int id)
    {
        return new Person { Name = "Fanie" };
    }
 
    // POST api/people
    [ExposeProxy]
    public void Post(Person person)
    {
        //...
 
    }
 
    // PUT api/people/5
    [ExposeProxy]
    public HttpResponseMessage Put(int id, string value)
    {
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
 
    // DELETE api/people/5
    [ExposeProxy]
    public HttpResponseMessage Delete(int id)
    {
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

So now we have the following endpoints:

- `GET /api/people` - Gets a list of People
- `GET /api/people/2` - Gets one Person with Id 2
- `POST /api/people` - Creates a new Person
- `**PUT** /api/people/2` - Updates Person with Id 2
- `DELETE /api/people/2` - Deletes Person with Id 2
- `GET /api/people?metadata` - Retrieves the metadata for the People API as a ContractInfo model response.

Testing the API gives with 'GET /api/people' gives me the following result:

```xml
<ArrayOfPerson xmlns:i="http://www.w3.org/2001/XMLSchema-instance" 
xmlns="http://schemas.datacontract.org/2004/07/TestApi.Controllers">
  <Person>
    <Name>Fanie</Name>
  </Person>
  <Person>
    <Name>Andrea</Name>
  </Person>
</ArrayOfPerson>
```

> Notice that only the end-points decorated with `ExposeProxy` are included in the meta-data response.

Requesting `GET /api/people?metadata` gives:

```xml
<ContractInfo xmlns:i="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://schemas.datacontract.org/2004/07/DynamicServerContract">
  <Classes>
    <ContractClassDefinition>
      <ClassName>Person</ClassName>
      <Data xmlns:d4p1="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
        <d4p1:KeyValueOfstringstring>
          <d4p1:Key>Name</d4p1:Key>
          <d4p1:Value>String</d4p1:Value>
        </d4p1:KeyValueOfstringstring>
      </Data>
    </ContractClassDefinition>
  </Classes>
  <Methods>
    <ContractMethodDefinition>
      <Name>Get</Name>
      <Parameters xmlns:d4p1="http://schemas.microsoft.com/2003/10/Serialization/Arrays" />
      <RequestType>GET</RequestType>
      <ReturnType>IEnumerable&lt;Person&gt;</ReturnType>
    </ContractMethodDefinition>
    <ContractMethodDefinition>
      <Name>Get</Name>
      <Parameters xmlns:d4p1="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
        <d4p1:KeyValueOfstringstring>
          <d4p1:Key>id</d4p1:Key>
          <d4p1:Value>Int32</d4p1:Value>
        </d4p1:KeyValueOfstringstring>
      </Parameters>
      <RequestType>GET</RequestType>
      <ReturnType>Person</ReturnType>
    </ContractMethodDefinition>
    <ContractMethodDefinition>
      <Name>Post</Name>
      <Parameters xmlns:d4p1="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
        <d4p1:KeyValueOfstringstring>
          <d4p1:Key>person</d4p1:Key>
          <d4p1:Value>Person</d4p1:Value>
        </d4p1:KeyValueOfstringstring>
      </Parameters>
      <RequestType>POST</RequestType>
      <ReturnType>void</ReturnType>
    </ContractMethodDefinition>
    <ContractMethodDefinition>
      <Name>Put</Name>
      <Parameters xmlns:d4p1="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
        <d4p1:KeyValueOfstringstring>
          <d4p1:Key>id</d4p1:Key>
          <d4p1:Value>Int32</d4p1:Value>
        </d4p1:KeyValueOfstringstring>
        <d4p1:KeyValueOfstringstring>
          <d4p1:Key>value</d4p1:Key>
          <d4p1:Value>String</d4p1:Value>
        </d4p1:KeyValueOfstringstring>
      </Parameters>
      <RequestType>PUT</RequestType>
      <ReturnType>HttpResponseMessage</ReturnType>
    </ContractMethodDefinition>
    <ContractMethodDefinition>
      <Name>Delete</Name>
      <Parameters xmlns:d4p1="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
        <d4p1:KeyValueOfstringstring>
          <d4p1:Key>id</d4p1:Key>
          <d4p1:Value>Int32</d4p1:Value>
        </d4p1:KeyValueOfstringstring>
      </Parameters>
      <RequestType>DELETE</RequestType>
      <ReturnType>HttpResponseMessage</ReturnType>
    </ContractMethodDefinition>
  </Methods>
</ContractInfo>
```

###The client side
Now that I have MEX end-points that give me all the meta-data for a API, I tell my T4 template to make sense of it all using the settings and generate the appropriate C# and/or JavaScript code to use the service.

Here are the files I've used:
<a href="http://2.bp.blogspot.com/-lwctvz0QsKw/UTyMIgHis0I/AAAAAAAAARY/fN3JaJYYAFU/s1600/Untitled.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="http://2.bp.blogspot.com/-lwctvz0QsKw/UTyMIgHis0I/AAAAAAAAARY/fN3JaJYYAFU/s1600/Untitled.png" /></a>

The `WebApi.CSharpProxy.tt` and `WebApi.JavaScriptProxy.tt` generates C# and JavaScript based on the configuration given in the WebApi.Proxy.config file. The `WebApi.Proxy.Core.tt` contains infrastructure code specifically for the T4 template.

Examining the WebApi.Proxy.config:

```xml
<?xml version="1.0"?>
<WebApiReferences xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                  xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Proxies>
    <ProxyItem baseurl="http://myapiservice.net/api/" 
               name="TestProxy" 
               metadataName="metadata"
               resourceSuffix="Client">
      <Resources>
        <ResourceItem path="people" name="People" />
        <ResourceItem path="some/other/endpoint" name="CoolResource" />
      </Resources>
    </ProxyItem>
  </Proxies>
</WebApiReferences>
```

As you see, the settings allow for multiple proxies. In each proxy you can specify the metadata identifier and the resource suffix. A list of applicable resource end-points are defined to tell the generator to only generate the code for specific resources.

This makes consuming my REST-based API a breeze with cool intellisense:

<a href="http://2.bp.blogspot.com/-KRMGFD-kUug/UTyYA2n0LGI/AAAAAAAAARo/UReuTwWakMs/s1600/Untitled.png" imageanchor="1"><img border="0" src="http://2.bp.blogspot.com/-KRMGFD-kUug/UTyYA2n0LGI/AAAAAAAAARo/UReuTwWakMs/s640/Untitled.png" height="205" width="640" /></a>

####Also generating JavaScript? This got me thinking...
Most of my API calls will originate from HTML apps. So instead of having the JavaScript guys generate their own proxies, why not do it for them?

So, I ended up adding another endpoint to my API called ProxyController that merely just relays the generated JavaScript file from the T4 template to the client, as a JavaScript file.

```csharp
public class ProxyController : ApiController
{
   public HttpResponseMessage Get()
   {
       var pth = System.Web.HttpContext.Current.Server.MapPath("~/Scripts/WebApi.Proxy/WebApi.JavaScriptProxy.js");
 
       HttpResponseMessage result = new HttpResponseMessage(HttpStatusCode.OK);
 
       result.Content = new ByteArrayContent(File.ReadAllBytes(pth));
       result.Content.Headers.ContentType = new MediaTypeHeaderValue("application/javascript");
 
       return result;
   }
}
```

Now, from the client HTML-based app, adding a reference to my JavaScript proxy end-point will get the latest JavaScript code to use my API.

Reference in the HTML file:

```html
<script type="text/javascript" src="http://myapiservice.net/api/proxy"></script>
```

This will expose the proxy code to the rest of the client-side scripts. This allows us to use it like:

```javascript
var peopleClient = new TestProxy.PeopleClient();
 
peopleClient.Get(function (data) {
  $(data).each(function (id, item) {
    $("<p>" + item.Name + "</p>").appendTo("body");
});
```

###Done and Done
After all this, I seriously felt like an API ninja. I wouldn't be surprised if the ASP.NET team brings out a similar feature in the near future. Although this is just a proof of concept, my aim is to round-it off a bit more and deploy it to Nuget. Also be on the lookout for the source-code soon to be on GitHub.

*Thats all folks!*

Please follow me on Twitter <a href="http://twitter.com/faniereynders">@FanieReynders</a> and feel free to ask any questions regarding this.