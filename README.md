# 健康小站

这是一个用于青少年健康教育的纯静态网站项目。

## 本地预览

```bash
python3 -m http.server 8000
# 浏览器访问 http://localhost:8000
```

## 更新内容

编辑 `content.json`，所有文字 / 视频 / PDF 都通过这个文件配置。

上传文件后：

```bash
git add -A
git commit -m "更新内容"
git push
```

详细说明见 `DEPLOYMENT_NOTES.md`。
