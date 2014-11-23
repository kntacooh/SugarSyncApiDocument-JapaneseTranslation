> *The Original: [SugarSync for Developers-API Examples: Uploading Data to a File](https://www.sugarsync.com/dev/upload-file-data-example.html)*

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

# Uploading Data to a File

Your app can upload data to a file by issuing an HTTP PUT request to the URL that represents the data resource for the target file. The URL for the data resource is the URL that represents the file followed by `/data`. For example, if the URL that represents the target file is `https://api.sugarsync.com/file/:sc:566494:190_113199132`, the URL that represents the data resource for the file is `https://api.sugarsync.com/file/:sc:566494:190_113199132/data`.

The PUT request needs to specify the location of the file that contains the source data, as well the size of the data (that is, its content length). If your app doesn't know the size of the source data, it can specify chunked transfer encoding. In this case, the server doesn't need to know the length of the content before it starts transmitting a response to your app.

* To upload data to a file, your app needs to issue an HTTP PUT request to the URL that represents the data resource for the target file. The PUT request needs to specify the location of the file that contains the source data, as well the size of the data.

Uploading data to a file creates a new file version in the SugarSync datastore and associates the uploaded data with the newly created file version.

Although this approach is relatively simple — it's a one-step process — it's important to understand that if the data upload does not successfully complete, that is, if all the data is not completely uploaded, the approach does not allow your app to resume the upload from the point of failure. Your app would have to re-request the entire file data upload.

However, there is a way to upload data to a file and allow your app to resume the data upload from the point of failure. But this approach involves more steps.

Here, we'll focus on the first approach, that is, the simpler approach. For more information about the resumable upload approach, see [Resumable Uploads](#resumeup).

## The Request

Here's what the request to upload data to a file might look like:

```xml
PUT https://api.sugarsync.com/file/:sc:566494:190_113199132/data HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
Content-Length: 1431
```

Notice that your app needs to submit the [access token](get-auth-token-example.md) as part of the request.

Note: The `Authorization` and `User-Agent` header field values shown in this and other examples are only illustrative. Your requests should specify the actual access token created for your application as well as an application-specific user agent string.

### Request Code Example

One type of request that you can make when you use the [sample application](using-api.md) is [upload a file from the current local directory to the Magic Briefcase](using-api.md#upfile). In processing that request, the sample app first creates the file in the Magic Briefcase and then uploads the data from the file in the local directory to the new file.

* To upload a file to a folder managed by SugarSync, an app first creates the file in the target folder and then uploads the data to the new file.

We've already described how the sample app creates a file in [Creating a File](create-file-example.md). Here, we'll cover how the sample app uploads data to the new file. Although this is a Java code example, you don't need to use Java to build and submit the request to upload data to a file. You can use any means to build and submit the HTTP request.

Recall that one of the items of information that the sample app retrieves when it creates a file is the location of the new file. The location appears in the response header, for example:

```xml
HTTP/1.1 201 Created
Content-Type: application/octet-stream; charset=UTF-8
Content-Length: 0
Date: Mon, 12 Dec 2011 16:48:39 GMT
Location: https://api.sugarsync.com/file/:sc:566494:190_113111357
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
```

An application can construct the URL of the new file's data resource by simply adding a `/data` suffix to the location of the new file. The sample app does that in the `handleUploadCommand()` method.

```java
private static void handleUploadCommand(String accessToken, String file)
        throws XPathExpressionException, IOException {
    ...

    HttpResponse resp = FileCreation.createFile(magicBriefcaseFolderLink, file, "", accessToken);

    String fileDataUrl = resp.getHeader("Location").getValue() + "/data";
    ...

}
```

The `handleUploadCommand()` method can then use the URL of the file data resource to upload data to the new file.

### Uploading Data to the File

After the `handleUploadCommand()` method constructs the URL of the file data resource, it calls the `uploadFile()` method in the FileUploadAPI class to upload the data to the file. You can find the FileUploadAPI class in the file folder.

When it makes the call to `uploadFile()`, the `handleUploadCommand()` method passes it three parameters: the URL of the target file's data resource, the path to the local file that contains the source data, and the access token.

```java
private static void handleUploadCommand(String accessToken, String file)
        throws XPathExpressionException, IOException {
    ...

    resp = FileUploadAPI.uploadFile(fileDataUrl, file, accessToken);

    System.out.println("\nUpload completed successfully. Check \"MagicBriefcase\" remote folder");

}
```

Here is the source code for the FileUploadAPI class:

```java
package com.sugarsync.sample.file;

import java.io.File;
import java.io.IOException;

import org.apache.commons.httpclient.Header;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.methods.FileRequestEntity;
import org.apache.commons.httpclient.methods.PutMethod;
import org.apache.commons.httpclient.methods.RequestEntity;

import com.sugarsync.sample.util.HttpResponse;

public class FileUploadAPI {

    /* The User-Agent HTTP Request header's value */
    private static final String API_SAMPLE_USER_AGENT = "SugarSync API Sample/1.0";

    /* Uploads a local file to the fileDataUrl (Make a HTTP PUT request to fileDataUrl ) */
    public static HttpResponse uploadFile(String fileDataUrl, String localFilePath, String accessToken)
            throws org.apache.commons.httpclient.HttpException, IOException {
        // make the HTTP PUT request
        HttpClient client = new HttpClient();
        PutMethod put = new PutMethod(fileDataUrl);
        File input = new File(localFilePath);
        RequestEntity entity = new FileRequestEntity(input, "Content-Length: " + input.length());
        put.setRequestEntity(entity);
        put.setRequestHeader("Authorization", accessToken);
        put.setRequestHeader("User-Agent",API_SAMPLE_USER_AGENT);
        client.executeMethod(put);

        // get HTTP response
        Integer statusCode = put.getStatusCode();
        String responseBody = put.getResponseBodyAsString();
        Header[] headers = put.getResponseHeaders();

        return new HttpResponse(statusCode, responseBody, headers);
    }

}
```

The `uploadFile()` method builds HTTP PUT request header that include the size of the data (that is, its content length) to be uploaded as well as the access token. It then issues the request.

The generated HTTP request should look something like this:

```xml
PUT https://api.sugarsync.com/file/:sc:566494:190_113199132/data HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
User-Agent: SugarSync API Sample/1.0
Host: api.sugarsync.com
Content-Length: 1431
```

After issuing the PUT request, the `uploadFile()` method gets the response, including the status code, response body, and response headers.

Notice that the class returns the response in HttpResponse.

```java
return new HttpResponse(statusCode, responseBody, headers);
```

The HttpResponse class is a Java bean class that you can find in the application's util folder. The Java bean contains properties to hold the status code, response headers, and response body. It also has get methods to get these values. Other classes in the application, such as those that get user information, use HttpResponse as a utility to return the HTTP response to the caller.

## The Response

In response, the SugarSync service returns a status line followed by response headers. The status line includes a status code — a status code in the 200 range indicates that the request was successful, that is, the file data was successfully uploaded. The response headers include the content type, content length, and the date and time of the response. The response should look something like this:

```xml
HTTP/1.1 204 OK
Content-Type: application/octet-stream; charset=UTF-8
Content-Length: 0
Date: Wed, 04 Jan 2012 21:58:25 GMT
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
```

The sample app then displays a message that the data upload operation was successful. The displayed message should look something like this:

```
Upload completed successfully. Check "Magic Briefcase" remote folder
```

<a name="resumeup"></a>
### Resumable Uploads

Here are the steps your app can take to upload data to a file and resume the upload if the upload does not complete:

1. Create a new version of the file.
2. Upload data to the new file version.
3. If the upload does not complete, determine the number of bytes of data that the server actually received. Then upload the remaining data to the file, starting after the last data byte that was uploaded.

### Creating a New Version of the File

The representation of a file contains a pointer to the version history of the file, as described in [File Resource](api/file-resource.md). Here's what the version history pointer might look like:

```xml
<versions>https://api.sugarsync.com/file/:sc:566494:6552993_66025/version</versions>
```

To create a new version of the file, your app issues an HTTP POST request to the version history URL, as described in [Creating a New File Version](api/method/create-new-version.md). The request might look like this:

```xml
POST https://api.sugarsync.com/file/:sc:566494:6552993_66025/version HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83...
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
```

The response will include the location of the new file version, for example:

```xml
Location: https://api.sugarsync.com/file/:sc:566494:6552993_66025/version/190_974926
```

### Uploading Data to the New File Version

After the new version is created, you app can upload to it by issuing an HTTP PUT request to the URL that represents the data resource for the new version. For example, if the URL that represents the new version of the file is `https://api.sugarsync.com/file/:sc:566494:190_113199132/version/190_974926`, the URL that represents the data resource for the new version is `https://api.sugarsync.com/file/:sc:566494:190_113199132/version/190_974926/data`.

The request might look like this:

```xml
PUT https://api.sugarsync.com/file/:sc:566494:190_113199132/version/190_974926/data HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
Content-Length: 150216
```

If the data upload completes successfully, the XML response body will include a `<presentOnServer>` element value of `true` and a `<size>` element that indicates the size of the file, for example:

```xml
<fileVersion>
  <size>150216</size>
  ...
  <presentOnServer>true</presentOnServer>
  ...
</fileVersion>
```

Because the upload completed successfully, there is no need to resume the upload.

### Determining the Number of Bytes that Were Uploaded

If the data upload does not complete successfully, the XML response body will include a `<presentOnServer>` element value of `false` and a `<storedSize>` element value that indicates how many bytes were uploaded to the server. The following example indicates that 140191 bytes were uploaded to the server, but that the upload did not complete successfully:

```xml
<fileVersion>
  <storedSize>140191</storedSize>
  ...

  <presentOnServer>false</presentOnServer>
  ...
</fileVersion>
```

### Uploading the Remaining Data

Once your app determines from the `<storedSize>` value how many bytes were successfully uploaded, it can upload the remaining data. To do that, your app reissues an HTTP PUT request to the URL that represents the data resource for the new version. However, it also needs to specify in the `Range` header of the request the `<storedSize>` value. This will request that the remaining data be uploaded from the point of failure. Here's an example of the request:

```xml
PUT https://api.sugarsync.com/file/:sc:566494:190_113199132/version/190_974926/data HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
Range: bytes=140191-
Content-Length: 10025
```

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
