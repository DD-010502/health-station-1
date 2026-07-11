# 视频目录

把视频文件放在这里，按模块分子目录：

```
videos/
├── diet/        # 营养饮食视频
├── exercise/    # 积极运动视频
├── sleep/       # 良好睡眠视频
├── screen/      # 合理视屏视频
├── habits/      # 禁烟禁酒视频
├── mental/      # 心理健康视频
├── loop/        # 环环相扣视频
└── action/      # 计划与行动视频
```

**两种使用方式**：

1. **放在本地**：上传 mp4 文件到对应子目录，然后在 `content.json` 写：
   ```json
   "videos": [{ "id": "diet-vid-1", "title": "...", "url": "assets/videos/diet/xxx.mp4", "poster": "..." }]
   ```

2. **用外链**（推荐）：把视频放在公众号文章里，URL 写：
   ```json
   "url": "https://mp.weixin.qq.com/s?__biz=xxx&mid=yyy"
   ```
   用户点击时跳转到微信观看。
