# 健康小站 · 纯前端版

> 「How do you keep healthy?」健康小站 — 静态 HTML + JSON 配置文件驱动

---

## 这是什么

一个**零后端**的健康教育主题网站，所有内容（文字、视频、PDF）都通过 `content.json` 配置文件管理。

**目标用户**：青少年
**核心功能**：
- 📖 一封信（团队介绍）
- 🧩 七巧板 / 🁡 多米诺 / 🪵 榫卯 — 互动科普
- 📚 6 大健康知识模块（饮食、运动、睡眠、视屏、烟酒、心理）+ 2 个综合模块
- ☑️ 每日打卡（localStorage 存浏览器本地）
- 📱 公众号引流（页面底部放公众号二维码）

---

## 文件结构

```
health-station-pure/
├── index.html              ← 主页
├── content.json            ← ★ 配置文件（你主要改这个）
├── HANDOVER.md             ← 交接文档
├── README.md               ← 本文件
├── components/             ← 5 个 iframe 子页面
│   ├── team.html           ← 一封信
│   ├── tangram.html        ← 七巧板
│   ├── domino.html         ← 多米诺
│   ├── mortise.html        ← 榫卯
│   └── checkin.html        ← 每日打卡
├── pages/                  ← 4 个独立子页面
│   ├── health-detail.html  ← 6 模块详情
│   ├── health-loop.html    ← 环环相扣
│   ├── mortise.html        ← 计划与行动
│   └── checkin.html        ← 打卡科学说明
├── assets/
│   ├── images/             ← 7 张占位图
│   ├── videos/             ← 视频文件目录
│   └── pdfs/               ← PDF 文件目录
└── uploads/                ← 旧的本地上传（如有）
```

---

## 怎么用

### 1. 编辑内容

**改文字 / 视频 / PDF → 编辑 `content.json`**

```json
{
  "diet": {
    "name": "营养饮食",
    "intro": {
      "title": "营养饮食 · 吃对每一餐",
      "paragraphs": ["第一段...", "第二段..."]
    },
    "pdfs": [
      { "id": "diet-pdf-1", "title": "膳食指南", "url": "assets/pdfs/diet/guide.pdf" }
    ],
    "videos": [
      { "id": "diet-vid-1", "title": "营养早餐", "url": "https://mp.weixin.qq.com/s?xxx" }
    ]
  }
}
```

### 2. 上传文件

#### 方式 A：放本地（推荐小文件）

```bash
# 把视频拖到对应目录
cp ~/Downloads/breakfast.mp4 assets/videos/diet/

# 把 PDF 拖到对应目录
cp ~/Downloads/diet-guide.pdf assets/pdfs/diet/
```

在 `content.json` 里写：
```json
"url": "assets/videos/diet/breakfast.mp4"
"url": "assets/pdfs/diet/diet-guide.pdf"
```

#### 方式 B：用公众号文章（推荐视频）

把视频嵌入公众号文章，发布后复制文章链接：

```json
"url": "https://mp.weixin.qq.com/s?__biz=MzA5...&mid=1001&idx=1&sn=abc"
```

**好处**：
- 视频不占仓库空间
- 用户点击跳到微信（顺便给公众号引流）
- 微信内看视频最流畅

#### 方式 C：外链（B 站、YouTube 等）

```json
"url": "https://www.bilibili.com/video/BV1xxx"
```

前端会自动判断：`.mp4/.webm/.mov` 内嵌播放，其他跳转新窗口。

### 3. 替换图片

把 7 张图替换成正式素材：

```
assets/images/hero.png         ← 主页 Hero
assets/images/title1.jpg       ← 健康知识页 Hero
assets/images/title2.jpg       ← 健康循环页 Hero
assets/images/title3.jpg       ← 计划行动页 Hero
assets/images/team.jpg         ← 团队照片
assets/images/joyride.png      ← 榫卯页插图
assets/images/mona-lisa.png    ← 打卡页蒙娜丽莎
assets/images/mp-qrcode.jpg    ← ★ 公众号二维码（自行提供）
```

### 4. 本地预览

**⚠️ 重要**：双击 `index.html`（file:// 协议）**不能加载 content.json**（浏览器 CORS 限制）。

需要起一个本地服务器：

```bash
# 方式 A：Python（系统自带）
cd /Users/dd/Documents/html/health-station-pure
python3 -m http.server 8000
# 浏览器打开 http://localhost:8000

# 方式 B：Node（需要 npx）
npx serve

# 方式 C：VS Code 装个 Live Server 插件，右键 index.html → Open with Live Server
```

### 5. 部署上线

**推荐方案**：GitHub Pages（零成本，国内外都可访问，套 Cloudflare 加速国内访问）

#### 5.1 推到 GitHub

```bash
cd /Users/dd/Documents/html/health-station-pure
git init
echo "node_modules/\n.DS_Store\n*.log" > .gitignore
git add -A
git commit -m "健康小站 v2.0 — 纯前端版"
git remote add origin https://github.com/你的用户名/health-station.git
git branch -M main
git push -u origin main
```

