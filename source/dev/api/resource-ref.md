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

| HTTP REQUEST | URL REQUEST | ACTION |
| :-- | :-- | :-- |
| **[Refresh Token](refresh-resource.md)**. A persistent identifier that allows applications to authenticate users to the SugarSync service without storing user credentials. Applications use refresh tokens to obtain access tokens. |||
| POST | `https://api.sugarsync.com/app-authorization` | [Creates a refresh token.](method/create-refresh-token.md) |
||||
| **[Access Token](auth-resource.md)**. Carries the credentials and authorization information needed for accessing other SugarSync resources through the Platform API. |||
| POST | `https://api.sugarsync.com/authorization` | [Creates an access token.](method/create-auth-token.md) |
||||
| **[User](user-resource.md)**. Represents information about a SugarSync user, including the user's name, current storage utilization, a list of the user's workspaces (computers), a list of the user's sync folders, a list of the user's shared folders, the contacts for those shared folders, and a list of the user's photo albums. |||
| GET | URL of the user resource <br> (for example, `https://api.sugarsync.com/user/566494`) | [Retrieves information about the user.](method/get-user-info.md) |
| **[Workspaces Collection](ws-list-resource.md)**. A collection that represents the list of workspaces in the user's SugarSync account. |||
| GET | URL of the workspaces collection resource <br> (for example, `https://api.sugarsync.com/user/566494/workspaces/contents`) | [Retrieves the collection of workspaces in the user's Sugarsync account.](method/get-workspaces.md) |
||||
| **[Workspace](ws-resource.md)**. A collection that represents a device, such as a laptop or desktop computer, in the user's SugarSync account. A workspace may contain other collections, such as, folders. |||
| GET | URL of the workspace <br> (for example, `https://api.sugarsync.com/workspace/:sc:566494:6552993_17004`) | [Retrieves information about the workspace.](method/get-workspace-info.md) |
| GET | URL of the workspace contents <br> (for example, `https://api.sugarsync.com/workspace/:sc:566494:6552993_17004/contents`) | [Retrieves the workspace contents.](method/get-workspace-contents.md) |
| PUT | URL of the workspace <br> (for example, `https://api.sugarsync.com/workspace/:sc:566494:6552993_17004`) | [Updates the workspace name.](method/update-workspace-name.md) |
||||
| **[Sync Folders Collection](syncfolders-list-resource.md)**. A collection that represents the list of sync folders in the user's SugarSync account. A sync folder is a top-level folder that the user has selected for syncing by SugarSync. A sync folder can contain other folders that are in the user's file system. However, the folders in a sync folder are simply "folders", and not "sync folders" themselves. |||
| GET | URL of the sync folders collection resource <br> (for example, `https://api.sugarsync.com/user/566494/folders/contents`) | [Retrieves the collection of sync folders in the user's Sugarsync account.](method/get-syncfolders.md) |
||||
| **[Folder](folder-resource.md)**. A collection that represents a folder in a user's file system that has been selected for inclusion in the user's SugarSync account. A folder can contain files, other folders, or both. |||
| GET | URL of the folders in the containing collection (workspace or parent folder) <br> (for example, `https://api.sugarsync.com/workspace/:sc:566494:6552993_17004/contents` or `https://api.sugarsync.com/folder/:sc:566494:5/contents?type=folder`) | Retrieves the folders in the containing collection ([workspace](method/get-workspaces.md) or [parent folder](method/get-folders.md)). |
| GET | URL of the folder <br> (for example, `https://api.sugarsync.com/folder/:sc:566494:6552993_17229`) | [Retrieves information about the folder.](method/get-folder-info.md) |
| GET | URL of the folder contents <br> (for example, `https://api.sugarsync.com/folder/:sc:566494:6552993_17229/contents`) | [Retrieves information about the folder contents.](method/get-folder-contents.md) |
| POST | URL of the target folder <br> (for example, `https://api.sugarsync.com/workspace/:sc:566494:6552993_17004`) | [Creates a subfolder in the target folder.](method/create-folder.md) |
| POST | URL of the target folder <br> (for example, `https://api.sugarsync.com/folder/:sc:566494:5`) | [Creates a file in the target folder.](method/create-file.md) |
| POST | URL of the target folder <br> (for example, `https://api.sugarsync.com/folder/:sc:566494:5`) | [Copies a file in the target folder.](method/copy-file.md) |
| DELETE | URL of the folder <br> (for example, `https://api.sugarsync.com/workspace/:sc:566494:6552993_17004`) | [Deletes the folder.](method/delete-folder.md) |
| PUT | URL of the folder <br> (for example, `https://api.sugarsync.com/workspace/:sc:566494:6552993_17004`) | [Updates the folder name or the link to the parent directory (this moves the file).](method/update-folder-info.md) |
||||
| **[Received Share](received-share-resource.md)**. Represents a shared folder owned by another user and to which this user has been granted access privileges by the owner. |||
| GET | URL of the received share resource <br> (for example, `https://api.sugarsync.com/receivedShare/566494/:sc:575745:29_1961610490`) | [Retrieves information about the received share.](method/get-received-shares.md) |
||||
| **[Received Shares List](received-share-list-resource.md)**. Represents the list of received shares that have been granted to the user. |||
| GET | URL of the received shares list resource <br> (for example, `https://api.sugarsync.com/user/566494/receivedShares/contents`) | [Retrieves the received shares list.](method/get-received-shares-list.md) |
||||
| **[Contact](shared-folder-contact-resource.md)**. Represents another SugarSync user who has shared a folder or folders with this user. |||
| GET | URL of the contact resource <br> (for example, `https://api.sugarsync.com/folder/:sc:566494:29_1961610490`) | [Retrieves information about the contact.](method/get-contacts.md) |
||||
| **[Albums Collection](album-list-resource.md)**. A collection that represents the list of albums in the user's SugarSync account. |
| GET | URL of the albums collection resource <br> (for example, `https://api.sugarsync.com/user/566494/albums/contents`) | [Retrieves the collection of albums in the user's Sugarsync account.](method/get-album-collection-contents.md) |
||||
| **[Album](album-resource.md)**. A collection that represents a photo album that has been selected for inclusion in the user's SugarSync account. An album corresponds to a folder that contains at least one image file. |||
| GET | URL of the album <br> (for example, `https://api.sugarsync.com/album/:sc:566494:6552993_127340`) | [Retrieves information about the album.](method/get-photo-album.md) |
| GET | URL of the album contents <br> (for example, `https://api.sugarsync.com/folder/:sc:566494:6552993_127340/contents`) | [Retrieves information about the contents of the album.](method/get-album-contents.md) |
||||
| **[File](file-resource.md)**. Represents a specific user file. |||
| GET | URL of the file <br> (for example, `https://api.sugarsync.com/file/:sc:2021868:6552993_66025`) | [Retrieves information about the file.](method/get-file-info.md) |
| PUT | URL of the file <br> (for example, `https://api.sugarsync.com/file/:sc:2021868:6552993_66025`) | [Updates information about the file.](method/update-file-info.md) |
| DELETE | URL of the file <br> (for example, `https://api.sugarsync.com/file/:sc:2021868:6552993_66025`) | [Deletes the file.](method/delete-file.md) |
| GET | URL of the file's version history <br> (for example, `https://api.sugarsync.com/file/:sc:566494:6552993_66025/version`) | [Retrieves the version history of the file.](method/get-version-history.md) |
| POST | URL of the file's version history <br> (for example, `https://api.sugarsync.com/file/:sc:566494:6552993_66025/version`) | [Creates a new version of the file.](method/create-new-version.md) |
||||
| **[File Data](file-data-resource.md)**. Represents the data in a specific user file, that is, the actual bytes in the file. |||
| GET | URL of the file data resource <br> (for example, `https://api.sugarsync.com/file/:sc:566494:6552993_66025/data`) | [Retrieves the data from the file.](method/get-file-data.md) |
| PUT | URL of the file data resource <br> (for example, `https://api.sugarsync.com/file/:sc:566494:6552993_66025/data`) | [Uploads the data to the file.](method/put-file-data.md) |
| GET | URL of the file data resource for the image file <br> (for example, `https://api.sugarsync.com/file/:sc:566494:190_114714236/data`) | [Retrieves a resized and/or rotated version of the image.](method/transcode-image.md) |
||||
| **[File Version](file-version-resource.md)**. Represents a specific version of a user file. |||
| GET | URL of the file version <br> (for example, `https://api.sugarsync.com/file/:sc:566494:6552993_66025/version/6552993_3436` ) | [Retrieves information about the file version.](method/get-file-version-info.md) |
||||
| **[File Version Data](file-version-data-resource.md)**. Represents the data in a specific user file version, that is, the actual bytes in the file version. |||
| GET | URL of the file version data resource <br> (for example, `https://api.sugarsync.com/file/:sc:566494:6552993_66025/version/6552993_3436/data`) | [Retrieves the data from the file version.](method/get-file-data.md) |
| PUT | URL of the file version data resource <br> (for example, `https://api.sugarsync.com/file/:sc:566494:6552993_66025/version/6552993_3436/data`) | [Uploads the data to the file version.](method/put-file-data.md) |

---

© 2012 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
