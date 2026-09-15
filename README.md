# 股票分账管家 PWA（GitHub Pages / 小米手机）

![应用图标](icons/icon-192.png)

这是把原始单文件《股票分账管家》包装成的 **PWA（Progressive Web App）版本**。  
核心记账、持仓、盈亏、分红、年度结算、JSON 备份等原有业务逻辑保持在 `index.html` 中；本包额外增加了 Web App Manifest、Service Worker、PWA 多尺寸图标和 GitHub Pages 部署说明。

> **最重要的数据提醒：** 如果你之前是直接打开本地 `.html` 文件使用，旧页面和 GitHub Pages PWA 属于不同的网址来源，浏览器不会自动把旧数据带过去。请先在旧程序里导出 **JSON 全量备份**，PWA 首次打开后再导入该 JSON。

## 一、压缩包里有什么

```text
stock-split-manager-pwa/
├─ index.html                 原程序 + PWA 元信息 + Service Worker 注册
├─ manifest.webmanifest       PWA 应用清单
├─ sw.js                      离线缓存与更新逻辑
├─ .nojekyll                  GitHub Pages 直接发布静态文件
├─ README.md                  本说明书
├─ 快速安装说明.txt            手机快速步骤
└─ icons/
   ├─ icon-32.png
   ├─ icon-48.png
   ├─ icon-72.png
   ├─ icon-96.png
   ├─ icon-128.png
   ├─ icon-144.png
   ├─ icon-152.png
   ├─ icon-180.png
   ├─ icon-192.png
   ├─ icon-384.png
   ├─ icon-512.png
   ├─ maskable-192.png
   ├─ maskable-512.png
   └─ icon-source-1024.png
```

## 二、上传到 GitHub

### 方法：直接用 GitHub 网页上传（最简单）

1. 登录 GitHub，新建一个仓库，例如：`stock-split-manager-pwa`。
2. 如果你的 GitHub 套餐只支持公共仓库 Pages，就选择 **Public**。  
   **不要把任何真实 JSON 备份、Excel 账单或个人财务数据上传到仓库。**
3. 解压本 ZIP。
4. 进入解压后的 `stock-split-manager-pwa` 文件夹，把里面的文件和 `icons` 文件夹 **上传到仓库根目录**。
   - `index.html` 必须在仓库发布目录的最上层。
   - 不要只把 ZIP 文件本身上传后就结束；GitHub Pages 不会自动解压 ZIP。
5. 提交（Commit）这些文件。

## 三、启用 GitHub Pages

1. 打开仓库的 **Settings**。
2. 左侧进入 **Pages**。
3. 在 **Build and deployment** 中，把 **Source** 设为 `Deploy from a branch`。
4. Branch 选择 `main`，目录选择 `/(root)`。
5. 点击 **Save**。
6. 等待 GitHub 完成部署，然后在 Pages 页面点击 **Visit site**。

常见网址形态：

```text
https://你的GitHub用户名.github.io/你的仓库名/
```

GitHub Pages 提供 HTTPS，适合 Service Worker / PWA 安装。

## 四、在小米手机上安装

推荐使用 **最新版 Chrome** 打开你的 GitHub Pages 地址。

1. 第一次打开时保持联网，让主程序、Manifest、图标和离线缓存完成初始化。
2. 在 Chrome 右上角点 **⋮ 更多**。
3. 找到 **“安装应用”**，或 **“添加到主屏幕 / 安装并建立快捷方式”**（不同 Chrome 版本文字可能略有不同）。
4. 点 **安装**。
5. 完成后，“股票分账管家”会像普通 App 一样出现在桌面/应用列表，可从图标独立启动。

如果小米自带浏览器没有出现 PWA 安装入口，直接改用 Chrome；不同 HyperOS / 浏览器版本的菜单与 PWA 支持可能不同。

## 五、从旧 HTML 迁移数据

如果你已经在原来的单文件 HTML 中录入过真实数据：

1. **先打开旧 HTML。**
2. 进入 **数据备份与恢复**。
3. 点击 **导出 JSON**，保存好全量备份。
4. 再打开 GitHub Pages 上的新 PWA。
5. 进入 **数据备份与恢复 → 选择 JSON 恢复**。
6. 核对首页、持仓、成交、资金、分红后，再把 PWA 固定到桌面。

这样迁移最安全。不要依赖浏览器自动搬运 IndexedDB / localStorage，因为本地 `file://` 与 GitHub `https://` 的存储空间相互独立。

## 六、离线与联网能力

### 可离线
- PWA 主程序界面
- 本机已保存的成交、收盘价、资金、分红、设置等数据
- 持仓与历史统计计算
- JSON 备份/恢复的核心逻辑

### 需要联网
- 自动获取腾讯 / 东方财富行情
- 历史行情补全
- 上证指数等联网数据
- Excel 组件第一次下载（原程序本身已有缓存机制；首次成功缓存后可离线继续使用）

Service Worker **不会缓存外部行情接口的响应**，这样可避免把过期股票价格当成最新行情。

## 七、数据安全建议

- GitHub 仓库只放程序文件；**不要提交真实备份数据**。
- PWA 的业务数据仍保存在手机浏览器的 IndexedDB / localStorage 中，不会因为把程序代码放到 GitHub 就自动上传你的账本。
- 卸载 PWA、清除 Chrome 网站数据、恢复出厂设置、系统清理存储，都可能影响本机数据。
- 建议经常使用程序自带的 **JSON 全量备份**，并把备份放到你自己可靠的位置。
- 换手机或换浏览器时，使用 **JSON 导出 → JSON 恢复** 迁移。

## 八、以后更新程序

如果以后你拿到新的 `index.html`：

1. 先保留本 PWA 包中的 `manifest.webmanifest`、`sw.js`、`icons/`。
2. 把新程序改名为 `index.html`。
3. 把本版本 `index.html` `<head>` 里的 PWA 标签和文件末尾的 Service Worker 注册代码合并到新文件。
4. 建议同时把 `sw.js` 中 `CACHE_NAME` 的版本号改一下，例如从 `pwa1` 改为 `pwa2`。
5. 上传并 Commit 到 GitHub。
6. 手机上的 PWA 重新打开/刷新后会逐步获取新版本。

## 九、常见问题

### 1. 为什么手机上没有“安装应用”？
确认你打开的是 GitHub Pages 的 `https://...github.io/.../` 地址，而不是 GitHub 的代码预览页面；第一次先联网完整打开一次，并优先使用 Chrome。

### 2. 为什么直接双击 `index.html` 不能测试 PWA？
普通 `file://` 页面不能正常注册 Service Worker。PWA 应通过 HTTPS（GitHub Pages）或开发时的 `localhost` 访问。

### 3. 为什么装好后行情离线不能更新？
这是正常的。PWA 可以离线运行核心账本，但股票行情本身需要网络。

### 4. GitHub 是公开的，我的数据会不会公开？
只要你没有把 JSON/Excel 备份提交到仓库，程序运行时录入的数据默认仍是手机浏览器本地数据。代码公开不等于账本数据公开。

### 5. 更新 GitHub 后手机还是旧界面？
先确认 Pages 已完成新部署，然后在 PWA 中刷新，或完全关闭再重新打开。Service Worker 使用版本化缓存，并采用“页面导航优先联网、离线再回退缓存”的方式。

---

原程序版本标识保持为 **V1.14.6**；本次只增加 PWA/GitHub Pages 外层能力，不主动改动你的证券计算口径。
