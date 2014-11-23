> *The Original: [Developer: Retrieving Folders - SugarSync](https://www.sugarsync.com/dev/api/method/get-folders.html)*

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

# Retrieving Folders

To retrieve the folders in a containing collection (a workspace or parent folder), an application submits an HTTP GET request to the URL that represents the folders in the containing collection. The URL is returned in the `<collections>` element in response to a [request for workspace information](get-workspace-info.md) or in response to a [request for folder information](get-folder-info.md).

## Request

### URL

The URL that represents the folders in a workspace begins with `https://api.sugarsync.com/workspace/` and ends with `contents`. The URL that represents the folders in a parent folder begins with `https://api.sugarsync.com/folder/` and ends with `contents`.

The URL parameter `type=folder` must be specified when retrieving folders from a parent folder because the parent folder may also contain files. For example, `https://api.sugarsync.com/folder/:sc:566494:5/contents?type=folder`. The URL parameter `type=folder` may be specified (but is not necessary) when retrieving folders from a workspace. For example, submitting an HTTP GET request to `https://api.sugarsync.com/workspace/:sc:566494:6552993_17004/contents` and `https://api.sugarsync.com/workspace/:sc:566494:6552993_17004/contents?type=folder` both retrieve the same list of folders from the workspace.

### Method

GET

### Parameters

The following optional query parameters can be specified in the HTTP GET request to filter or limit the response.

| PARAMETER | DESCRIPTION |
| :-- | :-- |
| type | Must be specified with a value of folder when retrieving folders from a parent folder. The parameter does not need to be specified when retrieving folders from a workspace. |
| start | Specifies the index within the indexed sequence of folders where the client wants folders to be retrieved. The index starts at origin 0. The default value is 0. |
| max | The maximum number of folders, beginning with the folder at the start index, that the client wants listed in the response. If this parameter is not specified, no limit is placed on the number of retrieved folders. |

### Authorization

An access token must be provided in the HTTP request header. See [Creating an Access Token](create-auth-token.md) for details.

### Request Header

The request header includes the following information:

| FIELD | DESCRIPTION |
| :-- | :-- |
| Authorization | The access token. |
| User-Agent | The client application implementing the network protocol for communication between the client and server. |
| Host | The domain name of the server. The domain name is always api.sugarsync.com |

### HTTP Request Example

```xml
GET https://api.sugarsync.com/folder/:sc:566494:5/contents?type=folder HTTP/1.1
Authorization: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83...
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
```

Note: The Authorization and User-Agent header field values shown in this and other examples are only illustrative. Your requests should specify the actual access token created for your application as well as an application-specific user agent string.

### Java Example

The following Java code example assumes that the URL that represents the collection of folders in the workspace is `https://api.sugarsync.com/folder/:sc:566494:5/contents?type=folder`.

```java
import java.io.IOException;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.HttpException;
import org.apache.commons.httpclient.methods.GetMethod;
import org.apache.commons.httpclient.Header;
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.DocumentHelper;

public class GetPresentation {

    public static Document getXmlRequest(String url) throws HttpException, IOException {

        HttpClient client = new HttpClient();
        /* Implement an HTTP GET method on the resource */
        GetMethod get = new GetMethod(url);
        /* Create an access token and set it in the request header*/
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

        Document response_xml = getXmlRequest("https://api.sugarsync.com/folder/:sc:566494:5/contents?type=folders");
    }

}
```

## Response

### Response Header

The response header includes the following information:

| FIELD | DESCRIPTION |
| :-- | :-- |
| Content-Type | The content type and character encoding of the response. |
| Date | The date and time of the response. |
| Access-Control-Allow-Origin | Specifies "*" to indicate that any origin may access the resource. |
| Server | The server that processed the request. |
| Transfer-Encoding | Specifies `chunked` to indicate that the server provided the content in a series of chunks. |

### Response Body

XML that contains the following elements for each retrieved folder:

| ELEMENT | DESCRIPTION |
| :-- | :-- |
| displayName | The user-visible name of the folder. |
| ref | A link to the folder. |
| contents | A link to the contents of the folder. |

### Status Codes

| STATUS CODE | DESCRIPTION |
| :-- | :-- |
| 200-299 | The request was successful. The information about the collection of folders is in the response body. |
| 400 | Bad request. Typically returned if required information, such as the folder contents URL, was not provided as input. |
| 401 | Authorization required. The presented credentials, if any, were not sufficient to access the resource. Returned if an application attempts to use an access token after it has expired. |
| 403 | Forbidden. The requester does not have permission to access the specified resource. |
| 404 | Not found. The resource was not found. |
| 500-599 | Server error. |

### Response Example

```xml
HTTP/1.1 200 OK
Content-Type: application/xml; charset=UTF-8
Date: Fri, 22 Oct 2011 08:01:54 GMT
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
Transfer-Encoding: chunked

<?xml version="1.0" encoding="UTF-8"?>
  <collectionContents start="0" hasMore="false" end="2">
    <collection type="folder">
    <displayName>.thumbnails</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:6552993_17246</ref>
    <contents>https://api.sugarsync.com/folder/:sc:566494:6552993_1724/contents</contents>
  </collection>
  <collection type="folder">
    <displayName>100ANDRO</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:5664948:6552993_17248</ref>
    <contents>https://api.sugarsync.com/folder/:sc:5664948:6552993_1724/contents</contents>
  </collection>
  <collection type="folder">
    <displayName>2010-10-10</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:5664948:6552993_17250</ref>
    <contents>https://api.sugarsync.com/folder/:sc:5664948:6552993_17250/contents</contents>
  </collection>
</collectionContents>
```

### Learn More

* See [Folder Resource](/source/dev/api/folder-resource.md).
* Download the [*:::sample app:::*](https://www.sugarsync.com/dev/resource/sugarsync-api-sample.zip).


---

© 2012 SugarSync, Inc. All Rights Reserved.  [API利用規約](/source/dev/terms.md)
