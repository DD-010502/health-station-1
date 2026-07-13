# 手机端适配 · 交接文档

> 写给下一个会话：**纯前端、无后端、GitHub Pages 部署** 的健康小站手机端适配完善指南。
> 
> **承接日期**：2026-07-12
> **远程 HEAD**：`61cea2d`
> **线上 URL**：`https://health.hdykh-dd.com/`

---

## 0. 一句话总结

健康小站（DD-010502/health-station-1）已完成 **v1-v7 共 7 轮手机端适配**。

- ✅ 电脑端（>700px）**完全不变**
- ✅ 手机端（<700px）所有改动都在 `@media (max-width: 700px)` 块内
- ✅ 所有改动可**一键回滚**到任何一个 tag

---

## 1. 项目基础信息

| 项 | 值 |
|----|----|
| 仓库 | https://github.com/DD-010502/health-station-1 |
| 线上 URL | `https://health.hdykh-dd.com/` |
| 备用 URL | `https://dd-010502.github.io/health-station-1/` |
| 远程 HEAD | `61cea2d` |
| CDN | Cloudflare 台北/东京节点 |
| SSL | 已签发（subject=CN=hdykh-dd.com） |
| 缓存 | 暂未配 Cloudflare 缓存规则（push 后要等 10 分钟） |

---

## 2. 7 轮改动的回滚 tag

**重要**：每轮改动都保留了一个 git tag，可以随时回滚。

| Tag | 内容 | HEAD 状态 |
|-----|------|----------|
| `before-mobile-adaptation` | 原始（v0，未做手机适配）| v0 |
| `before-mobile-adaptation-v2` | v1 改完前 | v1 改完前 |
| `before-mobile-adaptation-v3` | v2 改完前 | v2 改完前 |
| `before-mobile-adaptation-v4` | v3 改完前 | v3 改完前 |
| `before-mobile-adaptation-v5` | v4 改完前 | v4 改完前 |
| `before-mobile-adaptation-v6` | v5 改完前 | v5 改完前 |
| `before-mobile-adaptation-v7` | v6 改完前 | **当前 HEAD（v7）改完前** |

**回滚某个版本**（如回到 v6）：
```bash
cd /Users/dd/Documents/html/health-station-pure
git reset --hard before-mobile-adaptation-v7   # 回滚到 v6
git push --force
```

---

## 3. 7 轮改动总结

### v1：基础移动端适配（commit b9d6f53）
- index.html 加 hero/iframe/footer
- 3 个详情页加 @media (max-width: 700px)
- components/tangram.html SVG 自适应
- 桌面端完全不变

### v2：修 4 个手机 bug
- Section 标题溢出
- 七巧板只看到第一个（太挤）
- mortise 动画显示不全
- 多米诺骨牌 2 列横排

### v3：又 3 个微调
- 七巧板 1fr → 1 列（你说"太小"）
- toc 加不透明背景
- mortise 隐藏 joyride.png

### v4：又 4 个微调
- 七巧板 2 列（你说"再缩小"）
- toc 不滑动 + background #FFF
- iframe 高度 380→220
- 字体全局继承 (`*, *::before, *::after { font-family: inherit !important }`)

### v5：再 4 个微调
- 七巧板再缩 (.fly width: 70%)
- 多米诺删煤油灯 + 箭头单独一行
- mortise corner-link margin-top 18→8
- toc z-index: 1

### v6：多米诺排版优化
- 文字居中
- 文字行高 1.6
- linkText 缩短

### v7（当前 HEAD 61cea2d）：多米诺进一步缩短
- scene padding 12→8
- scene gap 8→4
- 牌 min-height 70→56
- 牌 column-gap 3→2
- corner-link margin-top 0→4
- arrow 24×24

---

## 4. 当前 4 个文件的 @media 块位置

