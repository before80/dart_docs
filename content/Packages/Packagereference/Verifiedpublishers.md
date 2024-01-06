+++
title = "已验证发布者"
date = 2024-01-05T20:29:36+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tools/pub/verified-publishers](https://dart.dev/tools/pub/verified-publishers)

## Verified publishers 已验证发布者

The pub.dev verified publisher badge ![pub.dev verified publisher logo](./Verifiedpublishers_img/verified-publisher.svg+xml) lets you know that the pub.dev site verified the identity of the publisher of a package. For example, [dart.dev](https://pub.dev/publishers/dart.dev/) is the verified publisher for packages that Google’s Dart team supports.
pub.dev 已验证发布者徽章 ![pub.dev verified publisher logo](https://dart.dev/assets/img/verified-publisher.svg) 可让您知道 pub.dev 网站已验证软件包发布者的身份。例如，dart.dev 是 Google 的 Dart 团队支持的软件包的已验证发布者。

The badge appears in several places on pub.dev, next to packages that verified publishers published:
该徽章在 pub.dev 的多个位置显示，位于已验证发布者发布的软件包旁边：

- Package search results
  软件包搜索结果
- Package detail pages
  软件包详情页面
- Publisher profile pages
  发布者个人资料页面
- The pub.dev front page
  pub.dev 首页

Each publisher has a page with a list of all packages belonging to that publisher, plus additional details such as the publisher’s contact email. To visit the publisher page, click the publisher identity link (for example, `dart.dev`) next to the verified publisher badge ![pub.dev verified publisher logo](https://dart.dev/assets/img/verified-publisher.svg).
每个发布者都有一个页面，其中列出了属于该发布者的所有软件包，以及其他详细信息，例如发布者的联系方式电子邮件。要访问发布者页面，请点击已验证发布者徽章 ![pub.dev verified publisher logo](https://dart.dev/assets/img/verified-publisher.svg) 旁边的发布者身份链接（例如， `dart.dev` ）。

## Verification process 验证流程

To ensure that creating verified publishers is low cost and available to anyone, pub.dev relies on DNS (domain name system) domains as an identification token. We chose DNS verification because many package authors already have a trusted domain and a homepage for that domain. During the [publisher creation process](https://dart.dev/tools/pub/publishing#create-verified-publisher), pub.dev verifies that the user creating the verified publisher has admin access to the associated [“Domain Property”](https://support.google.com/webmasters/answer/34592), based on existing logic in the [Google Search Console.](https://search.google.com/search-console/about)
为了确保创建经过验证的发布者成本低且对所有人可用，pub.dev 依靠 DNS（域名系统）域作为标识令牌。我们选择 DNS 验证，因为许多软件包作者已经拥有受信任的域和该域的主页。在发布者创建过程中，pub.dev 会根据 Google Search Console 中的现有逻辑验证创建经过验证的发布者的用户是否具有关联的“域属性”的管理员访问权限。

## Creating a verified publisher account 创建经过验证的发布者帐号

If you publish packages and want to create a new verified publisher, see the instructions on the [publishing page](https://dart.dev/tools/pub/publishing#create-verified-publisher).
如果您发布软件包并希望创建新的经过验证的发布者，请参阅发布页面上的说明。
