> *The Original: [SugarSync for Developers-Managing Your Access Keys and Applications](https://www.sugarsync.com/dev/managing-apps.html)*

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

# Managing Your Access Keys and Applications

SugarSync provides a [*:::Developer Console:::*](https://www.sugarsync.com/developer/account) that you can use to manage your applications that access SugarSync resources. You also can also use the Developer Console to manage your access keys, that is, your access key ID and private access key, which are used for authentication.

The Developer Console displays information about your access keys as well as your apps. The console allows you to perform the following management tasks:

* [Add keys](#addkeys)
* [Disable keys](#diablekeys)
* [Enable keys](#enablekeys)
* [Create an application](#createapp)
* [Edit an application](#editapp)
* [Disable an application](#disableapp)
* [Enable an application](#enableapp)
* [Delete an application](#deleteapp)

## Managing Access Keys

When you sign up as a developer, SugarSync creates an initial access key ID and private access key for you. For example:

> *See the following picture: https://www.sugarsync.com/images/AccessKeys1.png*

Notice that when the keys are created they are enabled

**Protect your keys:** Do not share these keys. Your access key ID and private access key provide an authorized connection to the SugarSync platform exclusively from your app. You don't want to give anyone else this unique access ability.

**Obtain a different key pair for each app:** Although not mandatory, it's good practice to obtain a different access key ID-private access key pair for each application you develop for use with SugarSync. SugarSync provides a maximum of ten access key ID-private access key pairs for each developer account.

<a name="addkeys"></a>
### Adding Keys

To obtain a new access key ID-private access key pair, click *Add Keys button*. You should see a new access key ID and private access key listed under "Your Access Keys" in the Developer Console.

> *See the following picture: https://www.sugarsync.com/images/AccessKeys2.png*

<a name="diablekeys"></a>
### Disabling Keys

There may be times when you need to disable a key pair. For example, suppose you're concerned that someone else has access to your keys. In this case, disabling the key pair will prevent that person from using those keys. To disable a key pair, check the box to the left of the key pair under "Your Access Keys" in the Developer Console, and click *Disable Keys button*. The status for the key pair will change to "Disabled".

> *See the following picture: https://www.sugarsync.com/images/DisableKeys.png*

<a name="enablekeys"></a>
### Enabling Keys

If you disable a key pair, you can re-enable it. To do that, check the box to the left of the key pair under "Your Access Keys" in the Developer Console, and click *Enable Keys button*. The status for the key pair will change to "Enabled".

## Managing Applications

Before your application can be used, it needs to be registered with SugarSync. In SugarSync terms, this is called "creating an application". Once you create an app, you can edit it. You can also disable the app, re-enable the app, and delete the app.

<a name="createapp"></a>
### Creating an Application

To create an application, click *Create app button*. This will display a "Create app" dialog for you to enter information about the application:

> *See the following picture: https://www.sugarsync.com/images/CreateAppDialog.png*

* Name: The name of the application.
* Publisher - The publisher of the application, for example, your company name.
* Description - A brief description of the application.
* URL - The URL of the application's home page.
* Support Email - An email address that can be used for application support purposes.
* Icon - A thumbnail image that represents the icon of the application. Input fields are available to specify icons of three different sizes: 16x16 pixels, 64x64 pixels, and 128x128 pixels. You can navigate to an existing thumbnail image by clicking "Browse ..." next to the respective icon field.

Enter the information in the "Create app" dialog, as appropriate, and click *Create app button*. In response, you should see an entry for the new application under "Your Apps" in the Developer Console.

> *See the following picture: https://www.sugarsync.com/images/AppCreated.png*

Notice that SugarSync enables the app and also assigns it an app ID. The app ID uniquely identifies the application. You app will need to specify the app ID as well as the pertinent access key ID-private access key pair when it requests a refresh token for authorization to access the SugarSync API. (See [Getting Authorization](get-auth-token-example.md) for further details about authorization.)

<a name="editapp"></a>
### Editing an Application

You can edit an application. You do this to change the registration information for the app. To edit an application, click *Edit app button* in the row that lists the app. This will display an "Edit app" dialog for you to change the registration information about the application:

> *See the following picture: https://www.sugarsync.com/images/EditAppDialog.png*

Enter the changed information in the "Edit app" dialog, as appropriate, and click *Update app button*. In response, the registration information for the app is updated.

<a name="disableapp"></a>
### Disabling an Application

There may be times when you need to disable an application. For example, the app has serious issues and so you want to remove it from use. To disable a registered app, click *Disable app button* in the row that lists the app. The status for the app will change to "Disabled".

> *See the following picture: https://www.sugarsync.com/images/AppDisabled.png*

Notice that when you disable an application, *Disable app button* changes to *Enable app button*.

<a name="enableapp"></a>
### Enabling an Application

To re-enable a disabled app, click *Enable app button* in the row that lists the disabled app. The status for the app will change to "Enabled".

Alternatively, you can click *Edit app button* in the row that lists the disabled app. This will display the "Edit app" dialog. Select "Enabled" in the drop-down menu next to "App Status" in the "Edit app" dialog, and click *Update app button*. The status for the app will change to "Enabled".

<a name="deleteapp"></a>
### Deleting an Application

You can delete a registered application. To do that, click *Delete app button* in the row that lists the app. You'll be prompted to ensure that you really want to delete the app. If you respond by clicking OK to the prompt, the app will be deleted and it will no longer appear under "Your Apps" in the Developer Console. You will no longer be able to use the app.

*Caution: Deleting an application is irreversible*. If you delete an application, you cannot reverse the process. You will no longer be able to use the app to access user accounts.

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API利用規約](/source/dev/terms.md)
