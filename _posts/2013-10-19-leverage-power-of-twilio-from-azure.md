---
layout: post
title: Leverage the power of Twilio from Azure Mobile Services
date: '2013-10-19T08:27:00.001+02:00'
author: Fanie Reynders
tags:
- mobile services
- GitHub
- Nodejs
- azure
- SMS
- Twilio
- Text message
- Pay it forward
modified_time: '2013-12-31T11:16:28.493+02:00'
thumbnail: http://2.bp.blogspot.com/-hCkrx6MmSfk/UmELlxAwcII/AAAAAAAAAZk/1HoUf-Y7jE0/s72-c/twilio.png
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-7546420584552633581
blogger_orig_url: http://www.faniereynders.com/2013/10/leverage-power-of-twilio-from-azure.html
---

Ever wanted to send a simple text message from your web app? Well, now you can by using <a href="http://twilio.com/" target="_blank">Twilio</a> - an awesome cloud-powered API for voice calls & text messaging. Okay, I know that statement sounds like something out of an <a href="http://en.wikipedia.org/wiki/Infomercial" target="_blank">infomercial</a>, but I recently had a simple need to send text messages from <a href="http://azure.com/" target="_blank">~~Windows Azure Mobile Services (WAMS)~~ Azure Mobile Apps</a>, and after a bit of research, I came across Twilio which allows me to do just that.

<!--more-->

<a href="http://2.bp.blogspot.com/-hCkrx6MmSfk/UmELlxAwcII/AAAAAAAAAZk/1HoUf-Y7jE0/s1600/twilio.png" imageanchor="1"><img border="0" height="196" src="http://2.bp.blogspot.com/-hCkrx6MmSfk/UmELlxAwcII/AAAAAAAAAZk/1HoUf-Y7jE0/s200/twilio.png" width="200" /></a>

Using Twilio from within WAMS can be quite tricky at first, but after a few practise rounds, I finally got the hang of it and realized that its actually very simple and easy to use, so I decided to write about how Twilio and WAMS together can solve a simple real-world scenario.

###Setting the scene
Ever returned to your car realizing you've left the lights on, leaving you with a dead battery and wished that you just knew earlier? I had a simple idea for an app that might solve this problem: To showcase some features, I've built a little app (also hosted in Azure) that allows any good citizen in your community to anonymously report incidents like these from their phone directly to the owner of the vehicle via text messaging, without knowing their contact details.

###How it works
Firstly, the owner of the car needs to register their car registration- and contact number on a secure website, then anyone with access to the special incident reporting app are able to submit details about the incident, like open windows or switched on lights. As a result, the owner of the car gets notified via a text message, informing him/her about the problem.

###Enough talking, let's get coding!
To get started you'd need the following:

- A Windows Azure Account - <a href="http://www.windowsazure.com/en-us/pricing/free-trial/" target="_blank">sign up here</a>
- <a href="https://www.twilio.com/try-twilio" target="_blank">A Twilio Account</a>
- Some <a href="http://nodejs.org/">Node.js</a> knowledge and <a href="http://nodejs.org/download/">Node Package Manager</a>
- Git-based source control, like <a href="http://windows.github.com/">GitHub for Windows</a>

####Creating the back-end
From within Azure, create a new Mobile Service:

<a href="http://4.bp.blogspot.com/-y4vf0fNOnS8/UmD2g5OKiwI/AAAAAAAAAXw/DAA_L-1l1u0/s1600/new+wams.png" imageanchor="1"><img border="0" height="411" src="http://4.bp.blogspot.com/-y4vf0fNOnS8/UmD2g5OKiwI/AAAAAAAAAXw/DAA_L-1l1u0/s640/new+wams.png" width="640" /></a>

<a href="http://2.bp.blogspot.com/-KSUZMJdY4Xk/UmD3lVTtlzI/AAAAAAAAAX4/gtYg6Khpz0s/s1600/new-wams2.png" imageanchor="1"><img border="0" height="428" src="http://2.bp.blogspot.com/-KSUZMJdY4Xk/UmD3lVTtlzI/AAAAAAAAAX4/gtYg6Khpz0s/s640/new-wams2.png" width="640" /></a>

Head to the API section and create a new API called '*incidents*':

<a href="http://4.bp.blogspot.com/-CVQ9iRVkQ_A/UmD5RI6gggI/AAAAAAAAAYE/9cPneGlBbM0/s1600/new-api.png" imageanchor="1"><img border="0" height="426" src="http://4.bp.blogspot.com/-CVQ9iRVkQ_A/UmD5RI6gggI/AAAAAAAAAYE/9cPneGlBbM0/s640/new-api.png" width="640" /></a>