| 文件 | @media 行号 | 内容 |
|------|------------|------|
| `index.html` | 约 line 237 | Hero/iframe/footer/章节间隔 |
| `pages/health-detail.html` | 约 line 900+ | toc + 卡片 + 弹窗 |
| `components/tangram.html` | 约 line 150 | 2 列 SVG 紧凑 |
| `components/domino.html` | 约 line 165 | 6 张牌 + 删煤油灯 + 文字居中 |
| `components/mortise.html` | 约 line 100+ | 铁锤/榫卯/链接 |

**所有改动**：
- 只在 `@media (max-width: 700px) { ... }` 内部
- 桌面端（>700px）**完全不变**
- 移动端（<700px）改 padding / margin / font-size / grid-template-columns 等

---

## 5. 常见问题（必须避免的坑）

### 5.1 七巧板 SVG 被切

**症状**：第二列只显示右半截  
**原因**：
- `.pieces { grid-template-columns: repeat(2, ...) }` 没限制 SVG 宽度
- 内部 `.fly svg` 没设 `max-width: 100%`

**修法**（在 @media 内）：
```css
.pieces { grid-template-columns: repeat(2, minmax(0, 1fr)); }
.fly { width: 70%; max-width: 110px; margin: 0 auto 3px; }
.fly svg { width: 100% !important; height: auto !important; max-width: 100%; }
```

### 5.2 health-detail toc 重叠

**症状**：toc 和下面的 PDF 卡片视觉重叠  
**原因**：`position: sticky` + 半透明背景 → 看穿

**修法**：
```css
.toc {
  position: static;          /* 改为不滑动 */
  background: #FFFFFF;
  border: 2px solid rgba(43, 42, 40, .25);
  border-radius: 14px;
  box-shadow: 0 4px 16px rgba(43, 42, 40, .15);
  z-index: 1;
  margin-bottom: 32px;       /* 加大间距 */
}
```

### 5.3 多米诺煤油灯不消失

**症状**：手机上煤油灯仍占位置  
**修法**：
```css
.lamp-wrap { display: none !important; }
```

### 5.4 字体不统一

**症状**：手机上有些字是方块字（Google Fonts 加载失败）  
**修法**：在每个 mobile @media 块开头加：
```css
*, *::before, *::after {
  font-family: inherit !important;
}
```

### 5.5 iframe 底部空白

**症状**：iframe 内容很矮但整个 iframe 区域很高  
**修法**：
```css
iframe.frame { min-height: 220px; height: 320px; }
```

### 5.6 章节之间空白太多

**症状**：每个 section 之间有大段空白  
**修法**：
```css
.chapter-anchor { padding: 12px 16px 6px; min-height: 28px; font-size: 17px; }
.section-wrap { margin: 0; padding: 0; }
```

### 5.7 编辑/回滚顺序

每次改完手机端前**先建回滚 tag**：
```bash
git tag before-mobile-adaptation-v8
# ... 编辑 ...
git add -A && git commit -m "fix(mobile-v8): ..."
git push origin main
```

回滚：
```bash
git reset --hard before-mobile-adaptation-v8
git push --force
```

---

## 6. 关键文件内容

### 6.1 content.json 路径规律

- PDF 路径：`assets/pdfs/<模块>/文件名.pdf`
  - 例：`assets/pdfs/habits/应对话术.pdf`
- 视频路径：通常用**外链**（公众号文章 / B 站）
  - 例：`https://mp.weixin.qq.com/s/xxx`
- 视频封面：`assets/videos/<模块>/<图片>.png`

**前端用 resolveUrl() 自动处理**：
- 在 `pages/health-detail.html`、`pages/health-loop.html`、`pages/mortise.html` 中
- 函数逻辑：计算从当前页面到仓库根的相对路径（如 `../../`）
- 兼容 GitHub Pages 子路径 `/health-station-1/`

### 6.2 已上传资源清单

```
7 张图片：hero, title1-3, team, mona-lisa, joyride
1 PDF：habits/应对话术.pdf (2.0MB)
2 视频封面：mental/image1.png, image2.png
2 视频链接：mental/vid-1（公众号）, mental/vid-2（B 站）
```

### 6.3 模块状态

