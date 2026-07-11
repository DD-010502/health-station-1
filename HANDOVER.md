# 健康小站 · 纯前端版 · 交接文档

> 写给**完全没有上下文**的新协作者 / 新会话。
> 通读此文档，5 分钟内可上手。

---

## 0. 一句话总结

**纯静态 HTML 网站**，用 `content.json` 配置文件管理内容（文字、视频、PDF），无后端、无数据库、零运维。适合挂到 GitHub Pages 或任意静态托管。

---

## 1. 项目位置与历史

| 项 | 值 |
|----|---|
| 当前目录 | `/Users/dd/Documents/html/health-station-pure/` |
| 旧项目目录 | `/Users/dd/Documents/html/try/`（**保留不动**，含旧后端代码）|
| 旧项目 GitHub | `https://github.com/DD-010502/health-station` |
| 当前项目 GitHub | 暂未推到 git（**用户计划推到新仓库**）|

**关系**：本项目是从 `try/` 抽取前端部分、删除后端、改为配置文件驱动而来。`try/` 旧项目**完整保留**，可作为对照参考。

---

## 2. 当前状态

| 状态 | 说明 |
|------|------|
| ✅ 已完成 | 文件抽取、删除后端、配置文件化、清理 track 事件 |
| 📋 待用户做 | 替换图片、上传视频/PDF、填 content.json 真实内容 |
| 📋 待用户做 | 推到新 git 仓库 |
| 📋 待用户做 | 部署到 GitHub Pages |

---

## 3. 核心文件清单

| 文件 | 作用 | 重要程度 |
|------|------|---------|
| `content.json` | ★ **所有可编辑内容**（信、模块、视频/PDF）| 🔴 必改 |
| `index.html` | 主页（5 个 iframe）| 🟡 已重构 |
| `components/team.html` | iframe: 一封信（文字从 content.json 读）| 🟡 已重构 |
| `components/checkin.html` | iframe: 每日打卡（纯 localStorage）| 🟡 已重构 |
| `components/{tangram,domino,mortise}.html` | 互动 iframe（**未改动**）| 🟢 不动 |
| `pages/health-detail.html` | 6 模块详情（fetch content.json）| 🟡 已重构 |
| `pages/health-loop.html` | 环环相扣（fetch content.json）| 🟡 已重构 |
| `pages/mortise.html` | 计划与行动（fetch content.json）| 🟡 已重构 |
| `pages/checkin.html` | 打卡科学说明（**未改动**）| 🟢 不动 |
| `assets/images/*.png` | 7 张占位图 | 🟡 待替换 |
| `assets/images/mp-qrcode.jpg` | **公众号二维码（需用户提供）**| 🔴 必加 |
| `assets/videos/` | 视频目录（空）| 🟡 待上传 |
| `assets/pdfs/` | PDF 目录（空）| 🟡 待上传 |
| `README.md` | 项目使用说明 | 🟢 |
| `HANDOVER.md` | 本文档 | 🟢 |

---

## 4. 用户使用流程

### 4.1 编辑内容（最常见操作）

```bash
# 1. 编辑 content.json
code /Users/dd/Documents/html/health-station-pure/content.json
# 或任何文本编辑器

# 2. 本地预览
cd /Users/dd/Documents/html/health-station-pure
python3 -m http.server 8000
# 浏览器打开 http://localhost:8000

# 3. 浏览器硬刷新（Mac: ⌘+Shift+R / Win: Ctrl+Shift+R）
```

### 4.2 上传视频/PDF

**两种方式**：

**A. 放本地**（文件 < 50MB 适用）
```bash
cp 我的视频.mp4 /Users/dd/Documents/html/health-station-pure/assets/videos/diet/
# 然后 content.json 写 "url": "assets/videos/diet/我的视频.mp4"
```

**B. 用公众号文章**（推荐视频，文件大时适用）
1. 在公众号后台写文章，插入视频
2. 发布后复制文章链接
3. content.json 写 `"url": "https://mp.weixin.qq.com/s?xxx"`

### 4.3 替换图片

把新图片拖到 `assets/images/`，文件名保持不变：
- `hero.png`、`title1.jpg`、`title2.jpg`、`title3.jpg`
- `team.jpg`、`mona-lisa.png`、`joyride.png`
- `mp-qrcode.jpg`（**新增**：公众号二维码）

### 4.4 部署上线

**最快路径**（GitHub Pages）：

```bash
cd /Users/dd/Documents/html/health-station-pure

# 1. git init
git init
echo "node_modules/" > .gitignore
echo ".DS_Store" >> .gitignore
echo "uploads/" >> .gitignore
git add -A
git commit -m "健康小站 v2.0 — 纯前端版"

# 2. 加远程仓库（用新仓库，不是旧的 try 仓库）
git remote add origin https://github.com/你的用户名/新仓库名.git
git branch -M main
git push -u origin main

# 3. 打开 GitHub → Settings → Pages
#    Source: Deploy from a branch
#    Branch: main / root
#    Save
#    → 1-2 分钟后访问 https://你的用户名.github.io/新仓库名/
```