<a href="http://1.bp.blogspot.com/-j0M2xxrSAqk/UmD5RmwcMWI/AAAAAAAAAYM/ujuadrBcF_Q/s1600/new-api2.png" imageanchor="1"><img border="0" height="548" src="http://1.bp.blogspot.com/-j0M2xxrSAqk/UmD5RmwcMWI/AAAAAAAAAYM/ujuadrBcF_Q/s640/new-api2.png" width="640" /></a>

The last thing to do is to create a new '*subscribers*' table from the data section. 

> Note: I've also pre-populated this table with an entry containing my name, number and car registration as identifier. 

<a href="http://3.bp.blogspot.com/-KbiRQyk-Lwk/UmIlPlKeq9I/AAAAAAAAAZ4/DoXKEE2wKeo/s1600/subscribers.png" imageanchor="1"><img border="0" height="422" src="http://3.bp.blogspot.com/-KbiRQyk-Lwk/UmIlPlKeq9I/AAAAAAAAAZ4/DoXKEE2wKeo/s640/subscribers.png" width="640" /></a>

####Setting up source-control
From the dashboard, under the '*Quick glance*' heading, click the '*Set up source control*' link and follow the prompts (it may ask you to create some credentials). Once it is done configuring the source control, go to the '*Configure*' section and copy the '*Git URL*' provided under the '*Source Control*' heading.

<a href="http://4.bp.blogspot.com/-yQCTLr8nMEU/UmD8Y_M3iAI/AAAAAAAAAYk/RfotgEqyqRY/s1600/sourcecontrol.png" imageanchor="1"><img border="0" height="424" src="http://4.bp.blogspot.com/-yQCTLr8nMEU/UmD8Y_M3iAI/AAAAAAAAAYk/RfotgEqyqRY/s640/sourcecontrol.png" width="640" /></a>

You can use the Git URL in your favourite Git manager. Personally, I like   *Git for Windows*, so I just drag-and-drop this URL into Git for Windows and it will add it to the client.

Once the repository is cloned, you can use your favourite text editor, like Sublime Text, to access and edit your mobile service from the '*services*' folder within the cloned location on your hard-drive.

<a href="http://1.bp.blogspot.com/-T8Kj7Ofw1fk/UmD-X4JzkaI/AAAAAAAAAYw/2wWTPHO3vN4/s1600/explorer.png" imageanchor="1"><img border="0" height="398" src="http://1.bp.blogspot.com/-T8Kj7Ofw1fk/UmD-X4JzkaI/AAAAAAAAAYw/2wWTPHO3vN4/s640/explorer.png" width="640" /></a>

Now that we have a local copy of the code, we can use the  *Node Package Manager* to install the *Twilio* package. To do this:

1. Open Command Prompt as administrator and navigate to the '*service*' folder.
2. Type `npm install twilio` and hit ENTER
3. This will download the Twilio client into the service folder.

> Remember to commit the changes back to the server once it has successfully downloaded the package!

The next thing we need to do is to open the '*incidents.js*' file from the '*api*' folder and replace the code with the following:

```javascript
exports.post = function(request, response) {
    var id = request.body["identifier"],
        comment = request.body["comment"],
        subscribers = request.service.tables.getTable("subscribers");
        
    subscribers.where({identifier: id})
               .read({
                    success: sendText
    });
    response.send(statusCodes.OK, request.body);

    function sendText(result){
        var twilio = require('twilio');
        var tel = result[0].tel, 
            message = "Hello " + result[0].name + "! We've received an incident " + 
                      "relating to your vehicle " + result[0].identifier + 
                      " with the following details: " + comment;
        
         var client = new twilio.RestClient('[Twilio Account SID here]', '[Twilio Auth Token here]');
    
         client.sendSms({
            to: '+' + tel,
            from:'[from number prefixed with a + ]',
            body:message
         }, handleTwilioResponse);
    }
 
    function handleTwilioResponse(error, message){
        if (!error) {
                console.log('Success! The SID for this SMS message is: ' + message.sid);
                console.log('Message sent on: ' + message.dateCreated);
        }
        else {
                console.log('Oops! There was an error.' + error.message);
        }
    }
 
};
```

The code above basically creates a '*POST*' endpoint that receives the '*Identifier*' (car registration number) and '*Comment*' from the request body and cross-references it against the '  *Subscribers  *' table. If a subscription is found for a particular registration number, it simply compiles a message and sends a text to the number defined in the table.

> Note that after installing the Twilio package from NPM, we can use the `require('twilio')` function to get an instance of the Twilio class.

After saving the '*incidents.js*' file, commit changes to source control and the changes should reflect online.

