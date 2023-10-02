---
title: "Shielding Your Android App"
seoTitle: "Securing Your Android App: Tips and Tricks for Developers"
seoDescription: "Android Mobile Security Strategies (VAPT)"
datePublished: Sun Oct 01 2023 07:27:00 GMT+0000 (Coordinated Universal Time)
cuid: cln752k2d000009mk34pnbtel
slug: shielding-your-android-app
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1696070887226/34296f35-1016-440d-ad06-c57508f038e0.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1696145112436/cb0fe348-bdf2-47a9-a763-d837ac35c921.jpeg
tags: android-app-development, opensource, android, vapt, android-app-security

---

Hi Guys! Do you recall learning in biology class about cells being the fundamental unit of life, with both structural and functional properties? Well, in today's world, mobile phones have become just as essential and fundamental to our daily lives.

However, with the rising number of cyber threats targeting mobile devices, developers must take measures to protect and safeguard our apps.

In this blog, we will learn about the VAPT security guidelines for Android and protecting the Android client ecosystem.

---

### **What is VAPT?**

Vulnerability Assessment and Penetration Testing (VAPT) for Android is like checking if your Android phone or tablet has any security problems and fixing them.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696072399959/1e95037f-d38e-41e8-81fa-8dd7dafff0f0.jpeg align="center")

### **For better understanding let's take one example**

Imagine your Android device is like a house, and VAPT is like checking if all the doors and windows are locked properly and if there are any hidden ways for bad people to get in. If we find any problems, we make sure to fix them so your device stays safe.

VAPT involves looking for things like weak spots in the software, unsafe ways your device talks to the internet, or any other issues that could make it easy for hackers to do bad things. It's all about making your Android device more secure and less vulnerable to cyber threats.

---

### Here are the common VAPT security guidelines for Android

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696077820601/432f766a-3dbe-4c9f-b1d8-40799307ad50.png align="center")

### Root Detection

Developers need to ensure that their apps can detect this security vulnerability. Hackers often use root devices or take advantage of devices that have been rooted by the owner to gain access to sensitive user data and other secrets stored in an app's source code

**Why is a rooted device potentially dangerous to users/apps?**

System security and safeguards cannot be guaranteed after the root. At root, device data is at risk, including gaining access to personal information such as contact lists, emails, and other data, or collecting data like credentials and passwords. With a rooted device, a user or malicious program can elevate their permissions to root and circumvent this protection giving them access to other apps’ private data.

So it is the best way to check in your application whether the device is rooted or not to avoid data theft.

### Solutions

### Free:

There are several open-source solutions for checking for root, such as RootBeer, AntiMagisk, JailMonkey, and others, but the solutions are not 100% effective since third-party apps, such as Magisk, can bypass the root detection.

Because all of these solutions are client-side implementations, the attacker can bypass the root detection with the Magisk app, which hides the root files of the selected app, and there is a tool called Frida available that brute forces code, such as updating boolean conditions during runtime.

