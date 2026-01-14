# 前端项目搭建 - Next.js

Next.js 多端基线与落地清单，覆盖技术选型、兼容、SEO、布局、性能、安全与目录示例。

## 一、项目背景

基于 Next.js 构建多端适配的现代化 Web 应用，目标：PC/移动一致体验、优异性能、主流浏览器/设备兼容，同时兼顾可维护性、扩展性与规范化。

## 二、技术选型

### 1. 底层运行环境

| 名称 | 作用 | 优势/适用场景 |
| --- | --- | --- |
| Node.js | 运行时，支撑 Next.js SSR/构建 | 生态成熟，性能可支撑 SSR/边缘渲染，前后同栈 |
| pnpm | 包管理器，支持 workspace | 硬链接省磁盘、安装快、依赖去重好，适配 monorepo |

### 2. 框架与样式

| 名称 | 作用 | 优势/适用场景 |
| --- | --- | --- |
| Next.js | React 全栈框架（App Router/SSR/SSG/ISR） | 内置路由、`next/image` 优化、Edge/Server Actions，SEO 友好 |
| Monorepo（pnpm workspace） | 管理多包/多应用 | 共享依赖与配置、组件复用、统一脚本；用 `pnpm --filter` 调度 |
| Tailwind CSS | 原子化 CSS | 开发快、设计一致性好、tree-shaking 体积小，断点/暗色模式内置 |
| i18n | 多语言 | 结合 `app/[locale]` 路由或 `next-intl`/`next-i18next` |

### 3. 工程支撑

| 名称 | 作用 | 优势/适用场景 |
| --- | --- | --- |
| ESLint | 代码质量检查 | 捕获常见错误，统一风格，支持自定义规则 |
| Prettier | 代码格式化 | 一键统一格式，降低 review 成本 |
| Husky | Git 钩子 | 提交前运行 lint/test/format，阻断不合规提交 |
| .browserslistrc | 目标浏览器声明 | 指导 SWC/Autoprefixer，统一编译/Polyfill 目标 |
| cspell | 拼写/术语检查 | 捕捉变量/文档拼写错误，统一术语 |
| stylelint | 样式规范检查 | 规范 CSS/Tailwind/@apply，避免样式错误 |
| markdownlint | Markdown 规范 | 统一标题/列表/链接/空行规则 |
| gitattributes | Git 行为控制 | 规范换行符、合并策略、文本/二进制识别 |

## 三、浏览器与设备兼容

### 1. 覆盖范围

1）PC：Win10/11、macOS Catalina+；Chrome/Edge/Firefox/Safari/QQ/微信内置；分辨率含 1080p/2K/4K/超宽屏。  
2）Android：近 2 年 20+ 机型，Android 9~14，含折叠/异形屏。  
3）iOS：10+ 机型，iOS 12~18，含刘海/直屏。  
4）浏览器：微信内置、系统浏览器（Safari/Chrome/厂商自带）。  

### 2. 实施清单

1）统一 browserslist（产/开分离），对齐 SWC/Autoprefixer 目标。  
2）样式兼容：启用 Autoprefixer；为 `position: sticky`、`backdrop-filter` 等提供降级，`@supports` 渐进增强。  
3）视口与安全区：设置 viewport；`env(safe-area-inset-*)` 适配刘海/折叠屏，关键区域预留内边距。  
4）断点与密度：Tailwind 断点（sm/md/lg/xl/2xl）；`next/image` `sizes/srcSet` 适配不同 DPR/分辨率。  
5）折叠/异形屏：CTA 避开折痕/刘海；流式/弹性布局减少固定宽高；测试横折/竖折旋转。  
6）输入与触控：点击区 ≥ 44px；表单类型匹配（tel/email/number）；滚动避免与下拉刷新冲突；长列表虚拟化或分页。  
7）媒体与文件：微信内验证音视频；上传下载覆盖常见类型与尺寸；必要时提示用系统浏览器。  
8）特征检测：`ResizeObserver`、`IntersectionObserver`、`requestIdleCallback` 等做 Feature Detection，按需 polyfill，避免 UA 判断。  
9）JS 体积与降级：核心路径不依赖可选特性；低端机遵循 `prefers-reduced-motion` 关闭动画。  
10）图片降级：优先 WebP/AVIF，后备 JPEG/PNG；CDN/`next/image` 自动协商。  
11）字体与图标：本地 iconfont/SVG sprite；WebFont `font-display: swap`，提供系统字体回退。  

