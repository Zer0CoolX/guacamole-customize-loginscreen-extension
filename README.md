# Guacamole Extension to Customize/Brand the Login Screen
This Apache Guacamole extension, in the form of a .jar file, is meant to act as a template for customizing or branding the Guacamole login screen to utilize different colors, wording and/or logo. As is, it has a default placeholder logo, wording and colors. These can be updated within the .jar file without needing to buid the whole extension from scratch.

# Examples of Guacamole Login Pages
### Default (not using this extension)
![default-login.png](https://github.com/Zer0CoolX/guacamole-customize-loginscreen-extension/blob/master/demo-resource/guac-default-login.PNG)
### Using this extension unchanged (branding.jar)
![custom-unchanged.png](https://github.com/Zer0CoolX/guacamole-customize-loginscreen-extension/blob/master/demo-resource/guac-cust-unchanged.PNG)

## Why use this extension vs x, y or z other options?
Other options may be perfectly valid. The benefit of using an extension (this one or another) to accomplish customizing the appearence of the login page in Apache Guacamole is that it should persist through updates/upgrades and can be easily re-implemented on additional Guacamole servers or in the event of needing to re-deploy an Apache Guacamole server with the same customized login screen. It is also easier to remove the customization if its no longer needed.

## Warnings
I highly recommend trying this in a test environment prior to attempting to use it in production. Its potential impact on an existing Apache Guacamole server should be nominal and easily reverible but it's better to be safe than sorry. Additionally, having a Guacamole server with the default login screen could prove helpful for comparisions and testing.

It would also be beneficial to check [the wiki in this repo](https://github.com/Zer0CoolX/guacamole-customize-loginscreen-extension/wiki) for detailed information on how this extension works, links to additional resources, etc. prior to attempting to use the Guacamole branding extension.

I can __NOT__ help with modifying the extension. As such follow the readme, wiki post(s) and the thread the wiki links to for guidance to get started.

__BEFORE ALL ELSE__ - attempt to load the extension __AS IS__, unmodified, to ensure it works __BEFORE__ trying to modify it.

## Downloading the extension
This should be as simple as:
```
wget https://github.com/Zer0CoolX/guacamole-customize-loginscreen-extension/raw/master/branding.jar
```
Just take note of where it downloads to and its file name so you may easily find, modify and eventually place it in the proper place to implement it on the Guacamole server

## How-to modify the extension to customize the Apache Guacamole login screen
I would first recommend looking at the default login page and using the developer tools in your browser to inspect the elements and CSS of the page. You can make tweaks using these tools and see the changes live without needing to commit to settings, alter the extension and implement the extension in Guacamole to see the results. Record your desired settings while doing so and use that as a guide when modifying the extension to your needs.

The `branding.jar` file is basically a container type file like a .zip archive, which means it contains files. You should be able to use archive manager programs like 7-zip to open the .jar file and view/edit the files contained within it.

The internal file structure is basically:
* css (folder) -> .css (file)
* images (folder) -> logo file as .png
* translations (folder) -> en.json (file) and other language files as needed
* guac-manifest.json (file) which ties all the parts together so Guacamole knows what to use.

The background colors, logo image used and other cosmetic changes to the page are implemented changing the stylesheets in the `.css` file. 

The images folder just contains the logo file. This has only been tested with a .png file. 

The .json file(s) in the translations folder dictate what text is displayed for the title (and according to what language). 

Lastly the guac-manifest.json basically ties all those parts together so Guacamole can use them to customize the login screen to the desired specifications.

## Steps to implement the extension to customize the Apache Guacamole login page
Once the branding.jar extension is updated to the desired parameters, it needs to be placed in the extensions folder used by Guacamole. This may differ according to Distro, method of installation and unique choices made when configuring the Apache Guacamole server:

* On RHEL/CentOS 7.x implementations the branding.jar file should then be placed in `/var/lib/guacamole/extensions`.
* On Debian 9 (Stretch/stable) implementations the branding.jar file should then be placed in  `/etc/guacamole/extensions/`.

If using SELinux, you will likely need to set the proper context on the extension for it to work. As a baseline example this may be something like:
```
semanage fcontext -a -t tomcat_exec_t "/var/lib/guacamole/extensions/branding.jar"
restorecon -v "/var/lib/guacamole/extensions/branding.jar"
```
After doing so restart at least the guacamole service:

* On RHEL/CentOS 7.x and up : `systemctl restart guacd` 
* On Debian 9 (Stretch/stable): `service guacd restart`

It might not be a bad idea to restart Tomcat, MariaDB/MySQL, Nginx and guacd as well.

Ensure the browser hasn't cached the old login page, if so clear the cache or try another method of accessing the site not previously used (another browser or another device).

If the parameters within the branding.jar are correct and the extension is in the proper directory the changes should be aparent on the login screen.

## Conclusion
This extension is meant to serve as a template for those looking to customize or brand the login page of an Apache Guacamole server. It does not alter any other aspect of the server. It does require a user to modify it to their needs which requires some base understandings like working with archive files, base CSS know-how, a working Apache Guacamole server (or the ability to set one up), etc.

If newly implementing a Guacamole server from scratch on RHEL/CentOS 7.x or up, may I recommend an installation script I have written to help more easily do so with additional options for things like LDAP, SSL, etc. including support to install your custom extension (your hopefully modified branding.jar file from this repo). This script can be found at:
https://github.com/Zer0CoolX/guacamole-install-rhel
