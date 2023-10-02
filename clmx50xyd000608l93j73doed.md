---
title: "Automate Android Development Workflow with GitHub Actions"
seoTitle: "Mastering Android Development Workflow with GitHub Actions | Step-by-S"
seoDescription: "Elevate your Android app development efficiency using GitHub Actions. Explore a comprehensive step-by-step guide to automate your workflow."
datePublished: Sun Sep 24 2023 07:28:03 GMT+0000 (Coordinated Universal Time)
cuid: clmx50xyd000608l93j73doed
slug: automate-android-development-workflow-with-github-actions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1695469065476/e513a557-82fe-4d37-ab13-835f52a41ffe.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1695540356868/89073caf-0e29-40e6-a726-bf4ab17d71e3.jpeg
tags: android-app-development, github, android, ci-cd, github-actions-1

---

Did you know that GitHub Actions is a powerful platform that enables you to automate your software development workflows directly in your repository? With this continuous integration and continuous delivery (CI/CD) tool, you can create, share, and discover actions to perform any task you need, from CI/CD to customized workflows.

The possibilities of GitHub Actions are vast, including building and testing your code, deploying it to production, releasing new versions of your software, managing your infrastructure, and even sending notifications and reports. Plus, it's entirely free for public and open use.

### What is CICD?

CI stands for continuous integration, which is an automated process that builds, tests, and integrates code changes. This helps teams identify and fix bugs early on, improve software quality, and release software more quickly and reliably.

CD stands for continuous deployment, which is a delivery process that automatically deploys code changes that pass automated tests to production. This means we are not required to publish the app manually on Play Store and Firebase distribution.

**When used together, these processes are called CICD, and there are numerous open-source CICD tools available in the market**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695469982522/9b7987cd-de18-4514-9584-545ec69be607.png align="center")

### Let's continue with GitHub Actions!

GitHub Actions is a powerful tool that can help improve the quality and efficiency of your software development process. It is also user-friendly and easy to get started with, even if you're new to CICD.

Configure the repository with GitHub actions

* Create a GitHub repository for your Android project, if you don't already have one.
    
* Create a `.github` folder in the root directory of your repository.
    
* Inside the `.github` folder, create a `workflows` folder.
    
* Inside the `workflows` folder, create a new file with the `.yml` extension. For example, you could name it `main.yml`.
    
* In the `.yml` file, define your workflow steps
    

Bonus Tip 🤩: You can try the suggested workflow option and this will create a basic workflow to help you get started with Actions

Let's create and run the basic workflow just to understand the actions

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695471527780/e661ded0-2c92-410c-8d30-7a8e4ad16ef6.png align="center")

This workflow will run on every push to the `main` branch of your repository. It will first check out the code, and print the **Hello, world!** string

You can customize the workflow steps to meet your specific needs. For example, you could add steps to sign the APK, deploy the app to a test environment, or send notifications to your team members.

Once you have created the workflow file, push it to your repository. GitHub Actions will automatically start running the workflow for you.