**国内访问加速**：注册 Cloudflare，套一层 CDN（README 有详细步骤）。

---

## 5. 当前踩过的坑（已修）

### 坑 1：双击 index.html 看不到内容

**症状**：浏览器打开 index.html 后页面正常，但视频/模块不显示

**原因**：浏览器对 `file://` 协议限制 fetch，无法加载 `content.json`

**正解**：必须用本地服务器
```bash
python3 -m http.server 8000
# 或
npx serve
```

### 坑 2：忘了 hard refresh

**症状**：改了 content.json 后浏览器没变化

**正解**：必须**硬刷新**
- Mac: `⌘ + Shift + R`
- Win: `Ctrl + Shift + R` 或 `Ctrl + F5`

### 坑 3：视频路径写错

**症状**：点视频没反应或显示 404

**检查清单**：
- 路径以 `/` 开头还是相对？用相对路径（`assets/videos/...`）
- 文件名大小写写对了吗？Mac 不区分大小写但 GitHub Pages 区分
- 文件确实上传了吗？用 `ls` 查

### 坑 4：公众号 URL 用了外链预览模式

**症状**：在 PC 浏览器打开 `mp.weixin.qq.com` 链接显示"请在微信中打开"

**正解**：这是正常的，告诉用户用手机扫码访问；PC 上只能看简版。

---

## 6. 数据流程（无后端版）

```
┌────────────────────────────────────┐
│  浏览器（用户）                     │
│  index.html 加载                    │
│    ↓                                │
│  fetch('content.json')              │
│    ↓ 拿到数据                        │
│  渲染 5 个 iframe                   │
│  （每个 iframe 自己 fetch content.json）│
│    ↓                                │
│  localStorage 保存打卡数据           │
└────────────────────────────────────┘
         ↓
    （无后端、无数据库）
```

**关键点**：
- **没有用户系统**：所有人看到的内容一样（除了 localStorage 里的个人打卡）
- **没有跨设备同步**：换电脑 = 重新开始
- **没有统计**：不知道有多少人访问（可以用 GitHub Pages 访问统计或 Cloudflare Analytics）

---

## 7. content.json 字段速查

| 字段 | 用途 | 必填 |
|------|------|------|
| `wechat` | 公众号配置（name, qrCode, intro）| ✅ |
| `teamLetter` | 一封信的内容 | ✅ |
| `diet` ~ `action` | 6+2 个模块（name, intro, pdfs, videos）| ✅ |
| `checkin` | 打卡页 UI 文案 | ✅ |
| `ui` | 主页 UI 文案（hero、sections）| ✅ |

每个模块的 `pdfs[].url` 和 `videos[].url` 支持：
- 相对路径（`assets/videos/diet/xxx.mp4`）
- 公众号文章 URL（`https://mp.weixin.qq.com/s?...`）
- 任何 https URL

前端**自动判断**：
- 扩展名是 `.mp4` / `.webm` / `.mov` / `.m3u8` → 内嵌 `<video>` 播放
- 其他 → 跳转 `<a target="_blank">` 新窗口

---

## 8. 给新会话的快速命令

```bash
# 切换到项目
cd /Users/dd/Documents/html/health-station-pure

# 启动本地预览
python3 -m http.server 8000
# 或
npx serve

# 看文件结构
find . -type f -not -path "*/.git/*" -not -name ".DS_Store" | head -30

# 看 content.json
cat content.json | python3 -m json.tool

# 推到新 git 仓库
git init
git add -A
git commit -m "init"
git remote add origin <新仓库URL>
git push -u origin main
```

---

## 9. 注意事项

- ❌ **不要**添加后端代码（除非用户明确要求）
- ❌ **不要**改 components/{tangram,domino,mortise}.html（没动过）
- ❌ **不要**改 pages/checkin.html（纯静态）
- ❌ **不要**提交大文件到 git（用 .gitignore 排除 *.mp4 *.pdf）
- ❌ **不要**用 file:// 协议预览（必须 http 服务器）
- ✅ **要做**：每次改 content.json 后**硬刷新**浏览器
- ✅ **要做**：视频优先用公众号文章链接
- ✅ **要做**：本地预览用 `python3 -m http.server`

---

## 10. 后续可能的扩展（参考）

| 需求 | 怎么做 |
|------|--------|
| 加新模块 | content.json 加新字段（如 `"oral": {...}`），HTML 也要加对应 section |
| 多语言 | content.json 加 `en: {...}` 副本，前端加语言切换器 |
| PWA 离线访问 | 加 `manifest.json` 和 service worker |
| 评论/反馈 | 嵌入腾讯兔小巢或 Formspree |
| 数据导出 | 加一个"导出 localStorage"按钮，下载 JSON |
| 加埋点 | 引入百度统计/Google Analytics 代码 |

---

**文档结束**。新会话读 §0 → §3 → §4 → §8 即可上手。
