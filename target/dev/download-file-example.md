> *The Original: [SugarSync for Developers-API Examples: Downloading a File](https://www.sugarsync.com/dev/download-file-example.html)*

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

# Downloading a File

Assume that a user has used your application to [list the contents of a folder](get-folder-info-example.md). Now you'd like the app to enable the user to download a selected file from that folder. Implementing that is easy.

## The Request

Recall that when your app lists the contents of a folder, the response message body includes XML that contains a representation of each collection or individual file in the folder. The representation of a file should look something like this:

```xml
<file>
  <displayName>Golden Gate Bridge.jpg</displayName>
  <ref>https://api.sugarsync.com/file/:sc:566494:6552993_814093</ref>
  <size>18241</size>
  <lastModified>2011-12-12T10:31:22.000-08:00</lastModified>
  <mediaType>image/jpeg</mediaType>
  <presentOnServer>true</presentOnServer>
  <fileData>https://api.sugarsync.com/file/:sc:566494:6552993_814093/data</fileData>
</file>
```

The `<fileData>` element points to the data resource for the file. To download a file, all your app needs to do is issue an HTTP GET request to the data resource for the file.

* To download a file, all your app needs to do is issue an HTTP GET request to the data resource for the file.

The request should look something like this:

```xml
GET https://api.sugarsync.com/file/:sc:566494:6552993_814093/data HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
```

Notice that your app needs to submit the [access token](get-auth-token-example.md) as part of the request.

Note: The `Authorization` and `User-Agent` header field values shown in this and other examples are only illustrative. Your requests should specify the actual access token created for your application as well as an application-specific user agent string.

### Request Code Example

One type of request that you can make when you use the [sample application](using-api.md) is [download a file from the user's Magic Briefcase to the current local directory](using-api.md#downfile). Let's examine how the sample app handles that request. Although this is a Java code example, you don't need to use Java to build and submit the request to download a file. You can use any means to build and submit the request.

### Initial Processing

The request is initially directed in the main method of the SampleTool class (the main class for the app in the tool folder) to the `handleDownloadCommand()` method:

```java
if (command.equals(downloadCmd)) {
  String file = argumentList.get(argumentList.size() - 1);
  handleDownloadCommand(accessToken, file);
}
```

Notice that the call to `handleDownloadCommand()` passes as parameters the access token and the file to be downloaded. The access token is passed as a parameter to other methods in the sample app — especially those that make a request to the Platform API.

The `handleDownloadCommand()` method, also in the SampleTool class, calls the `getMagicBriefcaseFolderContents()` method to list the contents of the Magic Briefcase. See [Listing the Contents of a Folder: Request Code Example](get-folder-info-example.md#finfreqxmp) for details about the `getMagicBriefcaseFolderContents()` method and how it is used to list the contents of the Magic Briefcase.

The `handleDownloadCommand()` method then verifies that the file to be downloaded is in the Magic Briefcase. If the file is not in the Magic Briefcase, the method displays a "not found" message.

```java
private static void handleDownloadCommand(String accessToken, String file) throws IOException,
        XPathExpressionException {
    HttpResponse magicBriefcaseContents = getMagicBriefcaseFolderContents(accessToken);

    List<String> fileDataLink = XmlUtil.getNodeValues(magicBriefcaseContents.getResponseBody(),
    "collectionContents/file[displayName=\"" + file + "\"]/fileData/text()");
    if (fileDataLink.size() == 0) {
        System.out.println("\nFile " + file + " not found in MagicBriefcase folder");
        System.exit(0);
    }
    ...
}
```

Notice that `handleDownloadCommand()` method uses the `getNodeValues()` method in the XmlUtil utility class to extract the displayName and fileData nodes for the files listed in the message response body. The getNodeValues() method uses XPath expressions to identify the targeted nodes in the XML response body. The fileData node contains the link to the data resource for the file and will be used in the download request.

### Downloading the File

After the `handleDownloadCommand()` method retrieves the link to the data resource for the file, it calls the `downloadFileData()` method in the FileDownloadAPI class to request the download. When it makes the call to `downloadFileData()`, the `handleDownloadCommand()` method passes it three parameters: the link to the data resource for the file, the path to the current local directory where the file will be downloaded, and the access token.

```java
private static void handleDownloadCommand(String accessToken, String file) throws IOException,
  XPathExpressionException {
    ...

    HttpResponse fileDownloadResponse = FileDownloadAPI.downloadFileData(fileDataLink.get(0), file,
    accessToken);
    validateHttpResponse(fileDownloadResponse);

    System.out.println("\nDownload completed successfully. The " + file
    + " from \"MagicBriefcase\" was downloaded to the local directory.");
  }
```

You can find the FileDownloadAPI class in the file folder. Here is the source code for the FileDownloadAPI class:

```java
package com.sugarsync.sample.file;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;

import org.apache.commons.httpclient.Header;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.methods.GetMethod;

import com.sugarsync.sample.util.HttpResponse;

public class FileDownloadAPI {

    /* The User-Agent HTTP Request header's value  */
    private static final String API_SAMPLE_USER_AGENT = "SugarSync API Sample/1.0";

    /* Downloads a remote file to the localDownloadPath */
    public static HttpResponse downloadFileData(String fileDataURL, String localDownloadPath, String accessToken)
            throws IOException {
        // makes the HTTP get request
        HttpClient client = new HttpClient();
        GetMethod get = new GetMethod(fileDataURL);
        get.setRequestHeader("Authorization", accessToken);
        get.setRequestHeader("User-Agent",API_SAMPLE_USER_AGENT);
        client.executeMethod(get);

        // get the input stream of the response and write its content to the
        // local file
        InputStream in = get.getResponseBodyAsStream();
        FileOutputStream out = new FileOutputStream(new File(localDownloadPath));
        byte[] b = new byte[1024];
        int len = 0;
        while ((len = in.read(b)) != -1) {
            out.write(b, 0, len);
        }
        in.close();
        out.close();

        // get HTTP response
        Integer statusCode = get.getStatusCode();
        Header[] headers = get.getResponseHeaders();

        return new HttpResponse(statusCode, null, headers);
    }

  }
```

The `downloadFileData()` method in the FileDownloadAPI class issues an HTTP GET request to the URL that represents the data resource for the targeted file. The request header includes the access token, as well as other header fields.

The generated HTTP request should look something like this:

```xml
GET https://api.sugarsync.com/file/:sc:566494:6552993_814093/data HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
User-Agent: SugarSync API Sample/1.0
Host: api.sugarsync.com
```

After issuing the GET request, the `downloadFileData()` method gets the response, including the status code and the response headers.

Notice that the class returns the response in HttpResponse.

```java
return new HttpResponse(statusCode, null, headers);
```

The HttpResponse class is a Java bean class that you can find in the application's util folder. The Java bean contains properties to hold the status code, response headers, and response body. It also has get methods to get these values. Other classes in the application, such as those that get user information use HttpResponse as a utility to return the HTTP response to the caller.

## The Response

In response, the SugarSync service returns a status line followed by response headers. The status line includes a status code — a status code in the 200 range indicates that the request was successful, that is, the file was successfully downloaded. The response headers include the content length and filename of the downloaded file. The response should look something like this:

```xml
HTTP/1.1 200 OK
Content-Type: application/octet-stream; charset=UTF-8
Content-Length: 18241
Date: Mon, 05 Dec 2011 19:04:51 GMT
Accept-Ranges: bytes
Content-Disposition: attachment; filename*=UTF-8''Golden Gate Bridge.jpg
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
```

After an app downloads a file, it can then display the file to the user.

> *See the following picture: https://www.sugarsync.com/images/Golden%20Gate%20Bridge.jpg*

The sample app doesn't display the file, but rather displays a message that the download was successful. The displayed message should look something like this:

```
Download completed successfully. The Golden Gate Bridge.jpg from "Magic Briefcase"
was downloaded to the local directory.
```

## What's Next?

Learn how your app can create a file in [Creating a File](create-file-example.md).

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API利用規約](/source/dev/terms.md)