- `habits`：1 PDF（应对话术）
- `mental`：2 视频（运动睡眠/为什么安静）
- `diet`/`exercise`/`sleep`/`screen`/`loop`/`action`：**空待填**
- `teamLetter`：信内容（content.json teamLetter）

---

## 7. 待办（未来会话可能要做）

按优先级：

| 优先级 | 任务 | 备注 |
|--------|------|------|
| 🥇 | **配 Cloudflare 缓存规则**（HTML 不缓存）| 改完 push 后立刻看到新内容 |
| 🥇 | **为每个模块填 PDF 和视频**（还差 7 个模块）| 改 content.json + 上传文件 |
| 🥈 | **加公众号二维码**到 footer | 提升引流 |
| 🥈 | **health-detail toc 优化**：考虑加"返回顶部"按钮 | 长页面体验 |
| 🥉 | **mortise 排版优化**：榫卯与链接间距更紧凑 | 用户反馈过 |
| 🥉 | **首页 section-corner-link** 移动端样式 | 已加 8 16 16px 紧凑 |
| 🕐 | **删除 before-mobile-adaptation-v1 到 v7 的 tag**（已不再需要）| 之后 v8+ |
| 🕐 | **GitHub Pages 缓存规则**：等所有改动稳定后再说 | - |

---

## 8. 服务器/域名/SSL 状态

| 项 | 状态 |
|----|------|
| 域名 | `hdykh-dd.com`（阿里云/万网）|
| Cloudflare NS | ✅ 已切到 bjorn.ns/cass.ns.cloudflare.com |
| Cloudflare SSL | ✅ Full（已签发 subject=CN=hdykh-dd.com 证书）|
| GitHub Pages Custom domain | ✅ health.hdykh-dd.com（DNS check successful）|
| Enforce HTTPS | ✅ 已勾上 |
| CAA 记录 | ✅ 2 条（issue + issuewild，letsencrypt.org）|
| Cloudflare 缓存规则 | ❌ 未配（push 后要等 10 分钟）|

**未来如要配缓存规则**：在 Cloudflare Dashboard → Caching → Cache Rules → 加 3 条：
1. URI Path contains `.html` → Cache eligible: No
2. URI Path ends with `/content.json` → Cache eligible: No
3. URI Path starts with `/assets/` → Cache eligible: Yes, Edge TTL: 30 days

---

## 9. 已知 bug 和改进

- **Google Fonts 偶发加载失败**：用 `font-family: inherit !important;` 兜底（v4 修过）
- **toc 在 iframe 内时不显示**：因为 toc 嵌在 health-detail.html 的 @media 内
- **内容全部写死在 content.json**：没有 Web CMS，加新模块要手动改 JSON

---

## 10. 未来会话快速上手

如果下次打开项目不知道从哪开始：

1. **看现状**：
   ```bash
   cd /Users/dd/Documents/html/health-station-pure
   git log --oneline -5
   git tag --list | tail -10
   ```

2. **看最近改了什么**：
   ```bash
   git show HEAD --stat
   ```

3. **看 mobile 适配位置**：
   ```bash
   grep -n "@media (max-width: 700px)" components/*.html pages/*.html index.html
   ```

4. **回滚到任意版本**：
   ```bash
   git reset --hard before-mobile-adaptation-v7 && git push --force
   ```

5. **新建回滚 tag**：
   ```bash
   git tag before-mobile-adaptation-v8
   ```

---

## 11. 联系信息

- GitHub 用户：DD-010502
- 远程仓库：https://github.com/DD-010502/health-station-1
- 本地项目路径：/Users/dd/Documents/html/health-station-pure
- 7 个回滚 tag：before-mobile-adaptation / v2 / v3 / v4 / v5 / v6 / v7

---

**这份文档是"未来手机端适配"的完整指南**。如果之后要继续完善：
- 读这份文档能快速了解现状
- 用 git tag 快速回滚到任意历史版本
- 改 CSS 时严格只在 `@media (max-width: 700px)` 块内加规则

辛苦了 🎯
