# 健康小站

青少年健康教育纯静态网站。线上地址：https://health.hdykh-dd.com/

## 👉 网站本体在 `docs/` 文件夹

以后所有工作都在 **`docs/`** 里进行，先读这一份：

**[docs/README.md](docs/README.md)** —— 目录地图、内容怎么改、上线怎么发、改坏怎么回滚。

## 仓库其他内容

| 位置 | 说明 |
|---|---|
| `docs/` | **网站本体**（唯一需要编辑的地方） |
| `_archive/` | 历史文档、旧截图、测试文件（不参与发布，可忽略） |
| 根目录的 `index.html` 等 | 旧的发布副本，切换发布源后会删除，暂时不要动 |

## 发布说明

网站通过 GitHub Pages 发布。发布源需要设置为 **`main` 分支的 `/docs` 目录**
（仓库 → Settings → Pages → Source → Deploy from a branch → `main` / `/docs`）。

设置完成前，网站仍然从仓库根目录发布，两边内容目前完全一致。
