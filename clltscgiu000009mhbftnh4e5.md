---
title: "Simplified Your Android Workflow"
seoTitle: "Manage Different Environment Build variants in Android"
datePublished: Sun Aug 27 2023 18:30:04 GMT+0000 (Coordinated Universal Time)
cuid: clltscgiu000009mhbftnh4e5
slug: simplified-your-android-workflow
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693196958423/e1f36f9c-0ef2-40ca-9b6e-c30e17f12fc6.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1693196296229/5608a3f0-188a-4205-850e-c1dc8c014814.jpeg
tags: android-app-development, android, kotlin, environment-variables, androiddevelopment-android-androiddeveloper-androidstudio-androiddev-androidapp-appdevelopment-androidapps-coding-kotlin-programming-iosdevelopment-programmer-webdevelopment-java-appdeveloper-mobileappdevelopment-androidgames-developer-androidonly-javaprogramming-mobiledevelopment-ios-androiddevelopers-daysofcode-androidography-flutter-iosdeveloper-softwaredeveloper-codinglife

---

### Why do we use Flavours | Android build variants?

Android build variants are a useful tool for developers who want to create multiple versions of their app from a single codebase. This can be helpful for various reasons, such as:

* Making separate app versions for different groups of users (e.g. free or paid)
    
* Developing different app versions for different stages (e.g. testing or final release)
    

One of the most common use cases for Android build variants is to build different versions of an app for different environments. For example, you can create a development version of your app with debug symbols enabled, making it easier to debug. Alternatively, you can create a production version of your app with debug symbols disabled for security reasons.

Finally, Android build variants can be utilized to test different features or configurations of the app. For instance, you can create a build variant that incorporates a new feature that is still undergoing testing and then deploy that build variant to a small group of users to receive feedback.

Different Use Cases for Android Build Flavors: A Quick Summary

* Free vs Paid
    
* Different language builds
    
* Development vs Production
    
* Different screen sizes and resolutions
    
* Different features and configurations
    

Enough Theory Let's start with a Practical Example (the given example is for the build.gradle.kts which is recommended by google)

1. Open the build.gradle file app level in your Android project.
    
2. In the `buildTypes` block, define two build types:`release` and `debug`.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693158616063/a491c07c-37ae-45ff-b45d-cca9d263ce61.png align="center")
    
3. For the `release` build type, set the following properties:
    
    * `debuggable` to `false`. This prevents you from debugging your app in Android Studio.
        
    * `minifyEnabled` to `true`. This means that your app will be minified, which makes it smaller and faster.
        
    * `proguardFiles` to an array of ProGuard files. These files will be used to obfuscate your app and remove unused code.
        
4. For the `debug` build type, set the following properties:
    
    1. `debuggable` to `true`. This allows you to debug your app in Android Studio.
        
    2. `minifyEnabled` to `false`. This means that your app will not be minified, which makes it easier to debug.
        
5. **Product Flavors:** Product flavors enable you to create different versions of your app for various use cases. For example, you might have flavors for "free" and "premium" versions of your app, each with unique features or branding. Each flavor can have its own resources, assets, and code.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693158978267/01ece80f-56bc-4184-8a07-ad4361cb6534.png align="center")
    

* I have created 4 different types of flavor ( for the different use cases )
    
    1. `dev`: This is the development build variant, with the application ID [`com.ashutoshwahane.dev`](http://com.ashutoshwahane.dev) and the version name suffix `_dev`. It also defines a constant named `TEST_DEV_API` that stores the API key for the development environment.
        
    2. `production`: This is the production build variant, with the application ID [`com.ashutoshwahane.prod`](http://com.ashutoshwahane.prod) and the version name suffix `_prod`. It also defines a constant named `TEST_PROD_API` that stores the API key for the production environment.
        
    3. `free`: This is the free build variant, with the application ID [`com.ashutoshwahane.free`](http://com.ashutoshwahane.free) and the version name suffix `_free`.
        
    4. `paid`: This is the paid build variant, with the application ID `com.ashutoshwahane.paid` and the version name suffix `_paid`.
        
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693159260549/729948f8-669a-4046-8095-0bd2208dd102.png align="center")
    
* On gradle.properties define your API key or any other config data. ( Bonus TIP: Add gradle.properties file to .gitIgnore to secure your API key )
    
* Once all the steps are done make sure to Re-Build your project.
    
* Now the keys are accessible anywhere in the app for ex:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693159997265/d1dcb3a9-a84c-4692-b67d-bce5a6ea41e4.png align="center")
    

The code `val api = BuildConfig.API_KEY` retrieves the API key for the current build variant in Kotlin. The `BuildConfig` class includes a static field named `API_KEY` that stores the API key for the current build variant. The `val`keyword declares a variable called `api`, which is initialized to the value of the `API_KEY` field.

1. Now open your Build Variants on Android Studio and you will see all the different flavors in release and debug variants.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693160481224/0f08f3a2-10ad-4c2e-a9a0-6880d947a3fc.png align="center")
    

No more changing the config manually just select the build variants and run the app

To learn more about build variants in Android Studio, please visit the official documentation: [https://developer.android.com/studio/build/build-variants](https://developer.android.com/studio/build/build-variants)

**If you have any questions about build variants, please feel free to leave a comment below. I hope this blog post was helpful. Thank you for reading!**

**Please subscribe to my blog for more Android development tips and tricks.**