> *The Original: [SugarSync for Developers-Resource Overview](https://www.sugarsync.com/dev/resources.html)*

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

<a name="resource"></a>
# Resource Overview

The SugarSync service presents a set of resources that your application can access through the Platform API. The resources represent objects that SugarSync recognizes and manages, such as user accounts, workspaces, folders, and files.

A workspace represents a device such as a computer that has been added to the user's SugarSync account. Each workspace and folder resource in a user account is treated as a group of objects called a collection. A collection can contain other collections. For example, a workspace, which is a collection, contains sync folders, which are themselves collections. A sync folder is a top-level folder that the user has selected for syncing by SugarSync. Another type of collection, called an album, contains image files.

Not all resources managed by SugarSync are collections. For example, an individual file is not a collection.

The following figure illustrates resources available through the Platform API.

> *See the following picture: https://www.sugarsync.com/images/old/sugarsync_new_resources.png*

Here are the types of resources available through the Platform API:

* **Refresh Token.** A persistent identifier that allows applications to authenticate users to the SugarSync service without storing user credentials. Applications use refresh tokens to obtain access tokens. See [Refresh Token Resource](api/refresh-resource.md) for further details.
* **Access Token.** A resource that carries the credentials and authorization information needed for accessing other SugarSync resources through the Platform API. An access token is acquired using a refresh token. See [Access Token Resource](api/auth-resource.md) for further details.
* **User.** Represents information about a SugarSync user, such as the user's name and nickname, as well as the user's current storage utilization. The resource also contains links to various collections, such as the user's workspaces, sync folders, and shared folders. A sync folder is a folder that the user has added to SugarSync for synchronization. A shared folder is a folder owned by another user to which this user has been granted access. See [User Resource](api/user-resource.md) for further details.
* **Workspaces Collection.** A collection that represents the list of workspaces in the user's SugarSync account. Because a workspace corresponds to a device in the user's SugarSync account, a workspaces collection corresponds to all the devices in the user's SugarSync account. See [Workspaces Collection Resource](api/ws-list-resource.md) for further details.
* **Workspace.** A collection that corresponds to a device, such as a laptop or desktop computer, in the user's SugarSync account. A workspace contains the folders, and the files within those folders, that a user has added to SugarSync from a particular device or synced locally to the device using the client software. See [Workspace Resource](api/ws-resource.md) for further details.
* **Sync Folders Collection.** A collection that represents the list of sync folders in the user's SugarSync account. A sync folder is a top-level folder that the user has selected for syncing by SugarSync. It can contain other folders that are in the user's file system. However, the folders in a sync folder are simply "folders", and not "sync folders" themselves. See [Sync Folders Collection Resource](api/syncfolders-list-resource.md) for further details.
* **Folder.** A collection that represents a folder in a user's file system that has been selected for inclusion in the user's SugarSync account. A folder can contain files, other folders, or both. See [Folder Resource](api/folder-resource.md) for further details.
* **Received Share.** Represents a shared folder owned by another user and to which this user has been granted access privileges by the owner. See [Received Share Resource](api/received-share-resource.md) for further details.
* **Received Shares List.** Represents the list of received shares that have been granted to the user. See [Received Shares List Resource](api/received-share-list-resource.md) for further details.
* **Contact.** Represents another SugarSync user who has shared a folder or folders with this user. See [Contact Resource](api/shared-folder-contact-resource.md) for further details.
* **Albums Collection.** A collection that represents the list of albums in the user's SugarSync account. See [Albums Collection Resource](api/album-list-resource.md) for further details.
* **Album.** A collection that represents a user's photo album. An album corresponds to a folder that contains at least one image file. See [Album Resource](api/album-resource.md) for further details.
* **File. Represents a specific user file. See [File Resource](api/file-resource.md) for further details.
* **File Data.** Represents the data in a specific user file, that is, the actual bytes in the file. See [File Data Resource](api/file-data-resource.md) for further details.
* **File Version.** Represents a specific version of a user file. See [File Version Resource](api/file-version-resource.md) for further details.
* **File Version Data.** Represents the data in a specific user file version, that is, the actual bytes in the file version. See [File Version Data Resource](api/file-version-data-resource.md) for further details.

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
