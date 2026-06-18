# 招聘情报站 · 部署说明

> 互联网行业 HR 每日情报站：每天 09:00 自动产出日报 + 后厂村八卦，长期可追溯。

## 站点结构

本站采用 **SPA 单页架构**（单 HTML + JS 切换视图），避免跨文件路径跳转在静态托管平台被拦截。

```
hr-intel-station/
├── index.html              # 站点主入口（含 4 个 view：home/industry/report/hiring）
└── reports/                # 归档目录（每日历史报告快照，仅作本地备份）
    └── 2026-06-17.html
```

部署到静态托管时，**只需上传 `index.html` 即可**。`reports/` 是本地归档，无需部署。

## 4 个 View 说明

| View | 入口位置 | 内容 |
| --- | --- | --- |
| `home` | 默认首页（无侧边栏） | Hero + 数据看板 + 招聘需求导览 |
| `industry` | 侧边栏"行业情报" | 历史日报归档卡片 |
| `report-2026-06-17` | 侧边栏"行业情报"子项 | 当日日报（行业动态 + 组织架构变化 + 后厂村八卦·近 7 日） |
| `hiring` | 侧边栏"招聘需求" | 招聘需求子卡（4 张） |

> 顶部 nav（仅 home 显示）：Logo + 行业情报 / 最新日报 / 招聘需求
> 侧边栏（仅非 home view 显示）：260px sticky + 4 个一级菜单

## 部署到腾讯云 EdgeOne Pages（推荐）

国内访问快、微信扫码、免费 10GB 流量/月、个人开发者友好。

### 1. 注册/登录腾讯云

- 访问 https://console.cloud.tencent.com/
- 微信扫码即可登录，无需绑卡
- 首次使用需完成实名认证（个人身份证，约 1 分钟）

### 2. 进入 EdgeOne Pages 控制台

- 顶部搜索框输入 `EdgeOne Pages`
- 首次进入会提示"开通服务"，点确认即可（**免费 10GB 流量/月**，个人站点基本用不完）

### 3. 创建项目

- 点击「创建项目」
- 模板选择：**纯静态（HTML）**
- 项目名称：`hr-intel-station`（自定义，仅支持英文+数字+短横线）
- 点击「创建」

### 4. 上传代码

进入项目详情页后，二选一：

**方式 A：上传 zip 压缩包（推荐）**
- 在项目根目录执行 `zip -r hr-intel-station.zip hr-intel-station/`
- 控制台点击「上传部署」→ 选择 zip 文件上传
- 等待 30 秒左右，部署完成会得到一个 `*.edgeone.app` 域名

**方式 B：直接拖拽文件**
- 控制台点击「部署」→ 拖入整个 `hr-intel-station` 文件夹
- 等待 30 秒左右即可

> **关键点**：确保 `index.html` 位于项目根目录（不要嵌套到子文件夹里），否则部署后访问根路径会 404。

### 5. 访问站点

部署成功后会得到类似这样的域名：

```
https://hr-intel-station.edgeone.app
```

点开就是站点首页，浏览器直接渲染（不是下载）。

### 6. 绑定自定义域名（可选）

- 项目详情 → 「域名管理」→ 添加自定义域名
- 按提示在域名 DNS 服务商处添加 CNAME 解析记录
- 等待 SSL 证书签发（通常 1-5 分钟），即可通过 `https://你的域名` 访问

## 替代部署方案

| 方案 | 优点 | 缺点 | 价格 |
| --- | --- | --- | --- |
| **腾讯云 EdgeOne Pages** ✅ | 国内访问快、微信扫码、免费 10GB/月 | 需实名 | 免费 |
| Cloudflare Pages | 全球 CDN 强、配置项多 | 国内访问偶尔慢 | 免费 |
| Vercel | 体验好、文档全 | 国内访问慢、需绑卡 | Hobby 免费 |
| Netlify | 老牌稳定 | 国内访问慢 | 免费 |
| GitHub Pages | 集成 Git 流程 | 国内访问慢、纯静态 | 免费 |

## 本地预览

```bash
# 在 hr-intel-station 目录下起一个静态服务
cd hr-intel-station
python3 -m http.server 8080
# 浏览器访问 http://localhost:8080
```

或直接双击 `index.html` 用浏览器打开（推荐 Chrome / Edge / Safari 都能正常渲染）。

## 视觉风格

参考腾讯云首页（https://cloud.tencent.com/）：

- 白底 + 腾讯云蓝 `#006EFF` 主色
- 蓝紫粉三段渐变大标题
- 大量留白 + 6-12px 圆角 + 极轻阴影
- 顶部简洁 nav + 居中 Hero + 数据看板 + 模块化卡片

### 颜色变量（在 `index.html` 的 `:root` 中）

```css
--tc-blue:        #006EFF;   /* 腾讯云蓝（主色）*/
--tc-blue-dark:   #0052CC;
--tc-blue-deep:   #003D99;
--tc-blue-50:     #F0F7FF;
--tc-purple:      #6E5BFF;   /* 紫色（组织变化模块）*/
--tc-pink:        #FF4D94;   /* 粉色（八卦卡片 hover）*/
--tc-orange:      #FF7A45;   /* 橙色（八卦模块主色 + 实操启示）*/
--tc-green:       #00C896;   /* 状态指示、NEW tag */
--text-1:         #1A1A1A;   /* 主文本 */
--text-2:         #4E5969;   /* 次文本 */
--text-3:         #86909C;   /* 弱化文本 */
--bg:             #FFFFFF;   /* 页面背景 */
--bg-soft:        #F7F9FC;   /* 浅灰背景 */
```

## 维护说明

### 站点内容更新

- 站点内容由扣子日历任务每天 09:00 自动更新（无需手动操作）
- 自动任务会跑 topic_tracking briefing + 八卦搜索（近 7 天过滤） + 重新生成 `index.html` + 重新部署

### 手动更新（调试用）

1. 修改 `index.html`
2. 重新打包：`zip -r hr-intel-station.zip hr-intel-station/`
3. 重新上传到 EdgeOne Pages 控制台

### 八卦栏目规范

- 来源限定：脉脉职言 + 小红书「大厂日爆」类自媒体 + CSDN/36氪/DoNews
- 时效：**近 7 天严格过滤**，超出 7 天的素材不收录
- 数量：4-6 条/期
- 样式：橙红配色（与日报蓝/紫形成对比），橙底 disclaimer "吃瓜提示：仅供娱乐参考"
- 每张卡必含：序号徽章 + 公司 chip + 热度 tag + 时间 + emoji 标题 + 描述 + 来源/互动数据

## 部署验证记录
- 2026-06-18 11:53 · GitHub 集成首次端到端链路验证（git push → EdgeOne Pages 自动部署）
