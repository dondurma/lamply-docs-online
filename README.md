# Lamply · 在线文档站（lamply-docs-online）

> 仓库：<https://github.com/dondurma/lamply-docs-online>
> 状态：三语静态骨架已就绪并推送；Cloudflare Pages 尚未连接；三份文档正文仍为占位。

Lamply（课灯）的公开文档站，供 App 内打开与浏览器独立访问。包含隐私政策、会员 / 订阅协议、App 使用指南，支持简体中文、繁体中文、英文。

- 技术：纯静态 HTML + 一份 CSS，无构建步骤、无第三方依赖
- 托管：GitHub 仓库 → Cloudflare Pages
- 参考：时予 App 的 `Shiyu-doc-online`（同样的纯静态 + Pages 方案）

## 1. 目录与 URL

```
/
  index.html          语言入口（默认跟随浏览器，可手动切换）
  style.css           全站样式（与 App 同一套纸感配色）
  zh-Hans/            简体中文
    privacy.html
    terms.html
    guide.html
  zh-Hant/            繁体中文
    privacy.html
    terms.html
    guide.html
  en/                 英文
    privacy.html
    terms.html
    guide.html
  README.md
```

| 页面 | 简体（默认） | 繁体 | English |
| --- | --- | --- | --- |
| 首页 | `/` | `/`（切语言） | `/`（切语言） |
| 隐私政策 | `/zh-Hans/privacy` | `/zh-Hant/privacy` | `/en/privacy` |
| 会员 / 订阅协议 | `/zh-Hans/terms` | `/zh-Hant/terms` | `/en/terms` |
| 使用指南 | `/zh-Hans/guide` | `/zh-Hant/guide` | `/en/guide` |

App 与 App Store Connect 用无扩展名地址。Cloudflare Pages 会把 `.html` 请求 308 到无扩展名地址，两种都能用；直接写无扩展名少一次跳转，也更像正式网址。

根路径默认显示浏览器语言对应的版本（简体 / 繁体 / 英文），并记住用户手动选择；没有 JavaScript 时显示简体中文。文档页顶部只放同一份文档的三语互链，页脚返回首页。

## 2. 本地预览

```bash
cd lamply-docs-online
python3 -m http.server 8080
# 打开 http://localhost:8080
```

`python3 -m http.server` 不支持无扩展名地址，本地请用 `.html` 结尾的路径测试；无扩展名地址在 Cloudflare Pages 上生效。

## 3. 部署到 Cloudflare Pages

1. 打开 Cloudflare Dashboard → **Workers & Pages → Pages → Create → Connect to Git**
2. 选择 `dondurma/lamply-docs-online` 仓库
3. 构建设置：
   - Framework preset：**None**
   - Build command：**留空**
   - Build output directory：**`/`**（仓库根）
4. 保存并部署，先访问 `<project>.pages.dev` 验证首页和三种语言
5. 之后每次 push 到 `main`，Cloudflare Pages 会自动重新部署

> 纯静态站点不需要 Node 或 npm，仓库里不要放 `node_modules`。

## 4. 自定义域名（可选）

1. Pages 项目 → **Custom domains → Add custom domain**，填 `docs.<你的域名>`
2. 按提示选择外部 DNS / CNAME setup
3. 在 DNS 服务商给该子域加一条 `CNAME`，指向 `<project>.pages.dev`
4. 回 Cloudflare 点 **Check DNS records**，等状态变为 Active；证书签发期间出现 SSL 握手失败属正常，等几分钟

本项目的域名尚未确定；确定后把上面的 `<domain>` 替换掉。Cloudflare Pages 部署在境外，部署本身与绑定自定义域名都不需要备案；App Store 中国区上架所需的备案按你现有渠道办理，与本站托管无关。

## 5. App 内与 App Store Connect

- App Store Connect 的 **Privacy Policy URL**：`https://<domain>/zh-Hans/privacy`（审核默认简体；如审核要求英文，可换 `/en/privacy`）
- **Support URL**：`https://<domain>/zh-Hans/guide`（或首页）
- App 内「关于 / 会员」入口：按 App 当前语言打开对应目录；当前 App 只有简体，可先用 `zh-Hans` 路径
- 三语路径固定，以后 App 增加繁体 / 英文界面时直接复用，不需要改 URL

## 6. 上线前检查

- [ ] 替换三语共 9 个文档页的占位正文
- [ ] 删除首页加 9 个文档页的 `<meta name="robots" content="noindex">`（共 10 页）
- [ ] 三语文案校对（繁体用词、英文拼写）
- [ ] 确认页脚联系邮箱（当前暂用 `ruanweidev@163.com`，沿用你另一款 App 的公开邮箱）
- [ ] 确定域名并加 CNAME
- [ ] 用无扩展名 URL 在 App 内实际打开一次

## 7. 维护约定

- 每份文档顶部「最后更新」与页脚日期同步
- 改导航或页脚时，三语 9 个文档页一起检查
- 三语版本内容保持同步；新增条目时三份 HTML 一起改
- 站内全部相对路径，换域名不用改 HTML
- 保持每个文档一个真实 HTML，不做 SPA；App 内、浏览器、缓存与分享都能直达
