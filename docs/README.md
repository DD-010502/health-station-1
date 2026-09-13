# 健康小站 · 工作说明

> **这个文件夹就是网站本体。** 以后只需要在这里工作，外面的文件都不用管。
> 线上地址：https://health.hdykh-dd.com/

---

## 1. 目录地图

| 文件 / 文件夹 | 作用 | 平时要不要动 |
|---|---|---|
| `content.json` | ★ **所有文字、PDF、视频都写在这里** | 🔴 经常动 |
| `index.html` | 首页（一封信 / 七巧板 / 多米诺 / 一锤入榫 / 每日打卡） | 🟡 偶尔动样式 |
| `pages/health-detail.html` | 6 个模块的知识页（点七巧板进入） | 🟡 偶尔动 |
| `pages/health-loop.html` | 环环相扣（点多米诺进入） | 🟡 偶尔动 |
| `pages/mortise.html` | 计划与行动（点榫卯进入） | 🟡 偶尔动 |
| `pages/checkin.html` | 打卡页的科普说明 | 🟢 基本不动 |
| `components/*.html` | 首页 5 个互动板块（被首页内嵌） | 🟢 基本不动 |
| `assets/pdfs/` | 放 PDF 手册 | 🟡 上传文件时动 |
| `assets/videos/` | 放视频封面图 | 🟡 上传图片时动 |
| `assets/images/` | 首页大图、插画 | 🟢 基本不动 |
| `CNAME` | 绑定域名用，**不要删** | ⛔ 绝对不动 |
| `.nojekyll` | 让 GitHub 直接托管文件，**不要删** | ⛔ 绝对不动 |

---

## 2. 想改内容？90% 都改 `content.json`

打开 `content.json`，找到对应位置改就行，不用碰任何页面代码。

**常用位置：**

| 想改什么 | 在 content.json 里找 |
|---|---|
| 某个模块的卷首语 / 介绍 | 该模块的 `intro` |
| 一封信的内容 | `teamLetter` |
| 首页大标题 | `ui.heroTitleHtml` |
| 首页板块标题 | `ui.sections` |
| 打卡页文案 | `checkin` |
| 公众号信息 | `wechat` |

**PDF 和视频的填写规则（很重要）：**

- 一个 PDF / 视频只有在 **`title` 和 `url` 都填了** 的情况下才会显示成卡片；留空 = 不显示。
- PDF 文件放到 `assets/pdfs/<模块>/`，`url` 就写 `assets/pdfs/<模块>/文件名.pdf`
- 视频封面图放到 `assets/videos/<模块>/`，`cover` 写 `assets/videos/<模块>/文件名.png`
- 视频本身建议用**外链**（公众号文章 / B 站），不要把大视频文件塞进仓库，仓库会变重、网站会变慢。
- `<模块>` 只能是这几个名字：`diet` `exercise` `sleep` `screen` `habits` `mental` `loop` `action`

---

## 3. 目前内容填充情况

PDF 手册已上传 **33 份**（2026-09-13）；视频目前只有心理健康有 2 条。

| 模块 | PDF | 视频 |
|---|---|---|
| 营养饮食 `diet` | ✅ 10 份 | ⚠️ 1 条示例数据（待替换） |
| 积极运动 `exercise` | ✅ 3 份 | 空 |
| 良好睡眠 `sleep` | ✅ 5 份 | 空 |
| 合理视屏 `screen` | ✅ 1 份 | 空 |
| 禁烟禁酒 `habits` | ✅ 6 份 | 空 |
| 心理健康 `mental` | ✅ 4 份 | ✅ 2 个（公众号 + B 站） |
| 环环相扣 `loop` | 空 | 空 |
| 计划与行动 `action` | ✅ 4 份 | 空 |

> 每个模块的 PDF / 视频上限各 10 条，是上限不是目标。

---

## 4. 已知问题（待修）

1. **营养饮食的视频"营养早餐怎么搭？"是假数据** —— 作者写的是"示例作者"，链接是微信示例参数，不是真文章。
2. **公众号二维码没接上** —— `content.json` 里预留了二维码位置，但图片文件不存在，而且目前没有任何页面去读取它。

修法都很简单：问题 1 要么补上真链接，要么把 `title` 和 `url` 清空（卡片就不显示了）；问题 2 需要先拿到二维码图片。

---

## 5. 本地预览

在 `docs` 文件夹里执行：

```bash
python3 -m http.server 8000
```

然后浏览器打开 http://localhost:8000

（必须这样预览，直接双击 `index.html` 打开会看不到内容，因为网页需要读取 `content.json`。）

---

## 6. 发布到线上

改完以后：

```bash
git add -A
git commit -m "更新内容"
git push
```

推送后大约 1–10 分钟线上生效（Cloudflare 有缓存时会久一点）。

---

## 7. 手机端适配规矩

手机端的改动**全部集中在**每个页面末尾的这段里：

```css
@media (max-width: 700px) { ... }
```

- 屏幕宽度 **大于 700px（电脑）完全不受影响**，这是刻意的，别破坏它。
- 改手机端时，只在上面这个块**里面**加规则。
- ⚠️ **这个块必须放在 `<style>` 的最末尾**。CSS 里同样优先级的规则「后写的赢」，如果手机端块写在前面，会被后面出现的基础样式悄悄覆盖，等于白写——2026-09-12 修过一次这个坑，别踩回去。
- 子页面（`components/*.html`）在手机上让 `html, body` 高度跟随内容，首页据此自动收紧 iframe 高度；电脑端仍用原来的固定高度逻辑。
- 容易踩的坑：七巧板 SVG 被切掉、详情页目录重叠、多米诺煤油灯不消失、字体变成方块字（Google Fonts 加载失败）—— 这些历史修法都记在 `_archive/legacy-docs/HANDOVER_MOBILE.md` 里。

---

## 8. 改坏了怎么回到之前

项目里存了几个"存档点"，可以一键回到过去：

```bash
git tag --list                     # 看有哪些存档点
git reset --hard <存档点名字>       # 回到那个版本
git push --force                   # 覆盖线上
```

现有的存档点：`pre-docs-restructure`（本次整理之前）、`before-mobile-adaptation` 到 `-v7`（手机端各轮改动之前）。

> ⚠️ `git push --force` 会覆盖远程历史，执行前先确认清楚。

---

## 9. 历史文档在哪

旧的交接文档、部署方案、待办清单都挪到了仓库根目录的 `_archive/` 里，它们描述的是**已经废弃的"带后台服务器"方案**，只在需要考古时翻一翻：

- `_archive/legacy-docs/HANDOVER.md` —— 早期总交接
- `_archive/legacy-docs/HANDOVER_MOBILE.md` —— 手机端适配详录（**这个还有参考价值**）
- `_archive/legacy-docs/DEPLOYMENT*.md` —— 旧的后端部署方案（已不适用）
- `_archive/legacy-docs/TODO.md` —— 旧的待办清单（已过时）

---

## 10. 网站架构一句话

纯静态网站，没有后台、没有数据库、零运维。所有内容由一个 JSON 文件驱动，托管在 GitHub Pages，套了 Cloudflare。打卡数据只存在访客自己浏览器的本地存储里。
