> *The Original: [SugarSync for Developers-API Examples: Creating a File](https://www.sugarsync.com/dev/create-file-example.html)*

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

# Creating a File

Suppose you want your application to enable users to create new files. To make that happen, all your app needs to do is issue an HTTP POST request to the URL that represents the target folder, and provide XML in the message body that identifies the display name and media type of the new file.

* To create a file, all your app needs to do is issue an HTTP POST request to the URL that represents the target folder, and provide XML in the message body that identifies the display name and media type of the new file.

## The Request

Here's what the request to create a file might look like:

```xml
POST https://api.sugarsync.com/folder/:sc:566494:5 HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
Content-Length: 294
Content-Type: application/xml; charset=UTF-8

<?xml version="1.0" encoding="UTF-8" ?>
<file>
  <displayName>GeorgeWashington.jpg</displayName>
  <mediaType>image/jpeg</mediaType>
</file>
```

Notice that your app needs to submit the [access token](get-auth-token-example.md) as part of the request.

Note: The `Authorization` and `User-Agent` header field values shown in this and other examples are only illustrative. Your requests should specify the actual access token created for your application as well as an application-specific user agent string.

<a name="crfreqxmp"></a>
### Request Code Example

The FileCreation class in the [sample application](using-api.md) provides an example of creating a file. You can find the FileCreation class in the application's file folder. Although this is a Java code example, you don't need to use Java to build and submit the request to create a file. You can use any means to build and submit the HTTP request.

The sample app creates a file as part of its processing of an [upload file request](using-api.md#upfile). To upload a file to a folder managed by SugarSync, an app first creates the file in the target folder and then uploads the data to the new file. The sample app handles requests to upload a file to the Magic Briefcase, so it first needs to create the file in the Magic Briefcase and then upload the data to the new file.

* To upload a file to a folder managed by SugarSync, an app first creates the file in the target folder and then uploads the data to the new file.

Let's examine how the sample app creates the file. We'll examine how the sample app uploads data to the file in [Uploading Data to a File](upload-file-data-example.md).

### Initial Processing

The request to upload a file is initially directed in the main method of the SampleTool class (the main class for the app in the tool folder) to the `handleUploadCommand()` method:

```java
if (command.equals(uploadCmd)) {
    String file = argumentList.get(argumentList.size() - 1);
    handleUploadCommand(accessToken, file);
}
```

Notice that the call to `handleUploadCommand()` passes as parameters the access token and the local file to be uploaded. The access token is passed as a parameter to other methods in the sample app — especially those that make a request to the Platform API.

The `handleUploadCommand()` method, also in the SampleTool class, verifies that the file to be uploaded is in the current local directory. If the file isn't in the current local directory, the method displays a message and exits. If the file is in the current local directory, the method calls the `getUserInfo()` method to retrieve information about the user. See [Getting the User Information](get-user-info-example.md#getuinf) for details about the `getUserInfo()` method and how it retrieves user information.

```java
private static void handleUploadCommand(String accessToken, String file)
        throws XPathExpressionException, IOException {
    if (!(new File(file).exists())) {
        System.out.println("\nFile " + file + "  does not exist in the current directory");
        System.exit(0);
    }
    HttpResponse userInfoResponse = getUserInfo(accessToken);

    String magicBriefcaseFolderLink = XmlUtil.getNodeValues(userInfoResponse.getResponseBody(),
            "/user/magicBriefcase/text()").get(0);

    ...

}
```

The `handleUploadCommand()` method then uses the `getNodeValues()` method in the XmlUtil utility class to extract the link to the Magic Briefcase listed in the message response body. The `getNodeValues()` method uses an XPath expression to identify the magicBriefcase node, which contains the link to the Magic Briefcase. For example:

```xml
<magicBriefcase>https://api.sugarsync.com/folder/:sc:566494:2</magicBriefcase>
```

The link will be used in the request to create the file.

### Creating the File

After the `handleUploadCommand()` method retrieves the link to the Magic Briefcase, it calls the `createFile()` method in the FileCreation class to create the file. When it makes the call to `createFile()`, the `handleUploadCommand()` method passes it four parameters: the link to the target folder (in this case, the link to the Magic Briefcase), the display name of the file to be created, the media type of the file to be created, and the access token.

```java
private static void handleUploadCommand(String accessToken, String file)
        throws XPathExpressionException, IOException {
    ...

    HttpResponse resp = FileCreation.createFile(magicBriefcaseFolderLink, file, "", accessToken);

    ...

}
```

You can find the FileCreation class in the file folder. Here is the source code for the FileCreation class:

```java
package com.sugarsync.sample.file;

import java.io.IOException;

import org.apache.commons.httpclient.Header;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.methods.PostMethod;
import org.apache.commons.httpclient.methods.RequestEntity;
import org.apache.commons.httpclient.methods.StringRequestEntity;

import com.sugarsync.sample.util.HttpResponse;

public class FileCreation {

    /* The User-Agent HTTP Request header's value */
    private static final String API_SAMPLE_USER_AGENT = "SugarSync API Sample/1.0";

    /* The template used for creating the file representation */

    private static final String CREATE_FILE_REQUEST_TEMPLATE = "<?xml version=\"1.0\" encoding=\"UTF-8\" ?>"
            + "<file>"
            + "<displayName>%s</displayName>"
            + "<mediaType>%s</mediaType>"
            + "</file>";

    /* Creates a file representation in the folder specified by folderURL parameter. */
    public static HttpResponse createFile(String folderURL, String displayName, String mediaType,
            String accessToken) throws IOException {
        // fill the request template with file representation details
        String request = fillRequest(displayName, mediaType);
        // make a HTTP POST to the folderURL where the file representation will
        // be created
        HttpClient client = new HttpClient();
        PostMethod post = new PostMethod(folderURL);
        RequestEntity entity = new StringRequestEntity(request, "application/xml", "UTF-8");
        post.setRequestHeader("Authorization", accessToken);
          post.setRequestHeader("User-Agent",API_SAMPLE_USER_AGENT);
        post.setRequestEntity(entity);
        client.executeMethod(post);

        // get HTTP response
        Integer statusCode = post.getStatusCode();
        String responseBody = post.getResponseBodyAsString();
        Header[] headers = post.getResponseHeaders();

        return new HttpResponse(statusCode, responseBody, headers);
    }

    /* Fills the request template with the file representation details */
    private static String fillRequest(String displayName, String mediaType) {
        return String.format(CREATE_FILE_REQUEST_TEMPLATE, new Object[] { displayName, mediaType });
    }
}
```

The `createFile()` method uses the `fillRequest()` method in the FileCreation class to build an XML input message body for the request that identifies the display name and media type of the new file. Then the `createFile()` method issues an HTTP POST request to the URL that represents the targeted folder (that is, the URL of the Magic Briefcase). The request header includes the access token, as well as other header fields.

The generated HTTP request should look something like this:

```xml
POST https://api.sugarsync.com/folder/:sc:566494:5 HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
User-Agent: SugarSync API Sample/1.0
Host: api.sugarsync.com
Content-Length: 294
Content-Type: application/xml; charset=UTF-8

<?xml version="1.0" encoding="UTF-8" ?>
<file>
  <displayName>GeorgeWashington.jpg</displayName>
  <mediaType>image/jpeg</mediaType>
</file>
```

After issuing the POST request, the `createFile()` method gets the response, including the status code, response body, and response headers.

Notice that the class returns the response in HttpResponse.

```java
return new HttpResponse(statusCode, responseBody, headers);
```

The HttpResponse class is a Java bean class that you can find in the application's util folder. The Java bean contains properties to hold the status code, response headers, and response body. It also has get methods to get these values. Other classes in the application, such as those that get user information, use HttpResponse as a utility to return the HTTP response to the caller.

## The Response

In response, the SugarSync service returns a status line followed by response headers. The status line includes a status code — a status code in the 200 range indicates that the request was successful, that is, the file was successfully created. The response headers include the content type, content length, date and time of the response, and location of the created file. The response should look something like this:

```xml
HTTP/1.1 201 Created
Content-Type: application/octet-stream; charset=UTF-8
Content-Length: 0
Date: Mon, 12 Dec 2011 16:48:39 GMT
Location: https://api.sugarsync.com/file/:sc:566494:190_113111357
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
```

The URL for the newly-created file's data resource is its `Location` value followed by `/data`. For example:

```
https://api.sugarsync.com/file/:sc:566494:190_113111357/data
```

The sample app will use the URL of the file data resource to upload data to the file.

## What's Next?

Now that the file is created, learn how to [upload data to the file](upload-file-data-example.md).

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