### 3. 测试矩阵与流程

1）矩阵：  
   a）PC：Win10/11 + Chrome/Edge/Firefox/QQ/微信；macOS Catalina+ + Safari/Chrome。  
   b）Android：20+ 机型，Android 9~14，含折叠/异形屏。  
   c）iOS：12~18，Safari、微信内置。  
2）流程：BrowserStack/远程真机 + WeChat DevTools 预检；首屏/路由/表单/上传/支付或分享/媒体/粘贴长按回归；问题按特性检测与样式降级，必要时提示切换浏览器。

## 四、SEO

### 1. 实现

| 要素 | 做法 | 备注 |
| --- | --- | --- |
| 动态 sitemap | `app/sitemap.ts` 或 route handler 动态生成，含 `lastModified/priority/changefreq` | 随内容更新 |
| 头部元信息 | `generateMetadata` 配置 `title/description/keywords` | 布局或页面级 |
| 结构化数据 | 输出 JSON-LD（WebSite/WebPage/Product/FAQ/Breadcrumb） | `application/ld+json` |
| 语义标签 | 每页仅一个 `h1`，分级使用 `h2+` | 层级清晰 |
| 图片可访问性 | 所有图片提供 `alt`；`next/image` 输出 `width/height`，必要时 `priority` | 稳定布局 |
| Open Graph | `og:title/description/url/image`、`og:image:type/width/height`、`og:type` | 便于分享 |
| Twitter Card | `twitter:card`（如 `summary_large_image`）、`twitter:title/description/image` | 与 OG 对齐 |
| 其他 meta | `robots`、`canonical`，多语言用 `alternate` hreflang | 防重复收录 |
| 性能与可见性 | 首屏轻量，避免阻塞脚本；meta/JSON-LD SSR 生成 | 便于爬虫 |

### 2. 检测

1）Google Search Console：https://search.google.com/search-console  
2）PageSpeed Insights：https://pagespeed.web.dev/  

## 五、页面布局

1）布局体系：流式/弹性（Grid/Flex）；PC 设 `max-width`（1280/1440）居中，移动全宽；间距/字号用 token（如 `--space-4`, `--font-body`）。  
2）断点策略：移动优先（base → sm → md → lg → xl → 2xl）；大屏需时增加 `max-width` 或分栏密度。  
3）头部导航：  
   a）PC：多级导航 + 搜索/登录/CTA，悬停下拉可键盘焦点，滚动 sticky。  
   b）移动：Logo/返回 + 汉堡 + 1-2 CTA，菜单抽屉/底部行动栏，点击区 ≥ 44px，安全区内边距 `env(safe-area-inset-top)`。  
4）主内容区：  
   a）信息分块；移动单列，PC 双/三列。  
   b）列表：懒加载/分页 + 骨架/空状态；行点击区充足。  
   c）表单：分段/步进，输入类型匹配，错误提示就近，显示进度。  
5）侧边/过滤：  
   a）PC：侧栏可 sticky。  
   b）移动：折叠为抽屉或顶部筛选条，避免遮挡主 CTA。  
6）底部：  
   a）PC：多列链接/备案/社交，低对比度背景。  
   b）移动：精简为折叠或少量主链路，版权置底。  
7）组件响应式：  
   a）按钮：断点化尺寸与内边距。  
   b）图片：`next/image + sizes`，PC 更大 `max-width`，移动自适应。  
   c）表格：PC 正常、移动卡片化或横向滚动。  
   d）卡片：移动单列、PC 2-4 列。  
8）可访问性与手势：  
   a）焦点态可见，键盘可导航，颜色对比度达 WCAG AA。  
   b）触控区 ≥ 44px，避免 hover-only，提供合适 aria。  
9）i18n：  
   a）预留文案长度，日期/数字/货币本地化。  
   b）支持 RTL 时容器可 `dir="rtl"`，方向性元素可翻转。  
10）交互降级：  
    a）弱机/弱网降低动画、降图质量、延后非核心。  
    b）Skeleton/占位提升感知；长任务分片或 `requestIdleCallback`（含特性检测）。  

## 六、性能优化