[https://gist.github.com/Ashutoshwahane/b0b2eca58d21857e006401d9f58c051a](https://gist.github.com/Ashutoshwahane/b0b2eca58d21857e006401d9f58c051a)

[https://github.com/GantMan/jail-monkey](https://github.com/GantMan/jail-monkey)

[https://github.com/scottyab/rootbeer](https://github.com/scottyab/rootbeer)

[https://stackoverflow.com/questions/18716808/how-to-check-usb-debugging-enabled-programmatically](https://stackoverflow.com/questions/18716808/how-to-check-usb-debugging-enabled-programmatically)

### Paid:

Play Integrity is a suite of tools and services that helps developers protect their apps and games from abuse and attacks. It provides a variety of signals and features that can be used to detect and prevent unauthorized access, cheating, fraud, and other malicious activity.

Developers can use Play Integrity in a variety of ways. For example, they can use the Play Integrity API to:

* Check that the app is running on a genuine Android device.
    
* Check that the app has not been tampered with.
    

It is the best way to secure the app because of server-side validation.

Official Documentation: [https://developer.android.com/google/play/integrity](https://developer.android.com/google/play/integrity)

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696077959416/3baff330-ace8-49a0-b651-3a1418a1ba08.jpeg align="center")

### Code obfuscation

Code obfuscation is a security technique that makes it more difficult for attackers to reverse engineer and understand your code. This can help to protect your app from being hacked or tampered with.

There are several different ways to obfuscate code for Android apps. One popular tool is ProGuard, which is a free and open-source tool that can be used to obfuscate Java code. ProGuard can rename classes, methods, and variables, and it can also remove unused code and resources.

In Simple words, Code Obfuscation is the process of converting the actual source code to unreadable and ununderstandable code.

How to obfuscate the code in app-level build.gradle file

```kotlin
android {
    buildTypes {
        getByName("release") {
            // Enables code shrinking, obfuscation, and optimization for only
            // your project's release build type. Make sure to use a build
            // variant with `isDebuggable=false`.
            isMinifyEnabled = true

            // Enables resource shrinking, which is performed by the
            // Android Gradle plugin.
            isShrinkResources = true

            // Includes the default ProGuard rules files that are packaged with
            // the Android Gradle plugin. To learn more, go to the section about
            // R8 configuration files.
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    ...
}
```

For more detail please refer to the official documentation: [https://developer.android.com/build/shrink-code](https://developer.android.com/build/shrink-code)

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696078091245/5db2a0d9-bd56-4c6f-b733-2cfb5f8d1e31.png align="center")

### Hardcoded Keys

Never hard code the API keys and other sensitive information that are essential for many Android apps. However, it is important to keep these keys safe, as they can be used to access your app's data or resources. One way to do this is to store the keys in a [local.properties](http://local.properties/) file.

The [local.properties](http://local.properties) file is a hidden file that is located in the root directory of your Android project. It is not included in version control systems(It should be added in the git ignore file), so it will not be shared with others when you commit your code.

To store an API key in [local.properties](http://local.properties), simply add a line with the key name and value. For example:

```kotlin
API_KEY=YOUR_API_KEY
```

```kotlin
// build.gradle.kts
        val apiKey = gradleLocalProperties(rootDir).getProperty("API_KEY")
        buildConfigField("String", "API_KEY", apiKey)
```

Clean and Re-Build the project

```kotlin
//access the BuildConfig the API key field anywhere in the module and It gets generated in the compile time
val apiKey = BuildConfig.API_KEY
```

* Do not hard-code the key value in your code. This would make it easier for someone to steal the key.
    
* Keep the [local.properties](http://local.properties) file secure. Do not share it with anyone who does not need to have access to it.
    

To make it more secure, we can fetch the keys from the backend server during runtime.

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696078232005/b8181e8f-4d5d-4b24-a4bd-5728993e6eea.png align="center")

### **Encrypted shared preference and Local Storage**

DataStore is a new and improved data storage solution aimed at replacing SharedPreferences. Built on Kotlin coroutines and Flow, DataStore provides two different implementations: Proto DataStore, which stores typed objects (backed by protocol buffers), and Preferences DataStore, which stores key-value pairs.

But there is a problem, DataStore creates a **.preferences\_pb** file in the app’s internal storage. As of now, the data is stored in an unsecured manner. Hence, storing sensitive data with DataStore is not recommended.

![](https://t3650425.p.clickup-attachments.com/t3650425/e5749daa-5252-4d00-9989-491ea0d63068/image.png align="left")

This can be exposed in the debug APK easily using the Android Studio device explorer. but for signed or released APK we won't be able to see this due to Android security policy and permission.

Only root devices can expose the signed or released APK, so If we are able to achieve 100% root detection It will be resolved or else we can encryption and decryption for dataStore values.

Check out the official documentation to encrypt and decrypt the shared preference or Datastore.  
[https://developer.android.com/topic/security/data](https://developer.android.com/topic/security/data)

---

If you have any questions, comments, or suggestions, please don't hesitate to reach out. Your feedback is invaluable as I continue to explore and share information on topics that matter.

**Please subscribe to my blog for more Android development tips and tricks.**