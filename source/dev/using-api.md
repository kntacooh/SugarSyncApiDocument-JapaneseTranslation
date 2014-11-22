> *The Original: [SugarSync for Developers-API Examples](https://www.sugarsync.com/dev/using-api.html)*

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

# Using the Platform API: Examples

We've provided a sample application to help you get started with the Platform API. The application, which is written in Java, uses the API to perform various tasks. You can download the application and examine the source code to see how these tasks are implemented.

**[*:::Download the Sample App:::*](https://www.sugarsync.com/dev/resource/sugarsync-api-sample.zip)**

The application is designed to work with a SugarSync user's Magic Briefcase. The Magic Briefcase is a folder in a user's Documents folder that is managed by SugarSync. Anything that the user puts into the Magic Briefcase folder is automatically backed up and synced to other devices on which the user has SugarSync installed.

Using the application, you can:

* Display the user's quota information. This includes the total SugarSync storage available to the user in bytes, the amount of storage currently used by the user, and the remaining free storage.
* List the folders and files in the user's Magic Briefcase.
* Download a file from the user's Magic Briefcase to the local directory. The file must be in the Magic Briefcase.
* Upload a file from the local directory to the user's Magic Briefcase. The file must be in the local directory.

### Installing and Running the Application

The application is structured as a Maven project and packaged in a zip file, sugarsync-api-sample.zip. If you don't already have Maven, you can download it from the [*:::Apache Maven site:::*](http://maven.apache.org/).

To install the sample application:

1. Unzip the sugarsync-api-sample.zip file.
2. Navigate to the root directory, sugarsync-api-sample.
3. Run the following command from the command line: `mvn package`.

Among other things, this process creates a target subdirectory in the sugarsync-api-sample directory. The target subdirectory contains the classes and JAR files for the sample application.

You can then run the application by command from a command line window. Alternatively, you can import the project to Eclipse and run it from the Eclipse IDE. See [Importing an Running the Application in Eclipse](#ineclipse) for further details.

If you run the application by command from a command line window, make sure you run the command from the sugarsync-api-sample directory. The syntax of the command is as follows:

```
java -jar ./target/sugarsync-api-sample.jar -user username -password password -application appId
-accesskey publicAccessKey -privatekey privateAccessKey quota | list | download fileToDownload | upload fileToUpload
```

where:

*username* - The SugarSync user's username (email address).  
*password* - The SugarSync user's password.  
*appId* - The ID assigned to your app.  
*publicAccessKey* - Your access key ID.  
*privateAccessKey* - Your private access key.  
*fileToDownload* - The file to be downloaded to the current directory from the user's Magic Briefcase.  
*fileToUpload* - The file to be uploaded from the current directory to the Magic Briefcase.

The app ID is the one assigned by SugarSync to your app when you created the app using the [*:::Developer Console:::*](https://www.sugarsync.com/developer/account). The access key ID and private access key are the pair of keys you received when you signed up as a developer with SugarSync, or they are an additional key-pair that you created using the Developer Console. Note that it's good practice to obtain a different access key ID-private access key pair for each application you develop for use with SugarSync.

Here are some examples that demonstrate how to run the application from the command line. Note that the username, password, app ID, access key ID, and private access key shown in the examples are only illustrative. Make sure to use the actual username and password of the pertinent user, the actual ID issued to your app, and the keys that you were issued.

<a name="dispquota"></a>
**Display the user's quota:**

```
java -jar ./target/sugarsync-api-sample.jar -user user@email.com -password userpw -application /sc/10098/4_235123
-accesskey  MTUzOTEyNjEzMjM4NzEwNDg0MTc -privatekey ZmNhMWY2MTZlY2M1NDg4OGJmZDY4OTExMjY5OGUxOWY quota
```

You should see results that look something like this:

```
---QUOTA INFO---
Total storage available: 5.375 GB
Storage usage: 2.377 GB
Free storage: 2.998 GB
```

<a name="listcont"></a>
**List the contents of the user's Magic Briefcase:**

```
java -jar ./target/sugarsync-api-sample.jar -user user@email.com -password userpw -application /sc/10098/4_235123
-accesskey MTUzOTEyNjEzMjM4NzEwNDg0MTc -privatekey ZmNhMWY2MTZlY2M1NDg4OGJmZDY4OTExMjY5OGUxOWY list
```

You should see results that look something like this:

```
-MagicBriefcase
-Folders:
Sample Documents
Sample Music
Sample Photos
Work Items
-Files:
What is Magic Briefcase.pdf
George_Washington.jpg
```

<a name="downfile"></a>
**Download a file named George_Washington.jpg to the local directory from the user's Magic Briefcase:**

```
java -jar ./target/sugarsync-api-sample.jar -user user@email.com -password userpw -application /sc/10098/4_235123
-accesskey  MTUzOTEyNjEzMjM4NzEwNDg0MTc -privatekey ZmNhMWY2MTZlY2M1NDg4OGJmZDY4OTExMjY5OGUxOWY download George_Washington.jpg
```

You should see results that look like this:

```
Download completed successfully. The George_Washington.jpg from "Magic Briefcase"
was downloaded to the local directory.
```

<a name="upfile"></a>
**Upload a file named Thomas-Jefferson.jpg from the local directory to the user's Magic Briefcase:**

```
java -jar ./target/sugarsync-api-sample.jar -user user@email.com -password userpw -application /sc/10098/4_235123
-accesskey  MTUzOTEyNjEzMjM4NzEwNDg0MTc -privatekey ZmNhMWY2MTZlY2M1NDg4OGJmZDY4OTExMjY5OGUxOWY upload Thomas-Jefferson.jpg
```

You should see results that look like this:

```
Upload completed successfully. Check "Magic Briefcase" remote folder
```

<a name="ineclipse"></a>
### Importing and Running the Application in Eclipse

After you install the sample application, you can import the project into Eclipse as follows:

1. Navigate to the root directory, sugarsync-api-sample.
2. Run the following command from the command line: `mvn eclipse:eclipse`.
3. Open the Eclipse IDE if not already opened.
4. Select Import from the File menu. Then select Existing Projects into Workspace.
5. Enter the root directory of the project, sugarsync-api-sample, in the Select an import source field.
6. Click Finish. You should see the sugarsync-api-sample project added to the Package Explorer. However, the project will be listed with build errors. To resolve the errors, you need to add a new classpath variable, M2_REPO, to the build path as follows:
7. Select Preferences from the Window menu. Then select Java->Build Path->Classpath Variables.
8. Click New. This will display the New Variable Entry dialog. Enter M2_REPO in the Name field, and the path to your Maven repository in the Path field. The default path is <USER_HOME>/.m2/repository, where <USER_HOME> is your home directory.
9. Click OK

You can then run the sample application from Eclipse as follows:

1. In the Package Explorer, expand src/main/java until you see SampleTool.java.
2. Right-click SampleTool.java and select Run As->Run Configurations.
3. Select the Arguments tab.
4. Enter the arguments for the command in the Program arguments field. Here's what the arguments to list a user's quota information might look like:

  ```
-user user@email.com -password userpw -application /sc/10098/4_235123 -accesskey  MTUzOTEyNjEzMjM4NzEwNDg0MTc
-privatekey ZmNhMWY2MTZlY2M1NDg4OGJmZDY4OTExMjY5OGUxOWY quota
  ```

  Remember to enter your actual user ID and password as well as the app ID, access key, and private key that you were issued by SugarSync.
5. Click Run

### Examining the Application Structure

If you navigate down through the src folder to the sample folder, you'll notice that the application source code is structured into the following folders:

* auth. Contains the following authorization-related classes:
  * AccessToken.java, a class that creates an access token.
  * RefreshToken.java, a class that creates a refresh token.
* file. Contains the following classes for file-related requests:
  * FileCreation.java, a class that creates a file representation in the Magic Briefcase.
  * FileDownloadAPI.java, a class that downloads a file.
  * FileUploadAPI.java, a class that uploads a file.
* tool. Contains SampleTool.java, the main class for the sample application. It initially processes the incoming command and calls other classes to perform additional operations such as making requests to the SugarSync API.
* userinfo. Contains UserInfo.java, a class that retrieves information about the SugarSync user.
* util. Contains the following utility files:
  * HttpResponse.java, a Java bean class used to contain an HTTP response.
  * SugarSyncHTTPGetUtil.java, a class used for making HTTP GET requests.
  * XmlUtilloadAPI.java, a class that performs various XML operations, such as formatting an unformatted XML file, and transforming an XML string.

You'll learn more about these files in the example pages, such as [Getting Authorization](get-auth-token-example.md) and Getting Information [About a User](get-user-info-example.md).

### What's Next?

As you examine the source code, you'll see how some key tasks are performed using the Platform API. These include:

* [Getting authorization](get-auth-token-example.md)
* [Getting information about a user](get-user-info-example.md)
* [Listing the contents of a folder](get-folder-info-example.md)
* [Downloading a file](download-file-example.md)
* [Creating a file](create-file-example.md)
* [Uploading data to a file](upload-file-data-example.md)

Each of these tasks will be covered in detail in the example pages that follow. Because authorization is needed to access any resource through the Platform API, learning [how to get authorization](get-auth-token-example.md) is a good place to start. So that's the first of these tasks we'll cover.

---

© 2014 SugarSync, Inc. All Rights Reserved.  [API Terms of Use](/source/dev/terms.md)