[Checkout the official documentation to understand the workflow file and syntax](https://docs.github.com/en/actions/using-workflows/about-workflows#understanding-the-workflow-file)

## Let's automate some Android development stuff

### **Improve your code with lint checks**

The lint tool helps find poorly structured code that can impact the reliability and efficiency of your Android apps and make your code harder to maintain. It is strongly recommended that you correct any errors that lint detects before publishing your app.  Lint can help you clean up these issues.

YAML code to run the lint job

```yaml
name: CI
on:
  push:
    branches: [ "dev" ]
  pull_request:
    branches: [ "dev" ]
  workflow_dispatch:
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the code
        uses: actions/checkout@v2
      - name: Set up Java
        uses: actions/setup-java@v2
        with:
              distribution: "temurin"
              java-version: 17
      - name: Run Lint Test
        run: ./gradlew lintDebug
      - name: Upload html test report
        uses: actions/upload-artifact@v2
        with:
          name: lint.html
          path: app/build/reports/lint-results-debug.html
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695496598617/e0be4c19-2a6b-4afd-a4d7-5448f851fdc4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695496931545/c63409f0-c702-4e7f-bb55-f780d9edf875.png align="center")

**When we push or merge pull requests to the dev branch. GitHub actions automatically run the workflow.**

---

### **Build APK and Upload to Artifacts**

For immediate app testing and debugging, you can build a debug APK. The debug APK is signed with a debug key provided by the SDK tools.

To build a debug APK, using the command line `./gradlew assembleDebug` task:

YAML code to build the APK and upload it to Artifacts

```yaml
  name: Build APK
on:
  push:
    branches: [ "dev" ]
jobs:
  Build-APK:
    name: Running on latest ubuntu machine
    runs-on: ubuntu-latest
 steps:
      - name: Checkout the latest code
        uses: actions/checkout@v2
      - name: Set up Java
        uses: actions/setup-java@v2
        with:
          distribution: "temurin"
          java-version: 17
      - name: API_KEY
        env:
          API_KEY: ${{secrets.API_KEY}}
        run: echo API_KEY=\"$API_KEY\" > local.properties
      - name: Grant Permission to Execute
        run: chmod +x gradlew
      - name: Build debug APK
        run: bash ./gradlew assembleDebug --stacktrace
      - name: Upload APK to Github Artifacts
        uses: actions/upload-artifact@v3
        with:
          name: app
          path: presentation/build/outputs/apk/debug/presentation-debug.apk
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695534369025/75d2f8f1-ad46-461a-8939-20a09c754945.png align="center")

---

### **Local Unit Test**

A *local* test runs directly on your workstation, rather than an Android device or emulator. It uses your local Java Virtual Machine (JVM), rather than an Android device, to run tests. Local tests enable you to evaluate your app's logic more quickly

A *unit* test verifies the behaviour of a small section of code, the *unit under test*. It does so by executing that code and checking the result.

YAML code to run the local unit test case

```yaml
name: Unit Test
on:
  push:
    branches: [ "dev" ]
jobs:
  Build-APK:
    name: Running on latest ubuntu machine
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the latest code
        uses: actions/checkout@v2
      - name: Set up Java
        uses: actions/setup-java@v2
        with:
          distribution: "temurin"
          java-version: 17
      - name: API_KEY
        env:
          API_KEY: ${{secrets.API_KEY}}
        run: echo API_KEY=\"$API_KEY\" > local.properties
      - name: Grant Permission to Execute
        run: chmod +x gradlew
      - name: Run unit test case
        run: ./gradlew :domain:testDebugUnitTest --tests "dev.ashutoshwahane.domain.usecase.GetApodImageUseCaseTest"
```

---

### Code static analysis using MobSF

mobsfscan is a static analysis tool that can find insecure code patterns in your Android and iOS source code. Supports Java, Kotlin, Swift, and Objective C Code. mobsfscan uses MobSF static analysis rules and is powered by semgrep and libsast pattern matcher.

YAML code to run code static analysis

```yaml
name: Mobsfscan
on:
  push:
    branches: [ "dev" ]
  pull_request:
    branches: [ "dev" ]
jobs:
  mobsfscan:
    runs-on: ubuntu-latest
    name: mobsfscan code scanning
    steps:
      - name: Checkout the code
        uses: actions/checkout@v2
      - name: Set up Java
        uses: actions/setup-java@v2
        with:
          distribution: "temurin"
          java-version: 17
      - name: API_KEY
        env:
          API_KEY: ${{secrets.API_KEY}}
        run: echo API_KEY=\"$API_KEY\" > local.properties
      - name: mobsfscan
        uses: MobSF/mobsfscan@main
        with:
          args: '. --sarif --output results.sarif || true'
      - name: Upload mobsfscan report
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: results.sarif
```

[For more details please check the official documentation](https://github.com/MobSF/mobsfscan)

---

[Get the Source Code from here!](https://github.com/Ashutoshwahane/Cosmos-Compose)  
**Let me know in the comment section if you would like to know about publishing an app on the Play Store using GitHub actions ( CICD)**

If you have any questions, comments, or suggestions, please don't hesitate to reach out. Your feedback is invaluable as I continue to explore and share information on topics that matter.

**Please subscribe to my blog for more Android development tips and tricks.**