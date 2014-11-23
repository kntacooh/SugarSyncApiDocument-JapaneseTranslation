> *The Original: [SugarSync for Developers-Developer Provisioning](https://www.sugarsync.com/dev/dev-provisioning.html)*

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

# Developer Provisioning

Developer provisioning is a SugarSync service that gives you the ability to programmatically create new SugarSync user accounts. That way users can create SugarSync accounts through your applications without having to visit the SugarSync site.

**Note:** You must have prior authorization to use the developer provisioning service. Contact us at developers@sugarsync.com if you have not already received the needed authorization.

Here's how to use the developer provisioning feature to create a new user account:

* Provide as input XML that identifies:
  * The email address of the new user
  * The password for the new user

    Your app can prompt the user to supply his or her email address and password.
  * Your accessKeyID
  * Your privateAccessKey

    The accessKeyID and privateAccessKey are the keys you received when you signed up as a developer with SugarSync. If you develop more than one app, you'll need to generate a new set of keys for each app you develop. SugarSync provides a maximum of ten sets of keys for each developer account.

* Submit an HTTP POST request to the URL for the developer provisioning service:
  ```
https://provisioning-api.sugarsync.com/users
  ```

Here's what a developer provisioning request might look like:

```xml
POST https://provisioning-api.sugarsync.com/users HTTP/1.1
User-Agent: Jakarta Commons-HttpClient/3.1
Host: provisioning-api.sugarsync.com
Content-Length: 294
Content-Type: application/xml; charset=UTF-8

<?xml version="1.0" encoding="UTF-8" ?>
<user>
  <email>user_dev10@user.com</email>
  <password>dev10pwd</password>
  <accessKeyId>AKIAJTXL5NNLKNIAEORA</accessKeyId>
  <privateAccessKey>QAzJKVkzSXbIXWFwEPbzmRYmP8VmdLyNn33AvjRP</privateAccessKey>
</user>
```

Note: The `User-Agent` header field as well as the `<email>`, `<password>`, `<accessKeyId>`, and `<privateAccessKey>` values shown in this and other examples are only illustrative. Your requests should specify an application-specific user agent string, the actual email address and password for the user, and the keys you were assigned by SugarSync.

Here's a Java code example that makes the request to create a user account. Although this is a Java code example, you don't need to build the request using Java. You can use any means to build and submit the request.

```java
import java.io.File;
import java.io.IOException;
import org.apache.commons.httpclient.Header;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.HttpException;
import org.apache.commons.httpclient.methods.FileRequestEntity;
import org.apache.commons.httpclient.methods.PostMethod;
import org.apache.commons.httpclient.methods.RequestEntity;

public class NewUser {

    /* set the developer provisioning URL */
    private static String END_POINT =
            "https://provisioning-api.sugarsync.com/users";

    public static void createUserAccount() throws HttpException, IOException {
        HttpClient client = new HttpClient();
        /*
         * Implement an HTTP POST method on the developer provisioning URL
         */
        PostMethod post = new PostMethod(END_POINT);
        /* Point to the input XML file */
        File input = new File("devprov/user.xml");
        RequestEntity entity = new FileRequestEntity(input, "application/xml;
                charset=UTF-8");
        post.setRequestEntity(entity);
        /* Execute the HTTP POST request */
        client.executeMethod(post);
        /* Display the response */
        System.out.println("Response status code: " + post.getStatusCode());
        System.out.println("Response body: ");
        System.out.println(post.getResponseBodyAsString());
        System.out.println("Response header: ");
        Header[] headers = post.getResponseHeaders();

        for (int i = 0; i < headers.length; i++) {
            System.out.println(headers[i]);
        }
    }

    public static void main(String[] args) throws HttpException, IOException {
        NewUser.createUserAccount();
    }
}
```

In response, the SugarSync service returns a status code that indicates whether or not the request was successful. Various status codes are also accompanied by a response body containing a message with further description. See [Status Codes and Response Body Messages](#codemessage) for a list of the status codes and messages that can be returned.

## Reference

### URL

`https://provisioning-api.sugarsync.com/users`

### Method

POST

### Request Header

The request header includes the following information:

| FIELD | DESCRIPTION |
| :-- | :-- |
| User-Agent | The client application implementing the network protocol for communication between the client and server. |
| Host | The domain name of the server: `https://provisioning-api.sugarsync.com/users` |
| Content-Length | The length of the request body. |
| Content-Type | The content type and character encoding of the response. The content type is always `application/xml`, and the character encoding is always `UTF-8`. |

### Request Body

An XML input document that contains the following elements:

| ELEMENT | DESCRIPTION |
| :-- | :-- |
| email | The user's email address. This is the email address that the user enters when accessing his or her SugarSync account. |
| password | The user's password. This is the password that the user enters when accessing his or her SugarSync account. |
| accessKeyId | The application developer's access key ID. An access key ID is assigned with a private access key, as a matched pair. |
| privateAccessKey | The application developer's private access key. An private access key is assigned with an access key ID, as a matched pair. |

<a name="codemessage"></a>
### Status Codes and Response Body Messages

| STATUS CODE | RESPONSE BODY | DESCRIPTION |
| :-- | :-- | :-- |
| 201 | Not applicable | The user account was successfully created. |
| 400 | One of the following messages: <ul> <li> Missing mandatory element </li> <li> Invalid xml request </li> <li> Invalid email </li> <li> Invalid password </li> </ul> | Bad request for one of the following reasons: <ul> <li> One of the required elements is missing from the XML input. </li> <li> The XML input is not valid (for example, the XML may not be well formed). </li> <li> The email element in the XML input is empty or invalid. </li> <li> The password element in the XML input is empty. </li> </ul> |
| 401 | Not applicable | Developer credentials cannot be verified. Either a developer with the specified accessKeyId does not exist or the privateKeyID does not match an assigned accessKeyId. |
| 403 | One of the following messages: <ul> <li> Provisioning is not enabled for the developer. </li> <li> Account already exists. </li> </ul> | The user account was not created for one of the following reasons: <ul> <li> Provisioning is not enabled for the developer. </li> <li> The user account already exists. </li> </ul> |
| 503 | One of the following messages: <ul> <li> Unable to generate nickname </li> <li> Server error </li> </ul> | A server error occurred for one of the following reasons: <ul> <li> The server is unable to generate a nickname for the user account. </li> <li> A server error occurred for a reason other than the one listed above.  </li> </ul> |

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API利用規約](/source/dev/terms.md)
