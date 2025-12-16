<div align="center">

<svg width="120" height="120" viewBox="0 0 120 120" fill="none" xmlns="http://www.w3.org/2000/svg">
  <path d="M60 10L105 35V85L60 110L15 85V35L60 10Z" fill="url(#paint0_linear)" stroke="#2D6CDF" stroke-width="2"/>
  <path d="M60 30L80 40V75L60 85L40 75V40L60 30Z" fill="white" stroke="#2D6CDF" stroke-width="2"/>
  <circle cx="60" cy="57" r="10" fill="#2D6CDF"/>
  <path d="M60 43V57L68 65" stroke="white" stroke-width="3" stroke-linecap="round"/>
  <defs>
    <linearGradient id="paint0_linear" x1="15" y1="60" x2="105" y2="60" gradientUnits="userSpaceOnUse">
      <stop stop-color="#61DAFB"/>
      <stop offset="1" stop-color="#2D6CDF"/>
    </linearGradient>
  </defs>
</svg>

# NaviHive - 现代化个人导航站

![NaviHive 导航站](https://img.shields.io/badge/NaviHive-导航站-blue)
![React](https://img.shields.io/badge/React-19.0.0-61dafb)
![TypeScript](https://img.shields.io/badge/TypeScript-5.7-3178c6)
![Material UI](https://img.shields.io/badge/Material_UI-7.0-0081cb)
![Cloudflare](https://img.shields.io/badge/Cloudflare-Workers-f38020)
![License](https://img.shields.io/badge/License-MIT-green)

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/timeflysoon/Cloudflare-Navihive)

**一个优雅、现代化的网站导航管理系统**
基于 Cloudflare Workers 构建 • 零成本部署 • 全球 CDN 加速 • 企业级安全

[📖 完整文档](https://zqq-nuli.github.io/Cloudflare-Navihive/) • [🎮 在线演示](https://navihive.chatbot.cab/) • [🚀 快速开始](https://zqq-nuli.github.io/Cloudflare-Navihive/deployment/) • [💬 问题反馈](https://github.com/zqq-nuli/Cloudflare-Navihive/issues)

</div>

> 部署过程中遇到问题，暂时可参阅 V1.1.0版本[部署教程](https://github.com/zqq-nuli/Cloudflare-Navihive/tree/v1.1.0)暂时我可能没有那么多时间来修正文档的问题，实在抱歉。

## 🎯 快速开始

### 在线演示

访问演示站点体验所有功能：[navihive.chatbot.cab](https://navihive.chatbot.cab/)

```
👤 演示账号：admin
🔑 演示密码：NaviHive2025!
```

### 立即部署

**5 分钟完成部署，零成本永久使用：**

1. **Fork 项目** → 点击右上角 Fork 按钮
2. **新建 wrangler.jsonc 文件** 从 wrangler.template.jsonc 复制然后修改
3. **一键部署** → [![Deploy](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/你的用户名/Cloudflare-Navihive)
4. **配置数据库** → 按照[部署指南](https://zqq-nuli.github.io/Cloudflare-Navihive/deployment/)创建 D1 数据库

> 详细步骤见[完整部署指南](https://zqq-nuli.github.io/Cloudflare-Navihive/deployment/)

---

## 🚀 部署指南

### 一、准备工作

在开始部署之前，您需要：

1. 一个 [Cloudflare 账号](https://dash.cloudflare.com/sign-up)（免费）
2. 一个 GitHub 账号（如果您想 fork 此项目或使用一键部署功能）
3. 基本的网络和浏览器操作知识

### 二、一键部署方法（推荐小白用户）

最简单的部署方式是使用一键部署功能：
1. fork本仓库，修改`wrangler.template.jsonc`为`wrangler.jsonc`
2. 点击上方的"Deploy to Cloudflare Workers"按钮
3. 登录您的 Cloudflare 账号
4. 在部署界面上，您需要配置以下内容：
    - **项目名称**：为您的导航站项目取个名字
    - **D1 数据库**：点击"创建新数据库"，命名为`navigation-db`
    - **环境变量**：
        - `AUTH_ENABLED`：设置为`true`启用登录认证
        - `AUTH_USERNAME`：管理员用户名
        - `AUTH_PASSWORD`：管理员密码
        - `AUTH_SECRET`：JWT 密钥（使用随机字符串）
        - https://bcrypt-generator.com/  哈希值粘贴到   AUTH_PASSWORD
        - https://bcrypt.online/    32位随机字符串   AUTH_SECRET
5. 点击"部署"按钮

部署完成后，您将获得一个类似`https://your-project-name.username.workers.dev`的网址，这就是您的导航站地址。

6. 初始化项目数据库  
   - 登录您的 [Cloudflare 控制台](https://dash.cloudflare.com/)
   - 进入"Workers & Pages"部分
   - 选择您刚刚部署的项目
   - 在左侧菜单中点击"设置" > "数据库"，您将看到已绑定的数据库（名为"navigation-db"）
   - 点击数据库名称以进入数据库管理界面：

   ![数据库管理界面](https://img.zhengmi.org/file/1743843332374_image.png)

   - 在数据库管理界面，点击"控制台"选项卡进入SQL编辑器
   - 在SQL编辑器中，逐个复制并粘贴以下SQL命令：

   ```sql
   -- 创建分组表
   CREATE TABLE IF NOT EXISTS groups (
       id INTEGER PRIMARY KEY AUTOINCREMENT, 
       name TEXT NOT NULL, 
       order_num INTEGER NOT NULL, 
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, 
       updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   -- 创建站点表
   CREATE TABLE IF NOT EXISTS sites (
       id INTEGER PRIMARY KEY AUTOINCREMENT, 
       group_id INTEGER NOT NULL, 
       name TEXT NOT NULL, 
       url TEXT NOT NULL, 
       icon TEXT, 
       description TEXT, 
       notes TEXT, 
       order_num INTEGER NOT NULL, 
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, 
       updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, 
       FOREIGN KEY (group_id) REFERENCES groups(id) ON DELETE CASCADE
   );

   -- 创建配置表
   CREATE TABLE IF NOT EXISTS configs (
       key TEXT PRIMARY KEY,
       value TEXT NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
       updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   -- 设置初始化标志
   INSERT INTO configs (key, value) VALUES ('DB_INITIALIZED', 'true');

   -- 创建只读模式所需索引
   CREATE INDEX IF NOT EXISTS idx_groups_is_public ON groups(is_public);
   CREATE INDEX IF NOT EXISTS idx_sites_is_public ON sites(is_public);
   ```

   - 点击"运行"按钮执行SQL命令：

   ![SQL编辑器界面](https://img.zhengmi.org/file/1743843528319_image.png)

   - 如果SQL命令执行成功，您将看到"查询成功"的提示信息
   - 至此，数据库初始化完成，您可以访问您的导航站首页并使用配置的管理员账号登录

### 三、手动部署方法（适合开发者）

如果您希望更细致地控制部署过程，可以按照以下步骤手动部署：

#### 1. 克隆仓库

```bash
git clone https://github.com/zqq-nuli/NaviHive.git
cd NaviHive
```

#### 2. 安装依赖

```bash
pnpm install
```

#### 3. 配置 Cloudflare

**安装 Wrangler 工具**

```bash
npm install -g wrangler
```

**登录 Cloudflare**

```bash
wrangler login
```

**创建 D1 数据库**

```bash
wrangler d1 create navigation-db
```

创建后，您会获得一个数据库 ID，记下这个 ID，稍后需要使用。

#### 4. 修改配置文件

编辑`wrangler.jsonc`文件：

```json
{
    "$schema": "node_modules/wrangler/config-schema.json",
    "name": "您的项目名称",
    "main": "worker/index.ts",
    "compatibility_date": "2025-04-05",
    "assets": {
        "not_found_handling": "single-page-application"
    },
    "observability": {
        "enabled": true
    },
    // D1数据库绑定
    "d1_databases": [
        {
            "binding": "DB",
            "database_name": "navigation-db",
            "database_id": "您的数据库ID"  // 替换为您刚创建的数据库ID
        }
    ],
    "vars": {
        "AUTH_ENABLED": "true",  // 是否启用认证
        "AUTH_USERNAME": "admin",  // 管理员用户名
        "AUTH_PASSWORD": "password",  // 管理员密码 (请修改为安全密码)
        "AUTH_SECRET": "your-secret-key"  // JWT密钥 (请使用随机字符串)
    }
}
```

#### 5. 开发模式

```bash
pnpm dev
```

#### 6. 构建项目

```bash
pnpm build
```

#### 7. 部署项目

```bash
pnpm deploy
```

部署完成后，您将获得一个类似`https://您的项目名称.您的用户名.workers.dev`的网址。

### 四、初始化与数据库设置

无论您使用哪种部署方法，部署完成后，需要进行数据库初始化。您有两种方式：

#### 方式一：通过SQL初始化（推荐）

如一键部署方法中的步骤5所述，通过Cloudflare控制台执行SQL命令初始化数据库。

#### 方式二：通过API初始化(弃用)

1. 访问`https://您的网站地址/init`
2. 如果看到"数据库初始化成功"的消息，说明初始化成功

初始化完成后，访问您的导航站首页，使用您配置的管理员用户名和密码登录。

## 📝 使用指南

### 登录系统

首次访问您的导航站时，您需要使用在部署时设置的管理员账号和密码登录。

### 配置您的导航站

登录后，您可以：

1. **添加新的分组**：点击页面上方的"新增分组"按钮
2. **添加网站**：在分组中点击"添加卡片"按钮
3. **自定义设置**：点击"网站设置"按钮，可以修改网站标题、名称和自定义 CSS
4. **拖拽排序**：点击"编辑排序"按钮，可以拖拽调整分组和网站的顺序

### 使用自定义域名（可选）

如果您想使用自己的域名，而不是 Cloudflare Workers 提供的子域名，您可以：

1. 在 Cloudflare 控制面板中，进入"Workers & Pages"
2. 选择您的导航站项目
3. 点击"触发器(Triggers)"选项卡
4. 在"自定义域(Custom Domains)"部分，点击"添加自定义域"
5. 输入您想使用的域名，并按照指示完成 DNS 配置

## 🔧 常见问题解答

**Q: 我忘记了管理员密码，怎么办？**  
A: 您可以通过修改环境变量重置密码。在 Cloudflare 控制面板中，进入您的项目，点击"设置" > "环境变量"，修改`AUTH_PASSWORD`的值。

**Q: 我想关闭登录认证，可以吗？**  
A: 可以。将环境变量`AUTH_ENABLED`设置为`false`即可关闭认证功能。

**Q: 部署后如何更新到最新版本？**  
A: 如果使用的是一键部署，可以再次点击部署按钮；如果是手动部署，拉取最新代码后重新构建并部署。

**Q: 我想备份我的数据，应该怎么做？**  
A: 您可以使用 Wrangler 工具导出 D1 数据库：

```bash
wrangler d1 database export navigation-db
```

**Q: 数据库结构是什么样的？**  
A: NaviHive 使用两个主要表格：

-   `groups`: 存储分组信息
-   `sites`: 存储网站信息
-   `configs`: 存储配置信息

## 🗂️ 项目结构

```
├── worker/               # Cloudflare Workers函数
│   └── index.ts          # Workers入口文件
├── public/               # 静态资源
├── src/                  # 前端源码
│   ├── API/              # API客户端
│   ├── components/       # React组件
│   └── App.tsx           # 主应用组件
├── wrangler.jsonc        # Cloudflare Workers配置
├── vite.config.ts        # Vite配置文件
├── package.json          # 项目依赖
└── README.md             # 项目说明
```

---

## 📖 完整文档

### 📚 用户指南
- [**项目介绍**](https://zqq-nuli.github.io/Cloudflare-Navihive/introduction) - 了解 NaviHive 的特点和优势
- [**为什么选择 NaviHive**](https://zqq-nuli.github.io/Cloudflare-Navihive/guide/why-navihive) - 与其他方案的对比
- [**功能截图**](https://zqq-nuli.github.io/Cloudflare-Navihive/guide/screenshots) - 11 张精美功能截图展示
- [**常见问题**](https://zqq-nuli.github.io/Cloudflare-Navihive/guide/faq) - FAQ 和故障排除
- [**更新日志**](https://zqq-nuli.github.io/Cloudflare-Navihive/guide/changelog) - 版本历史和变更记录

### 🔧 开发者文档
- [**部署指南**](https://zqq-nuli.github.io/Cloudflare-Navihive/deployment/) - 详细的部署步骤
- [**架构设计**](https://zqq-nuli.github.io/Cloudflare-Navihive/architecture/) - 技术栈和系统架构
- [**API 文档**](https://zqq-nuli.github.io/Cloudflare-Navihive/api/) - RESTful API 参考
- [**安全指南**](https://zqq-nuli.github.io/Cloudflare-Navihive/security/) - 14+ 安全加固说明
- [**贡献指南**](https://zqq-nuli.github.io/Cloudflare-Navihive/contributing/) - 如何参与项目

### 🎯 功能特性
- [**功能概览**](https://zqq-nuli.github.io/Cloudflare-Navihive/features/) - 完整功能列表和说明

> 📝 访问 [NaviHive 文档站点](https://zqq-nuli.github.io/Cloudflare-Navihive/) 查看完整文档

---

## 🛠️ 技术栈

**前端**: React 19 • TypeScript 5.7 • Material UI 7.0 • Tailwind CSS 4.1 • DND Kit • Vite 6

**后端**: Cloudflare Workers • Cloudflare D1 (SQLite) • JWT + bcrypt • TypeScript Strict Mode

**开发**: pnpm • Wrangler CLI • ESLint + Prettier

## 🤝 贡献

欢迎所有形式的贡献！查看 [贡献指南](https://zqq-nuli.github.io/Cloudflare-Navihive/contributing/) 了解如何参与项目。

---

## 📄 许可证

本项目基于 [MIT License](LICENSE) 开源协议发布。

---

## 🙏 致谢

感谢以下开源项目和服务：

- [React](https://reactjs.org/) • [TypeScript](https://www.typescriptlang.org/) • [Vite](https://vitejs.dev/)
- [Material UI](https://mui.com/) • [DND Kit](https://dndkit.com/) • [Tailwind CSS](https://tailwindcss.com/)
- [Cloudflare Workers](https://workers.cloudflare.com/) • [Cloudflare D1](https://developers.cloudflare.com/d1/)
- [Claude Code](https://claude.ai/code) • [Cursor](https://www.cursor.com)

感谢所有提交 Issue、PR 和 Star 的开发者们！🌟

---

## ⭐ 支持项目

如果 NaviHive 对你有帮助，欢迎通过以下方式支持：

### 💝 给项目点赞
- 点击右上角的 ⭐ **Star** 按钮，这是对开发者最大的鼓励
- **Fork** 项目，参与改进和定制
- 分享给你的朋友和同事

### 💰 赞赏支持
你的赞赏将用于项目的持续开发和维护：

<div align="center">
  <img src="https://img.zhengmi.org/file/1743956440128_4b965550184c06d8164f8077fa42b5d.jpg" alt="微信赞赏码" width="300">
  <p><em>微信扫码赞赏</em></p>
</div>

### 🤝 其他支持方式
- 💬 提交有价值的 Issue 和 Feature Request
- 📝 改进文档和教程
- 🐛 报告 Bug 并提供复现步骤
- 💻 贡献代码（欢迎提交 PR）

---

## 📈 Star History

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=zqq-nuli/Cloudflare-Navihive&type=Date&theme=dark" />
  <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=zqq-nuli/Cloudflare-Navihive&type=Date" />
  <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=zqq-nuli/Cloudflare-Navihive&type=Date" />
</picture>

---

<div align="center">

## 🎉 让导航管理更简单

**NaviHive** - 你的专属网络导航中心

[立即部署](https://deploy.workers.cloudflare.com/?url=https://github.com/zqq-nuli/Cloudflare-Navihive) • [在线演示](https://navihive.chatbot.cab/) • [完整文档](https://zqq-nuli.github.io/Cloudflare-Navihive/) • [提交问题](https://github.com/zqq-nuli/Cloudflare-Navihive/issues)

Made with ❤️ by [zqq-nuli](https://github.com/zqq-nuli)

⭐ 如果觉得有用，别忘了点个 Star 哦 ⭐

</div>
