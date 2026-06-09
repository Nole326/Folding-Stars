# Folding Stars

Folding Stars 是一个低饱和马卡龙风格的个人备忘录网页应用。它把待办、备忘、情绪记录和“折星星”成就机制结合在一起：用户创建便签、拖拽摆放、编辑内容，完成后便签会折叠成星星并被收入星星罐。应用支持账号登录、云端同步、图片上传、个人资料、星星收集展示，以及 iPhone 主屏幕 PWA 使用。

## 项目定位

这个项目不是传统效率工具，而是一个带有情绪陪伴感的轻量备忘录。它希望把“完成一件小事”转化成可视化奖励：每完成一条备忘录，就收集一颗星星。十颗星星装满一瓶，形成持续记录和自我鼓励的反馈。

整体视觉采用奶油米白背景、低饱和马卡龙色块、圆润无衬线字体和轻微颗粒肌理。界面中的星形、圆形、方形、六边形、多边形、爱心形和不规则形便签都带有柔和投影，背景几何图案可互动，形成干净、松弛、带童趣的使用体验。

## 功能特性

- 用户注册与登录
- 本地与 Supabase 云端数据同步
- 个人资料编辑
- 默认动物头像与自定义头像
- 备忘录新建、编辑、删除
- 标题、正文、分类标签、天气、心情记录
- 便签图片上传与展示
- 多种便签形状与马卡龙配色
- S / L 背景板模式切换
- 便签自由拖拽与物理排斥布局
- 完成便签后折叠成星星
- 星星罐成就展示
- 已完成星星记录管理
- 搜索便签并定位未完成便签
- iPhone 主屏幕 PWA 支持
- 轻量提醒功能

## 页面结构

### 登录 / 注册页

登录页用于进入个人空间。注册时可以选择默认动物头像，也可以上传本地图片作为头像。账号数据通过 Supabase Auth 管理。

### Remember 页

Remember 是核心备忘录画布。用户可以通过右上角加号新建便签，并选择：

- 形状
- 颜色
- 天气
- 心情
- 分类标签
- 标题
- 备忘录内容
- 图片
- 提醒时间

便签会出现在画布上，支持拖拽移动。完成后点击 `Finish`，便签会转化为星星并进入星星罐。

### 我的页面

我的页面包含：

- 个人资料卡片
- 头像、用户名、性别、签名
- 我的星星收集罐
- 星星收集进度
- 搜索便签
- 提醒设置

## 技术栈

项目目前是一个静态前端 PWA，主要由以下部分组成：

- HTML
- CSS
- JavaScript
- Service Worker
- Web App Manifest
- Supabase Auth
- Supabase Database
- Supabase Storage
- 腾讯云 CloudBase 静态部署

项目没有使用复杂前端框架，主体逻辑集中在 `index.html` 中，便于直接部署和维护。

## 数据存储

项目使用 Supabase 保存用户数据。

主要数据包括：

- 用户账号
- 用户个人资料
- 未完成便签
- 已完成星星
- 星星数量
- 背景板模式
- 图片资源

核心工作区数据存储在 `folding_workspaces` 中。图片可通过 Supabase Storage 保存。

## 提醒功能说明

当前版本采用轻量提醒方案。

提醒逻辑由前端完成：当应用处于前台、保留在后台、锁屏后仍被系统保留，或用户重新进入应用时，程序会检查到期便签并弹出通知。

需要注意：

- iPhone 主屏幕 PWA 可以使用通知权限。
- 如果用户把 PWA 从后台强制划掉，iOS 通常不会继续运行网页逻辑。
- 因此，删除后台后无法保证提醒送达。
- 项目曾尝试接入 Supabase Edge Function + Web Push，但在 iOS PWA 强制关闭场景下仍不能稳定解决问题，因此当前版本回归轻量提醒，以减少后端资源消耗。

推荐使用方式：

> 为保证提醒效果，请不要从后台强制关闭 Folding Stars。保持应用在后台或锁屏状态，提醒会更加稳定。

## PWA 支持

项目支持作为 PWA 添加到 iPhone 主屏幕。

需要包含：

- `manifest.webmanifest`
- `service-worker.js`
- `icons/icon-192.png`
- `icons/icon-512.png`
- `icons/apple-touch-icon.png`

iPhone 使用方式：

1. 使用 Safari 打开部署后的网页。
2. 点击分享按钮。
3. 选择“添加到主屏幕”。
4. 从桌面图标打开 Folding Stars。

## 部署方式

项目可直接作为静态网页部署。

推荐部署平台：

- 腾讯云 CloudBase
- Vercel
- Netlify
- GitHub Pages
- 任意静态网站服务器

如果使用腾讯云 CloudBase，上传内容应包含：

```text
index.html
manifest.webmanifest
service-worker.js
icons/
```

如果项目中包含额外说明文档或 Supabase SQL 文件，它们不一定需要上传到静态部署目录。

## 本地运行

项目可以直接打开 `index.html` 查看界面，但为了测试 Service Worker、PWA 和通知功能，建议通过本地服务器运行。

例如：

```bash
npx serve .
```

或使用任意静态服务器。

## Supabase 配置

项目需要 Supabase 项目提供：

- Project URL
- Publishable Key
- Auth
- Database
- Storage

前端代码中需要配置 Supabase 地址与 publishable key。

示例：

```js
const SUPABASE_URL = "你的 Supabase Project URL";
const SUPABASE_PUBLISHABLE_KEY = "你的 Supabase publishable key";
```

不建议把 service role key 写入前端。

## 目录结构示例

```text
folding-stars/
├─ index.html
├─ manifest.webmanifest
├─ service-worker.js
├─ icons/
│  ├─ icon-192.png
│  ├─ icon-512.png
│  └─ apple-touch-icon.png
├─ supabase/
│  ├─ sql/
│  └─ functions/
└─ README.md
```

## 设计说明

Folding Stars 的视觉核心是“柔和、轻量、治愈”。设计上避免强烈对比和复杂装饰，使用大面积平面色块、圆润按钮和轻微颗粒背景，营造类似手账与折纸的氛围。

便签不是简单列表，而是被放置在一个可自由整理的画布中。用户可以通过移动、选择形状、选择颜色，把备忘录变成一个更个人化的视觉对象。完成动作不只是删除任务，而是把它转化为星星收藏，从而让完成感被保留下来。

## 已知限制

- iOS PWA 被用户强制划掉后台后，无法保证通知送达。
- 项目是单页静态应用，复杂多人协作能力有限。
- 大量图片会增加本地缓存和云端存储压力。
- Service Worker 更新后，iPhone 可能需要刷新网页或重新打开主屏幕应用才能获取最新版本。

## 后续优化方向

- 更完整的图片压缩与存储管理
- 星星罐历史瓶子展示
- 日 / 周 / 月完成统计
- 标签筛选与批量管理
- 更细腻的拖拽碰撞与布局算法
- 原生 iOS App 封装
- Android PWA Web Push 支持
- 数据导出与恢复

## 项目状态

Folding Stars 目前已经具备完整的个人备忘录使用流程，适合部署为个人网页应用或 iPhone 主屏幕 PWA 使用。
