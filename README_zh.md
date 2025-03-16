# Google 索引脚本

<div align="center">
  <a href="README.md">English</a> | <a href="README_zh.md">中文</a>
</div>

使用此脚本可在48小时内将您的整个网站索引到 Google。没有技巧，没有黑客手段，只是一个简单的脚本和 Google API。

> [!重要]
>
> 1. 此脚本使用 [Google 索引 API](https://developers.google.com/search/apis/indexing-api/v3/quickstart)，它仅适用于包含 `JobPosting` 或 `BroadcastEvent` 结构化数据的页面。
> 2. 索引 != 排名。这不会帮助您的页面在 Google 上获得更好的排名，它只会让 Google 知道您页面的存在。

## 要求

- 安装 [Node.js](https://nodejs.org/en/download)
- 拥有 [Google Search Console](https://search.google.com/search-console/about) 账户，并验证您想要索引的网站
- 拥有 [Google Cloud](https://console.cloud.google.com/) 账户

## 准备工作

1. 按照 Google 的[指南](https://developers.google.com/search/apis/indexing-api/v3/prereqs)操作。完成后，您应该在 Google Cloud 上有一个启用了索引 API 的项目，以及一个对您的网站具有 `Owner` 权限的服务账户。
2. 确保在您的 [Google 项目 ➤ API 服务 ➤ 已启用的 API 和服务](https://console.cloud.google.com/apis/dashboard) 中同时启用 [`Google Search Console API`](https://console.cloud.google.com/apis/api/searchconsole.googleapis.com) 和 [`Web Search Indexing API`](https://console.cloud.google.com/apis/api/indexing.googleapis.com)。
3. [下载 JSON 文件](https://github.com/goenning/google-indexing-script/issues/2)，其中包含您的服务账户的凭据，并将其保存在与脚本相同的文件夹中。该文件应命名为 `service_account.json`。

## 安装

### 使用 CLI

在您的机器上全局安装 CLI。

```bash
npm i -g google-indexing-script
```

### 使用仓库

将仓库克隆到您的机器上。

```bash
git clone https://github.com/goenning/google-indexing-script.git
cd google-indexing-script
```

安装并构建项目。

```bash
npm install
npm run build
npm i -g .
```

> [!注意]
> 确保您使用的是最新版本的 Node.js，最好是 v20 或更高版本。使用 `node -v` 检查您当前的版本。

## 使用方法

<details open>
<summary>使用 <code>service_account.json</code> <i>(推荐)</i></summary>

在您的主文件夹中创建一个 `.gis` 目录，并将 `service_account.json` 文件移动到那里。

```bash
mkdir ~/.gis
mv service_account.json ~/.gis
```

使用您想要索引的域名或 URL 运行脚本。

```bash
gis <域名或URL>
# 示例
gis seogets.com
```

以下是运行脚本的其他方式：

```bash
# 自定义 service_account.json 的路径
gis seogets.com --path /path/to/service_account.json
# 长版本命令
google-indexing-script seogets.com
# 克隆的仓库
npm run index seogets.com
```

</details>

<details>
<summary>使用环境变量</summary>

打开 `service_account.json` 并复制 `client_email` 和 `private_key` 值。

使用您想要索引的域名或 URL 运行脚本。

```bash
GIS_CLIENT_EMAIL=your-client-email GIS_PRIVATE_KEY=your-private-key gis seogets.com
```

</details>

<details>
<summary>使用参数 <i>(不推荐)</i></summary>

打开 `service_account.json` 并复制 `client_email` 和 `private_key` 值。

获取这些值后，使用您想要索引的域名或 URL、客户端电子邮件和私钥运行脚本。

```bash
gis seogets.com --client-email your-client-email --private-key your-private-key
```

</details>

<details>
<summary>作为 npm 模块</summary>

您也可以在自己的项目中将脚本用作 [npm 模块](https://www.npmjs.com/package/google-indexing-script)。

```bash
npm i google-indexing-script
```

```javascript
import { index } from "google-indexing-script";
import serviceAccount from "./service_account.json";

index("seogets.com", {
  client_email: serviceAccount.client_email,
  private_key: serviceAccount.private_key,
})
  .then(console.log)
  .catch(console.error);
```

阅读 [API 文档](https://jsdocs.io/package/google-indexing-script) 了解更多详情。

</details>

以下是您应该看到的示例：

![](./output.png)

> [!重要]
>
> - 您的网站必须向 Google Search Console 提交一个或多个站点地图。否则，脚本将无法找到要索引的页面。
> - 您可以根据需要多次运行脚本。它只会索引尚未索引的页面。
> - 拥有大量页面的网站可能需要一段时间才能完成索引，请耐心等待。

## 配额

根据您的账户，API 配置了多个配额（参见[文档](https://developers.google.com/search/apis/indexing-api/v3/quota-pricing#quota)）。默认情况下，一旦超出速率限制，脚本就会退出。您可以为按分钟时间框架应用的读取请求配置重试机制。

<details>
<summary>使用环境变量</summary>

```bash
export GIS_QUOTA_RPM_RETRY=true
```

</details>

<details>
<summary>作为 npm 模块</summary>

```javascript
import { index } from 'google-indexing-script'
import serviceAccount from './service_account.json'

index('seogets.com', {
  client_email: serviceAccount.client_email,
  private_key: serviceAccount.private_key
  quota: {
    rpmRetry: true
  }
})
  .then(console.log)
  .catch(console.error)
```

</details>

## 📄 许可证

MIT 许可证

## 💖 赞助商

本项目由 [SEO Gets](https://seogets.com) 赞助

![](https://seogets.com/og.png)