# Lamply · 在线文档站（lamply-docs-online）

> 仓库：<https://github.com/dondurma/lamply-docs-online>
> 状态：三语静态站已上线，部署在 Cloudflare Pages，域名为 `https://docs.lamply.cn`。
> 文档：隐私政策、用户协议、使用指南、关于；三语内容已补齐，不再使用 noindex 占位。

Lamply（课灯）的公开文档站，供 App 内打开与浏览器独立访问。包含隐私政策、用户协议、使用指南与关于页，支持简体中文、繁体中文、英文。

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
    about.html
  zh-Hant/            繁体中文
    privacy.html
    terms.html
    guide.html
    about.html
  en/                 英文
    privacy.html
    terms.html
    guide.html
    about.html
  README.md
```

| 页面 | 简体（默认） | 繁体 | English |
| --- | --- | --- | --- |
| 首页 | `/` | `/`（切语言） | `/`（切语言） |
| 隐私政策 | `/zh-Hans/privacy` | `/zh-Hant/privacy` | `/en/privacy` |
| 用户协议 | `/zh-Hans/terms` | `/zh-Hant/terms` | `/en/terms` |
| 使用指南 | `/zh-Hans/guide` | `/zh-Hant/guide` | `/en/guide` |
| 关于 | `/zh-Hans/about` | `/zh-Hant/about` | `/en/about` |

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

本项目使用 `docs.lamply.cn`；Cloudflare Pages 项目为 `lamply-docs-online`，CNAME 指向 `lamply-docs-online.pages.dev`。Cloudflare Pages 部署在境外，部署本身与绑定自定义域名都不需要备案；App Store 中国区上架所需的备案按你现有渠道办理，与本站托管无关。

## 5. App 内与 App Store Connect

- App Store Connect 的 **Privacy Policy URL**：`https://docs.lamply.cn/zh-Hans/privacy`（多数地区审核可接受简体；需要英文时换 `/en/privacy`）
- **Support URL**：`https://docs.lamply.cn/zh-Hans/guide`
- App 内「设置 → 关于与条款」：已按当前界面语言打开 `/{lang}/about|terms|privacy|guide`；首启引导底部也提供协议/隐私链接
- 三语路径固定，App 切换语言后直接复用，不需要改 URL

## 6. 上线前检查

- [x] 补齐三语共 12 个文档页正文（privacy / terms / guide / about）
- [x] 删除首页与 12 个文档页的 noindex 占位
- [x] 三语文案校对（繁体用词、英文拼写）
- [x] 确认页脚联系邮箱 `ruanweidev@163.com`
- [x] 绑定 `docs.lamply.cn`
- [ ] App Store Connect 三语 Listing 与 Support/Privacy URL 最终填写

## 7. 维护约定

- 每份文档顶部「最后更新」与页脚日期同步
- 改导航或页脚时，三语 12 个文档页一起检查
- 三语版本内容保持同步；新增条目时 12 份 HTML 一起改
- 站内全部相对路径，换域名不用改 HTML
- 保持每个文档一个真实 HTML，不做 SPA；App 内、浏览器、缓存与分享都能直达
