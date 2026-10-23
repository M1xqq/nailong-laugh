# 🍋 奶龙大笑模拟器

一个极简的网页小玩具：点一下奶龙，它就开始上下抖动、播放正宗的魔性大笑；再点一下停止。

## 这是做什么的

致敬 2026 年开年最魔性的梗——**大笑奶龙**。网页中间放一张奶龙大笑贴图，
点击后：

1. 奶龙开始上下抖动（CSS 动画，无需任何图片帧）
2. 播放从 B 站原版视频里提取的"八级哥"魔性笑声
3. 屏幕上不停飘出"哈哈哈哈哈"
4. 四角各有一只跳舞的奶龙动图（来自 [Nannan-1234](https://github.com/nannan-1234) 的 PR）
5. 底部实时显示本站累计被围观次数
6. 计数器下方一行红字「点击左下角一口气看到大结局」，配一个朝左下的摆动箭头做视线引导
7. 左下角挂着一块招牌「上海奶龙大学.com」，点进去是**上海工程技术大学**官网
   （学朋友网站的做法，纯属整活，与该校无关）
8. 底部有本站域名的署名：`本站"上海奶隆工程技术大学.club" is made by M1xqq`

> 第 6、7 条是刻意做得土一点：红字、宋体、默认蓝色下划线链接。小网站就得有小网站的样子。

## 怎么运行

### 方式一：零部署，双击就玩（最简单）

直接下载 **`奶龙大笑模拟器.html`**（图片和音频已内联进这一个文件里），
双击用浏览器打开即可，不需要服务器、不需要联网。

### 方式二：本地运行

纯静态页面，无需安装任何依赖：

- 直接双击 `index.html` 用浏览器打开即可（需保证 `assets/` 目录和它同级）；
- 或者起一个静态服务器：`python -m http.server 8000`，然后访问 <http://localhost:8000>。

> 注意浏览器自动播放策略：第一次需要用户点击页面（正好我们的交互就是点击，天然没问题）。

> ⚠️ 本地调试声音时有个坑：`python -m http.server` **不支持 Range 请求**，
> 浏览器 seek 音频会失败，表现为"点了没声音"。想本地听声音，直接双击
> `奶龙大笑模拟器.html`（无服务器、无 iframe），或者用 nginx / `npx serve` 之类
> 支持 `206 Partial Content` 的服务器。线上站点用的是 Cloudflare，无此问题。
### 方式三：docker部署(Nannan-1234贡献)

适合linux环境和服务器。欢迎将其部署到你的服务器上。

暂未发布镜像，使用源码构建：

```bash
git clone https://github.com/m1xqq/nailong-laugh.git
cd nailong-laugh
docker build -t nailong-laugh .

```
或通过compose构建：
```bash
   docker compose up -d --build
```
compose.yml
```yaml
services:
  web:
    build: .
    image: nailong-laugh
    container_name: nailong_laugh
    ports:
      - "8080:80"
    restart: unless-stopped
```
运行：
```bash
docker compose up -d
```
访问：
<http://localhost:8080>，或者，你的服务器ip:8080

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
      ├── feature/laugh-audio   （大笑音频）        → 已合并
      ├── feature/jump-shake    （抖动交互）        → 已合并
      ├── feature/laugh-counter （大笑计数器）      → 通过 PR 合并
      ├── feature/view-counter  （访问量统计）      → 已合并
      └── feature/ending-tip    （大结局标语 + 伪链接 + 署名）→ 通过 PR 合并
```

其中 `feature/laugh-counter` 和 `feature/ending-tip` 都是在 Gitee 上走 **Pull Request**
合并的，用来完整体验"分支开发 → PR → 合并"的流程。

主要提交记录：

```
b4e040c feat: 初始化奶龙大笑模拟器页面骨架
235109e feat: 添加B站原版提取的奶龙大笑音频，点击播放
9ca75a4 feat: 点击时奶龙上下抖动，并飘出哈哈哈哈
999eb3f feat: 新增大笑次数计数器
a33d480 docs: 补充 README 和开发复盘
058b6b2 feat: 增加网站访问量统计（不蒜子，失败时降级为本机计数）
84509b6 Merge pull request #1 from nannan-1234/main   ← 朋友贡献四角动图
feb7a09 feat: 加大结局引导标语、左下角伪链接与底部署名
        + 修复音频播放顺序、本地环境计数虚高、小屏裁切
```

详见 [开发复盘.md](开发复盘.md)。

## 线上地址

| 地址 | 说明 |
|---|---|
| **https://上海奶隆工程技术大学.club** | 主站，Cloudflare Pages + 自定义中文域名 |
| https://nailong-laugh.pages.dev | Cloudflare Pages 默认域名 |
| https://m1xqq.github.io/nailong-laugh/ | GitHub Pages（备用，国内访问更稳时可用） |

代码同时托管在两个远端：`origin` → Gitee，`github` → GitHub，`main` / `dev` 双推。

## 怎么部署（Cloudflare Pages）

纯静态页面，不需要构建步骤：

```bash
npm i -g wrangler
wrangler pages deploy dist --project-name=nailong-laugh
```

`dist/` 里就是 `index.html` + `assets/` + 单文件版，直接上传即可。
自定义域名在 Pages 项目的「自定义域」里绑定，DNS 加一条
`CNAME  @ → nailong-laugh.pages.dev`（代理状态开启），证书由 Cloudflare 自动签发。

> 为什么不用 Gitee Pages：新仓库默认不显示该入口，且必须完成实名认证。
> 想用 Gitee Pages 也可以，部署分支选 `main`、目录留空即可。

## 目录结构

```
├── index.html              # 页面本体
├── 奶龙大笑模拟器.html      # 单文件版（图片音频已内联，双击即玩）
├── assets/
│   ├── nailong.png         # 奶龙大笑贴图（视频截帧）
│   ├── laugh.mp3           # 奶龙大笑音频（B站原版提取）
│   └── image.gif           # 四角跳舞奶龙（朋友 PR 贡献）
├── Dockerfile              # 容器化部署
├── compose.yaml
├── README.md
└── 开发复盘.md
```

## 免责声明

本项目仅用于课程作业与技术学习。奶龙形象及笑声素材版权归原作者所有，
音频来源：B站视频 [奶龙大笑【原版】](https://www.bilibili.com/video/BV1p2Tp6REay)。
