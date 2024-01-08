+++
title = "发布包"
date = 2024-01-05T20:29:36+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tools/pub/publishing](https://dart.dev/tools/pub/publishing)

## Publishing packages 发布软件包

[The pub package manager](https://dart.dev/guides/packages) isn’t just for using other people’s packages. It also allows you to share your packages with the world. If you have a useful project and you want others to be able to use it, use the `dart pub publish` command.

​	pub 软件包管理器不仅仅用于使用其他人的软件包。它还允许您与全世界共享您的软件包。如果您有一个有用的项目并且希望其他人能够使用它，请使用 `dart pub publish` 命令。

*info* **Note:** To publish to a location other than pub.dev, or to prevent publication anywhere, use the `publish_to` field, as defined in the [pubspec](https://dart.dev/tools/pub/pubspec).

​	注意：要发布到 pub.dev 以外的位置，或防止在任何地方发布，请使用 `publish_to` 字段，如 pubspec 中定义的那样。

Watch the following video for an overview of building and publishing packages.

​	观看以下视频，了解构建和发布软件包的概述。

<iframe width="560" height="315" src="https://www.youtube.com/embed/8V_TLiWszK0" title="Learn how to build and publish Dart packages" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" loading="lazy" style="box-sizing: border-box; color: rgb(33, 33, 33); font-family: &quot;Google Sans Text&quot;, Roboto, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: left; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>



## Publishing is forever 发布是永久的

Keep in mind that publishing is forever. As soon as you publish your package, users can depend on it. Once they start doing that, removing the package would break theirs. To avoid that, the [pub.dev policy](https://pub.dev/policy) disallows unpublishing packages except for very few cases.
请记住，发布是永久的。一旦您发布软件包，用户就可以依赖它。一旦他们开始这样做，删除软件包就会破坏他们的软件包。为了避免这种情况，pub.dev 政策禁止取消发布软件包，除非极少数情况。

You can always upload new versions of your package, but old ones will continue to be available for users that aren’t ready to upgrade yet.
您始终可以上传软件包的新版本，但旧版本将继续可供尚未准备好升级的用户使用。

For already published packages that are no longer relevant or being maintained, you can [mark them as discontinued](https://dart.dev/tools/pub/publishing#discontinue).
对于已发布但不再相关或不再维护的软件包，您可以将它们标记为已停用。

## Preparing to publish 准备发布

When publishing a package, it’s important to follow the [pubspec format](https://dart.dev/tools/pub/pubspec) and [package layout conventions](https://dart.dev/tools/pub/package-layout). Some of these are required in order for others to be able to use your package. Others are suggestions to help make it easier for users to understand and work with your package. In both cases, pub tries to help you by pointing out what changes will help make your package play nicer with the Dart ecosystem. There are a few additional requirements for uploading a package:
在发布软件包时，遵循 pubspec 格式和软件包布局约定非常重要。其中一些是必需的，以便其他人能够使用您的软件包。其他一些是建议，以帮助用户更轻松地理解和使用您的软件包。在这两种情况下，pub 会尝试通过指出哪些更改将有助于使您的软件包与 Dart 生态系统更好地配合来帮助您。对于上传软件包，还有一些其他要求：

- You must include a `LICENSE` file. We recommend the [BSD 3-clause license](https://opensource.org/licenses/BSD-3-Clause), which the Dart and Flutter teams typically use. However, you can use any license that’s appropriate for your package. You must also have the legal right to redistribute anything that you upload as part of your package.
  您必须包含 `LICENSE` 文件。我们推荐 BSD 3 条款许可证，Dart 和 Flutter 团队通常使用该许可证。但是，您可以使用任何适合您软件包的许可证。您还必须有权重新分发作为软件包一部分上传的任何内容。
- Your package must be smaller than 100 MB after gzip compression. If it’s too large, consider splitting it into multiple packages, using a `.pubignore` file to remove unnecessary content, or cutting down on the number of included resources or examples.
  您的软件包在 gzip 压缩后必须小于 100 MB。如果太大，请考虑将其拆分为多个软件包，使用 `.pubignore` 文件删除不必要的内容，或减少包含的资源或示例的数量。
- Your package should depend only on hosted dependencies (from the default pub package server) and SDK dependencies (`sdk: flutter`). These restrictions ensure that dependencies of your packages cannot become unavailable in the future.
  您的软件包应仅依赖托管的依赖项（来自默认的 pub 软件包服务器）和 SDK 依赖项（ `sdk: flutter` ）。这些限制可确保您的软件包的依赖项在将来不会变得不可用。
- You must have a [Google Account](https://support.google.com/accounts/answer/27441), which pub uses to manage package upload permissions. Your Google Account can be associated with a Gmail address or with any other email address.
  您必须拥有一个 Google 帐号，pub 使用该帐号来管理软件包上传权限。您的 Google 帐号可以与 Gmail 地址或任何其他电子邮件地址相关联。

### Important files 重要文件

Pub uses the contents of a few files to create a page for your package at `pub.dev/packages/<your_package>`. Here are the files that affect how your package’s page looks:
Pub 使用几个文件的内容在 `pub.dev/packages/<your_package>` 为您的软件包创建一个页面。以下文件会影响软件包页面的外观：

- **README.md:** The `README.md` file is the main content featured in your package’s page. The file’s contents are rendered as [Markdown.](https://pub.dev/packages/markdown) For guidance on how to write a great README, see [Writing package pages](https://dart.dev/guides/libraries/writing-package-pages).
  README.md： `README.md` 文件是软件包页面中显示的主要内容。该文件的内容以 Markdown 形式呈现。有关如何编写出色自述文件的指导，请参阅编写软件包页面。
- **CHANGELOG.md:** Your package’s `CHANGELOG.md` file, if found, is also featured in a tab on your package’s page, so that developers can read it right from pub.dev. The file’s contents are rendered as [Markdown.](https://pub.dev/packages/markdown)
  CHANGELOG.md：如果找到软件包的 `CHANGELOG.md` 文件，它也会显示在软件包页面的某个标签中，以便开发者可以直接从 pub.dev 中阅读它。该文件的内容以 Markdown 形式呈现。
- **The pubspec:** Your package’s `pubspec.yaml` file is used to fill out details about your package on the right side of your package’s page, like its description, homepage, etc.
  pubspec：软件包的 `pubspec.yaml` 文件用于填写软件包页面右侧有关软件包的详细信息，例如其说明、主页等。

### Advantages of using a verified publisher 使用经过验证的发布者的优势

You can publish packages using either a verified publisher (recommended) or an independent Google Account. Using a verified publisher has the following advantages:
您可以使用经过验证的发布者（推荐）或独立的 Google 帐号发布软件包。使用经过验证的发布者具有以下优势：

- The consumers of your package know that the publisher domain has been verified.
  您软件包的使用者知道发布者域名已得到验证。
- You can avoid having pub.dev display your personal email address. Instead, pub.dev displays the publisher domain and contact address.
  您可以避免让 pub.dev 显示您的个人电子邮件地址。相反，pub.dev 会显示发布者域名和联系地址。
- The pub.dev site displays a verified publisher badge ![pub.dev verified publisher logo](./Publishingpackages_img/verified-publisher.svg+xml) next to your package name on search pages and individual package pages.
  pub.dev 网站会在搜索页面和各个软件包页面上，在您的软件包名称旁边显示一个已验证发布者徽章 ![pub.dev verified publisher logo](https://dart.dev/assets/img/verified-publisher.svg) 。

### 创建已验证发布者 Creating a verified publisher 

To create a verified publisher, follow these steps:

​	要创建已验证发布者，请按照以下步骤操作：

1. Go to [pub.dev.](https://pub.dev/)
2. 转到 pub.dev。
3. Log in to pub.dev using a Google Account.
4. 使用 Google 帐号登录 pub.dev。
5. In the user menu in the top-right corner, select **Create Publisher**.
6. 在右上角的用户菜单中，选择创建发布者。
7. Enter the domain name that you want to associate with your publisher (for example, `dart.dev`), and click **Create Publisher**.
8. 输入您要与您的发布者关联的域名（例如， `dart.dev` ），然后点击创建发布者。
9. In the confirmation dialog, select **OK**.
10. 在确认对话框中，选择确定。
11. If prompted, complete the verification flow, which opens the [Google Search Console.](https://search.google.com/search-console/about)
12. 如果出现提示，请完成验证流程，这会打开 Google Search Console。
    - When adding DNS records, it may take a few hours before the Search Console reflects the changes.
    - 添加 DNS 记录时，Search Console 可能需要几小时才能反映这些更改。
    - When the verification flow is complete, return to step 4.
    - 验证流程完成后，返回步骤 4。

## 发布您的软件包 Publishing your package 

Use the [`dart pub publish`](https://dart.dev/tools/pub/cmd/pub-lish) command to publish your package for the first time, or to update it to a new version.

​	使用 `dart pub publish` 命令首次发布您的软件包，或将其更新到新版本。

### 执行空运行 Performing a dry run 

To test how `dart pub publish` will work, you can perform a dry run:

​	要测试 `dart pub publish` 的工作方式，您可以执行空运行：

```
$ dart pub publish --dry-run
```

Pub makes sure that your package follows the [pubspec format](https://dart.dev/tools/pub/pubspec) and [package layout conventions](https://dart.dev/tools/pub/package-layout), and then uploads your package to [pub.dev.](https://pub.dev/) Pub also shows you all of the files it intends to publish. Here’s an example of publishing a package named `transmogrify`:

​	Pub 会确保您的软件包遵循 pubspec 格式和软件包布局约定，然后将您的软件包上传到 pub.dev。Pub 还会向您显示它打算发布的所有文件。以下是如何发布名为 `transmogrify` 的软件包的示例：

```nocode
Publishing transmogrify 1.0.0
    .gitignore
    CHANGELOG.md
    README.md
    lib
        transmogrify.dart
        src
            transmogrifier.dart
            transmogrification.dart
    pubspec.yaml
    test
        transmogrify_test.dart

Package has 0 warnings.
```

### 发布 Publishing 

When you’re ready to publish your package, remove the `--dry-run` argument:

​	当您准备好发布您的软件包时，请删除 `--dry-run` 参数：

```
$ dart pub publish
```

*info* **Note:** The pub command currently doesn’t support publishing a new package directly to a verified publisher. As a temporary workaround, publish new packages to a Google Account, and then [transfer the package to a publisher](https://dart.dev/tools/pub/publishing#transferring-a-package-to-a-verified-publisher).

​	注意：pub 命令目前不支持直接将新软件包发布到已验证的发布者。作为临时解决方法，将新软件包发布到 Google 帐号，然后将软件包转移到发布者。

Once a package has been transferred to a publisher, you can update the package using `dart pub publish`.

​	软件包转移到发布者后，您可以使用 `dart pub publish` 更新软件包。

After your package has been successfully uploaded to pub.dev, any pub user can download it or depend on it in their projects. For example, if you just published version 1.0.0 of your `transmogrify` package, then another Dart developer can add it as a dependency in their `pubspec.yaml`:

​	软件包成功上传到 pub.dev 后，任何 pub 用户都可以下载该软件包或在项目中依赖该软件包。例如，如果您刚刚发布了 `transmogrify` 软件包的 1.0.0 版本，那么其他 Dart 开发人员可以将其作为依赖项添加到他们的 `pubspec.yaml` 中：

```
dependencies:
  transmogrify: ^1.0.0
```

### 自动发布 Automated publishing 

Once the first version of a package has been published, it is possible to configure automated publishing through GitHub Actions or Google Cloud service accounts. To learn more about automated publishing, see [Automated publishing of packages to pub.dev](https://dart.dev/tools/pub/automated-publishing).

​	发布软件包的第一个版本后，可以通过 GitHub Actions 或 Google Cloud 服务帐号配置自动发布。要详细了解自动发布，请参阅将软件包自动发布到 pub.dev。

### 将软件包转移到已验证的发布者 Transferring a package to a verified publisher 

To transfer a package to a verified publisher, you must be an [uploader](https://dart.dev/tools/pub/publishing#uploaders) for the package and an admin for the verified publisher.

​	要将软件包转移到已验证的发布者，您必须是该软件包的上传者，并且是已验证发布者的管理员。

*info* **Note:** This process isn’t reversible. Once you transfer a package to a publisher, you can’t transfer it back to an individual account.

​	注意：此过程不可逆。将软件包转移到发布者后，您无法将其转移回个人帐号。

Here’s how to transfer a package to a verified publisher:

​	如何将软件包转让给已验证的发布者：

1. Log in to [pub.dev](https://pub.dev/) with a Google Account that’s listed as an uploader of the package.
2. 使用列为软件包上传者的 Google 帐号登录 pub.dev。
3. Go to the package details page (for example, `https://pub.dev/packages/http`).
4. 转到软件包详情页面（例如， `https://pub.dev/packages/http` ）。
5. Select the **Admin** tab.
6. 选择“管理”标签。
7. Enter the name of the publisher, and click **Transfer to Publisher**.
8. 输入发布者的名称，然后点击“转让给发布者”。

## 哪些文件已发布？ What files are published? 

**All files** under the package root directory are included in the published package, with the following exceptions:

​	已发布软件包中包含软件包根目录下的所有文件，但以下文件除外：

- Any *hidden* files or directories—that is, files with names that begin with dot (`.`)
- 任何隐藏文件或目录，即名称以点号 ( `.` ) 开头的文件
- Files and directories ignored by a `.pubignore` or `.gitignore` file
- 由 `.pubignore` 或 `.gitignore` 文件忽略的文件和目录

If you want different ignore rules for `git` and `dart pub publish`, then overrule the `.gitignore` file in a given directory by creating a `.pubignore` file. (If a directory contains both a `.pubignore` file and a `.gitignore` file, then `dart pub publish` doesn’t read that directory’s `.gitignore` file.) The format of `.pubignore` files is the same as the [`.gitignore` file format](https://git-scm.com/docs/gitignore#_pattern_format).

​	如果您希望对 `git` 和 `dart pub publish` 使用不同的忽略规则，那么可以通过在给定目录中创建一个 `.pubignore` 文件来覆盖 `.gitignore` 文件。（如果目录同时包含 `.pubignore` 文件和 `.gitignore` 文件，那么 `dart pub publish` 不会读取该目录的 `.gitignore` 文件。） `.pubignore` 文件的格式与 `.gitignore` 文件格式相同。

To avoid publishing unwanted files, follow these practices:

​	为避免发布不需要的文件，请遵循以下做法：

- Either delete any files that you don’t want to include, or add them to a `.pubignore` or `.gitignore` file.
- 删除任何您不想包含的文件，或将它们添加到 `.pubignore` 或 `.gitignore` 文件中。
- When uploading your package, carefully examine the list of files that `dart pub publish` says it’s going to publish. Cancel the upload if any undesired files appear in that list.
- 上传软件包时，请仔细检查 `dart pub publish` 所述将要发布的文件列表。如果该列表中出现任何不需要的文件，请取消上传。

*info* **Note:** Most packages don’t need a `.pubignore` file. More information about useful scenarios for this can be found in this [StackOverflow answer](https://stackoverflow.com/a/69767697).

​	注意：大多数软件包不需要 `.pubignore` 文件。有关此类文件的有用场景的更多信息，请参阅此 StackOverflow 答案。

## 平台支持 Platform support 

The [pub.dev site](https://pub.dev/) detects which platforms a package supports, displaying these platforms on the package page. Users of pub.dev can filter searches by platform.

​	pub.dev 网站会检测软件包支持哪些平台，并在软件包页面上显示这些平台。pub.dev 的用户可以按平台过滤搜索结果。

To change the automatically generated list of supported platforms, [specify supported platforms](https://dart.dev/tools/pub/pubspec#platforms) in the pubspec.

​	要更改自动生成的受支持平台列表，请在 pubspec 中指定受支持的平台。

## 上传者 Uploaders 

Whoever publishes the first version of a package automatically becomes the first and only person authorized to upload additional versions of that package.

​	第一个发布软件包版本的人会自动成为唯一有权上传该软件包的其他版本的授权人员。

To allow or disallow other people to upload versions, either:

​	要允许或不允许其他人上传版本，请执行以下任一操作：

- Manage authorized uploaders on the admin page for the package: `https://pub.dev/packages/<package>/admin`.
- 在软件包的管理页面上管理授权上传者： `https://pub.dev/packages/<package>/admin` 。
- Transfer the package to a [verified publisher](https://dart.dev/tools/pub/verified-publishers); all members of a publisher are authorized to upload.
- 将软件包转让给经过验证的发布者；发布者所有成员均有权上传。

## 查找软件包发布者 Locating the package publisher 

If a package has a verified publisher, then the pub.dev page for that package displays the publisher domain.

​	如果软件包有经过验证的发布者，那么该软件包的 pub.dev 页面会显示发布者域名。

For packages published without a publisher, the publisher is not disclosed for privacy reasons (the Publisher field just says “unverified uploader”).

​	对于未经发布者发布的软件包，出于隐私原因，不会披露发布者（发布者字段只显示“未经验证的上传者”）。

## 发布预发行版 Publishing prereleases 

As you work on a package, consider publishing it as a prerelease. Prereleases can be useful when *any* of the following are true:

​	在处理软件包时，请考虑将其发布为预发行版。在满足以下任何条件时，预发行版可能很有用：

- You’re actively developing the next major version of the package.
- 您正在积极开发软件包的下一个主要版本。
- You want beta testers for the next release candidate of the package.
- 您想要为软件包的下一个候选版本寻找测试人员。
- The package depends on an unstable version of the Dart or Flutter SDK.
- 软件包依赖于不稳定的 Dart 或 Flutter SDK 版本。

As described in [semantic versioning](https://semver.org/spec/v2.0.0-rc.1.html), to make a prerelease of a version you append a suffix to the version. For example, to make a prerelease of version `2.0.0` you might use the version `2.0.0-dev.1`. Later, when you release version `2.0.0`, it will take precedence over all `2.0.0-XXX` prereleases.

​	如语义版本控制中所述，要制作某个版本的预发布版本，您需要在版本后追加一个后缀。例如，要制作版本 `2.0.0` 的预发布版本，您可以使用版本 `2.0.0-dev.1` 。稍后，当您发布版本 `2.0.0` 时，它将优先于所有 `2.0.0-XXX` 预发布版本。

Because pub prefers stable releases when available, users of a prerelease package might need to change their dependency constraints. For example, if a user wants to test prereleases of version 2.1, then instead of `^2.0.0` or `^2.1.0` they might specify `^2.1.0-dev.1`.

​	由于 pub 在可用时更喜欢稳定版本，因此预发布软件包的用户可能需要更改其依赖项约束。例如，如果用户想要测试版本 2.1 的预发布版本，那么他们可能会指定 `^2.1.0-dev.1` ，而不是 `^2.0.0` 或 `^2.1.0` 。

*info* **Note:** If a stable package in the dependency graph depends on a prerelease, then pub chooses that prerelease instead of a stable release.

​	注意：如果依赖关系图中的稳定软件包依赖于预发布版本，那么 pub 会选择该预发布版本，而不是稳定版本。

When a prerelease is published to pub.dev, the package page displays links to both the prerelease and the stable release. The prerelease doesn’t affect the analysis score, show up in search results, or replace the package `README.md` and documentation.

​	将预发布版本发布到 pub.dev 时，软件包页面会显示指向预发布版本和稳定版本的链接。预发布版本不会影响分析得分，不会显示在搜索结果中，也不会替换软件包 `README.md` 和文档。

## 发布预览版 Publishing previews 

Previews can be useful when **all** of the following are true:

​	当所有以下情况都为真时，预览版可能有用：

- The next stable version of the package is complete.
- 软件包的下一个稳定版本已完成。
- That package version depends on an API or feature in the Dart SDK that hasn’t yet been released in a stable version of the Dart SDK.
- 该软件包版本依赖于 Dart SDK 中尚未在稳定版本中发布的 API 或功能。
- You know that the API or feature that the package depends on is API-stable and won’t change before it reaches the stable SDK.
- 您知道软件包所依赖的 API 或功能是 API 稳定的，并且在达到稳定 SDK 之前不会更改。

As an example, consider a new version of `package:args` that has a finished version `2.0.0` but that depends on a feature in Dart `3.0.0-417.1.beta`, where Dart SDK version `3.0.0` stable hasn’t been released yet. The pubspec might look like this:

​	例如，考虑一个新版本的 `package:args` ，它具有已完成的版本 `2.0.0` ，但依赖于 Dart `3.0.0-417.1.beta` 中的功能，其中 Dart SDK 版本 `3.0.0` 稳定版尚未发布。pubspec 可能如下所示：

```
name: args
version: 2.0.0

environment:
  sdk: '>=3.0.0-417.1.beta <4.0.0'
```

When this package is published to pub.dev, it’s tagged as a preview version, as illustrated by the following screenshot, where the stable version is listed as `1.6.0` and the preview version is listed as `2.0.0`.

​	当此软件包发布到 pub.dev 时，它被标记为预览版本，如下面的屏幕截图所示，其中稳定版本列为 `1.6.0` ，预览版本列为 `2.0.0` 。

![Illustration of a preview version](./Publishingpackages_img/preview-version.png)

When Dart `3.0.0` stable is released, pub.dev updates the package listing to display `2.0.0` as the stable version of the package.

​	当 Dart `3.0.0` 稳定版发布时，pub.dev 会更新软件包列表以显示 `2.0.0` 作为软件包的稳定版本。

If all of the conditions at the beginning of this section are true, then you can ignore the following warning from `dart pub publish`:

​	如果本节开头的所有条件都为真，那么您可以忽略 `dart pub publish` 中的以下警告：

*“Packages with an SDK constraint on a pre-release of the Dart SDK should themselves be published as a pre-release version. If this package needs Dart version 3.0.0-0, consider publishing the package as a pre-release instead.”*

​	“对 Dart SDK 预发布版本具有 SDK 约束的软件包本身应作为预发布版本发布。如果此软件包需要 Dart 版本 3.0.0-0，请考虑将该软件包作为预发布版本发布。”*

## 撤回软件包版本 Retracting a package version 

To prevent new package consumers from adopting a recently published version of your package, you can retract that package version within 7 days of publication. The retracted version can be restored again within 7 days of retraction.

​	为了防止新软件包使用者采用最近发布的软件包版本，您可以在发布后 7 天内撤回该软件包版本。撤回的版本可以在撤回后 7 天内再次恢复。

A retracted package version isn’t deleted. It appears in the version listing of the package on pub.dev in the **Retracted versions** section. Also, the detailed view of that package version has a **RETRACTED** badge.

​	撤回的软件包版本不会被删除。它会显示在 pub.dev 上软件包的版本列表中，位于“已撤回版本”部分。此外，该软件包版本的详细视图有一个“已撤回”徽章。

Before retracting a package, consider publishing a new version instead. Retracting a package causes churn and can have a negative impact on package users.

​	在撤回软件包之前，请考虑发布新版本。撤回软件包会导致混乱，并可能对软件包使用者产生负面影响。

If you accidentally publish a new version with either a *missing dependency constraint* or a *dependency constraint that is too lax*, then retracting the package version might be the only solution. Publishing a newer version of your package is insufficient to stop the version solver from picking the old version, which might be the only version pub can choose. By retracting the package version that has incorrect dependency constraints, you force users to either upgrade other dependencies or get a dependency conflict.

​	如果您意外发布了一个新版本，其中缺少依赖项约束或依赖项约束过于宽松，那么撤回软件包版本可能是唯一的解决方案。发布软件包的较新版本不足以阻止版本解析器选择旧版本，这可能是 pub 可以选择的唯一版本。通过撤回具有不正确依赖项约束的软件包版本，您可以强制用户升级其他依赖项或获得依赖项冲突。

However, if your package merely contains a minor bug, then retraction is probably not necessary. Publishing a newer version with the bug fixed and a description of the fixed bug in `CHANGELOG.md` helps users to understand what happened. And publishing a newer version is less disruptive to package users.

​	但是，如果您的软件包仅包含一个次要错误，那么可能不需要撤回。发布已修复错误的较新版本，并在 `CHANGELOG.md` 中提供已修复错误的说明，可帮助用户了解发生了什么。发布较新版本对软件包用户的影响较小。

*merge_type* **Version note:** Package retraction was introduced in Dart 2.15. In pre-2.15 SDKs, the pub version solver ignores the retracted status.

​	版本说明：软件包撤回在 Dart 2.15 中引入。在 2.15 之前的 SDK 中，pub 版本解析器会忽略已撤回的状态。

### 如何使用已撤回的软件包版本 How to use a retracted package version 

If a package depends on a package version that later is retracted, it can still use that version as long as that version is in the dependent package’s `pubspec.lock` file. To depend on a specific version that’s already retracted, the dependent package must pin the version in the `dependency_overrides` section of the `pubspec.yaml` file.

​	如果某个软件包依赖于某个稍后被撤回的软件包版本，只要该版本在依赖软件包的 `pubspec.lock` 文件中，它仍然可以使用该版本。要依赖于某个已被撤回的特定版本，依赖软件包必须在 `pubspec.yaml` 文件的 `dependency_overrides` 部分固定该版本。

### 如何从已撤回的软件包版本迁移 How to migrate away from a retracted package version 

When a package depends on a package version that is retracted, there are different ways to migrate away from this version depending on what other versions are available.

​	当某个软件包依赖于某个已被撤回的软件包版本时，根据其他可用版本，有不同的方法可以从该版本迁移。

#### 升级到较新版本 Upgrade to a newer version 

In most cases a newer version has been published to replace the retracted version. In this case run `dart pub upgrade <package>`.

​	在大多数情况下，已发布较新版本来替换已撤回的版本。在这种情况下，运行 `dart pub upgrade <package>` 。

#### 降级到最新的未撤回版本 Downgrade to the newest non-retracted version 

If there is no newer version available, the best action might be to downgrade to the newest non-retracted version. There are two ways to get this version.

​	如果没有可用的较新版本，最佳操作可能是降级到最新的未撤回版本。有两种方法可以获取此版本。

The first way is by using [pub tool](https://dart.dev/tools/pub/cmd) commands:

​	第一种方法是使用 pub 工具命令：

1. Run `dart pub downgrade <package>` to get the lowest version of the specified package that matches the constraints in the `pubspec.yaml` file.
2. 运行 `dart pub downgrade <package>` 以获取与 `pubspec.yaml` 文件中的约束匹配的指定软件包的最低版本。
3. Run `dart pub upgrade <package>` to get the newest compatible and non-retracted version available.
4. 运行 `dart pub upgrade <package>` 以获取最新的兼容且未撤回的可用版本。

The second way is by editing the `pubspec.lock` file manually:

​	第二种方法是手动编辑 `pubspec.lock` 文件：

1. Delete the entire package entry for the package with the retracted version.
2. 删除已撤回版本的软件包的整个软件包条目。
3. Run `dart pub get` to get the newest compatible and non-retracted version available.
4. 运行 `dart pub get` 以获取可用的最新兼容且未撤回的版本。

It is also possible to completely delete the `pubspec.lock` file and then run `dart pub get`. However, this might also result in version changes for other dependencies.

​	也可以完全删除 `pubspec.lock` 文件，然后运行 `dart pub get` 。但是，这也可能导致其他依赖项的版本发生变化。

#### 升级或降级到指定版本约束之外的版本 Upgrade or downgrade to a version outside the specified version constraint 

If there is no alternative version available that satisfies the current version constraint, edit the version constraint in the `pubspec.yaml` file and run `dart pub upgrade`.

​	如果没有可用的替代版本满足当前版本约束，请编辑 `pubspec.yaml` 文件中的版本约束并运行 `dart pub upgrade` 。

### 如何撤回或恢复软件包版本 How to retract or restore a package version 

To retract or restore a package version, first sign in to pub.dev using a Google Account that’s either an uploader or a [verified publisher](https://dart.dev/tools/pub/verified-publishers) admin for the package. Then go to the package’s **Admin** tab, where you can retract or restore recent package versions.

​	要撤回或恢复软件包版本，请首先使用 Google 帐号登录 pub.dev，该帐号是软件包的上传者或经过验证的发布者管理员。然后转到软件包的“管理员”标签，您可以在其中撤回或恢复最近的软件包版本。

## 将软件包标记为已停用 Marking packages as discontinued 

Although packages always remain published, it can be useful to signal to developers that a package is no longer being actively maintained. For this, you can mark a package as **discontinued**. A discontinued package remains published and viewable on pub.dev, but it has a clear **DISCONTINUED** badge and doesn’t appear in pub.dev search results.

​	尽管软件包始终保持发布状态，但向开发者发出软件包不再积极维护的信号可能很有用。为此，您可以将软件包标记为已停产。已停产的软件包仍会在 pub.dev 上发布并可供查看，但它有一个清晰的已停产徽章，并且不会出现在 pub.dev 搜索结果中。

To mark a package as discontinued, first sign in to pub.dev using a Google Account that’s either an uploader or a [verified publisher](https://dart.dev/tools/pub/verified-publishers) admin for the package. Then go to the package’s **Admin** tab, where you can mark the package as discontinued. If you change your mind, you can remove the discontinued mark at any time.

​	要将软件包标记为已停产，首先使用作为软件包的上传者或经过验证的发布者管理员的 Google 帐号登录 pub.dev。然后转到软件包的“管理”标签，您可以在其中将软件包标记为已停产。如果您改变主意，可以随时取消已停产标记。