#### 5.2 开启 GitHub Pages

1. 打开 GitHub 仓库 → Settings → Pages
2. Source: **Deploy from a branch**
3. Branch: **main** / **(root)**
4. 保存 → 1-2 分钟后访问 `https://你的用户名.github.io/health-station/`

#### 5.3 套 Cloudflare 加速（国内访问）

1. 注册 https://dash.cloudflare.com
2. 添加你的域名（如 `health.example.com`）
3. DNS 加 CNAME：`www` → `你的用户名.github.io`
4. 改域名的 nameserver 为 Cloudflare 给的 ns1/ns2.cloudflare.com
5. GitHub Pages Settings → Custom domain 填 `health.example.com`

---

## content.json 配置字段说明

```json
{
  "wechat": {
    "name": "健康小站",
    "intro": "扫码关注公众号",
    "qrCode": "assets/images/mp-qrcode.jpg"
  },

  "teamLetter": {
    "eyebrow": "Letter · No. 01",
    "title": "给<span class=\"cn\">青少年</span>的一封信",
    "date": "写于 二〇二六年 · 初夏",
    "cornerTag": "Team · Letter 01",
    "greeting": "亲爱的同学/朋友：",
    "paragraphs": ["段1...", "段2..."],
    "signOff": "—— 你的健康小团队 ❤️",
    "signAlign": "right",
    "signLabel": "你的健康小团队<br/>于二〇二六年夏"
  },

  "diet": {
    "name": "营养饮食",
    "intro": { "title": "...", "paragraphs": ["..."] },
    "pdfs": [ { "id": "...", "title": "...", "desc": "...", "url": "..." } ],
    "videos": [ { "id": "...", "title": "...", "desc": "...", "url": "...", "poster": "..." } ]
  },

  "exercise": { ... },
  "sleep": { ... },
  "screen": { ... },
  "habits": { ... },
  "mental": { ... },
  "loop": { ... },
  "action": { ... },

  "checkin": { "title": "...", "placeholder": "...", "addButton": "...", "emptyHint": "...", "monaCaption": "..." },

  "ui": {
    "heroTitleHtml": "How do you keep <span class=\"click\">healthy</span>?",
    "heroTip": "...",
    "sections": [ { "id": "sec-team", "num": "Section 01", "title": "..." } ],
    "mortiseLinkHtml": "..."
  }
}
```

---

## 调试技巧

### 浏览器开发者工具

按 `F12`（Mac: `⌘+⌥+I`）打开开发者工具。

```javascript
// 1. 看 content.json 是否加载成功
fetch('content.json').then(r=>r.json()).then(console.log)

// 2. 看 localStorage 打卡数据
JSON.parse(localStorage.getItem('healthTodos') || '[]')
JSON.parse(localStorage.getItem('healthDoneDates') || '[]')

// 3. 打开调试日志（事件追踪会输出到 console）
window.__HEALTH_DEBUG__ = true
location.reload()
```

### 常见问题

| 问题 | 解决 |
|------|------|
| 双击 index.html 显示空白 | 用本地服务器，不要 file:// 打开 |
| 改了 content.json 没生效 | 浏览器**硬刷新** `⌘+Shift+R` / `Ctrl+Shift+R` |
| 视频点不开 | 检查 URL 路径，公众号 URL 包含 `mp.weixin.qq.com` 会跳转到微信 |
| 移动端访问慢 | 配 Cloudflare CDN |

---

## 部署清单

### 必做

- [ ] 替换 7 张图（hero、title1/2/3、team、mona-lisa、joyride）
- [ ] 添加公众号二维码 `assets/images/mp-qrcode.jpg`
- [ ] 编辑 `content.json` 填入正式内容（信、模块、视频/PDF URL）
- [ ] 上传视频/PDF 到对应目录（或用公众号文章链接）
- [ ] 本地预览验证（`python3 -m http.server`）

### 选做

- [ ] 推到 GitHub
- [ ] 开启 GitHub Pages
- [ ] 注册域名 + 套 Cloudflare 加速
- [ ] 配 CDN 缓存规则（HTML 不缓存，图片缓存 30 天）

---

## 与旧版（v1.0）对比

| 维度 | v1.0 | v2.0（当前）|
|------|------|-------------|
| 后端 | Node.js + Express + SQLite | **无** |
| 数据库 | SQLite 文件 | **无** |
| 用户系统 | 昵称 + 跨设备同步 | **无**（纯匿名访问）|
| 数据存储 | localStorage + 后端双写 | **localStorage 单写** |
| 事件追踪 | 上报到后端 | **本地日志**（不发送）|
| 内容更新 | 改后端数据库 | **改 content.json + git push** |
| 文件存储 | 阿里云 OSS | **本地 / 公众号 / OSS 都行** |
| 部署复杂度 | 需 Node 服务器 | **任意静态托管** |
| 月费 | ~120 元 | **~0-5 元**（CDN/OSS 可选）|

---

**项目版本**：v2.0（纯前端）
**最后更新**：2026-07-11
**许可**：内部项目，版权归项目所有者