####The front-end
Doing the front-end is very easy. Simply create a new HTML and JavaScript files containing:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <title></title>
    <link href="Content/jquery.mobile-1.3.2.min.css" rel="stylesheet" />
    <link href="Content/jquery.mobile.structure-1.3.2.min.css" rel="stylesheet" />
    <link href="Content/app.css" rel="stylesheet" />
    <!--[if lt IE 9]><script src="https://cdnjs.cloudflare.com/ajax/libs/html5shiv/3.6.1/html5shiv.js"></script><![endif]-->
</head>
<body>
    <div data-role="page" id="page1">
        <div data-theme="b" data-role="header" data-position="fixed">
            <h3>New Incident
        </h3>
        </div>
        <div data-role="content">
            <div data-role="fieldcontain">
                <label for="identifier">
                    Identifier
           
                </label>
                <input name="" id="identifier" placeholder="Car registration, ID number, etc."
                    value="" type="text">
            </div>
            <div data-role="fieldcontain">
                <label for="details">
                    Details
           
                </label>
                <textarea name="" id="details" placeholder="Extra information"></textarea>
            </div>
            <input id="submitIncident" type="submit" value="Report">
        </div>
    </div>
    <div data-role="dialog" id="dialog">
        <div data-role="header">
            <h3>Thanks!</h3>
        </div>
        <div data-role="content">
            <p>This incident has been reported successfuly!</p>
        </div>
    </div>
    <div data-role="dialog" id="error">
        <div data-role="header">
            <h3>Oops!</h3>
        </div>
        <div data-role="content">
            <p class="message"></p>
        </div>
    </div>
    <script src="Scripts/jquery-1.9.1.min.js"></script>
    <script src="Scripts/jquery.mobile-1.3.2.min.js"></script>
    <script src="https://[mobile service name].azure-mobile.net/client/MobileServices.Web-1.0.0.min.js"></script>
    <script src="page.js"></script>
</body>
</html>
```

```javascript
$(function () {
    var client = new WindowsAzure.MobileServiceClient('[Mobile Services URL]', '[WAMS Application Key]'), 
        $submit = $("#submitIncident");
 
    $submit.on("click", submit);
    $.mobile.changePage.defaults.allowSamePageTransition = true;
 
    function submit() {
        
        var id = $("#identifier").val(),
            comment = $("#details").val();
 
        $submit.prop('disabled', true);
 
        $.mobile.showPageLoadingMsg();
 
 
        client.invokeApi("incidents", {
            body: {
                identifier: id,
                comment: comment
            },
            method: "post"
        }).done(function (results) {
            
            $.mobile.changePage('#dialog', 'slidedown', true, true);
        }, function (error) {
            $.mobile.changePage('#error', 'slidedown', true, true);
            $("#error .message").html(error.message);
            $submit.prop('disabled', false);
        });
    }
 
 
});
```

####Taking it for a spin
<a href="http://3.bp.blogspot.com/-IbFJKz-3YsY/UmEI83vsrYI/AAAAAAAAAZA/Hyu0z0-ZaIc/s1600/wp_ss_20131018_0001.png" imageanchor="1"><img border="0" height="400" src="http://3.bp.blogspot.com/-IbFJKz-3YsY/UmEI83vsrYI/AAAAAAAAAZA/Hyu0z0-ZaIc/s400/wp_ss_20131018_0001.png" width="240" /></a>

<a href="http://1.bp.blogspot.com/-O9SsAuRNFS8/UmEJJ5ZANxI/AAAAAAAAAZQ/pKRDTDJEDa8/s1600/wp_ss_20131018_0002.png" imageanchor="1"><img border="0" height="400" src="http://1.bp.blogspot.com/-O9SsAuRNFS8/UmEJJ5ZANxI/AAAAAAAAAZQ/pKRDTDJEDa8/s400/wp_ss_20131018_0002.png" width="240" /></a>

<a href="http://4.bp.blogspot.com/-sgs_1Cmax4U/UmEJLgVir6I/AAAAAAAAAZY/N32TTeBHvoA/s1600/wp_ss_20131018_0003.png" imageanchor="1"><img border="0" height="400" src="http://4.bp.blogspot.com/-sgs_1Cmax4U/UmEJLgVir6I/AAAAAAAAAZY/N32TTeBHvoA/s400/wp_ss_20131018_0003.png" width="240" /></a>

###Conclusion
As you can see, its that easy to use all the cool tools out there on the cloud to do exactly what we want. By combining the power of Azure and the functionality of Twilio, gives us new opportunities to solve modern problems.

Hope this quick tutorial might help someone somewhere out there.

*Till next time*