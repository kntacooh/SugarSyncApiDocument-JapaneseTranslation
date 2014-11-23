> *The Original: [SugarSync for Developers-API Examples: Getting User Information](https://www.sugarsync.com/dev/get-user-info-example.html)*

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

# Getting Information About a User

Suppose you want to create an application that retrieves information about a user who has an account in SugarSync. This is an important task because your app can use the information it retrieves to perform other tasks such as navigate through the user's workspaces or retrieve the user's sync folders.

To get information about a user, your app first needs to create an access token as described in [Getting Authorization](get-auth-token-example.md). Then your app needs to submit the token as part of the user information request. Specifically, it adds the token to the Authorization header.

## The Request

To get user information, your app makes an HTTP GET request to the user resource URL that is returned in the response to the create access token request. The request must also include the access token in the HTTP request header.

* Make an HTTP GET request to the user resource URL to get information about a user's SugarSync account.

The request should look something like this:

```xml
GET https://api.sugarsync.com/user/5664947 HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83...
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
```

Note: The `Authorization` and `User-Agent` header field values shown in this and other examples are only illustrative. Your requests should specify the actual access token created for your application as well as an application-specific user agent string.

<a name="uinfreqxmp"></a>
### Request Code Example

The UserInfo class in the [sample application](using-api.md) provides an example of making a request for user information. You can find the UserInfo class in the application's userinfo folder. Although this is a Java code example, you don't need to use Java to build and submit the request for user information. You can use any means to build and submit the HTTP request.

The sample app requires user information to respond to various requests. For example, it needs information about the user to [display the user's quota data](using-api.md#dispquota), or to list the contents of the user's Magic Briefcase. Let's examine how the sample app requests user information in response to a request for the user's quota data.

### Initial Processing

The request is initially directed in the main method of the SampleTool class (the main class for the app in the tool folder) to the `handleQuotaCommand()` method:

```java
if (command.equals(quotaCmd)) {
  handleQuotaCommand(accessToken);
}
```

Notice that the call to `handleQuotaCommand()` passes the access token as a parameter. The access token is passed as a parameter to other methods in the sample app — especially those that make a request to the Platform API.

<a name="getuinf"></a>
### Getting the User Information

The `handleQuotaCommand()` method, also in the SampleTool class, does the bulk of the processing. The method calls the `getUserInfo()` method (also in the SampleTool class) to retrieve the user's information:

```java
private static void handleQuotaCommand(String accessToken) throws IOException,
        XPathExpressionException {
    HttpResponse httpResponse = getUserInfo(accessToken);

    ...
}
```

Then it extracts the quota data from the returned user information. This is further described in [The Response section](#uinfresphead).

The `getUserInfo()` method in the SampleTool class calls to the `getUserInfo()` method in the UserInfo class:

```java
private static HttpResponse getUserInfo(String accessToken) throws IOException {
    HttpResponse httpResponse = UserInfo.getUserInfo(accessToken);
    validateHttpResponse(httpResponse);
    return httpResponse;
}
```

The `getUserInfo()` method in the SampleTool class then validates the response to ensure that the request was processed successfully (indicated by a status code in the 200 range). If the request is successful, the method returns the response.

Here is the source code for the UserInfo class:

```java
package com.sugarsync.sample.userinfo;

import java.io.IOException;

import org.apache.commons.httpclient.Header;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.methods.GetMethod;

import com.sugarsync.sample.util.HttpResponse;

public class UserInfo {
    /* SugarSync User info API URL */
    private static final String USER_INFO_API_URL = "https://api.sugarsync.com/user";

    /* The User-Agent HTTP Request header's value */
    private static final String API_SAMPLE_USER_AGENT = "SugarSync API Sample/1.0";

    /* Returns a UserInfo java bean containing all user information */
    public static HttpResponse getUserInfo(String accessToken) throws IOException {
        HttpClient client = new HttpClient();
        GetMethod get = new GetMethod(USER_INFO_API_URL);
        get.setRequestHeader("Authorization", accessToken);
        get.setRequestHeader("User-Agent",API_SAMPLE_USER_AGENT);
        client.executeMethod(get);

        // get HTTP response
        Integer statusCode = get.getStatusCode();
        String responseBody = get.getResponseBodyAsString();
        Header[] headers = get.getResponseHeaders();

        return new HttpResponse(statusCode, responseBody, headers);
    }

}
```

The `getUserInfo()` method in the UserInfo class issues an HTTP GET request to the URL for the user resource. Notice that the URL specified in the method is `https://api.sugarsync.com/user`, rather than the user resource URL returned in the response to the create authorization token request. Although issuing an HTTP GET request to `https://api.sugarsync.com/user` will return the user's information, it's a shortcut and not the preferred approach. Your app should specify the user resource URL returned by the create access token request.

Notice too that the request header includes the access token, as well as other header fields.

The generated HTTP request should look something like this:

```xml
GET https://api.sugarsync.com/user HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83...
User-Agent: SugarSync API Sample/1.0
Host: api.sugarsync.com
```

<a name="uinfresphead"></a>
## The Response

In response, the SugarSync service returns a status line followed by response headers and a response body. The status line includes a status code — a status code in the 200 range indicates that the request was successful. If the request was successful, your app will also receive in the response message body, XML that contains the contents of the user resource.

The response should look something like this:

```xml
HTTP/1.1 200 OK
Content-Type: application/xml; charset=UTF-8
Date: Fri, 22 Oct 2011 08:01:54 GMT
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
Transfer-Encoding: chunked

<?xml version="1.0" encoding="UTF-8"?>
<user>
  <username>jsmith@sugarsync.com</username>
  <nickname>jsmith</nickname>
  <quota>
    <limit>2000000000</limit>
    <usage>345000000</usage>
  </quota>
  <workspaces>https://api.sugarsync.com/user/566494/workspaces/contents</workspaces>
  <syncfolders>https://api.sugarsync.com/566494/folders/contents</syncfolders>
  <deleted>https://api.sugarsync.com/folder/:sc:566494:9</deleted>
  <magicBriefcase>https://api.sugarsync.com/folder/:sc:566494:2</magicBriefcase>
  <webArchive>https://api.sugarsync.com/folder/:sc:566494:1</webArchive>
  <mobilePhotos>https://api.sugarsync.com/folder/:sc:566494:3</mobilePhotos>
  <albums>https://api.sugarsync.com/566494/albums/contents</albums>
  <recentActivities>https://api.sugarsync.com/user/566494/recentActivities/contents</recentActivities>
  <receivedShares>https://api.sugarsync.com/user/566494/receivedShares/contents</receivedShares>
  <publicLinks>https://api.sugarsync.com/user/566494/publicLinks/contents</publicLinks>
  <maximumPublicLinkSize>25</maximumPublicLinkSize>
</user>
```

In addition to listing the user's name and nickname, the response message body lists the following:

* The user's current storage utilization in terms of the total storage available to the user and the total storage in use.
* A link to a collection listing the user's workspaces
* A link to a collection listing the user's top-level folders synced to SugarSync.
* A link to a collection listing the user's Deleted Files folder. The Deleted Files folder contains synced files that have been deleted by the user. Files in the Deleted Files folder can be restored.
* A link to a collection listing the user's Magic Briefcase folder. Anything the user puts into the Magic Briefcase is automatically backed up and synced to other devices.
* A link to a collection listing the user's Web Archive folder. The Web Archive is a location for backing up and storing files online with no syncing or updates.
* A link to a collection listing the user's Mobile Photos folder. The Mobile Photos folder is used to store photos (or videos) that the user uploads from a mobile device. The folder can also contain folders and other types of files if the user chooses to put them there.
* A link to a collection listing the user's photo albums
* A link to a collection listing the user's recent activities.
* A link to a collection listing the user's shared folders. A shared folder is a folder which is owned by another user and to which this user has been granted access.
* A link to a collection listing the user's public links. A public link is a link to a resource that the owner sends to a user. This allows the user to access the owner's resource through that link.
  The maximum size of a file for which the user can create a public link.

Notice that some of the links end with a suffix (`:sc:566494:...`). For example, the mobilePhotos link ends with `:sc:566494:3`. The suffix is a SugarSync identifier that uniquely identifies the resource.

### Taking Further Actions

After an application retrieves the user information, it can take further actions on that information. For example, depending on the user request, the sample app extracts certain values from the response message body. Case in point: if the user requests quota information, the sample app extracts the quota values from the response. This is done in the `handleQuotaCommand()` method in the SampleTool class. After calling the `getUserInfo()` method to get the user information, the `handleQuotaCommand()` method uses a utility class, XmlUtil, to extract the quota values from the message response body. It then uses those values to display the total storage available to the user, the storage used, and remaining free storage.

```java
private static void handleQuotaCommand(String accessToken) throws IOException,
        XPathExpressionException {
    HttpResponse httpResponse = getUserInfo(accessToken);

    // read the <quota> node values
    String limit = XmlUtil.getNodeValues(httpResponse.getResponseBody(), "/user/quota/limit/text()").get(
            0);
    String usage = XmlUtil.getNodeValues(httpResponse.getResponseBody(), "/user/quota/usage/text()").get(
            0);

    DecimalFormat threeDForm = new DecimalFormat("#.###");
    // print quota info
    String storageAvailableInGB = threeDForm.format(Double.valueOf(limit) / ONE_GB);
    String storageUsageInGB = threeDForm.format(Double.valueOf(usage) / ONE_GB);
    String freeStorageInGB = threeDForm.format(((Double.valueOf(limit) - Double.valueOf(usage)) / ONE_GB));

    System.out.println("\n---QUOTA INFO---");
    System.out.println("Total storage available: " + storageAvailableInGB + " GB");
    System.out.println("Storage usage: " + storageUsageInGB + " GB");
    System.out.println("Free storage: " + freeStorageInGB + " GB");
}
```

Notice that `getNodeValues()`, the method in XmlUtil that does the actual extraction of the quota values, uses XPath expressions such as `/user/quota/limit/text` to identify the targeted nodes in the XML response body.

The displayed result should look something like this:

```
---QUOTA INFO---
Total storage available: 5.375 GB
Storage usage: 2.377 GB
Free storage: 2.998 GB
```
##  What's Next?

Now that your app has retrieved information about the user, it can use that information to get further information about the user's resources. For example, it can use that information to [list the contents of a folder in the user's workspace](get-folder-info-example.md).

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
