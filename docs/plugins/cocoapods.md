---
title: Building Plugins That Use CocoaPods
description: Learn how to build NativeScript plugins that use CocoaPods on iOS, including how to install CocoaPods, and how to configure your apps to use them.
position: 40
slug: cocoapods
---

# Building Plugins That Use CocoaPods

When you develop for iOS, you can quickly add third-party libraries to your NativeScript projects via the [CocoaPods](https://cocoapods.org/) dependency manager.

To work with such libraries, you need to wrap them as a custom NativeScript plugin and add them to your project.

## Install CocoaPods
You need to install CocoaPods. If you haven't yet, you can do so by running:

```bash
$ sudo gem install cocoapods
```
> **NOTE:** The minimum required version of CocoaPods is 1.0.0.

To check your current version, run the following command.

```bash
$ pod --version
```

To update CocoaPods, just run the installation command again.

```
sudo gem install cocoapods
```

## Create CLI Project
To start, create a project and add the iOS platform.

```bash
$ tns create MYCocoaPodsApp
$ cd MYCocoaPodsApp
$ tns platform add ios
```

## Wrap the Library as NativeScript Plugin

For more information about working with NativeScript plugins, click [here](plugins.md).

```Bash
cd ..
mkdir my-plugin
cd my-plugin
```

Create a `package.json` file with the following content:

```JSON
{
  "name": "my-plugin",
  "version": "0.0.1",
  "nativescript": {
    "platforms": {
      "ios": "1.3.0"
    }
  }
}
```

Create a [Podfile](https://guides.cocoapods.org/syntax/podfile.html) which describes the dependency to the library that you want to use. Move it to the `platforms/ios` folder.

```
my-plugin/
├── package.json
└── platforms/
    └── ios/
        └── Podfile
```

Podfile:
```
pod 'GoogleMaps'
```

## Install the Plugin

Next, install the plugin:

```bash
tns plugin add ../my-plugin
```

> **NOTE:** Installing CocoaPods sets the deployment target of your app to iOS 8, if not already set to iOS 8 or later. This change is required because CocoaPods are installed as shared frameworks to ensure that all symbols are available at runtime.

## Build the Project

```bash
tns build ios
```

This modifies the `MYCocoaPodsApp.xcodeproj` and creates a workspace with the same name.

> **IMPORTANT:** You will no longer be able to run the `xcodeproj` alone. NativeScript CLI will build the newly created workspace and produce the correct package.

## Troubleshooting

In case of post-build linker errors, you might need to resolve missing dependencies to native frameworks required by the installed CocoaPod. For more information about how to create the required links, see the [build.xcconfig specification](./plugin-reference#buildxcconfig-specification).