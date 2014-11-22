> *The Original: [SugarSync for Developers-Getting Started](https://www.sugarsync.com/dev/getting-started.html)*

---

---

* [Developer Home](/source/dev/home.md)
* [Getting Started](/source/dev/getting-started.md)
* [Managing Keys and Apps](/source/dev/managing-apps.md)
* [Resource Overview](/source/dev/resources.md)
* [Examples](/source/dev/using-api.md)
* [Best Practices](/source/dev/best-practices.md)
* [Resource Reference](/source/dev/api/resource-ref.md)
* [*:::Developer's Forum:::*](http://groups.google.com/a/developers.sugarsync.com/group/platform-api/subscribe)
* [*:::App Showcase:::*](https://www.sugarsync.com/partners/)
* [Glossary](/source/dev/glossary.md)
* [Developer Provisioning](/source/dev/dev-provisioning.md)

---

# Getting Started

Welcome, SugarSync Developers! Our API is easy to incorporate into your applications and the advantage to your users is huge. With a few simple API calls to SugarSync, your application can open an authenticated session and interact with your users' files and folders in the SugarSync cloud.

**It's super easy to get started as a Developer.**

1. [*:::Sign up for a SugarSync account:::*](https://www.sugarsync.com/signup?startsub=5) if you don't have one already.
2. [*:::Register your SugarSync account as a developer account:::*](https://www.sugarsync.com/developer/signup). In response, you'll receive a set of access keys that you'll need to use the API.
3. Create your app using the [*:::Developer Console:::*](https://www.sugarsync.com/developer/account). This identifies the app to SugarSync. After your app is identified in this way to SugarSync, you can develop it. See [Managing Your Access Keys and Applications](/source/dev/managing-apps.md) for further details.
4. Read the remainder of this Getting Started guide. Learn about the [resources available through the API](/source/dev/resources.md) and examine the [code examples](/source/dev/using-api.md). Then browse through our [Resource Reference](/source/dev/api/resource-ref.md) for details on the syntax and semantics of Platform API requests, and start developing.

**[Can't wait? Get started with our Sample App](/source/dev/using-api.md)**

To share ideas and get answers when you need them, be sure to join our [*:::Developer Forum:::*](http://groups.google.com/a/developers.sugarsync.com/group/platform-api/subscribe).

<a name="apiover"></a>
## API Overview

The free SugarSync Platform API is a [REST-based programming interface](#apirest) that enables an application to create, access, or update a user's SugarSync [resources](#resource). Using the API, your application can navigate through the space of computers in a user's account, the folders in any computer, and all the files in a folder. You have the ability toupload and download files to and from the user's account. Plus, additional functions are available, like managing a user's public links or displaying a user's photo albums in a gallery.

<a name="apirest"></a>
## The Platform API and REST

The SugarSync API is a REST-based API. If you're familiar with the REST style of architecture you know that it's based on a number of key principles. Perhaps the most important of these principles are:

<a name="resource"></a>
* Clients interact with servers by requesting resources. A resource is any meaningful concept that can be addressed. In the context of the Platform API, this means that:

  * Your application makes requests through the API for resources such as users, folders, files, and collections (that is, groups of objects).

  See the [Resource Overview](/source/dev/resources.md) for further information about SugarSync resources that you can access through the Platform API.

* All resources are addressed by a global identifier, called a Uniform Resource Identifier or URI. Most often, resources are identified by location, so typically the identifier is a Uniform Resource Locator or URL (that is, a certain type of URI). When you use the Platform API to request a resource, you're actually submitting a request to a web service provided by SugarSync.

  * The SugarSync service is hosted at the URL `https://api.sugarsync.com` and all SugarSync resources are provided from that location. This means that the URL of all available SugarSync resources begins with `https://api.sugarsync.com`.

* Requests are made through a standard interface. Typically, the interface used for making REST-based requests is HTTP, and that's the case in SugarSync.

  * Your application makes requests through the Platform API using HTTP methods such as GET, PUT, POST, and DELETE.

  <a name="apihttp"></a>
## The Platform API and HTTP

If you're not familiar with HTTP, there are some good resources to learn from such as [*:::HTTP Made Really Easy:::*](http://www.jmarshall.com/easy/http/) and the [*:::HTTP Tutorial:::*](http://www.tutorialspoint.com/http/index.htm). And if you want to dig into HTTP in depth, there's the [*:::HTTP specification:::*](http://www.w3.org/Protocols/rfc2616/rfc2616.html).

At the very least, it's important to understand that HTTP is a protocol for sending requests and getting responses over the Internet. HTTP requests have the following format:

* Initial line: Specifies the HTTP method (such as GET or PUT), the path to the requested resource, and the version of HTTP in use.
* One or more header lines that provide additional information about the request.
* Optionally, a message body that provides input data for the request.

For example, an HTTP request that retrieves information about a SugarSync user, might look like this:

```
GET https://api.sugarsync.com/user/566494 HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
```

Here, `https://api.sugarsync.com/user/566494` is the path (or URL) to the user resource for a hypothetical user. Authorization, User-Agent, and Host are header fields, each listed with their respective values. No message body is specified in this request.

Note that the URL of the user resource in the HTTP request example is only illustrative. To obtain the actual URL, your application needs to obtain a refresh token, which it then uses to get an access token. See [Authorization to Make API Requests](#apiauth) for more details. In the process of generating an access token, the SugarSync service returns the URL of the user resource.

The Authorization and User-Agent header field values shown in this and other examples are also illustrative. Your requests should specify in the Authorization field the actual access token created for your application, as well as an application-specific user agent string in the User-Agent field.

* Your requests should specify the access token created for your application as well as an application-specific user agent string.

An HTTP response is formatted similarly to an HTTP request, except that the initial line (called the status line) specifies the HTTP version, a response status code that indicates whether the request was successful or not, and an English reason phrase that describes the status code. The header lines that follow the initial line provide additional information about the response. And, the optional message body provides the output data for the response.

For example, here is what the HTTP response might look like in response to the previous HTTP request (the XML data is formatted for readability):

```
HTTP/1.1 200 OK
Content-Type: application/xml; charset=UTF-8
Date: Wed, 30 Nov 2011 20:00:59 GMT
Accept-Ranges: bytes
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
Transfer-Encoding: chunked

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<user>
<username>jsmith@sugarsync.com</username>
<nickname>jsmith</nickname>
<quota>
<limit>5771362304</limit>
<usage>2550443805</usage>
</quota>
<workspaces>https://api.sugarsync.com/user/5664947/workspaces/contents</workspaces>
<syncfolders>https://api.sugarsync.com/user/5664947/folders/contents</syncfolders>
<deleted>https://api.sugarsync.com/folder/:sc:5664947:9</deleted>
<magicBriefcase>https://api.sugarsync.com/folder/:sc:5664947:2</magicBriefcase>
<webArchive>https://api.sugarsync.com/folder/:sc:5664947:1</webArchive>
<mobilePhotos>https://api.sugarsync.com/folder/:sc:5664947:3</mobilePhotos>
<albums>https://api.sugarsync.com/user/5664947/albums/contents</albums>
<recentActivities>https://api.sugarsync.com/user/5664947/recentActivities/contents</recentActivities>
<receivedShares>https://api.sugarsync.com/user/5664947/receivedShares/contents</receivedShares>
<publicLinks>https://api.sugarsync.com/user/5664947/publicLinks/contents</publicLinks>
<maximumPublicLinkSize>25</maximumPublicLinkSize>
</user>
```

A response status code of 200 means that the request was successful, that is, the user information was successfully retrieved. The Content-Type, Date, Accept-Ranges, Access-Control-Allow-Origin, Server, and Transfer-Encoding lines are response header fields with their values.

The XML data that follows the header is the response body, which contains the requested resource. SugarSync represents resources as XML, as you'll see in [The Platform API and XML](#apixml).

<a name="apixml"></a>
## The Platform API and XML

When you make a request through the Platform API, you either provide XML as input to the SugarSync service to process, or the service generates XML in response to the request. Some requests, such as a request to create an access token, take both XML as input and return XML in response.

The XML consists of elements that describe the resource. For example, if you want to create a file, you need to provide XML input that identifies the name of the file (as it will be displayed to users), and the Internet media type of the file. Here's an example of what the HTTP request and the XML input might look like to create a file (the XML input is formatted for readability):

```
POST https://api.sugarsync.com/folder/:sc:566494:5 HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
Content-Length: 294
Content-Type: application/xml; charset=UTF-8

<?xml version="1.0" encoding="UTF-8" ?>
<file>
<displayName>Winter2012.jpg</displayName>
<mediaType>image/jpeg</mediaType>
</file>
```

As you saw previously in the HTTP response example in [The Platform API and HTTP](#apihttp), if you request information about a user resource, the service generates XML that includes elements that display information such as the user's name, nickname, and current storage utilization. The complete HTTP response should look something like this (the XML output is formatted for readability):

```
HTTP/1.1 200 OK
Content-Type: application/xml; charset=UTF-8
Date: Fri, 22 Oct 2011 08:01:54 GMT
Accept-Ranges: bytes
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
Transfer-Encoding: chunked

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<user>
<username>jsmith@sugarsync.com</username>
<nickname>jsmith</nickname>
<quota>
<limit>5771362304</limit>
<usage>2550443805</usage>
</quota>
<workspaces>https://api.sugarsync.com/user/5664947/workspaces/contents</workspaces>
<syncfolders>https://api.sugarsync.com/user/5664947/folders/contents</syncfolders>
<deleted>https://api.sugarsync.com/folder/:sc:5664947:9</deleted>
<magicBriefcase>https://api.sugarsync.com/folder/:sc:5664947:2</magicBriefcase>
<webArchive>https://api.sugarsync.com/folder/:sc:5664947:1</webArchive>
<mobilePhotos>https://api.sugarsync.com/folder/:sc:5664947:3</mobilePhotos>
<albums>https://api.sugarsync.com/user/5664947/albums/contents</albums>
<recentActivities>https://api.sugarsync.com/user/5664947/recentActivities/contents</recentActivities>
<receivedShares>https://api.sugarsync.com/user/5664947/receivedShares/contents</receivedShares>
<publicLinks>https://api.sugarsync.com/user/5664947/publicLinks/contents</publicLinks>
<maximumPublicLinkSize>25</maximumPublicLinkSize>
</user>
```

Notice that in both of these XML examples, the XML encoding is specified as UTF-8.

* The SugarSync service requires XML input to be encoded using UTF-8 encoding, and generates XML output using UTF-8 encoding.

* Take advantage of persistent URLs: The URLs for a specific user account that your app retrieves from the SugarSync service are persistent for that user account. For example, the URL in the `<magicBriefcase>` element that your app retrieves is a persistent URL that represents the user's Magic Briefcase. Your app can store that URL and reuse it in the future without having to retrieve it every time from that user's user resource.

<a name="apiauth"></a>
## Authorization to Make API Requests

Your application needs to be authorized to access SugarSync resources through the Platform API. Your application gets that authorization through an access token which you create through the API. To get an access token, your app first needs to get a refresh token. Your app can then use the refresh token to create an access token. After creating the access token, your application needs to specify it in the Authorization header when it makes an HTTP request through the API to access a resource.

When your app makes a request to obtain a refresh token, it needs to provide XML input that specifies the username and password of the SugarSync user whose resources your application will access — your app can prompt the user to supply those credentials. The username is the email address associated with the user's SugarSync account.

* Your app does not need to store user credentials: An access token is transient — it can be used to access user resources for a short period time (approximately one hour). However, a refresh token is persistent. Your app can obtain additional access tokens using the same refresh token. There is no need for your app to store the user's credentials between API access requests.

In addition to specifying the username and password, the XML input needs to specify your app's ID as well as your access key ID and private access key. Your app is assigned an ID when you create the app using the Developer Console.. This is the app ID you need to specify in the request for a refresh token. For instructions on creating an app, see [Creating an Application](/source/dev/managing-apps.md#createapp).

You receive your access key ID and private access key when you sign up as a developer with SugarSync.

* Protect your keys: Do not share these keys. Your access key ID and private access key provide an authorized connection to the SugarSync platform exclusively from your app. You don't want to give anyone else this unique access ability.

Although not mandatory, it's good practice to obtain a different access key ID-private access key pair for each application you develop for use with SugarSync. This establishes unique authentication credentials for each application. If you develop more than one app, you'll need to create the additional apps using the Developer Console, and you should also obtain a new access key ID-private access key pair for each additional app you develop. SugarSync provides a maximum of ten access key ID-private access key pairs for each developer account. For instructions on adding access key ID-private access key pairs, see [Adding Keys](/source/dev/managing-apps.md#addkeys).

For more information about creating refresh tokens and access tokens, as well as a code example, see Getting Authorization.

<a name="apiuse"></a>
## Using the API

In general, here's what your application needs to do to use the Platform API:

1. Obtain the pertinent SugarSync user's name and password.
2. Use the user's credentials, your access key ID and private access key, and your app's ID to create a refresh token.
3. Use the refresh token to create an access token.
4. Make an API request to the URL of the pertinent resource.

  * Your app receives the URL of the user resource as part of the response when it creates an access token. Your app can get the URL of other resources, such as the workspaces owned by the user, by making an HTTP GET request to the user resource URL. Your app can get the URL of resources within a workspace, such as the folders in a workspace or the files in a folder, by making an HTTP GET request to the URL of the pertinent workspace or folder contents.

  When your app makes the API request, it needs to:

  * Specify the pertinent HTTP method for the request, such as GET, PUT, POST, or DELETE.
  * Set one or more header lines in the HTTP request header, including the access token.
  * Include XML in the message body if required by the request. For example, if you want to create a subfolder in an existing folder, your app needs to make an HTTP POST request and provide XML that indicates the name of the subfolder that will be displayed to users.

5. Retrieve and process the response. Requests that retrieve information, such as a request to get information about a user, return XML in the message body whose elements provide the information. All requests return a status code indicating whether the request was successful or not.

For code examples, see [Using the Platform API: Examples](/source/dev/using-api.md). Also see the [Resource Reference](/source/dev/api/resource-ref.md) for details on the syntax and semantics of Platform API requests. Good luck with your project!

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
