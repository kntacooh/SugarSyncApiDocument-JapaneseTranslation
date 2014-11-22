> *The Original: [SugarSync for Developers - Build a Cloud Sync App](https://www.sugarsync.com/dev/home.html)*

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

# Developers, Bring the Power of the Cloud to Your App!

At SugarSync, we provide exceptional online backup, cloud storage, and file syncing to millions of users. In addition, our mobile apps offer our users unprecedented access to files and photos from their mobile devices.

> *See the following picture:
https://www.sugarsync.com/images/old/dev_cloud.png*

With the free SugarSync Platform API, your users can reach all the way to the cloud to access their files from any of their computers. Cloud-enable your web, mobile, or desktop application with SugarSync's easy to implement API:

* Allow your users the full breadth of access to their files and folders stored in the cloud.
* Let users create new files and folders within their cloud environment for instant backup and syncing.
* Provide a robust sharing environment by leveraging SugarSync's file and photo sharing options.


**[*:::Join our Program :::*](https://www.sugarsync.com/developer/signup)**

... and then ...

**[Get started with our Sample App](using-api.md)**

Also, see [what's new in the Platform API](#apinew).

---

## About the SugarSync API

The SugarSync Platform API is currently in use by large and small partners with various levels of integration and complexity. These integrations range from simple document editor extensions to full customer-facing implementations of SugarSync launched worldwide to hundreds of thousands of users.

Wondering how the API works? Here are the basics.

* The SugarSync API uses a REST-style architecture over HTTPS.
* Requests and responses are formatted using straightforward XML.
* Files, folders and other objects in a SugarSync user's account are represented as resources.
* Each resource has a unique, user-specific, persistent Uniform Resource Identifier (URI).
* Resources are read and modified using basic HTTP operations such as GET, POST, PUT and DELETE.

We hope you find the API straightforward and easy to implement. Read our [Getting Started guide](getting-started.md) or explore our [code examples](using-api.md) that demonstrate the API in use. Or you can jump right in and explore the syntax and semantics of the API in the SugarSync API [Resource Reference](./api/resource-ref.md).

## Let Us Promote Your App

We love new apps! When yours is complete and production-ready, let us and others know about it by announcing it in our [*:::Developer forum:::*](http://groups.google.com/a/developers.sugarsync.com/group/platform-api/subscribe). Announcing your app in this way makes it eligible for additional promotional opportunities. We'll review your app and can post it in our [*:::App Showcase:::*](https://www.sugarsync.com/partners/) if it meets our quality standards. Every month, we also feature a new SugarSync-integrated app in our newsletter — to which users are automatically subscribed when they open a SugarSync account. Wouldn't it be great to promote your app to millions of SugarSync customers? [*:::Sign up as a SugarSync Developer:::*](https://www.sugarsync.com/developer/signup) and get started!

## Create User Accounts With Your Apps

Using the Platform API you can create apps that allow users with SugarSync accounts to access, create, and share their SugarSync-managed resources. In addition, SugarSync provides a free service called developer provisioning that gives you the ability to develop apps that create new SugarSync user accounts. That way users can create SugarSync accounts through your apps without having to visit the SugarSync site. See [Developer Provisioning](dev-provisioning.md) for further details.

**Note:** You need prior authorization to use the developer provisioning service in your app. Contact us at developers@sugarsync.com if you have not already received the needed authorization.

<a name="apinew"></a>
## What's New

As you look through the documentation, you'll notice some new features. Some of these were recently added to the Platform API. Some have been around for a while, but haven't been documented until now. These features include:

* Updated authentication mechanism. Developers can now authenticate a user to the SugarSync service without storing the user's ID and password. Learn more about the updated authenticated mechanism in [Getting Authorization](get-auth-token-example.md).
* Resumable uploads. Applications can now resume upload operations that were interrupted due to network activity or user actions. See [Resumable Uploads](upload-file-data-example.md#resumeup) for details.
* Shared folder support. Developers can allow users to download, add, and edit files in folders shared with them by other users. See [Received Share Resource](api/received-share-resource.md) to learn how a shared folder is represented, and [Retrieving Received Share Information](api/method/get-received-shares.md) to learn how to retrieve information about a shared folder.
* Sorting. Applications can sort the contents of folders by name, modification date, size, or extension. See [Tailoring the Results](get-folder-info-example.md#tailorres) to learn more about sorting as well as other ways to arrange the results to fit your requirements.
* Thumbnail generation. Developers can request transformed versions of image files in order to implement thumbnail galleries or slideshows. See [Transcoding Images](api/method/transcode-image.md) for details.
* Developer provisioning. Developers can programmatically create new SugarSync user accounts. That way users can create SugarSync accounts through developer-created applications without having to visit the SugarSync site. Learn more in [Developer Provisioning](dev-provisioning.md).

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
