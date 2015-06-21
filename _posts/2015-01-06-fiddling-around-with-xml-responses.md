---
layout: post
title: Fiddling around with XML responses
date: '2015-01-06T12:58:00.003+02:00'
author: Fanie Reynders
tags:
- XML
- Fiddler
- C#
modified_time: '2015-01-06T13:02:04.374+02:00'
thumbnail: http://1.bp.blogspot.com/-EYYRmPnyY-M/VKuza40vM2I/AAAAAAAAAUc/8h_H8I9_taI/s72-c/Untitled-1.jpg
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-8735706301551138789
blogger_orig_url: http://www.faniereynders.com/2015/01/fiddling-around-with-xml-responses.html
---

My year started with new challenges, first of which is malformed and unformatted XML. One of the requirements of a new project I'm working on is to read a simple XML response from a web service and do something with the data. How hard could it be?

<!--more-->
	
Like any other developer, debugging tools like <a href="http://www.telerik.com/fiddler">Fiddler</a> is my friend. After capturing the response, I noticed that the contents were malformed and not properly formatted, but still "valid" XML.

<a href="http://1.bp.blogspot.com/-EYYRmPnyY-M/VKuza40vM2I/AAAAAAAAAUc/8h_H8I9_taI/s1600/Untitled-1.jpg" ><img border="0" src="http://1.bp.blogspot.com/-EYYRmPnyY-M/VKuza40vM2I/AAAAAAAAAUc/8h_H8I9_taI/s1600/Untitled-1.jpg" /></a>

Viewing the actual raw response, I realized that the apparent XML elements within the root '<i>string</i>' element where actually encoded literal strings:
	
<a href="http://2.bp.blogspot.com/-sDU4mdfuN1Y/VKu1AIruLYI/AAAAAAAAAUo/6nuXy375I4I/s1600/Untitled-2.jpg" imageanchor="1" ><img border="0" src="http://2.bp.blogspot.com/-sDU4mdfuN1Y/VKu1AIruLYI/AAAAAAAAAUo/6nuXy375I4I/s1600/Untitled-2.jpg" /></a>

After accepting that <a href="https://www.goodreads.com/quotes/14821-what-you-re-supposed-to-do-when-you-don-t-like-a">I can't change the world, but definitely change the way you think about it</a>, I decided to create a helping aid in the form of a <a href="http://docs.telerik.com/fiddler/extend-fiddler/extendwithdotnet">Fiddler extension</a> to help me make sense of the so-called XML.

####Creating a Fiddler Extension
The first thing we have to do is create a new class library project in Visual Studio. We are going to target Fiddler version 4, so make sure the .NET target framework is set to 4.0.

Next, we need to add a reference to our project to the Fiddler assembly located at `%programfiles(x86)%\Fiddler2\Fiddler.exe`:

<a href="http://2.bp.blogspot.com/-D1F-V_d83GY/VKu3PIvQeEI/AAAAAAAAAU0/2wnsfUMZ53o/s1600/newref.png" imageanchor="1" ><img border="0" src="http://2.bp.blogspot.com/-D1F-V_d83GY/VKu3PIvQeEI/AAAAAAAAAU0/2wnsfUMZ53o/s1600/newref.png" /></a>

<a href="http://4.bp.blogspot.com/-Wvq16NtiQxs/VKu4bQCnxQI/AAAAAAAAAU8/bes7FbpYieQ/s1600/newref.png" imageanchor="1" ><img border="0" src="http://4.bp.blogspot.com/-Wvq16NtiQxs/VKu4bQCnxQI/AAAAAAAAAU8/bes7FbpYieQ/s1600/newref.png" height="440" width="640" /></a>

Now we can get coding! Add a new class to the project with the following assembly attribute:

```csharp
using Fiddler;
[assembly: Fiddler.RequiredVersion("2.3.5.0")]
public class XMLFormatter   
{
  
}
```

To make this class visible to Fiddler we need to implement the `IAutoTamper` interface and in the `AutoTamperResponseBefore` function, we can modify the response before it is served to the client.

```csharp
public void AutoTamperResponseBefore(Session oSession)
{
  var value = Encoding.UTF8.GetString(oSession.ResponseBody);
  value = value.Replace("&lt;", "<").Replace("&gt;", ">");
  oSession.oResponse.headers.Add("Response-Hijacked-By", "Fanie");
  oSession.ResponseBody = Encoding.UTF8.GetBytes(value);
}
```

This will use the provided Session object and replace the XML encoded angle brackets <i>&amp;lt; </i>and <i>&amp;gt; </i>as < and > respectfully. While we at it, I've also decided to brand the response by adding a "<i>Response-Hijacked-By</i>" response header just because we can.

Lastly we need to build the project and copy the DLL to the Fiddler Scripts directory at `%programfiles(x86)%\Fiddler2\Scripts\`

> You may need to restart Fiddler if it was running.

####Testing the goodness
Now when we execute the request, the extension kicks in and alters the response as proper XML:

<a href="http://2.bp.blogspot.com/-YOZI6sDkyJ4/VKu9_4Tm1bI/AAAAAAAAAVM/6qpIe-IFmE8/s1600/Untitled-3.jpg" imageanchor="1" ><img border="0" src="http://2.bp.blogspot.com/-YOZI6sDkyJ4/VKu9_4Tm1bI/AAAAAAAAAVM/6qpIe-IFmE8/s1600/Untitled-3.jpg" /></a>

We even got a special signature:

<a href="http://4.bp.blogspot.com/-Ju_CuNhdwQI/VKu-iiDVMtI/AAAAAAAAAVU/m7BpkVDn1jg/s1600/Untitled-4.jpg" imageanchor="1" ><img border="0" src="http://4.bp.blogspot.com/-Ju_CuNhdwQI/VKu-iiDVMtI/AAAAAAAAAVU/m7BpkVDn1jg/s1600/Untitled-4.jpg" /></a>

*Till next time!*