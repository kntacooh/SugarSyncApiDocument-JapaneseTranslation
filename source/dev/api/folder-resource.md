> *The Original: [SugarSync for Developers-Folder Resource](https://www.sugarsync.com/dev/api/folder-resource.html)*

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

# Folder Resource

A folder resource is a collection that represents a folder in a user's file system that has been selected for inclusion in the user's SugarSync account. A folder can contain files, other folders, or both. One type of folder, called a sync folder, is a top-level folder that the user has selected for syncing by SugarSync. A sync folder can contain other folders that are in the user's file system. However, the folders in a sync folder are simply "folders", and not "sync folders" themselves.

## URL

Begins with `https://api.sugarsync.com/folder`, for example, `https://api.sugarsync.com/folder/:sc:566494:6552993_17229`.

Appending `/contents` to the end of the URL points to the contents of the folder. For example, the URL `https://api.sugarsync.com/folder/:sc:566494:6552993_17229/contents` points to the contents of the folder whose URL is `https://api.sugarsync.com/folder/:sc:566494:6552993_17229`.

## XML Representation

A folder resource is represented in XML with the following elements:

* **displayName**. The user-visible name of the folder.
* **dsid**. A SugarSync identifier that uniquely identifies the folder resource.
* **timeCreated**. A dateTime value that specifies the date and time the folder was created.
* **parent**. A link to the parent collection (workspace or folder) that contains the folder.
* **collections**. A link to the collections (folders) contained by the folder.
* **files**. A link to the files contained by the folder.
* **contents**. A link to the contents of the folder.
* **sharing**. Indicates whether the folder is a shared folder (`enabled="true"`) or not a shared folder (`enabled="false"`).

Here is an example of a folder resource representation:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<folder>
  <displayName>100A</displayName>
  <dsid>/sc/566494/6552993_17248<dsid>
  <timeCreated>2011-11-23T16:36:48.000-08:00<timeCreated>
  <parent>https://api.sugarsync.com/folder/:sc:566494:5</parent>
  <collections>https://api.sugarsync.com/folder/:sc:566494:6552993_17248/contents?type=folder</collections>
  <files>https://api.sugarsync.com/folder/:sc:566494:6552993_17248/contents?type=file</files>
  <contents>https://api.sugarsync.com/folder/:sc:566494:6552993_17248/contents</contents>
  <sharing enabled="false"/>
</folder>
```

## Folder Resource Contents

The contents of a folder resource are represented in XML. Each folder contained by the folder is represented by the following elements:

* **collection**. The type of the collection (`type="folder"`).
* **displayName**. The user-visible name of the folder.
ref. A link to the folder.
* **contents**. A link to the contents of the folder.

Each file contained by the folder is represented by the following elements:

* **file**. An indicator that this is a file.
* **displayName**. The user-visible name of the file.
* **ref**. A link to the file.
* **size**. The size in bytes of the file.
* **lastModified**. A dateTime value that specifies the date and time the file was last modified.
* **mediaType**. The media type of the file, such as image/jpeg.
* **presentOnServer**. Whether the file is on the server (`true`) or not (`false`).
* **fileData**. A link to the data for the file.

Notice that file and folder information in the folder contents representation is a subset of the information provided in the folder resource representation.

Here is an example of a folder contents representation:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<collectionContents start="0" hasMore="false" end="4">
  <collection type="folder">
    <displayName>Presidents</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:6552993_17246</ref>
    <contents>https://api.sugarsync.com/folder/:sc:566494:6552993_17246/contents>
  </collection>
  <collection type="folder">
    <displayName>DC-102010</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:6552993_17250</ref>
    <contents>https://api.sugarsync.com/folder/:sc:566494:6552993_17250/contents>
  </collection>
  <file>
    <displayName>Abe-Lincoln.jpg</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:6552993_17252</ref>
    <size>38539</size>
    <lastModified>2010-02-11T15:26:52.000-08:00</lastModified>
    <mediaType>image/jpeg</mediaType>
    <presentOnServer>true</presentOnServer>
    <fileData>https://api.sugarsync.com/file/:sc:566494:6552993_17252/data</fileData>
  </file>
  <file>
    <displayName>George-Washington.jpg</displayName>
    <ref>https://api.sugarsync.com/folder/:sc:566494:6552993_17253</ref>
    <size>69089</size>
    <lastModified>2011-11-11T07:48:10.000-08:00</lastModified>
    <mediaType>image/jpeg</mediaType>
    <presentOnServer>true</presentOnServer>
    <fileData>https://api.sugarsync.com/file/:sc:566494:6552993_17253/data</fileData>
  </file>
</collectionContents>
```

## Methods

The folder resource supports the following methods:

* GET. See:
  * [Retrieving Folders](method/get-folders.md)
  * [Retrieving Folder Information](method/get-folder-info.md)
  * [Retrieving Folder Contents](method/get-folder-contents.md)
* POST. See:
  * [Creating a Folder](method/create-folder.md)
  * [Creating a File in a Folder](method/create-file.md)
  * [Copying a File in a Folder](method/copy-file.md)
* DELETE. See [Deleting a Folder](method/delete-folder.md)
* PUT. See [Updating the Folder Name](method/update-folder-info.md)

---

© 2012 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