| 领域 | 要点 | 细节/措施 |
| --- | --- | --- |
| 渲染策略 | Server Components 优先 | 减小前端 bundle；仅需浏览器能力的组件用 `use client`；首屏核心用 SSR/ISR，次要区块懒加载 |
| 代码分割 | 按需加载 | `next/dynamic` 加载非首屏组件；仅客户端组件可 `ssr: false`；大依赖子路径导入/分包；`next build --profile` 分析 |
| 资源优化 | 图片/字体/静态 | `next/image` + CDN + AVIF/WebP；`sizes/srcSet` 适配断点；首屏关键图用 `priority`；字体自托管/预加载，`font-display: swap`，可变字体降请求；静态资源 gzip/br，合理 `Cache-Control` |
| 缓存策略 | 数据/页面/资源 | 数据：Route Handler/Edge 缓存 + SWR/React Query 缓存重试；页面：SSG/ISR 降 TTFB，动态路由结合 revalidate；资源：静态长缓存带 hash，HTML 短缓存 |
| 预加载/预取 | 提前获取关键资源 | `<link rel="preload">` 字体/首图/脚本；路由预取用 Next.js `prefetch`（视口内链接）；高频路由数据预热（SWR mutate 或预 fetch） |
| 体积控制 | 精简依赖与 polyfill | 剔除未用依赖；polyfill 按需加载（Feature Detection）；可选 `experimental.optimizePackageImports`（按版本评估） |
| 数据层 | 请求复用与健壮性 | SWR/React Query 缓存、去抖、重试、失效策略；后端压缩与合理超时/重试；慢接口分片或后台更新 |
| 监控与回退 | 指标与降级 | 上报 LCP/FID/CLS/TTFB/INP，捕获长任务/阻塞资源；低端机/弱网降低动画、降图质量、延迟非核心模块 |

## 七、安全

| 领域 | 要点 | 细节/措施 |
| --- | --- | --- |
| 传输与域名 | HTTPS/HSTS | 全站 HTTPS；HSTS；微信内需备案合规域名 |
| 安全响应头 | CSP / XFO / XCTO 等 | `headers()` 或中间件设置 CSP（含 nonce/hash）、`X-Frame-Options`/`frame-ancestors`、`X-Content-Type-Options: nosniff`、`Referrer-Policy: strict-origin-when-cross-origin`、`Permissions-Policy` |
| XSS 与注入 | 转义与消毒 | React 默认转义；富文本用白名单消毒（DOMPurify）；不将未校验输入拼入脚本/样式/HTML；URL 参数需校验编码 |
| CSRF 与认证 | Token/Cookie 策略 | 跨站接口用 CSRF Token 或 SameSite=Lax/Strict Cookie；敏感接口需鉴权；推荐 JWT/Session + HttpOnly + Secure + SameSite；中间件保护路由（可结合 NextAuth/session） |
| SSR/ISR 安全 | 数据输出与缓存 | SSR/ISR 输出前转义，避免敏感数据下发客户端；ISR 生成时也做鉴权/校验，防缓存污染 |
| 依赖与密钥 | 最小暴露 | `.env*` 不入库；密钥走环境变量/平台配置；定期 `pnpm audit`；校验三方 SDK/Polyfill 来源 |
| 上传与文件 | 限制与防护 | 限制类型/大小/数量；上传做病毒扫描；文件名随机化；下载接口设 `Content-Disposition`，预签名 URL 有限期与权限 |
| 日志与隐私 | 脱敏与合规 | 最小化采集，敏感字段脱敏，受控存储；如需合规提供删除/导出通道 |
| 运行时防护 | 配置与校验 | 关闭不必要实验特性；动态 import 外部 URL 白名单；中间件校验 Host/Origin 拒绝伪造 |
| 第三方与 SDK | 来源与加载 | 微信/支付等只用官方源，校验版本/域名；按需懒加载减少暴露面 |

## 八、项目结构示例

```
.
├── eslint.config.js
├── package.json
├── pnpm-workspace.yaml
├── stylelint.config.mjs
├── packages-app
│   └── template          # Next.js 应用模板
│       ├── next.config.ts
│       ├── package.json
│       ├── postcss.config.mjs
│       ├── public
│       ├── src
│       └── tsconfig.json
├── packages-ui           # 组件库
├── packages-utils        # 工具库
├── packages-styles       # 设计/主题样式
└── README.md
```
注：
- 根目录：lint/样式配置、workspace、全局脚本。  
- `packages-app/template`：Next.js App（可按需复制多应用）。  
- `packages-ui`：通用 UI 组件，暴露给各应用。  
- `packages-utils`：工具函数、请求封装等。  
- `packages-styles`：设计 token、全局样式、Tailwind 预设等。  
- `pnpm --filter <pkg> <cmd>` 调度各包的 dev/lint/build/test。
