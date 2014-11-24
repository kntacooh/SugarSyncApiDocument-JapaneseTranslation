> *The Original: [Developer: Retrieving Folder Contents - SugarSync](https://www.sugarsync.com/dev/api/method/get-folder-contents.html)*

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

* **Refresh Token**
* [Creating a Refresh Token](/source/dev/api/method/create-refresh-token.md)
* **Access Token**
* [Creating an Access Token](/source/dev/api/method/create-auth-token.md)
* **User**
* [Retrieving User Information](/source/dev/api/method/get-user-info.md)
* **Workspaces Collection**
* [Retrieving Workspaces Collection](/source/dev/api/method/get-workspaces.md)
* **Workspace**
* [Retrieving Workspace Information](/source/dev/api/method/get-workspace-info.md)
* [Retrieving Workspace Contents](/source/dev/api/method/get-workspace-contents.md)
* [Updating Workspace Information](/source/dev/api/method/update-workspace-name.md)
* **Sync Folders Collection**
* [Retrieving Sync Folders Collection](/source/dev/api/method/get-syncfolders.md)
* **Folder**
* [Retrieving Folders](/source/dev/api/method/get-folders.md)
* [Retrieving Folder Information](/source/dev/api/method/get-folder-info.md)
* [Retrieving Folder Contents](/source/dev/api/method/get-folder-contents.md)
* [Creating a Folder](/source/dev/api/method/create-folder.md)
* [Creating a File in a Folder](/source/dev/api/method/create-file.md)
* [Copying a File in a Folder](/source/dev/api/method/copy-file.md)
* [Deleting a Folder](/source/dev/api/method/delete-folder.md)
* [Updating Folder Information](/source/dev/api/method/update-folder-info.md)
* **Received Share**
* [Retrieving Received Share Information](/source/dev/api/method/get-received-shares.md)
* **Received Share List**
* [Retrieving Received Shares List](/source/dev/api/method/get-received-shares-list.md)
* **Contact**
* [Retrieving Contact Information](/source/dev/api/method/get-contacts.md)
* **File**
* [Retrieving File Information](/source/dev/api/method/get-file-info.md)
* [Updating File Information](/source/dev/api/method/update-file-info.md)
* [Deleting a File](/source/dev/api/method/delete-file.md)
* [Retrieving File Version History](/source/dev/api/method/get-version-history.md)
* [Creating a New File Version](/source/dev/api/method/create-new-version.md)
* **File Data**
* [Retrieving File Data](/source/dev/api/method/get-file-data.md)
* [Uploading File Data](/source/dev/api/method/put-file-data.md)
* [Transcoding Image Files](/source/dev/api/method/transcode-image.md)
* **File Version**
* [Retrieving File Version Information](/source/dev/api/method/get-file-version-info.md)
* **File Version Data**
* [Retrieving File Version Data](/source/dev/api/method/get-file-version-data.md)
* [Uploading File Version Data](/source/dev/api/method/put-file-version-data.md)
* **Albums Collection**
* [Retrieving Albums Collection](/source/dev/api/method/get-album-collection-contents.md)
* **Album**
* [Retrieving Album Information](/source/dev/api/method/get-photo-album.md)
* [Retrieving Album Contents](/source/dev/api/method/get-album-contents.md)

---

# Retrieving Folder Contents

To retrieve the contents of a folder, an application submits an HTTP GET request to the URL that represents the folder contents.

## Request

### URL

The URL that represents the folder resource contents. The URL begins with `https://api.sugarsync.com/folder/` followed by `/contents`, for example, `https://api.sugarsync.com/folder/:sc:566494:5/contents`.

### Parameters

The URL can be specified with the following optional query parameters:

| ARGUMENT | VALUE | DESCRIPTION |
| :-- | :-- | :-- |
| type | folder or file | The type of folder contents to be listed in the response: folders or files. If no object of the listed type is in the folder, no folder contents are listed in the response. If no type is specified, both folders and files contained in the folder are listed in the response. |
| start | 0 or positive integer | The index within the indexed sequence of objects in the folder to start listing folder contents. The default is 0, meaning that objects in the folder are listed starting with the first object in the sequence. |
| max | Positive integer | The maximum number of objects, beginning with the object at the start index, that the client wants listed in the response. If the folder contains fewer objects of the requested type than the specified maximum, the smaller number of objects is listed. The default is 500, meaning that no more than 500 objects in the folder will be listed. |
| order | name, last_modified, size, or extension | The value to be used for sorting the contents, as follows: <ul> <li> name: The display name of the items </li> <li> last_modified: The last-modified date (if available) of the items </li> <li> size: The size (in bytes) of the items </li> <li> extension: The filename extension (if available) of the items </li> </ul>  |

For example, by default, the following URL requests a maximum of 500 folders and files contained in the folder:

```
https://api.sugarsync.com/folder/:sc:566494:5/contents
```

The following URL and parameters can be used to request only the files starting at index 30 in the folder, and limits retrieval to 50 files:

```
https://api.sugarsync.com/folder/:sc:566494:5/contents?type=file&start=30&max=50
```

The following URL and parameters can be used to request only the files in the folder, limit retrieval to 50 files, and sort the files by display name:

```
https://api.sugarsync.com/folder/:sc:566494:5/contents?type=file&max=50&order=name
```

### Method

GET

### Authorization

An access token must be provided in the HTTP request header. See [Creating an Access Token](create-auth-token.md) for details.

### Request Header

The request header includes the following information:

| FIELD | DESCRIPTION |
| :-- | :-- |
| Authorization | The access token. |
| User-Agent | The client application implementing the network protocol for communication between the client and server. |
| Host | The domain name of the server. The domain name is always `api.sugarsync.com` |

### HTTP Request Example

```xml
GET https://api.sugarsync.com/folder/:sc:566494:5/contents HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83...
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
```

Note: The Authorization and User-Agent header field values shown in this and other examples are only illustrative. Your requests should specify the actual access token created for your application as well as an application-specific user agent string.

### Java Example

The following Java code example assumes that the URL for the pertinent folder resource contents is `https://api.sugarsync.com/folder/:sc:566494:5/contents`.

```java
import java.io.IOException;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.HttpException;
import org.apache.commons.httpclient.methods.GetMethod;
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.DocumentHelper;

public class GetPresentation {

    public static Document getXmlRequest(String url) throws HttpException, IOException {

        HttpClient client = new HttpClient();
        /* Implement an HTTP GET method on the resource */
        GetMethod get = new GetMethod(url);
        /* Create an access token and set it in the request header */
        String token = GetAuth.getAuthQuest();
        get.addRequestHeader("Authorization", token);
        /* Execute the HTTP GET request */
        client.executeMethod(get);
        /* Display the response */
        System.out.println("Response status code: " + get.getStatusCode());
        System.out.println("Response body: ");

        byte[] responseBody = get.getResponseBody();
        System.out.println(new String(responseBody));
        Document document = DocumentHelper.createDocument();

        return document;
    }

    public static void main(String[] args) throws HttpException, IOException {

        Document response_xml = getXmlRequest("https://api.sugarsync.com/folder/:sc:566494:5/contents");
        System.out.println(response_xml.asXML());
    }
}
```

## Response

### Response Header

The response header includes the following information:

| FIELD | DESCRIPTION |
|:-- | :-- |
| Content-Type | The content type and character encoding of the response. |
| Date | The date and time of the response. |
| Access-Control-Allow-Origin | Specifies "****" to indicate that any origin may access the resource. |
| Server | The server that processed the request. |
| Transfer-Encoding | Specifies `chunked` to indicate that the server provided the content in a series of chunks. |

### Response Body

XML for each retrieved folder or file in the folder. The XML contains the following elements for each retrieved folder:

| ELEMENT | DESCRIPTION |
| :-- | :-- |
| displayName | The user-visible name of the folder. |
| ref | A link to the folder. |
| contents | A link to the contents of the folder. |

The XML contains the following elements for each retrieved file:

| ELEMENT | DESCRIPTION |
| :-- | :-- |
| displayName | The user-visible name of the file. |
| ref | A link to the file. |
| size | The size of the file. |
| lastModified | A dateTime value that specifies the date and time the file was last modified. |
| mediaType | The media type of the file. |
| presentOnServer | Whether the file is on the server (true) or not (false). |
| fileData | A link to the data for the file. |

Note: The retrieved information for each folder is a subset of the full representation of the folder resource. Similarly, the retrieved information for each file is a subset of the full representation of the file resource.

### Status Codes

| STATUS CODE | DESCRIPTION |
| :-- | :-- |
| 200-299 | The request was successful. The folder contents are in the response body. |
| 400 | Bad request. Typically returned if required information, such as the folder contents URL, was not provided as input. |
| 401 | Authorization required. The presented credentials, if any, were not sufficient to access the folder contents. Returned if an application attempts to use an access token after it has expired. |
| 403 | Forbidden. The requester does not have permission to access the specified resource. |
| 404 | Not found. The resource was not found. |
| 500-599 | Server error. |

### Response Example

```xml
HTTP/1.1 200 OK
Content-Type: application/xml; charset=UTF-8
Date: Tue, 29 Nov 2011 23:06:11 GMT
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
Transfer-Encoding: chunked

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<collectionContents start="0" hasMore="false" end="4">
  <collection type="folder">
    <displayName>100ANDRO</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:6552993_17248</ref>
    <contents>https://api.sugarsync.com/folder/:sc:566494:6552993_17248/contents</contents>
  </collection>
  <collection type="folder">
    <displayName>2010-10-10</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:6552993_17250</ref>
    <contents>https://api.sugarsync.com/folder/:sc:566494:6552993_17250/contents</contents>
  </collection>
  <file>
    <displayName>AbeLincoln.jpg</displayName>
    <ref>https://api.sugarsync.com/file/:sc:566494:6552993_17252</ref>
    <size>38539</size>
    <lastModified>2010-02-11T15:26:52.000-08:00</lastModified>
    <mediaType>image/jpeg</mediaType>
    <presentOnServer>true</presentOnServer>
    <fileData>https://api.sugarsync.com/file/:sc:566494:6552993_17252/data</fileData>
  </file>
  <file>
    <displayName>GeorgeWashington.jpg</displayName>
    <ref>https://api.sugarsync.com/file/:sc:566494:6552993_17254</ref>
    <size>956022</size>
    <lastModified>2011-11-11T07:48:10.000-08:00</lastModified>
    <mediaType>image/jpeg</mediaType>
    <presentOnServer>true</presentOnServer>
    <fileData>https://api.sugarsync.com/file/:sc:566494:6552993_17254/data</fileData>
  </file>
</collectionContents>
```

### Learn More

* See [Listing the Contents of a Folder](/source/dev/get-folder-info-example.md).
* See [Folder Resource](/source/dev/api/folder-resource.md).
* Download the [*:::sample app:::*](https://www.sugarsync.com/dev/resource/sugarsync-api-sample.zip).

---

© 2012 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
