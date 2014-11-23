> *The Original: [SugarSync for Developers-API Examples: Listing the Contents of a Folder](https://www.sugarsync.com/dev/get-folder-info-example.html)*

---

---

* [ホーム](/target/dev/home.md)
* [はじめに](/target/dev/getting-started.md)
* [アプリの管理](/target/dev/managing-apps.md)
* [リソースの概要](/target/dev/resources.md)
* [使用例](/target/dev/using-api.md)
* [ベストプラクティス](/target/dev/best-practices.md)
* [リソースのプラクティス](/target/dev/api/resource-ref.md)
* [*:::開発者フォーラム:::*](http://groups.google.com/a/developers.sugarsync.com/group/platform-api/subscribe)
* [*:::アプリ紹介:::*](https://www.sugarsync.com/partners/)
* [用語解説](/target/dev/glossary.md)
* [プロビジョニング](/target/dev/dev-provisioning.md)

---

# Listing the Contents of a Folder

Assume that your application has retrieved information about a user who has an account in SugarSync, as described in [Getting Information About a User](get-user-info-example.md). Your application can use that information to list the contents of resources in the user's account. This includes resources such as workspaces, synced folders, albums, and files. See [Resources Accessible Through the Platform API](resources.md) for a complete list of resources.

Suppose you wanted to give users of your app the ability to list the contents of a specific folder in their SugarSync account, such as their Magic Briefcase. The Magic Briefcase, like other folders managed by SugarSync, is a collection that can contain other folders and files. Anything that the user puts into the Magic Briefcase folder is automatically backed up and synced to other devices on which the user has SugarSync installed.

## The Request

One of the items of information returned when your app retrieves information about a user, that is, when it issues an HTTP GET request to a user resource, is a link to the collection that represents the user's Magic Briefcase, for example:

```xml
<magicBriefcase>https://api.sugarsync.com/folder/:sc:566494:2</magicBriefcase>
```

Your app can retrieve the contents of the collection, and in this way list the contents of the user's Magic Briefcase, by making an HTTP GET request to the URL that represents the collection's contents.

Your app can retrieve the contents of any collection by appending `/contents` to the URL that represents the collection. So the contents of the Magic Briefcase collection whose URL is listed in the previous example would be the following:

```xml
<magicBriefcase>https://api.sugarsync.com/folder/:sc:566494:2/contents</magicBriefcase>
```

* Make an HTTP GET request to the URL that represents a collection resource to get information about the collection. Make an HTTP GET request to the URL that represents a collection's contents to list the collection's contents.

Here is what the HTTP request to list the Magic Briefcase contents might look like:

```xml
GET https://api.sugarsync.com/folder/:sc:566494:5/contents HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83...
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
```

Notice that your app needs to submit the [access token](get-auth-token-example.md) as part of the request.

Note: The `Authorization` and `User-Agent` header field values shown in this and other examples are only illustrative. Your requests should specify the actual access token created for your application as well as an application-specific user agent string.

<a name="finfreqxmp"></a>
### Request Code Example

One type of request that you can make when you use the [sample application](using-api.md) is [list the contents of a user's Magic Briefcase](using-api.md#listcont). Let's examine how the sample app handles that request. Although this is a Java code example, you don't need to use Java to build and submit the request to list the contents of a folder. You can use any means to build and submit the request.

### Initial Processing

The request is initially directed in the main method of the SampleTool class (the main class for the app in the tool folder) to the handleListCommand() method:

```java
if (command.equals(listCmd)) {
    handleListCommand(accessToken);
}
```

Notice that the call to `handleListCommand()` passes the access token as a parameter. The access token is passed as a parameter to other methods in the sample app — especially those that make a request to the Platform API. The `handleListCommand()` method, also in the SampleTool class, calls the `getMagicBriefcaseFolderContents()` method to retrieve the contents of the Magic Briefcase, and the `printFolderContents()` method to display the contents.

```java
private static void handleListCommand(String accessToken) throws IOException,
        XPathExpressionException, TransformerException {
    HttpResponse folderContentsResponse = getMagicBriefcaseFolderContents(accessToken);

    printFolderContents(folderContentsResponse.getResponseBody());
}
```

### Getting the Link to the Magic Briefcase

To retrieve the contents of the Magic Briefcase, the application needs to get the information about the user and then extract the link to the Magic Briefcase from the information. The `getMagicBriefcaseFolderContents` method calls the `getMagicBriefcaseFolderRepresentation()` method to perform those actions.

```java
private static HttpResponse getMagicBriefcaseFolderContents(String accessToken)
        throws IOException, XPathExpressionException {
    HttpResponse folderRepresentationResponse = getMagicBriefcaseFolderRepresentation(accessToken);
    validateHttpResponse(folderRepresentationResponse);

    ...
}

private static HttpResponse getMagicBriefcaseFolderRepresentation(String accessToken)
        throws IOException, XPathExpressionException {
    HttpResponse userInfoResponse = getUserInfo(accessToken);

    // get the magicBriefcase folder representation link
    String magicBriefcaseFolderLink = XmlUtil.getNodeValues(userInfoResponse.getResponseBody(),
            "/user/magicBriefcase/text()").get(0);

    // make a HTTP GET to the link extracted from user info
    HttpResponse folderRepresentationResponse = SugarSyncHTTPGetUtil.getRequest(magicBriefcaseFolderLink,
            accessToken);
    validateHttpResponse(folderRepresentationResponse);

    return folderRepresentationResponse;
}
```

The call to the `getUserInfo()` method subsequently calls the `getUserInfo()` method in the UserInfo class to request the user information. The `UserInfo.getUserInfo()` method issues an HTTP GET request to the URL for the user resource to retrieve that information. See [Getting Information About a User: Request Code Example](get-user-info-example.md#uinfreqxmp) for details about these methods and the UserInfo class.

If the HTTP GET request for user information is successful, the SugarSync service returns in the response message body, XML that contains the contents of the user resource. See [Getting Information About a User: The Response](get-user-info-example.md#uinfresphead) to see the full response body.

One of the items in the message body is the link to the Magic Briefcase. The `getMagicBriefcaseFolderRepresentation()` uses a utility class, XmlUtil, to extract the Magic Briefcase link from the message response body. Notice that `getNodeValues()`, the method in XmlUtil that does the actual extraction of the link, uses an XPath expression, `/user/magicBriefcase/text`, to identify the targeted node in the XML response body.

### Getting the Link to the Magic Briefcase Contents

Now that the sample app has retrieved the link to the Magic Briefcase, it uses it to get its representation. The representation contains information about the Magic Briefcase, such as its display name, the date and time the Magic Briefcase was created, and significantly, a link to the Magic Briefcase contents. To get the representation, the `getMagicBriefcaseFolderRepresentation()` method uses a utility class named SugarSyncHTTPGetUtil, which you can find in the util folder.

Here is the source code for the SugarSyncHTTPGetUtil class:

```java
package com.sugarsync.sample.util;

import java.io.IOException;

import org.apache.commons.httpclient.Header;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.methods.GetMethod;

public class SugarSyncHTTPGetUtil {
    /* The User-Agent HTTP Request header's value */
    private static final String API_SAMPLE_USER_AGENT = "SugarSync API Sample/1.0";

    /* Makes a HTTP GET request to the url */
    public static HttpResponse getRequest(String url, String authToken) throws IOException {
        // make the HTTP GET request
        HttpClient client = new HttpClient();
        GetMethod get = new GetMethod(url);
        get.setRequestHeader("Authorization", authToken);
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

The `getRequest()` method in the SugarSyncHTTPGetUtil class issues an HTTP GET request to the URL for the resource, in this case, the URL represents the user's Magic Briefcase. The request header includes the access token, as well as other header fields.

The generated HTTP request should look something like this:

```xml
GET https://api.sugarsync.com/folder/:sc:566494:5 HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83...
User-Agent: SugarSync API Sample/1.0
Host: api.sugarsync.com
```

After issuing the GET request, the `getRequest()` method gets the response, including the status code, response body, and response headers.

Notice that the class returns the response in HttpResponse.

```java
return new HttpResponse(statusCode, responseBody, headers);
```

The HttpResponse class is a Java bean class that you can find in the application's util folder. The Java bean contains properties to hold the status code, response headers, and response body. It also has get methods to get these values. Other classes in the application, such as those that download a file use HttpResponse as a utility to return the HTTP response to the caller.

## The Response

In response to the request for a folder representation, the SugarSync service returns a status line followed by response headers and a response body. The status line includes a status code — a status code in the 200 range indicates that the request was successful. If the request was successful, the app will also receive in the response message body, XML that contains the folder representation. If the folder is the user's Magic Briefcase (as it is in the sample application), the response should look something like this:

```xml
HTTP/1.1 200 OK
Content-Type: application/xml; charset=UTF-8
Date: Tue, 29 Nov 2011 23:06:11 GMT
Accept-Ranges: bytes
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
Transfer-Encoding: chunked

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<folder>
  <displayName>Magic Briefcase</displayName>
  <dsid>/sc/566494/2</dsid>
  <timeCreated>2011-11-15T13:41:35.000-08:00</timeCreated>
  <collections>https://api.sugarsync.com/folder/:sc:566494:2/contents?type=folder</collections>
  <files>https://api.sugarsync.com/folder/:sc:566494:2/contents?type=file</files>
  <contents>https://api.sugarsync.com/folder/:sc:566494:2/contents</contents>
  <sharing enabled="false"/>
</folder>
```

  Notice that some of the links end with a suffix (`:sc:566494:...`). The suffix is a SugarSync identifier that uniquely identifies the resource.

### Taking Further Actions

After retrieving the folder representation, an app can take further actions. For example, the sample app extracts the contents link and uses it to retrieve the Magic Briefcase contents. This happens in the `getMagicBriefcaseFolderContents()` method.

```java
private static HttpResponse getMagicBriefcaseFolderContents(String accessToken)
        throws IOException, XPathExpressionException {
    ...

    String magicBriefcaseFolderContentsLink = XmlUtil.getNodeValues(
            folderRepresentationResponse.getResponseBody(), "/folder/contents/text()").get(0);
    HttpResponse folderContentsResponse = SugarSyncHTTPGetUtil.getRequest(
            magicBriefcaseFolderContentsLink, accessToken);
    validateHttpResponse(folderContentsResponse);

    return folderContentsResponse;
}
```

Notice that once again, the `SugarSyncHTTPGetUtil.getRequest()` method is used. But this time the URL passed to the method is the link to the Magic Briefcase contents (previously it was the link to the Magic Briefcase resource).

If the request is successful, the app will receive in the response message body, XML that contains the folder contents. For the Magic Briefcase contents request, the full response should look something like this:

```xml
HTTP/1.1 200 OK
Content-Type: application/xml; charset=UTF-8
Date: Tue, 29 Nov 2011 23:06:11 GMT
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
Transfer-Encoding: chunked

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<collectionContents start="0" hasMore="false" end="7">
  <collection type="folder">
    <displayName>Sample Documents</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:190_53764886</ref>
    <contents>https://api.sugarsync.com/folder/:sc:566494:190_53764886/contents</contents>
  </collection>
  <collection type="folder">
    <displayName>Sample Music</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:190_53765326</ref>
    <contents>https://api.sugarsync.com/folder/:sc:566494:190_53765326/contents</contents>
  </collection>
  <collection type="folder">
    <displayName>Sample Photos</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:190_53765358</ref>
    <contents>https://api.sugarsync.com/folder/:sc:566494:190_53765358/contents</contents>
  </collection>
  <collection type="folder">
    <displayName>Work Items</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:6552993_104375</ref>
    <contents>https://api.sugarsync.com/folder/:sc:566494:6552993_104375/contents</contents>
  </collection>
  <file>
    <displayName>What is Magic Briefcase.pdf</displayName>
    <ref>https://api.sugarsync.com/file/:sc:566494:6552993_17237</ref>
    <size>94055</size>
    <lastModified>2011-11-23T16:35:34.000-08:00</lastModified>
    <mediaType>application/octet-stream</mediaType>
    <presentOnServer>true</presentOnServer>
    <fileData>https://api.sugarsync.com/file/:sc:566494:6552993_17237/data</fileData>
  </file>
  <file>
    <displayName>George_Washington.jpg</displayName>
    <ref>https://api.sugarsync.com/file/:sc:566494:6552993_814093</ref>
    <size>10165</size>
    <lastModified>2011-12-12T10:31:22.000-08:00</lastModified>
    <mediaType>image/jpeg</mediaType>
    <presentOnServer>true</presentOnServer>
    <fileData>https://api.sugarsync.com/file/:sc:566494:6552993_814093/data</fileData>
  </file>
</collectionContents>
```

### Displaying the Results

The last step in the process of handling the Magic Briefcase list request is to display the list. This happens in the printFolderContents() method.

```java
private static void printFolderContents(String responseBody) {
    try {
        List folderNames = XmlUtil.getNodeValues(responseBody,
        "/collectionContents/collection[@type=\"folder\"]/displayName/text()");
        List fileNames = XmlUtil.getNodeValues(responseBody,
                "/collectionContents/file/displayName/text()");
        System.out.println("\n-MagicBriefcase");
        System.out.println("\t-Folders:");
        for (String folder : folderNames) {
            System.out.println("\t\t" + folder);
        }
        System.out.println("\t-Files:");
        for (String file : fileNames) {
            System.out.println("\t\t" + file);
        }
    } catch (XPathExpressionException e1) {
        System.out.println("Error while printing the folder contents:");
        System.out.println(responseBody);
    }
}
```

Here too the sample app uses the `XmlUtil.getNodeValues()` method with XPath expression parameters to extract the targeted nodes in the XML response for display.

The displayed result should look something like this:

```
-MagicBriefcase
      -Folders:
              Sample Documents
              Sample Music
              Sample Photos
              Work Items
      -Files:
              What is Magic Briefcase.pdf
              George_Washington.jpg
```

<a name="tailorres"></a>
### Tailoring the Results

The results shown in the Magic Briefcase example only include a small list of folders and files. Imagine that a folder had a very large number of folders and files. In that case, you might want your app to limit, filter, or in some other way arrange the results to fit your requirements. You can do that by specifying query parameters in the request to retrieve the contents of a folder.

Your app can specify any of the following parameters:

* type, to filter the results. You can specify `type=folder` to restrict the results to folders, or `type=file` to restrict the results to files.
* start, to set the index within the indexed sequence of objects in the folder to start listing folder contents. The default value is 0, meaning that objects in the folder are listed starting with the first object in the sequence. However, you can specify any positive integer value for the starting index.
* max, to limit the number of items in the result. The default value is 500, meaning that no more than 500 objects in the folder will be listed in the results. However, you can specify any positive integer value for the maximum.
* order, to sort the results. You can sort by:
  * name: The display name of the items
  * last_modified: The last-modified date (if available) of the items
  * size: The size (in bytes) of the items
  * extension: The filename extension (if available) of the items

For example, the following URL and parameters can be used to request only the files starting at index 30 in the folder, and limit retrieval to 50 files:

```
https://api.sugarsync.com/folder/:sc:566494:5/contents?type=file&start=30&max=50
```

The following URL and parameters can be used to request only folders and to sort the folders by display name:

```
https://api.sugarsync.com/folder/:sc:566494:5/contents?type=folder&order=name
```
Note that your app can use the start and max parameters in combination to retrieve the contents of a folder in chunks. For example, it can specify `start=0` and `max=50` to retrieve the first 50 objects in the folder; `start=50` and `max=50` to retrieve the next 50 objects, and so on.

See [Retrieving Folder Contents](api/method/get-folder-contents.md) for further information about the parameters that can be specified in a request to retrieve the contents of a folder.

## What's Next?
Now that your app has retrieved the contents of a folder, let's download a file in that folder. You'll learn how to do that in [Downloading a File](download-file-example.md).

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API利用規約](/source/dev/terms.md)
