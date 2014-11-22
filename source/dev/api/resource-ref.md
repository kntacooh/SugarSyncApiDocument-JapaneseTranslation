> *The Original: [SugarSync for Developers-Resource Reference](https://www.sugarsync.com/dev/api/resource-ref.html)*

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

# Resource Reference

The SugarSync service presents a set of resources that your application can access through the Platform API. The service is hosted at the URL `https://api.sugarsync.com` and all resources are provided from that location. This means that each resource is identified with a URL that begins with `https://api.sugarsync.com`. Your application can make requests through the Platform API using HTTP methods such as GET, PUT, POST, and DELETE.

The following table lists the resources accessible through the Platform API. It also lists the URL for the resource and the HTTP methods that your application can use to make requests related to that resource. Last, it describes the action performed by each method operating on that resource. Click on a resource for further details, such as the XML representation of the resource. Click on an action for further details about the associated request, such as the XML required as input for the request or the XML generated in the response.

|HTTP REQUEST|URL REQUEST|ACTION|
|:--|:--|
|Refresh Token. A persistent identifier that allows applications to authenticate users to the SugarSync service without storing user credentials. Applications use refresh tokens to obtain access tokens.|
|POST|https://api.sugarsync.com/app-authorization|Creates a refresh token.|

Access Token. Carries the credentials and authorization information needed for accessing other SugarSync resources through the Platform API.
POST	https://api.sugarsync.com/authorization	Creates an access token.

User. Represents information about a SugarSync user, including the user's name, current storage utilization, a list of the user's workspaces (computers), a list of the user's sync folders, a list of the user's shared folders, the contacts for those shared folders, and a list of the user's photo albums.
GET	URL of the user resource
(for example, https://api.sugarsync.com/user/566494)	Retrieves information about the user.

Workspaces Collection. A collection that represents the list of workspaces in the user's SugarSync account.
GET	URL of the workspaces collection resource
(for example, https://api.sugarsync.com/user/566494/workspaces/contents)	Retrieves the collection of workspaces in the user's Sugarsync account.

Workspace. A collection that represents a device, such as a laptop or desktop computer, in the user's SugarSync account. A workspace may contain other collections, such as, folders.
GET	URL of the workspace
(for example, https://api.sugarsync.com/workspace/:sc:566494:6552993_17004)	Retrieves information about the workspace.
GET	URL of the workspace contents
(for example, https://api.sugarsync.com/workspace/:sc:566494:6552993_17004/contents)	Retrieves the workspace contents.
PUT	URL of the workspace
(for example, https://api.sugarsync.com/workspace/:sc:566494:6552993_17004)	Updates the workspace name.

Sync Folders Collection. A collection that represents the list of sync folders in the user's SugarSync account. A sync folder is a top-level folder that the user has selected for syncing by SugarSync. A sync folder can contain other folders that are in the user's file system. However, the folders in a sync folder are simply "folders", and not "sync folders" themselves.
GET	URL of the sync folders collection resource
(for example, https://api.sugarsync.com/user/566494/folders/contents)	Retrieves the collection of sync folders in the user's Sugarsync account.

Folder. A collection that represents a folder in a user's file system that has been selected for inclusion in the user's SugarSync account. A folder can contain files, other folders, or both.
GET	URL of the folders in the containing collection (workspace or parent folder) (for example, https://api.sugarsync.com/workspace/:sc:566494:6552993_17004/contents or https://api.sugarsync.com/folder/:sc:566494:5/contents?type=folder)	Retrieves the folders in the containing collection (workspace or parent folder).
GET	URL of the folder
(for example, https://api.sugarsync.com/folder/:sc:566494:6552993_17229)	Retrieves information about the folder.
GET	URL of the folder contents
(for example, https://api.sugarsync.com/folder/:sc:566494:6552993_17229/contents)	Retrieves information about the folder contents.
POST	URL of the target folder
(for example, https://api.sugarsync.com/workspace/:sc:566494:6552993_17004)	Creates a subfolder in the target folder.
POST	URL of the target folder
(for example, https://api.sugarsync.com/folder/:sc:566494:5)	Creates a file in the target folder.
POST	URL of the target folder
(for example, https://api.sugarsync.com/folder/:sc:566494:5)	Copies a file in the target folder.
DELETE	URL of the folder
(for example, https://api.sugarsync.com/workspace/:sc:566494:6552993_17004)	Deletes the folder.
PUT	URL of the folder
(for example, https://api.sugarsync.com/workspace/:sc:566494:6552993_17004)	Updates the folder name or the link to the parent directory (this moves the file).

Received Share. Represents a shared folder owned by another user and to which this user has been granted access privileges by the owner.
GET	URL of the received share resource
(for example, https://api.sugarsync.com/receivedShare/566494/:sc:575745:29_1961610490)	Retrieves information about the received share.

Received Shares List. Represents the list of received shares that have been granted to the user.
GET	URL of the received shares list resource
(for example, https://api.sugarsync.com/user/566494/receivedShares/contents)	Retrieves the received shares list.

Contact. Represents another SugarSync user who has shared a folder or folders with this user.
GET	URL of the contact resource
(for example, https://api.sugarsync.com/folder/:sc:566494:29_1961610490)	Retrieves information about the contact.

Albums Collection. A collection that represents the list of albums in the user's SugarSync account.
GET	URL of the albums collection resource
(for example, https://api.sugarsync.com/user/566494/albums/contents)	Retrieves the collection of albums in the user's Sugarsync account.

Album. A collection that represents a photo album that has been selected for inclusion in the user's SugarSync account. An album corresponds to a folder that contains at least one image file.
GET	URL of the album
(for example, https://api.sugarsync.com/album/:sc:566494:6552993_127340)	Retrieves information about the album.
GET	URL of the album contents
(for example, https://api.sugarsync.com/folder/:sc:566494:6552993_127340/contents)	Retrieves information about the contents of the album.

File. Represents a specific user file.
GET	URL of the file
(for example, https://api.sugarsync.com/file/:sc:2021868:6552993_66025)	Retrieves information about the file.
PUT	URL of the file
(for example, https://api.sugarsync.com/file/:sc:2021868:6552993_66025)	Updates information about the file.
DELETE	URL of the file
(for example, https://api.sugarsync.com/file/:sc:2021868:6552993_66025)	Deletes the file.
GET	URL of the file's version history
(for example, https://api.sugarsync.com/file/:sc:566494:6552993_66025/version)	Retrieves the version history of the file.
POST	URL of the file's version history
(for example, https://api.sugarsync.com/file/:sc:566494:6552993_66025/version)	Creates a new version of the file.

File Data. Represents the data in a specific user file, that is, the actual bytes in the file.
GET	URL of the file data resource
(for example, https://api.sugarsync.com/file/:sc:566494:6552993_66025/data)	Retrieves the data from the file.
PUT	URL of the file data resource
(for example, https://api.sugarsync.com/file/:sc:566494:6552993_66025/data)	Uploads the data to the file.
GET	URL of the file data resource for the image file
(for example, https://api.sugarsync.com/file/:sc:566494:190_114714236/data)	Retrieves a resized and/or rotated version of the image .

File Version. Represents a specific version of a user file.
GET	URL of the file version
(for example, https://api.sugarsync.com/file/:sc:566494:6552993_66025/version/6552993_3436)	Retrieves information about the file version.

File Version Data. Represents the data in a specific user file version, that is, the actual bytes in the file version.
GET	URL of the file version data resource
(for example, https://api.sugarsync.com/file/:sc:566494:6552993_66025/version/6552993_3436/data)	Retrieves the data from the file version.
PUT	URL of the file version data resource
(for example, https://api.sugarsync.com/file/:sc:566494:6552993_66025/version/6552993_3436/data)	Uploads the data to the file version.


---

© 2014 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
