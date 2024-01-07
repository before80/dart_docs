+++
title = "Web 部署"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/web/deployment](https://dart.dev/web/deployment)

## Web deployment  - Web 部署

Deploying a Dart web app works like deploying any other web app. This page describes how to compile your app, tips for making it smaller and faster, and points you to resources for serving the app.

​	部署 Dart Web 应用与部署任何其他 Web 应用类似。此页面介绍如何编译您的应用，提供有关如何让应用更小、更快的提示，并向您介绍用于提供应用的资源。

## 构建您的应用 Building your app 

Use the `webdev` tool to build your app. It compiles Dart to JavaScript and generates all the assets you need for deployment. When you build using the production mode of the compiler, you get a JavaScript file that’s reasonably small, thanks to the compiler support for tree shaking.

​	使用 `webdev` 工具构建您的应用。它将 Dart 编译为 JavaScript 并生成您部署所需的所有资源。当您使用编译器的生产模式进行构建时，您会获得一个相当小的 JavaScript 文件，这要归功于编译器对 tree shaking 的支持。

With a little extra work, you can make your deployable app [smaller, faster, and more reliable](https://dart.dev/web/deployment#make-your-app-smaller-faster-and-more-reliable).

​	通过一些额外的工作，您可以让可部署的应用更小、更快、更可靠。

### 使用 webdev 编译 Compile using webdev 

[Use the `webdev build` command](https://dart.dev/tools/webdev#build) to create a deployable version of your app. This command converts your code to JavaScript and saves the result as `build/web/main.dart.js`. You can use [any option available to `dart compile js`](https://dart.dev/tools/dart-compile#prod-compile-options) with `webdev build`.

​	使用 `webdev build` 命令创建可部署的应用版本。此命令会将您的代码转换为 JavaScript，并将结果另存为 `build/web/main.dart.js` 。您可以使用 `dart compile js` 中可用的任何选项与 `webdev build` 一起使用。

### 让您的应用更小、更快、更可靠 Make your app smaller, faster, and more reliable 

The following steps are optional. They can help make your app more reliable and responsive.

​	以下步骤是可选的。它们可以帮助让您的应用更可靠、响应速度更快。

- [Use deferred loading to reduce your app’s initial size
  使用延迟加载来减小应用的初始大小](https://dart.dev/web/deployment#use-deferred-loading-to-reduce-your-apps-initial-size)
- [Follow best practices for web apps
  遵循适用于网络应用的最佳做法](https://dart.dev/web/deployment#follow-best-practices-for-web-apps)
- [Remove unneeded build files
  删除不必要的构建文件](https://dart.dev/web/deployment#remove-unneeded-build-files)

#### 使用延迟加载来减小应用的初始大小 Use deferred loading to reduce your app’s initial size 

You can use Dart’s support for deferred loading to reduce your app’s initial download size. For details, see the language tour’s coverage of [deferred loading](https://dart.dev/language/libraries#lazily-loading-a-library).

​	您可以使用 Dart 对延迟加载的支持来减小应用的初始下载大小。有关详细信息，请参阅语言之旅中有关延迟加载的内容。

####  遵循适用于网络应用的最佳做法 Follow best practices for web apps

The usual advice for web apps applies to Dart web apps. Here are a few resources:

​	适用于 Web 应用的常规建议也适用于 Dart Web 应用。以下是一些资源：

- [Fast load times](https://web.dev/fast/)
- 快速加载时间
- [Web Fundamentals](https://developers.google.com/web/fundamentals/) (especially [Optimizing Content Efficiency](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/))
- Web 基础知识（尤其是优化内容效率）
- [Progressive Web Apps](https://web.dev/progressive-web-apps/)
- 渐进式 Web 应用
- [Lighthouse](https://developers.google.com/web/tools/lighthouse/)

#### 删除不必要的构建文件 Remove unneeded build files 

Web compilers can produce files that are useful during development, such as Dart-to-JavaScript map files, but unnecessary in production.

​	Web 编译器可以生成在开发过程中有用的文件，例如 Dart 到 JavaScript 的映射文件，但在生产中却不需要。

To remove these files, you can run a command like the following:

​	要删除这些文件，您可以运行类似以下的命令：

```sh
# From the root directory of your app:
$ find build -type f -name "*.js.map" -exec rm {} +
```

## 提供您的应用 Serving your app 

You can serve your Dart Web app just like you’d serve any other web app. This section points to tips for serving Dart Web apps, as well as Dart-specific resources to help you use GitHub Pages or Firebase to serve your app.

​	您可以像提供任何其他 Web 应用一样提供您的 Dart Web 应用。本部分介绍了提供 Dart Web 应用的技巧，以及帮助您使用 GitHub Pages 或 Firebase 提供应用的 Dart 特定资源。

### GitHub Pages

If your app doesn’t use routing or require server-side support, you can serve the app using [GitHub Pages](https://pages.github.com/). The [peanut](https://pub.dev/packages/peanut) package is an easy way to automatically produce a gh-pages branch for any Dart web app.

​	如果您的应用不使用路由或不需要服务器端支持，您可以使用 GitHub Pages 提供应用。peanut 包是一种简单的方法，可自动为任何 Dart Web 应用生成 gh-pages 分支。

The [startup_namer example](https://filiph.github.io/startup_namer/) is hosted using GitHub Pages. Its files are in the **gh-pages** branch of the [filiph/startup_namer repo](https://github.com/filiph/startup_namer) and were built using [peanut.](https://pub.dev/packages/peanut)

​	startup_namer 示例使用 GitHub Pages 托管。它的文件位于 filiph/startup_namer 代码库的 gh-pages 分支中，并使用 peanut 构建。

### Firebase

To learn more about deploying with Firebase, see the following resources:

​	要详细了解如何使用 Firebase 部署，请参阅以下资源：

- The [Firebase Hosting documentation](https://firebase.google.com/docs/hosting/) describes how to deploy web apps with Firebase.
- Firebase 托管文档介绍了如何使用 Firebase 部署 Web 应用。
- In the Firebase Hosting documentation, [Configure Hosting Behavior](https://firebase.google.com/docs/hosting/full-config) covers redirects, rewrites, and more.
- 在 Firebase 托管文档中，“配置托管行为”介绍了重定向、重写等内容。
