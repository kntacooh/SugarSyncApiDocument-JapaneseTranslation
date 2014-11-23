> *The Original: [SugarSync for Developers-API Examples: Getting Authorization](https://www.sugarsync.com/dev/get-auth-token-example.html)*

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

# Getting Authorization

Your application needs to be authorized to access a user's SugarSync-managed resources through the Platform API. To do that, the app needs to create a *refresh token*. When a user first runs your application, the app requests a refresh token. If the request is successful, the SugarSync service grants your app permission to access the user's account and returns a refresh token.

Once your app obtains a refresh token, it can use it to create an access token, which allows the app to access files, folders, and other resources within a user's account. After the access token is created, your app needs to specify it in the Authorization header when it makes an HTTP request through the API to access a resource.

An access token is transient and can be used to access user resources for a short period time (approximately one hour). However, a refresh token is persistent. Your app can obtain additional access tokens using the same refresh token. There is no need for your app to store the user's credentials between API access requests — all your app needs to do is store the refresh token.

* After your app obtains a refresh token, it can use it to get access tokens and make subsequent requests to access user resources. There is no need for your app to store the user's credentials between API access requests — all your app needs to do is store the refresh token.

## Requesting a Refresh Token

Your app requests a refresh token by making an HTTP POST request to the URL `https://api.sugarsync.com/app-authorization`. In addition, it needs to provide XML in the message body that identifies the username (that is, the email address) and password of the user whose resources the application will access — your app can prompt the user to supply those credentials. The XML input also needs to specify your app's ID as well as your access key ID and private access key.

Your app was assigned an ID when you created it using the SugarSync [*:::Developer Console:::*](https://www.sugarsync.com/developer/account). You received your access key ID and private access key when you signed up as a developer with SugarSync. If you develop more than one app, you'll need to create the additional apps using the Developer Console. You should also consider obtaining a new access key ID-private access key pair for each app you develop. You can create additional access key ID-private access key pairs using the Developer Console. SugarSync allows a maximum of ten access key ID-private access key pairs for each developer account.

> *See the following picture: https://www.sugarsync.com/images/create-refresh-token.png

Here's what the request to create a refresh token might look like:

```xml
POST https://api.sugarsync.com/app-authorization HTTP/1.1
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
Content-Length: 364
Content-Type: application/xml; charset=UTF-8

<?xml version="1.0" encoding="UTF-8" ?>
<appAuthorization>
  <username>jsmith127@sugarsync.com</username>
  <password>sugar20P$</password>
  <application>/sc/10061/3_21053</application>
  <accessKeyId>AKIAJTXL5NNLKNIAEORA</accessKeyId>
  <privateAccessKey>QAzJKVkzSXbIXWFwEPbzmRYmP8VmdLyNn33AvjRP</privateAccessKey>
</appAuthorization>
```

Note: The `User-Agent` header field as well as the `<username>`, `<password>`, `<application>`, `<accessKeyId>`, and `<privateAccessKey>` values shown in this and other examples are only illustrative. Your requests should specify an application-specific user agent string, the actual username and password for the user, the actual ID assigned to your app, and the keys you were assigned by SugarSync.

Notice that the input XML is encoded using UTF-8 encoding and has the content type application/xml.

* All XML input that your application sends to the SugarSync service must be encoded using UTF-8 encoding and must use the application/xml content type.

### Refresh Token Request Example

The Refresh Token class in the [sample application](using-api.md) provides a good example of making a request to create a refresh token. You can find the Refresh Token class in the application's auth folder. Although this is a Java code example, you don't need to use Java to build and submit the request to create a refresh token. You can use any means to build and submit the HTTP request.

Creating a refresh token is one of the first actions taken by the sample app. If you examine the main method in the SampleTool class (the main class for the app in the tool folder), you'll see the following call:

```java
String refreshToken = getRefreshToken(username, password, applicationId, accessKey, privateAccessKey);
```

The call to the `getRefreshToken()` method includes parameters for the user's username and password, the ID assigned to the app, as well as the pertinent access key ID and private access key. These values are supplied as input parameters when the sample application is executed from a command line.

The `getRefreshToken()` method, also in the SampleTool class, makes the call to the RefreshToken class as follows:

```java
private static String getRefreshToken(String username, String password, String applicationId, String accessKey,
        String privateAccessKey) throws IOException {
    HttpResponse httpResponse = null;
    httpResponse = RefreshToken.getAuthorizationResponse(username, password,applicationId, accessKey, privateAccessKey);

    if (httpResponse.getHttpStatusCode() > 299) {
        System.out.println("Error while getting refresh token!");
        printResponse(httpResponse);
        System.exit(0);
    }

    return httpResponse.getHeader("Location").getValue();
}
```

The `getRefreshToken()` method calls the `getAuthorizationResponse()` method in the RefreshToken class to create the refresh token. Status codes above the 200 range indicate that the request was not successful, so the method prints an error message if the status code is above that range. If the request is successful, the method returns the "Location" value extracted from the response header. You'll learn more about the Location value in [Response to a Refresh Token Request](#refresp).

Here is the source code for the RefreshToken class:

```java
package com.sugarsync.sample.auth;

import java.io.IOException;

import org.apache.commons.httpclient.Header;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.methods.PostMethod;
import org.apache.commons.httpclient.methods.RequestEntity;
import org.apache.commons.httpclient.methods.StringRequestEntity;

import com.sugarsync.sample.util.HttpResponse;

public class RefreshToken {
    /* SugarSync App Authorization API url */
    private static final String APP_AUTH_REFRESH_TOKEN_API_URL = "https://api.sugarsync.com/app-authorization";

    /* The User-Agent HTTP Request header's value */
    private static final String API_SAMPLE_USER_AGENT = "SugarSync API Sample/1.1";

    /* The template used for creating the request */
    private static final String APP_AUTH_REQUEST_TEMPLATE = "<?xml version=\"1.0\" encoding=\"UTF-8\" standalone=\"yes\"?>"
            + "<appAuthorization>"
            + "<username>%s</username>"
            + "<password>%s</password>"
            + "<application>%s</application>"
            + "<accessKeyId>%s</accessKeyId>"
            + "<privateAccessKey>%s</privateAccessKey>"
            + "</appAuthorization>";

    /**
     * Fills the APP_AUTH_REQUEST_TEMPLATE with the request details */
    private static String fillRequestTemplate(String username, String password, String application,
            String accessKey, String privateAccessKey) {
        return String.format(APP_AUTH_REQUEST_TEMPLATE, new Object[] { username, password, application,
                accessKey, privateAccessKey });
    }

    /* Makes a HTTP POST request to SugarSync app authorization API and returns a HttpResponse */
    public static HttpResponse getAuthorizationResponse(String username, String password, String application,
            String accessKey, String privateAccessKey) throws IOException {
        // fill the request xml template with developer details
        String request = fillRequestTemplate(username, password, application, accessKey, privateAccessKey);

        // make a HTTP POST to APP_AUTH_REFRESH_TOKEN_API_URL with the app authorization request
        HttpClient client = new HttpClient();
        PostMethod post = new PostMethod(APP_AUTH_REFRESH_TOKEN_API_URL);
        RequestEntity entity = new StringRequestEntity(request, "application/xml", "UTF-8");
        post.setRequestEntity(entity);
        post.setRequestHeader("User-Agent", API_SAMPLE_USER_AGENT);
        client.executeMethod(post);

        // get HTTP response
        Integer statusCode = post.getStatusCode();
        String responseBody = post.getResponseBodyAsString();
        Header[] headers = post.getResponseHeaders();

        return new HttpResponse(statusCode, responseBody, headers);
        }
    }
```

The class builds the XML input message body for the request using the user's username, password, app ID, access key ID, and private access key (which are passed to it as input parameters). Then the class issues an HTTP POST request to the authorization URL: `https://api.sugarsync.com/app-authorization`.

After issuing the POST request, the class gets the response, including the status code, the response message body, and the response headers.

Notice that the class returns the response in HttpResponse.

```java
return new HttpResponse(statusCode, responseBody, headers);
```

The HttpResponse class is a Java bean class that you can find in the application's util folder. The Java bean contains properties to hold the status code, response headers, and response body. It also has get methods to get these values. Other classes in the application, such as those that get user information or that create and download a file use HttpResponse as a utility to return the HTTP response to the caller.

<a name="refresp"></a>
### Response to a Refresh Token Request

In response to the refresh token request, the SugarSync service verifies your keys and ensures that the application specified in the request exists and is enabled. If the request is successful (indicated by a status code in the 200 range), the SugarSync service returns a response whose header contains a refresh token in the `Location` field.

The response should look something like this:

```xml
HTTP/1.1 201 Created
Content-Type: application/xml; charset=UTF-8
Date: Wed, 28 Mar 2012 19:29:00 GMT
Location: https://api.sugarsync.com/app-authorization/A31303036322f335f3237303337
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
Transfer-Encoding: chunked
```
In the sample app, the `getRefreshToken()` method retrieves the Location value from the response header and returns it to the caller.

```java
return httpResponse.getHeader("Location").getValue();
```

## Requesting an Access Token

After you app receives a refresh token, it can use it to request an access token. Your app can then specify the access token in the Authorization header when it makes an HTTP request through the API to access a resource from the user's account. Your app can use the same refresh token to get access tokens for subsequent requests to access the user's resources. Your app does not need to request another refresh token and so there is no need for your app to store the user's credentials between API access requests.

To obtain an access token, your app makes an HTTP POST request to the SugarSync authorization URL `https://api.sugarsync.com/authorization`. In addition, it needs to provide XML in the message body that identifies the refresh token and specifies your access key ID and private access key.

Here's what the request to obtain an access token might look like:

```xml
POST https://api.sugarsync.com/authorization HTTP/1.1
User-Agent: Jakarta Commons-HttpClient/3.1
Host: api.sugarsync.com
Content-Length: 358
Content-Type: application/xml; charset=UTF-8

<?xml version="1.0" encoding="UTF-8" ?>
<tokenAuthRequest>
  <accessKeyId>AKIAJTXL5NNLKNIAEORA</accessKeyId>
  <privateAccessKey>QAzJKVkzSXbIXWFwEPbzmRYmP8VmdLyNn33AvjRP</privateAccessKey>
  <refreshToken>https://api.sugarsync.com/app-authorization/A31303036322f335f3237303337</refreshToken>
</tokenAuthRequest>
```

### Access Token Request Example

The AccessToken class in the [sample application](using-api.md) provides a good example of making a request to create an access token. You can find the AccessToken class in the application's auth folder. Although this is a Java code example, you don't need to use Java to build and submit the request to create an access token. You can use any means to build and submit the HTTP request.

As you might expect, the the main method in the SampleTool class requests an access token immediately after requesting and successfully receiving a refresh token. Here is what the call looks like:

```java
String accessToken = getAccessToken(accessKey,privateAccessKey,refreshToken);
```

The call to the `getAccessToken()` method includes parameters for the user's username and password, the pertinent access key ID and private access key, as well as the refresh token. the

The `getAccessToken()` method, also in the SampleTool class, makes the call to the Authorization class as follows:

```java
private static String getAccessToken(String accessKey, String privateAccessKey, String refreshToken) throws IOException {
        HttpResponse httpResponse = AccessToken.getAccessTokenResponse(accessKey, privateAccessKey,
        refreshToken);

    if (httpResponse.getHttpStatusCode() > 299) {
        System.out.println("Error while getting access token!");
        printResponse(httpResponse);
        System.exit(0);
    }

    return httpResponse.getHeader("Location").getValue();
}
```

The `getAccessToken()` method calls the `getAccessTokenResponse()` method in the AccessToken class to create the access token. Status codes above the 200 range indicate that the request was not successful, so the method prints an error message if the status code is above that range. If the request is successful, the method returns the "Location" value extracted from the response header. See [Response to an Access Token Request](#authresp) for more details.

Here is the source code for the AccessToken class:

```java
package com.sugarsync.sample.auth;

import java.io.IOException;

import org.apache.commons.httpclient.Header;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.methods.PostMethod;
import org.apache.commons.httpclient.methods.RequestEntity;
import org.apache.commons.httpclient.methods.StringRequestEntity;

import com.sugarsync.sample.util.HttpResponse;

/**
 * Sample code used for getting access tokens
 */
public class AccessToken {
    /* SugarSync Access Token API url */
    private static final String AUTH_ACCESS_TOKEN_API_URL = "https://api.sugarsync.com/authorization";

    /** The User-Agent HTTP Request header's value */
    private static final String API_SAMPLE_USER_AGENT = "SugarSync API Sample/1.0";

    /* The template used for creating the request */
    private static final String ACCESS_TOKEN_AUTH_REQUEST_TEMPLATE = "<?xml version=\"1.0\" encoding=\"UTF-8\" standalone=\"yes\"?>"
            + "<tokenAuthRequest>"
            + "<accessKeyId>%s</accessKeyId>"
            + "<privateAccessKey>%s</privateAccessKey>"
            + "<refreshToken>%s</refreshToken>"
            + "</tokenAuthRequest>";

    /* Fills the ACCESS_TOKEN_AUTH_REQUEST_TEMPLATE with the request details */
    private static String fillRequestTemplate(String accessKey, String privateAccessKey, String refreshToken) {
        return String.format(ACCESS_TOKEN_AUTH_REQUEST_TEMPLATE, new Object[] { accessKey, privateAccessKey,
                refreshToken });
    }

    /* Makes a HTTP POST request to SugarSync authorization API and returns a HttpResponse */
    public static HttpResponse getAccessTokenResponse(String accessKey, String privateAccessKey,
            String refreshToken) throws IOException {
        // fill the request xml template with developer details
        String request = fillRequestTemplate(accessKey, privateAccessKey, refreshToken);

        // make a HTTP POST to AUTH_ACCESS_TOKEN_API_URL with the authorization
        // request
        HttpClient client = new HttpClient();
        PostMethod post = new PostMethod(AUTH_ACCESS_TOKEN_API_URL);
        RequestEntity entity = new StringRequestEntity(request, "application/xml", "UTF-8");
        post.setRequestEntity(entity);
        post.setRequestHeader("User-Agent", API_SAMPLE_USER_AGENT);
        client.executeMethod(post);

        // get HTTP response
        Integer statusCode = post.getStatusCode();
        String responseBody = post.getResponseBodyAsString();
        Header[] headers = post.getResponseHeaders();

        return new HttpResponse(statusCode, responseBody, headers);
    }
}
```

The class builds the XML input message body for the request using the access key ID, private access key, and refresh token (which are passed in as input parameters). Then the class issues an HTTP POST request to the authorization URL: `https://api.sugarsync.com/authorization`.

After issuing the POST request, the class gets the response, including the status code, the response message body, and the response headers. It then returns the response in HttpResponse.

<a name="authresp"></a>
### Response to an Access Token Request

If the request is successful (indicated by a status code in the 200 range), the SugarSync service returns a response whose header contains the access token in the Location field. The response should look something like this:

```xml
HTTP/1.1 201 Created
Content-Type: application/xml; charset=UTF-8
Date: Wed, 28 Mar 2012 19:30:44 GMT
Location: https://api.sugarsync.com/authorization/SmZ8zlrkR8j0oefVmmD4dUD83
Access-Control-Allow-Origin: *
Server: Noelios-Restlet-Engine/1.1.5
Transfer-Encoding: chunked

<?xml version="1.0" encoding="utf-8"?>
<authorization>
  <expiration>2012-03-28T23:30:44.463+03:00</expiration>
  <user>https://api.sugarsync.com/user/5664947</user>
</authorization>
```

Notice that the response XML message body contains the expiration date for the access token and a link to the user resource for the user. The message body also specifies UTF-8 encoding. All XML generated by the SugarSync service uses UTF-8 encoding.

Notice too that the expiration date, `2012-03-28T23:30:44.463+03:00`, is represented as a dateTime value as described the [*:::XML Schema:::*](http://www.w3.org/TR/2004/REC-xmlschema-2-20041028/) specification. All timestamps in the XML returned by the SugarSync service are represented as dateTime values. Also notice the link to the user resource, `https://api.sugarsync.com/user/5664947`. The URL of a user resource always begins with `https://api.sugarsync.com/user/`.

In the sample app, the getAccessToken() method retrieves the Location value from the response header and returns it to the caller.

```java
return httpResponse.getHeader("Location").getValue();
```

Remember, the access token is transient. Your app can use an access token to access a resource for a short period of time: approximately one hour. However, your app can obtain additional access tokens using the same refresh token.

## What's Next?

Now that your app has created an access token, you can use it and the link to the user resource to retrieve information about the user. And that's what we'll cover next in [Getting Information about a User](get-user-info-example.md).

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API利用規約](/source/dev/terms.md)
