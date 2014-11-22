> *The Original: [SugarSync - Glossary](https://www.sugarsync.com/dev/glossary.html)*

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

# Glossary

**[A](#aterms)**
B
**[C](#cterms)**
**[D](#dterms)**
E
**[F](#fterms)**
G
**[H](#hterms)**
I
J
K
L
**[M](#mterms)**
N
O
**[P](#pterms)**
Q
**[R](#rterms)**
**[S](#sterms)**
T
**[U](#uterms)**
V
**[W](#wterms)**
**[X](#xterms)**
Y
Z

<a name="aterms"></a>
A

<a name="acctoken"></a>
**access token**. A software credential that is used for authorizing a requester to access a resource. The token is used in addition to or in place of a password to prove that the requester is who it claims to be. An application needs to create an access token for authorization to access SugarSync resources through the Platform API. An access token is acquired using a [refresh token](#rtoken).

**album**. A collection that represents a photo album that has been selected for inclusion in the user's SugarSync account. An album corresponds to a folder that contains at least one image file.

**albums collection**. A resource that represents the list of photo albums in the user's SugarSync account.

**authorization token**. See [access token](#acctoken).

<a name="cterms"></a>
C

**collection**. A resource that constitutes a group of objects managed by SugarSync. Workspaces and folders, both of which typically contain groups of objects, are collections.

**contact**. See [shared folder contact](#sfcontact).

<a name="dterms"></a>
D

**dateTime**. A datatype specified in the XML Schema Definition Language that represents an instant of time, optionally marked with a particular time zone offset. All timestamps in XML documents returned by the SugarSync service are represented as dateTime values.

**Deleted Files**. A folder that contains synced files that have been deleted by the user. Files in the Deleted Files folder can be restored.

<a name="fterms"></a>
F

**file**. A resource that represents a specific user file.

**file data**. A resource that represents the data in a specific user file.

**file version**. A resource that represents a specific version of a user file.

**file version data**. A resource that represents the data in a specific user file version, that is, the actual bytes in the file version.

**folder**. A collection that represents a folder in a user's file system that has been selected for inclusion in the user's SugarSync account. See also [shared folder](#sharef) and [sync folder](#syncf).

<a name="hterms"></a>
H

**HTTP (Hypertext Transfer Protocol)**. An application protocol for exchanging files and other data on distributed systems such as the World Wide Web. In SugarSync, HTTP is used as the standard protocol for exchanging resource representations between a client and the SugarSync server.

**HTTP request**. A message sent using the HTTP protocol from a client to a server. The request consists of an initial line that specifies an HTTP method (such as GET or PUT), the path to the requested resource, and the version of HTTP to use; one or more request header lines that provide additional information (metadata) about the request; and optionally, a message body that provides input data for the request.

**HTTP response**. A message sent by a server in response to an HTTP request send from a client. The response consists of a response status code that indicates whether the request was successful or not, one or more response header lines that provide additional information (metadata) about the response; and optionally, a message body that provides data returned in response to the request.

<a name="mterms"></a>
M

**Magic Briefcase**. A folder that is used to automatically sync files and folders across all computers and mobile devices in a user's account. Any files or folders that are in the Magic Briefcase folder are instantly accessible on the computers and mobile devices identified in the user's account.

**Mobile Photos**. A folder that is used to is used to store photos (or videos) that the user uploads from a mobile device. The folder can also contain folders and other types of files if the user chooses to put them there.

<a name="pterms"></a>
P

**Platform API**. See [SugarSync Platform API](#splatapi).

public link. A link to a resource that the owner sends to a user. This allows the user to access the owner's resource through that link.

<a name="rterms"></a>
R

**received share**. Represents a shared folder owned by another user and to which this user has been granted access privileges by the owner.

**received shares list**. A resource that represents the list of received shares that have been granted to the user.

<a name="rtoken"></a>
**refresh token**. A persistent identifier that allows applications to authenticate users to the SugarSync service without storing user credentials. Applications use refresh tokens to obtain [access tokens](#acctoken).

**resource**. In the REST style of architecture, any meaningful concept that can be addressed. Resources represent objects that SugerSync recognizes and manages, such as user accounts, folders, and files.

**REST (Representational State Transfer)**. A style of software architecture for distributed systems such as the World Wide Web. An important concept in REST is the existence of resources, each of which can be referred to using a global identifier, that is, a URI. In order to manipulate these resources, components of the network, clients and servers, communicate using a standardized interface such as HTTP and exchange representations of these resources. The SugarSync Platform API is a REST-based API.

<a name="sterms"></a>
S

<a name="sharef"></a>
**shared folder**. A collection that represents a folder that is owned by another user and to which this user has been granted access privileges by the owner.

<a name="sfcontact"></a>
**shared folder contact**. A resource that represents a SugarSync user who has shared a folder or folders with the recipient SugarSync user.

<a name="splatapi"></a>
**SugarSync Platform API**. A programming interface that enables applications to access resources managed by SugarSync.

<a name="syncf"></a>
**sync folder**. A top-level folder that the user has selected for syncing by SugarSync. A sync folder can contain other folders that are in the user's file system. However, the folders in a sync folder are simply folders, and not sync folders themselves.

**sync folders collection**. A resource that represents the list of sync folders in the user's SugarSync account.

<a name="uterms"></a>
U

**URL (uniform resource locator)**. A character string that constitutes a reference to an Internet resource. A SugarSync resource is identified by its URL.

**user**. A resource that represents information about a SugarSync user, such as the user's name and nickname, as well as the user's storage current storage utilization. The resource also contains links to various collections, such as the user's workspaces, sync folders, and shared folders.

**UTF-8 (Universal Character Set Transformation Format-8 bit)**. A variable-width encoding that can represent every character in the Unicode character set using one to four 8-bit bytes (termed "octets" in the Unicode Standard). The SugarSync service requires XML input to be encoded using UTF-8 encoding, and generates XML output using UTF-8 encoding.

<a name="wterms"></a>
W

**Web Archive**. A special sync folder for backing up and storing files online. Files in the Web Archive are not synced to any devices.

**workspace**. A collection that corresponds to a device, such as a laptop or desktop computer, on which a user has installed the SugarSync client. A workspace contains the folders, and the files within those folders, that a user has added to SugarSync from a particular device or synced locally to the device using the client software.

**workspaces collection**. A resource that represents the list of workspaces in the user's SugarSync account.

<a name="xterms"></a>
X

**XML (Extensible Markup Language)**. A markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. When an application makes a request through the Platform API, it either provides as input XML for the SugarSync service to process, or the service generates XML in response to the request.

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
