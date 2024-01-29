# WebJET CMS - demo

Demo project in WebJET CMS, contains integrated demo templates [bare](https://github.com/webjetcms/templates-bare), [creative](https://github.com/webjetcms/templates-creative) and MariaDB database backup for easier startup. It is based on the [basecms](https://github.com/webjetcms/basecms) project.

To get started, ask InterWay for access to the WebJET Maven repository and sample database backup. Rename the `gradle.sample.properties` file to `gradle.properties` and set the credentials for the WebJET Maven repository in it.

To open directly in VS Code, rename the file `.settings/default-org.eclipse.buildship.core.prefs` to `org.eclipse.buildship.core.prefs`. If VS Code shows you an error in the project, click on the `View/Command Palette` menu and enter: `Java: Clean Java Language Server Workspace` in the window. This will cause VS Code to restart and re-initialize the Java environment. After restarting, the project should not show you any red folder/error.

## Restoring the database

The sample database is for the MySQL/MariaDB server, unzip the downloaded database backup ZIP file. Create a new database schema with the following command (change the password value of course):

```sql
CREATE DATABASE democms_web DEFAULT CHARACTER SET utf8mb4 DEFAULT COLLATE utf8mb4_general_ci;
CREATE USER democms_web IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON democms_web.* TO `democms_web`@`%`;
FLUSH PRIVILEGES;
```

and refresh the database with the command:

```sh
mysql -u democms_web -p -h localhost democms_web < democms_web.sql
```

while setting instead `localhost` the address of your MariaDB server (if it is not running locally).

Set the entered password and server address in the `src/main/resources/poolman.xml` file.

Enter the license number you received from InterWay into the database with the following SQL command:

```sql
UPDATE _conf_ SET value='XXX' WHERE name='license';
```

## Starting the server

You start the application server with the command:

```
gradlew.bat appStart
```

Don't worry, if the `appStart` command keeps writing 92%, it is started after the `INFO Tomcat 9.0.52 started and listening on port 80` message is displayed, the server will run until you stop it via the `ctrl+C` keyboard shortcut.

After starting the server, open the administration in the browser at the address `http://localhost/admin`.

The templates have the mirroring of the structure set, if you want to automatically translate page names, set up [translator](http://docs.webjetcms.sk/v2022/#/admin/setup/translation).

## Integrated design templates

This project and database backup contains the integration of multiple design templates. You can base your project on them. They are all located in the `src/main/webapp/templates` folder.

### basecms

The basic template is still in the old JSP format, it is here only for demonstration and understanding of use in old projects. We recommend basing new projects exclusively on [Thymeleaf templates](http://docs.webjetcms.sk/v2022/#/frontend/thymeleaf/README).

### Bare

Basic template in Thymeleaf format using Bootstrap and npm modules. We recommend using it as a starting template for preparing your designs. It allows you to easily prototype changes even without running WebJET using the `npm run start` command.

More information in the [Bare template] documentation(http://docs.webjetcms.sk/v2022/#/frontend/examples/template-bare/README).

### Creative

A single page template based on Bare based on the [Start Bootstrap - Creative](https://startbootstrap.com/theme/creative) template.

Using/editing files is similar to [Bare template](http://docs.webjetcms.sk/v2022/#/frontend/examples/template-bare/README), it also includes [Font Awesome](https://fontawesome .com) integrated through the npm module.

## WebJET update

In the file [build.gradle](build.gradle) there is an `ext` section in which the version of WebJET CMS used in the project is set:

```javascript
ext {
     webjetVersion = "2022.0-SNAPSHOT";
}
```

in the preview it's version `2022.0-SNAPSHOT`, where `SNAPSHOT` means the latest version of the 2022 series. The latest version may always contain work in progress, so consider using it according to [changelist](http://docs. webjetcms.sk/v2022/#/CHANGELOG).

You can find a list of all available versions in the documentation in the [installation section](http://docs.webjetcms.sk/v2022/#/install/README).

If you are using the SNAPSHOT version, use the following command to reload the latest version from the Maven server:

```
gradlew.bat compileJava --refresh-dependencies --info
```