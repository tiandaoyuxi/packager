[English](README.md)

# TurboWarp 打包器

https://packager.turbowarp.org/

将 Scratch 项目转换为 HTML 文件、zip 压缩包或适用于 Windows、macOS 和 Linux 的可执行程序。

## 开发

安装依赖：

```
npm ci
```

以开发模式启动：

```
npm start
```

然后访问 http://localhost:8947。手动刷新以查看更改。

在开发模式下生成的打包项目不应分发。相反，您应该运行生产构建以显著减小网站和打包器本身的文件大小。

```
npm run build-prod
```

输出将位于 `dist` 文件夹中。

`src` 的大致布局是：

 - packager: 下载和打包项目的代码。
 - p4: 打包器的 Svelte 网站。"p4" 是打包器内部用来指代自身的名称。
 - scaffolding: 一个最小化的 Scratch 项目播放器。处理运行 Scratch 项目的大多数繁琐细节，例如处理鼠标输入。
 - common: scaffolding 和 packager 都使用的一些文件。
 - addons: 可选的插件，例如手柄支持或指针锁定。
 - locales: 翻译。en.json 包含原始英文消息。其他语言由志愿者翻译并通过自动化脚本导入（[您可以帮忙](https://docs.turbowarp.org/translate)）。
 - build: 各种构建时脚本，例如 webpack 插件和加载器。

## Fork 的提示

我们努力使打包器易于 Fork，即使是并非基于 TurboWarp 的修改版本。阅读本节，至少是前半部分，将使其更容易实现。

### 包

如果您想更改使用的 scratch-vm/scratch-render/scratch-audio/scratch-storage/等，这很简单：

 - `npm install` 或 `npm link` 您的包。包名称无关紧要。
 - 更新 src/scaffolding/scratch-libraries.js 以使用您拥有的名称导入包。（我们的一些包前缀为 `@turbowarp/`，而另一些仍只是 `scratch-vm` —— 只需确保它们与您的匹配）

然后重新构建即可。您甚至可以安装原版 scratch-vm，所有核心功能仍然可以正常工作（但可选功能，例如插值、高质量画笔、舞台大小等可能无法工作）。

请注意，npm 是一个非常容易出错的软件，我们的依赖树非常庞大。偶尔您可能会遇到缺少依赖项的错误，如果您运行 `npm install`，这些错误应该会消失。

### 部署

打包器部署为一个简单的静态网站。您可以通过在构建后复制 `dist` 文件夹将其托管在任何地方。

我们使用 GitHub Actions 和 GitHub Pages 来管理我们的部署。如果您也想这样做：

 - 在 GitHub 上 Fork 仓库并推送您的更改。
 - 转到您在 GitHub 上的 Fork 的设置，并将 GitHub Pages 的源设置为 GitHub Actions。
 - 转到“Actions”选项卡，如果尚未启用，则启用 GitHub Actions。
 - 将提交推送到“master”分支。
 - 几分钟后，您的网站将自动构建并部署到 GitHub Pages。

### 品牌

我们要求您至少花点时间通过编辑 `src/packager/brand.js` 来用您自己的应用程序名称、链接等重命名网站。

### 大文件

NW.js、Electron 和 WKWebView 可执行文件等大文件存储在此仓库之外的外部服务器上。虽然我们没有积极删除旧文件（服务器仍在提供自 2020 年 11 月以来未使用的文件），但我们无法保证它们将永远存在。打包器使用安全校验和来验证这些下载。Fork 可以自由使用我们的服务器，但如果您愿意，可以轻松设置自己的服务器（它只是一个静态文件服务器；有关更多信息，请参阅 `src/packager/large-assets.js`）。

### Service Worker

将环境变量 `ENABLE_SERVICE_WORKER` 设置为 `1` 以启用 Service Worker 以提供离线支持（实验性，并非 100% 可靠）。不建议在开发中使用。我们的 GitHub Actions 部署脚本默认使用此功能。

## 独立构建

打包器支持生成“独立构建”，即包含整个打包器的单个 HTML 文件。Electron 二进制文件等大文件仍将根据需要从远程服务器下载。您可以从[我们的 GitHub 版本](https://github.com/TurboWarp/packager/releases)下载预构建的独立构建。如果我们的网站被阻止或您没有可靠的互联网连接，这些可能会很有用。请注意，独立构建不包含更新检查器，因此请偶尔自行检查。

要在本地进行生产独立构建：

```
npm run build-standalone-prod
```

构建输出到 `dist/standalone.html`。

## Node.js 模块和 API

有关 Node.js API 文档，请参阅 [node-api-docs/README.md](node-api-docs/README.md)。

要在本地构建 Node.js 模块：

```
npm run build-node-prod
```

## 许可证

<!-- Make sure to also update COPYRIGHT_NOTICE in src/packager/brand.js -->

版权所有 (C) 2021-2024 Thomas Weber

本源代码形式受 Mozilla 公共许可证 v2.0 的条款约束。如果本文件未随附 MPL 的副本，您可以在 https://mozilla.org/MPL/2.0/ 获取。
