# 🍋 奶龙大笑模拟器

一个极简的网页小玩具：点一下奶龙，它就开始上下抖动、播放正宗的魔性大笑；再点一下停止。

## 这是做什么的

致敬 2026 年开年最魔性的梗——**大笑奶龙**。网页中间放一张奶龙大笑贴图，
点击后：

1. 奶龙开始上下抖动（CSS 动画，无需任何图片帧）
2. 播放从 B 站原版视频里提取的"八级哥"魔性笑声
3. 屏幕上不停飘出"哈哈哈哈哈"
4. 按钮下方累计"今日已开笑 N 次"

## 怎么运行

纯静态页面，无需安装任何依赖：

- 直接双击 `index.html` 用浏览器打开即可；
- 或者起一个静态服务器：`python -m http.server 8000`，然后访问 <http://localhost:8000>。

> 注意浏览器自动播放策略：第一次需要用户点击页面（正好我们的交互就是点击，天然没问题）。

## 使用了什么 Agent

使用 **WorkBuddy**（AI Agent）完成开发：

- 用 Agent 在 B 站检索"奶龙大笑【原版】"视频（BV1p2Tp6REay），用 yt-dlp 下载音频轨
- 用 ffmpeg 剪掉头尾静音、转成 mp3（`assets/laugh.mp3`，约 15.7 秒）
- 从原视频截帧 + 裁剪掉黑边和平台水印，得到奶龙大笑贴图（`assets/nailong.png`）
- 编写页面 HTML/CSS/JS

## 开发过程

采用 `main` / `dev` / `feature/xxx` 三分支流程：

```
main（稳定版）
 └── dev（开发整合）
      ├── feature/laugh-audio   （大笑音频）    → 已合并
      ├── feature/jump-shake    （抖动交互）    → 已合并
      └── feature/laugh-counter （大笑计数器）  → 通过 PR 合并
```

其中 `feature/laugh-counter` 是在 Gitee 上走 **Pull Request** 合并进 `dev` 的，
用来完整体验"分支开发 → PR → 合并"的流程。

主要提交记录：

```
b4e040c feat: 初始化奶龙大笑模拟器页面骨架
235109e feat: 添加B站原版提取的奶龙大笑音频，点击播放
9ca75a4 feat: 点击时奶龙上下抖动，并飘出哈哈哈哈
999eb3f feat: 新增大笑次数计数器
a33d480 docs: 补充 README 和开发复盘
```

详见 [开发复盘.md](开发复盘.md)。

## 目录结构

```
├── index.html          # 页面本体
├── assets/
│   ├── nailong.png     # 奶龙大笑贴图（视频截帧）
│   └── laugh.mp3       # 奶龙大笑音频（B站原版提取）
├── README.md
└── 开发复盘.md
```

## 免责声明

本项目仅用于课程作业与技术学习。奶龙形象及笑声素材版权归原作者所有，
音频来源：B站视频 [奶龙大笑【原版】](https://www.bilibili.com/video/BV1p2Tp6REay)。
