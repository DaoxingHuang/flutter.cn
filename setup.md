---
layout: page
title: Flutter Setup
nav_title: Flutter Setup
permalink: /setup/
---

(Please remember, Flutter is in _technology preview_. Thanks
for trying it out, and please don't hesitate to
[file issues or bugs](https://github.com/flutter/flutter/issues/new)
for us.)

This guide describes how to set up your development environment to run Flutter apps on
iOS or Android.

* [System requirements](#system-requirements)
* [iOS setup](#ios-setup)
* [Android setup](#android-setup)
* [Get the Flutter SDK](#get-the-flutter-sdk)
* [Atom editor](#atom-editor)

## System requirements

To install and run Flutter, your development environment must meet these minimum requirements:

* **Operating Systems**: Mac or Linux (64-bit). Windows support is planned.
* **Tools**: Flutter depends on these command-line tools being available on your environment.
  * `bash`, `mkdir`, `rm`, `git`, `curl`, `unzip`

## iOS setup

_This step is optional, and can be performed at any time before
using Flutter to develop apps for iOS._

To develop Flutter apps for iOS, you need a Mac with Xcode 7.2 or newer:

1. Install Xcode 7.2 or newer (via [web download](https://developer.apple.com/xcode/) or
the [Mac App Store](https://itunes.apple.com/us/app/xcode/id497799835)).
2.  Make sure the Xcode EULA is signed by either opening Xcode once and confirming or
running `sudo xcodebuild -license` from the command-line.

With Xcode, you’ll be able to run Flutter apps on the iOS simulator.

### Set up the iOS simulator ###

To prepare to run and test your Flutter app on the iOS simulator, follow these steps:

1. On your Mac, find the Simulator via Spotlight or by using the following command:

    ```
    $ open -a Simulator
    ```

2. Make sure your simulator is using a 64-bit device (iPhone 5s or later) by checking the settings
in the simulator's **Hardware > Device** menu.
3. Depending on your development machine's screen size, simulated high-screen-density iOS devices
may overflow your screen. Set the device scale under the **Window > Scale** menu in the simulator.


### Deploy to iOS devices

To deploy your Flutter app to a physical iOS device, you’ll need some additional tools:

1. Install [homebrew](http://brew.sh/).
2. Open the terminal and run these command to install the toools for deploying Flutter apps to
iOS devices.

    <pre>
    $ brew tap flutter/flutter
    $ brew install ideviceinstaller ios-deploy
    </pre>

## Android setup

_This step is optional, and can be performed at any time before
using Flutter to develop apps for Android._

To develop Flutter apps for Android, you can use either a Mac or a Linux (64-bit) machine:

1. Install [Android Studio](https//developer.android.com/sdk/index.html).
2. From Android Studio, install the latest Android SDK and Android SDK Platform-Tools, as described
in [Adding SDK Packages](https://developer.android.com/sdk/installing/adding-packages.html).

### Set up your Android device

To prepare to run and test your Flutter app on Android, you'll need an Android device running
Android 4.1 (API level 16) or higher.

1. Enable developer mode on your device by visiting **`Settings > About phone`** and
tapping the **`Build number`** line seven times.
2. Enable **`Android debugging`** in **`Settings > Developer options`**.
3. Using a USB cable, plug your phone into your computer. If prompted on your
device, authorize your computer to access your device.
4. In the terminal, run the `flutter devices` command to verify that Flutter recognizes your
connected Android device.

By default, Flutter uses the version of the Android SDK where your `adb` tool is based. If
you want Flutter to use a different installation of the Android SDK, you must set the
`ANDROID_HOME` environment variable to that specific installation directory.

## Get the Flutter SDK

To get Flutter, use `git` to clone the repository and then add the `flutter` tool to your path:

<pre>
$ git clone https://github.com/flutter/flutter.git -b alpha
$ export PATH=`pwd`/flutter/bin:$PATH
</pre>

To complete the setup and to validate that you’ve set everything up correctly, run the following
command:

```
$ flutter doctor
```

The first time your run the `flutter` command, it will download its dependencies and compile
itself. Subsequent runs should be much faster.

## Atom editor

We recommend using [Atom](https://atom.io/) for editing, running, and debugging Flutter apps.
However, using our command-line tools, you can use any editor to develop Flutter applications.

Download Atom from [atom.io](https://atom.io/) and then install the
[Flutter package](https://atom.io/packages/flutter). If you need help installing
packages, consult the [Atom documentation](https://atom.io/docs/v1.3.2/using-atom-atom-packages)
or [email us](flutter-dev@googlegroups.com).

### Configure Atom for Flutter development

Before creating your Flutter project in Atom, you’ll need to perform these pre-flight tasks:

1. In the terminal window, run the `atom` command to launch the editor.
  ```
  $ atom
  ```
2. Open **Packages > Flutter > Package Settings**, and set the **FLUTTER_ROOT** field to the
root directory where you installed the Flutter SDK.
3. Open **Packages > Dart > Package Settings**, and set the **Dart SDK Location** field to the
`bin/cache/dart-sdk` directory in your Flutter SDK’s root folder.