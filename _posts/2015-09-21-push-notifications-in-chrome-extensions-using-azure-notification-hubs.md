---
layout: post
title: "Push notifications in Chrome Extensions using Azure Notification Hubs"
author: Fanie Reynders
tags:
- Google
- Chrome
- Microsoft
- Azure
- Notification Hubs
- Toast notifications
- Push notifications
---
In the past couple of months I've been co-organizing a monthly user group called [Fixxup](http://fixxup.nl), to create a community platform for techies out there to share their knowledge, listen to their peers or even just enjoy the social living by having a beer or two.

Fixxup also has a [website hosted on GitHub](https://github.com/Fixxup/fixxup.github.io) Pages and we recently started to [live stream our sessions](http://fixxup.nl/live) to the rest of the world. When organizing such events it's important to do the proper marketing to target the right audience. 

Although we are making use of social media to promote these events, I wanted an alternative way to get people's attention in realtime; so I built a [Chrome Extension](https://developer.chrome.com/extensions) called Fixxup Live that will only do one thing: Receive real-time push notifications when we're broadcasting live.

> The code is open source and [available on Github](https://github.com/Fixxup/live-notifier)

So I thought it would be fun creating this guide to make use of [Azure Notification Hubs](https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-overview) for receiving push notifications on any platform, in this case Google Chrome.

<!--more-->

## What you'll need
- A Microsoft Azure Subscription ([get a free trial here](https://azure.microsoft.com/en-us/pricing/free-trial))
- Your favorite text editor (I use [Visual Studio Code](https://code.visualstudio.com), it's awesome)
- A Google Chrome Browser ([download here](https://www.google.com/chrome/browser/desktop))
- Some knowledge on Chrome Extensions (ramp up [here](https://developer.chrome.com/extensions) & [here](http://fixxup.nl/events/a-lap-around-chrome-apps-and-extensions.html))

## Overview of push notifications
![Overview of how push notifications work](https://acomdpsstorage.blob.core.windows.net/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/notification-hubs-overview/20150831045959/registration-diagram.png)

*Image source: [https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-overview](https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-overview)*

As you can see, the app back-end is responsible for keeping track and storing of PNS (Post Notification Service) handle registrations. 

By using Azure Notification Hubs we move the management of PNS registrations to an external managed service.

![Overview of push notifications using Azure Notification Hubs](https://acomdpsstorage.blob.core.windows.net/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/notification-hubs-overview/20150831045959/notification-hub-diagram.png)

*Image source: [https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-overview](https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-overview)*

## 1. Create a Notification Hub
Assumingly you've already signed up for Microsoft Azure, go to the Azure Portal by pointing your browser to [https://manage.windowsazure.com](https://manage.windowsazure.com).

On the *Service Bus* section click *Create*:

![]({{ site.url }}/post-images/21-sep-2015/new-ns.png)

Next, fill in an unique name that will identify your namespace and choose *Notification Hub* as the type. You can also set the region and applicable subscription if you want.

![]({{ site.url }}/post-images/21-sep-2015/new-ns-box.png)

After the namespace is created, open it by clicking on its name and then click *Create a new notification hub* from the *Notification Hubs* section.

![]({{ site.url }}/post-images/21-sep-2015/new-nh.png)

You will be prompted for a Hub name, region, subscription and namespace.

![]({{ site.url }}/post-images/21-sep-2015/new-nh-box.png)

Submit the details by clicking the *Create a new notification hub* button. We now should have a Notification Hub called *Fixxup-Live* inside the *Fixxup* namespace.

## 2. Take note of the connection string
Open the details of the Notification Hub and then click on the *View Connection String* link.

![]({{ site.url }}/post-images/21-sep-2015/view-connection.png)

You'll notice two connection strings or SAS (Shared Access Signature) tokens:
- DefaultListenSharedAccessSignature - for read-only use by client app
- DefaultFullSharedAccessSignature - read & write access by app backend

Make a note of the *DefaultListenSharedAccessSignature*.

## 3. Create a Chrome Extension
By not going into too much detail on how to create a Chrome Extension, I am just quickly going to run through some code snippets of the actual extension.

> I've committed the full intial (working) version [on GitHub](https://github.com/Fixxup/live-notifier), so go check it out

The starting point of the extension is *start.js* which calls the `start()` function in the *app* module that gets injected in using RequireJs.

```javascript
//...

requirejs(['app'], function(app) {
  app.start();
});

//...
```

The *app* module has dependencies on the *notifications*- and *config* modules. All the subscription, unsubscription and push message receipt logic can be found in the *notifications* module and the app-specific configuration information (like the *SAS read-only connection signature*) in the *config* module 

When invoking the `start()` function, it calls out to `notifications.subscribe()` for subscribing to push notifications as well as adding an *onClicked* event to fire when theuser clicks on the eventual toast popup message.

```javascript
define('app', ['notifications', 'config'], function (notifications, config) {

  function start() {
    notifications.subscribe();
    chrome.notifications.onClicked.addListener(openSite);
  }

  function openSite() {
    chrome.tabs.create({ url: config.siteUrl });
  }
  
  return {
    start: start
  }

});
```

When moving our attention over to the *notifications* module, we see that it is dependent on the *anh* module (Azure Notification Hubs) and also the *config* module.

Note that we hook into the *onMessage* event upon load, which is provided by the `chrome.gcm` API. When a push notification is received it will call `messageReceived` which renders out a toast message on screen.

```javascript
define('notifications', ['config','anh'], function (config, azureNotificationHubs) {

	chrome.gcm.onMessage.addListener(messageReceived);
	
	function messageReceived(message) {
		console.debug("Message received: " + message);
		
		var data = message.data || {};
		// Pop up a notification to show the GCM message.
		chrome.notifications.create(generateId(), {
			title: data.title,
			iconUrl: "logo.png",
			type: 'basic',
			message: data.body
		});
	}

	function generateId() {
		var id = Math.floor(Math.random() * 9007199254740992) + 1;
		return id.toString();
	}

	var subscribe = function () {
		chrome.gcm.register([config.senderId], gcmRegisterCallback);
	};

	var unsubscribe = function () {
		chrome.gcm.unregister([config.senderId], gcmUnregisterCallback);
	};

	function gcmRegisterCallback(registrationHandle) {
		if (chrome.runtime.lastError) {
			console.error("Registration failed: " + chrome.runtime.lastError.message);
			return;
		}

		console.debug("Registration with GCM succeeded.");

		azureNotificationHubs.register(registrationHandle);
	}

	function gcmUnregisterCallback() {
		if (chrome.runtime.lastError) {
			console.error("Un-registration failed: " + chrome.runtime.lastError.message);
			return;
		}

		console.debug("Un-registration with GCM succeeded.");
	}
	
	return {
		subscribe: subscribe,
		unsubscribe: unsubscribe
	};

});
```

In order to receive these messages we must subscribe to them first, so moving further through the code we see the `subscribe()` function is registering the subscription with [Google Cloud Messaging](https://developers.google.com/cloud-messaging) via the *[chrome.gcm](https://developer.chrome.com/apps/gcm)* API, passing a sender ID (that's the project ID from the [Google Developer Console](https://console.developers.google.com)) and callback method that will be invoked upon return with the PNS handle.

The `gcmRegisterCallback` method receives the registration handle and then invokes the `register()` function on the *anh* module with the given PNS handle.

```javascript
define('anh', ['config'], function (config) {


	var register = function(registrationHandle) {
		var connection = splitConnection();
		var token = generateToken(connection.originalUri, connection.sasKeyName, connection.sasKeyValue);
		sendRequest(connection.originalUri, registrationHandle, token);
	}

	function splitConnection() {
		var parts = config.connectionString.split(';');
		var endpoint = "", sasKeyName = "", sasKeyValue = "";
		if (parts.length != 3) {
			throw "Error parsing connection string";
		}

		parts.forEach(function (part) {
			if (part.indexOf('Endpoint') == 0) {
				endpoint = 'https' + part.substring(11);
			} else if (part.indexOf('SharedAccessKeyName') == 0) {
				sasKeyName = part.substring(20);
			} else if (part.indexOf('SharedAccessKey') == 0) {
				sasKeyValue = part.substring(16);
			}
		});

		var originalUri = endpoint + config.hubName;

		return {
			originalUri: originalUri,
			endpoint: endpoint,
			sasKeyName: sasKeyName,
			sasKeyValue: sasKeyValue
		};
	}

	function generateToken(originalUri, sasKeyName, sasKeyValue) {
		var targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
		var expiresInMins = 10; // 10 minute expiration

		// Set expiration in seconds.
		var expireOnDate = new Date();
		expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
		var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
			.getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
				.getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
					.getUTCSeconds()) / 1000;
		var tosign = targetUri + '\n' + expires;

		// Using CryptoJS.
		var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
		var base64signature = signature.toString(CryptoJS.enc.Base64);
		var base64UriEncoded = encodeURIComponent(base64signature);

		// Construct authorization string.
		var sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
			+ base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
		return sasToken;
	}

	function sendRequest(originalUri, registrationHandle, sasToken) {
		var registrationPayload =
			"<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
			"<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
			"<content type=\"application/xml\">" +
			"<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
			"<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
			"</GcmRegistrationDescription>" +
			"</content>" +
			"</entry>";

		// Update the payload with the registration ID obtained earlier.
		registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationHandle);

		var url = originalUri + "/registrations/?api-version=2014-09";
		var client = new XMLHttpRequest();

		client.onload = function () {
			if (client.readyState == 4) {
				if (client.status == 200) {
					console.debug("Notification Hub Registration succesful!");
					console.debug(client.responseText);
				} else {
					console.debug("Notification Hub Registration did not succeed!");
					console.debug("HTTP Status: " + client.status + " : " + client.statusText);
					console.debug("HTTP Response: " + "\n" + client.responseText);
				}
			}
		};

		client.onerror = function () {
			console.error("ERROR - Notification Hub Registration did not succeed!");
		}

		client.open("POST", url, true);
		client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
		client.setRequestHeader("Authorization", sasToken);
		client.setRequestHeader("x-ms-version", "2014-09");

		try {
			client.send(registrationPayload);
		}
		catch (err) {
			console.error(err.message);
		}
	}

	return {
		register: register
	};

});
```

Inside the `register()` function, it receives the PNS handle and generates a token before it invokes the registration via the Azure Notfication Hubs RESTful API.

> Azure handles duplicate registrations and will always make sure there are only unique ones, therefore it is important to register for push notifications every time the application process starts if the user opted in for it. You can make use of the local storage to accomplish this.

## 4. Bundle & install extension
Now that we're finished, go ahead and compile the *manifest.json* file, stating the required files & permissions.

> All application files are loaded from one single `app.js` file that was combined using a simple Grunt task.

```json
{
  "name": "Fixxup Live Notifier",
  "description": "Stay informed with live notifications from Fixxup.",
  "manifest_version": 2,
  "version": "0.1",
  "background": {
      "scripts": [
        "require.js",
        "hmac-sha256.js",
        "enc-base64-min.js",
        "app.js"
      ]
    },
  "permissions": [
    "tabs",
    "gcm",
    "notifications",
    "https://*.servicebus.windows.net/*"
  ],
  "icons":
    { "128": "logo.png" }
}
```

When that's done, install the extension to Chrome by dragging the containing folder to the `chrome://extensions` section.

## 5. Fire away!
I've built a small console app that sends push notifications to a specific hub.

Create a blank Console Application with Visual Studio and make use of the Nuget Package Manager Console to install the `Microsoft.Azure.NotificationHubs` and `Newtonsoft.Json` packages then call this `sendNotificationAsync` function:

```csharp
private static async Task sendNotificationAsync(string message)
{
	var hub = NotificationHubClient.CreateClientFromConnectionString("<Read/Write SAS key here>", "<hub name here>");
	var googleNotification = new {
			data = new {
				title = message,
				body = "New broadcast starting soon"
			}
	};         
	await hub.SendGcmNativeNotificationAsync(JsonConvert.SerializeObject(googleNotification));         
}
```

> Instead of using Newtonsoft Json's `JsonConvert` you could just send a normal string payload.

## That's all folks!
There you have it, it is that easy receiving push notifications in Chrome Extensions with Azure Notification Hubs.

*Till next time!*

[@FanieReynders](https://twitter.com/faniereynders)