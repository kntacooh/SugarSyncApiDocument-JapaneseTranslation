> *The Original: [開発者のためのSugarSync - フォルダ・リソース](https://www.sugarsync.com/dev/api/folder-resource.html)*

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

#フォルダ・リソース

フォルダ・リソースは、ユーザーのSugarSyncアカウント内に含めるために選ばれている、ユーザーのファイルシステム内に存在するフォルダを表すコレクションです。
フォルダは、ファイル、他のフォルダ、もしくはその両方を含むことができます。同期フォルダと呼ばれるフォルダの種類は、SugarSyncによる同期のためにユーザーに選ばれた最上位のフォルダです。同期フォルダは、ユーザーのファイルシステム内に存在する他のフォルダを含むことができます。しかしながら、同期フォルダ内のフォルダは単純に「フォルダ」であり、「同期フォルダ」自身ではありません。

## URL

Begins with `https://api.sugarsync.com/folder`, for example, `https://api.sugarsync.com/folder/:sc:566494:6552993_17229`.

Appending `/contents` to the end of the URL points to the contents of the folder. For example, the URL `https://api.sugarsync.com/folder/:sc:566494:6552993_17229/contents` points to the contents of the folder whose URL is `https://api.sugarsync.com/folder/:sc:566494:6552993_17229`.

## XMLの表記

フォルダ・リソースは、以下の要素を持つXMLで表されます:

* **displayName**. ユーザーに見えるフォルダの名前。* **dsid**. そのフォルダ・リソースを一意に識別するSugarSyncのID。* **timeCreated**. フォルダが作成された時刻を明記する DateTime 値。* **parent**. A link to the parent collection (workspace or folder) that contains the folder.
* **collections**. フォルダに含まれるコレクション(フォルダ)へのリンク。* **files**. フォルダに含まれるファイルへのリンク。* **contents**. フォルダのコンテンツへのリンク。* **sharing**. フォルダが共有フォルダか(`enabled="true"`)共有フォルダでないか(`enabled="false"`)を示します。

フォルダ・リソースの表記例:

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

## フォルダ・リソースのコンテンツ

フォルダ・リソースのコンテンツは、XMLで表されます。フォルダに含まれるそれぞれのフォルダは以下の要素で表されます:

* **collection**. そのコレクションのタイプ(`type="folder"`)。* **displayName**. ユーザーに見えるフォルダの名前。ref. フォルダへのリンク。* **contents**. フォルダのコンテンツへのリンク。

フォルダに含まれるそれぞれのファイルは以下の要素で表されます:

* **file**. これがファイルだという指標。* **displayName**. ユーザーに見えるファイルの名前。* **ref**. ファイルへのリンク。* **size**. ファイルのサイズ (単位:バイト)。* **lastModified**. ファイルが更新された時刻を明記する DateTime 値。* **mediaType**. image/jpeg のような、ファイルのMIME。* **presentOnServer**. ファイルがサーバー上にあるか(`true`)ないか(`false`)。* **fileData**. ファイルのデータへのリンク。

フォルダのコンテンツの表記内のファイルとフォルダの情報は、フォルダ・リソースの表記内に提供された情報の一部であることに注意してください。 

フォルダのコンテンツの表記例:

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

## メソッド

フォルダ・リソースは以下のメソッドをサポートします:

* GET。See:
  * [フォルダの取得](method/get-folders.md)
  * [フォルダ情報の取得](method/get-folder-info.md)
  * [フォルダのコンテンツの取得](method/get-folder-contents.md)
* POST. See:
  * [フォルダの作成](method/create-folder.md)
  * [フォルダ内のファイルの作成](method/create-file.md)
  * [フォルダ内のファイルのコピー](method/copy-file.md)
* DELETE. [See Deleting a Folder](method/delete-folder.md)
* PUT. See [フォルダ名の更新](method/update-folder-info.md)

---

© 2012 SugarSync, Inc. All Rights Reserved.  [API利用規約](/source/dev/terms.md)
