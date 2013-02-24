Tab bar for Cordova on iOS
==========================

This plugin lets you create and control a native tab bar.

License
-------

Parts that are not marked otherwise are licensed under the [MIT license](http://www.opensource.org/licenses/mit-license.html).

PhoneGap itself (parts of it included in this repository as test project dependency) is licensed under the [Apache License](http://phonegap.com/about/license/).

Contributors
------------

See TabBar.m for the history.

Versions
--------

Choose the right folder according to your Cordova version:

- 2.0.0 (not developed anymore) for Cordova 2.0.0
- 2.1.0 (not developed anymore), tested with Cordova 2.1.0 and 2.2.0
- 2.4.0, tested with Cordova 2.4.0 but should also work older versions (2.1.0 and newer)

Installing the plugin
---------------------

- Drag *.m and *.h files to your project's "Plugins" group in Xcode and choose to copy the files to the project
- Open "config.xml" and under "plugins", add a key with the plugin name "TabBar" and a string value of "TabBar"

Note regarding the tab bar
--------------------------

Don't forget to add an event handler for orientation changes as follows:

    window.addEventListener("resize", function() {
        var tabBar = cordova.require("cordova/plugin/iOSTabBar")
        tabBar.resize()
    ), false)

Using the tab bar and navigation bar plugin together
----------------------------------------------------

In order to use the [tab bar plugin](https://github.com/AndiDog/phonegap-plugins/tree/master/iOS/TabBar) and [navigation bar plugin](https://github.com/AndiDog/phonegap-plugins/tree/master/iOS/NavigationBar) together, you must initialize both plugins before calling any of their methods, i.e. before creating a navigation/tab bar. For example right when your application starts:

    document.addEventListener("deviceready", function() {
        console.log("Cordova ready")

        var tabBar = cordova.require("cordova/plugin/iOSTabBar")
        var navBar = cordova.require("cordova/plugin/iOSNavigationBar")

        navBar.init()
        tabBar.init()

        navBar.create()
        tabBar.create()

        // ...

This is because both plugins are aware of each other and resize Cordova's web view accordingly, but therefore they have to know the web view's initial dimensions. If for example you only initialize the tab bar plugin, create the tab bar and later decide to also create a navigation bar, the navigation bar plugin would think the original web view size is 320x411 instead of 320x460 (on iPhone). Layouting *could* be done using the screen size as well but it's currently implemented like this.

Example
-------

This example shows how to use the tab bar:

    document.addEventListener("deviceready", function() {
        console.log("PhoneGap ready")

        var tabBar = cordova.require("cordova/plugin/iOSTabBar")

        tabBar.init()

        tabBar.create()
        // or with an orange tint:
        tabBar.create({selectedImageTintColorRgba: "255,40,0,255"})

        tabBar.createItem("contacts", "Unused, iOS replaces this text by Contacts", "tabButton:Contacts")
        tabBar.createItem("recents", "Unused, iOS replaces this text by Recents", "tabButton:Recents")

        // Example with selection callback
        tabBar.createItem("another", "Some text", "/www/your-own-image.png", {
            onSelect: function() {
                alert("another tab selected")
            }
        })

        tabBar.show()
        // Or with custom style (defaults to 49px height, positioned at bottom): tabBar.show({height: 80, position: "top"})
        tabBar.showItems("contacts", "recents", "another")

        window.addEventListener("resize", function() { tabBar.resize() }, false)
    }, false)

Retina images
-------------

You can also have different images for the normal and retina quality like "image.png" and "image@2x.png". The code to assign the image would be:

    tabBar.createItem("home", "Home", "image.png", {
        onSelect: function() {
            alert("tab selected")
        }
    })

Known issues
------------

Layouting problems still to be solved.

Reporting issues or requests for improvement
--------------------------------------------

Please report problems on the [GitHub repository](https://github.com/AndiDog/phonegap-ios-tabbar-plugin/issues).