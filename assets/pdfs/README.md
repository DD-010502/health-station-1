# PDF 目录

把 PDF 手册放在这里，按模块分子目录：

```
pdfs/
├── diet/        # 营养饮食手册
├── exercise/    # 积极运动手册
├── sleep/       # 良好睡眠手册
├── screen/      # 合理视屏手册
├── habits/      # 禁烟禁酒手册
├── mental/      # 心理健康手册
├── loop/        # 环环相扣手册
└── action/      # 计划与行动手册
```

**两种使用方式**：

1. **放在本地**：上传 PDF 到对应子目录，在 `content.json` 写：
   ```json
   "pdfs": [{ "id": "diet-pdf-1", "title": "...", "url": "assets/pdfs/diet/xxx.pdf" }]
   ```

2. **用公众号文章链接**：把 PDF 内容做成公众号文章，URL 写：
   ```json
   "url": "https://mp.weixin.qq.com/s?__biz=xxx&mid=yyy"
   ```
