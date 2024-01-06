+++
title = "Troubleshooting pub"
date = 2024-01-05T20:29:36+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tools/pub/troubleshoot](https://dart.dev/tools/pub/troubleshoot)

# Troubleshooting pub 排查 pub 问题

## Getting a “403” error when publishing a package 发布软件包时出现“403”错误

You receive the following error when running `pub publish`:
运行 `pub publish` 时收到以下错误：

```nocode
HTTP error 403: Forbidden
...
You aren't an uploader for package '<foo>'
```

This problem can occur if one of your accounts was granted permission to publish a package, but the pub client registers you with another account.
如果您的某个帐号被授予发布软件包的权限，但 pub 客户端使用另一个帐号注册您，则可能会出现此问题。

You can reset pub’s authentication process by deleting the pub credentials file:
您可以通过删除 pub 凭据文件来重置 pub 的身份验证过程：

#### Linux

If `$XDG_CONFIG_HOME` is defined:
如果定义了 `$XDG_CONFIG_HOME` ：

```
$ rm $XDG_CONFIG_HOME/dart/pub-credentials.json
```

Otherwise:
否则：

```
$ rm $HOME/.config/dart/pub-credentials.json
```

#### macOS

```
$ rm $HOME/Library/Application Support/dart/pub-credentials.json
```

#### Windows

If you’re using Command Prompt:
如果您使用的是命令提示符：

```
$ del "%APPDATA%\dart\pub-credentials.json"
```

If you’re using PowerShell:
如果您使用的是 PowerShell：

```
$ Remove-Item -Path "%APPDATA%\dart\pub-credentials.json"
```

*merge_type* **Version note:** In Dart 2.14 or earlier, you should instead delete the `credentials.json` file found in the [`PUB_CACHE`](https://dart.dev/tools/pub/environment-variables) folder.
版本说明：在 Dart 2.14 或更早版本中，您应该删除 `PUB_CACHE` 文件夹中找到的 `credentials.json` 文件。

## Getting an “UnauthorizedAccess” error when publishing a package 发布软件包时出现“UnauthorizedAccess”错误

You receive the following error when running `pub publish`:
运行 `pub publish` 时收到以下错误：

```nocode
UnauthorizedAccess: Unauthorized user: <username> is not allowed to upload versions to package '<foo>'.
```

You will see this message if you are not on the list of people authorized to publish new versions of a package. See [Uploaders](https://dart.dev/tools/pub/publishing#uploaders).
如果您不在被授权发布软件包新版本的名单中，您将看到此消息。请参阅上传者。

## Pub build fails with HttpException error Pub 构建因 HttpException 错误而失败

You receive an HttpException error similar to the following when running `pub build`:
运行 `pub build` 时收到类似于以下内容的 HttpException 错误：

```nocode
Pub build failed, [1] IsolateSpawnException: 'HttpException: Connection closed while receiving data,
...
library handler failed
...
```

This can happen as a result of some antivirus software, such as the AVG 2013 Internet security suite. Check the manual for your security suite to see how to temporarily disable this feature. For example, see [How to Disable AVG Components](https://support.avg.com/SupportArticleView?urlName=How-to-disable-AVG).
这可能是由于某些防病毒软件（例如 AVG 2013 Internet 安全套件）导致的。查阅安全套件的手册，了解如何暂时禁用此功能。例如，请参阅如何禁用 AVG 组件。

## Pub get fails from behind a corporate firewall 在公司防火墙后 pub get 失败

From the command line, pub honors the `http_proxy` and `https_proxy` environment variables. You can set the proxy server environment variable as follows.
在命令行中，pub 遵守 `http_proxy` 和 `https_proxy` 环境变量。您可以按如下方式设置代理服务器环境变量。

On Linux/macOS:
在 Linux/macOS 上：

```
$ export https_proxy=hostname:port
```

On Windows Command Prompt:
在 Windows 命令提示符中：

```
$ set https_proxy=hostname:port
```

On Windows PowerShell:
在 Windows PowerShell 中：

```
$ $Env:https_proxy="hostname:port"
```

If the proxy requires credentials, you can set them as follows.
如果代理需要凭据，您可以按如下方式设置它们。

On Linux/macOS:
在 Linux/macOS 上：

```
$ export https_proxy=username:password@hostname:port
```

On Windows Command Prompt:
在 Windows 命令提示符中：

```
$ set https_proxy=username:password@hostname:port
```

On Windows PowerShell:
在 Windows PowerShell 中：

```
$ $Env:https_proxy="username:password@hostname:port"
```

## Localhost unreachable after sign-in 登录后无法访问 Localhost

When you run `dart pub publish` in a container or over an SSH session, the `localhost` that `dart pub` is listening to might be different from the `localhost` that’s accessible in your browser. Although you can sign in using the browser, the browser then complains that `http://localhost:<port>?code=...` is not reachable.
在容器中或通过 SSH 会话运行 `dart pub publish` 时， `localhost` 侦听的 `dart pub` 可能与浏览器中可访问的 `localhost` 不同。虽然您可以使用浏览器登录，但浏览器随后会抱怨无法访问 `http://localhost:<port>?code=...` 。

Try this workaround, which uses the command line to complete sign-in:
尝试此解决方法，该方法使用命令行完成登录：

1. In a terminal window, run `dart pub publish`.
   在终端窗口中，运行 `dart pub publish` 。

2. In the browser window that comes up, sign in.
   在弹出的浏览器窗口中，登录。
   The browser is redirected to a *new localhost URL* (`http://localhost:<port>?code=...`) but complains that the URL isn’t reachable.
   浏览器会重定向到新的 localhost URL（ `http://localhost:<port>?code=...` ），但会抱怨无法访问该 URL。

3. Copy the *new localhost URL* from the browser.
   从浏览器复制新的 localhost URL。

4. In another terminal window in the same container or on the same host as the one where `dart pub publish` was called, use the `curl` command to complete sign-in using the *new localhost URL*:
   在同一容器中的另一个终端窗口中，或在调用 `dart pub publish` 的同一主机上，使用 `curl` 命令使用新的 localhost URL 完成登录：

   ```
   $ curl 'http://localhost:<port>?code=...'
   ```

## Getting a socket error trying to find a package 尝试查找软件包时出现套接字错误

The following error might occur if you have no internet access, your ISP is blocking `pub.dev`, or security software is blocking internet access from `dart`.

​	如果您没有互联网访问权限、您的 ISP 正在阻止 `pub.dev` 或安全软件正在阻止 `dart` 的互联网访问，则可能会出现以下错误。

```nocode
Got socket error trying to find package ... at https://pub.dev.
pub get failed (server unavailable) -- attempting retry 1 in 1 second...
```

Check your internet connection, and verify that you don’t have a firewall or other security software that blocks internet access from `dart`.

​	检查您的互联网连接，并确认您没有防火墙或其他阻止互联网访问的安全软件 `dart` 。

> **卡巴斯基网络安全详细说明**
>
> When you have turned off *Kaspersky Internet Security* protection from the menu bar, the VPN application filter `sysextctrld` still runs in the background. This filter causes a failure to connect to `pub.dev`. To resolve this issue, add both `https://pub.dev` and `https://pub.dartlang.org` to the trusted zone:
> 当您从菜单栏关闭卡巴斯基网络安全保护时，VPN 应用程序过滤器 `sysextctrld` 仍会在后台运行。此过滤器会导致无法连接到 `pub.dev` 。要解决此问题，请将 `https://pub.dev` 和 `https://pub.dartlang.org` 都添加到受信任区域：
>
> 1. Open Kaspersky Internet Security.
>    打开卡巴斯基网络安全。
> 2. Click the **Privacy** icon.
>    单击隐私图标。
> 3. Under the **Block website tracking** section, click the **Preferences** button.
>    在阻止网站跟踪部分下，单击首选项按钮。
> 4. In the top icon bar, select **Threats**.
>    在顶部图标栏中，选择威胁。
> 5. Under **Threats**, click **Trusted Zone**.
>    在威胁下，单击受信任区域。
> 6. Select the **Trusted web addresses** tab.
>    选择受信任的 Web 地址选项卡。
> 7. Click the **+** button, and add the URL `https://pub.dev`.
>    单击 + 按钮，并添加 URL `https://pub.dev` 。
> 8. Click **OK**.
>    单击确定。
> 9. Repeat the previous two steps for `https://pub.dartlang.org`
>    对 `https://pub.dartlang.org` 重复前两个步骤

