> *The Original: [SugarSync - Best Practices](https://www.sugarsync.com/dev/best-practices.html)*

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

# Best Practices

Here you'll find a number of recommended practices when using the SugarSync Platform API in your applications.

## Protect Your Keys

When you sign up as a developer with SugarSync, we'll issue you two access keys: one is your unique developer key and one is your private access key. Do not share these keys. Your developer keys provide an authorized connection to the SugarSync platform exclusively from your app. You don't want to give anyone else this unique access ability.

In addition, use a different developer key-private access key pair for each application. This means that if you develop more than one app, you'll need to generate a new set of keys for each app you develop. In this way, not all users of your applications will be impacted if a single set of keys is compromised.

For more information about access keys, see [Authorization to Make API Requests](getting-started.md#apiauth).

## Take Advantage of Persistent URLs

The URLs for a specific user account that your app retrieves from the SugarSync service are persistent for that user account. For example, the URL in the `<magicBriefcase>` element that your app retrieves when it requests information about a SugarSync user account is a persistent URL that represents the user's Magic Briefcase. Your app can store that URL and reuse it in the future without having to retrieve it every time from that user's user resource. For more information about retrieving these URLs, see [Getting Information About a User](get-user-info-example.md).

## Specify Application-Specific User Agent Strings

Each application that you develop should provide a User-Agent header in all requests to the SugarSync API. The header should identify the application, including the specific version of the application. Note that all the examples in the User's Guide that show request headers specify a User-Agent header field value of Jakarta Commons-HttpClient/3.1. This value is only illustrative. Your requests should specify a User-Agent header field value that is specific to your application.

## Use UTF-8 Encoding

When your application specifies XML input as part of a request — for example, when it makes a request to create an authorization token or when it creates a file — the XML must specify UTF-8 encoding. The SugarSync service requires XML input to be encoded using UTF-8 encoding, and generates XML output using UTF-8 encoding.

## Remove Folder Contents Before You Delete a Folder

If your application needs to delete a folder, it's important to first remove the contents of the folder. That's because removing a folder through the SugarSync service does not remove the objects contained within the folder. If your app deletes a folder without first deleting its contents, the contents may be rendered inaccessible.

An alternative to permanently deleting a folder is moving it to the Deleted Files folder. A folder (and all its contents) in the Deleted Files folder can be recovered. Your app can retrieve the link to the user's Deleted Files folder from the `<deleted>` element in the user resource representation.

For more information about deleting a folder, see [Deleting a Folder](api/method/delete-folder.md).

## Remove Files With Care

Be certain that the user truly wants to delete a file. Files deleted using the SugarSync service are permanently removed from the system and cannot be recovered. Alternatively, a file can be moved to the Deleted Files folder. A user can recover files from the Deleted Files folder. Your app can retrieve the link to the user's Deleted Files folder from the `<deleted>` element in the user resource representation. For more information about deleting a file, see [Deleting a File](api/method/delete-file.md).

## Wait for a Response

When your app performs an operation that updates the status of a resource, wait for SugarSync to acknowledge the update before requesting status information. For example, if your app uploads data to a file, make sure it examines the response header for acknowledgement that the request was successfully processed (for example, a response status line of `HTTP/1.1 200 OK`) before retrieving information about the file through an HTTP GET request to the file resource. If your app doesn't wait for and examine the acknowledgement before requesting status, it might retrieve status information that doesn't accurately reflect the latest update.

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
