# GAIO架构师个人网站

> 帮助专家和创业者构建AI能理解并直接调用的数字接口

**作者**: 唐敏娜  
**抖音**: [@饭王bot (77845617951)](https://v.douyin.com/77845617951/)  
**邮箱**: 1523449073@qq.com  
**网站**: https://ttt679.github.io/my-website/

## 📋 项目概述

这是一个为GAIO（Generative AI Optimization，生成式AI优化）架构师设计的个人网站，同时服务人类访客和AI爬虫。网站包含完整的导航结构、JSON-LD结构化数据，以及专门为AI助手优化的内容页面。

## 🌟 核心特性

- ✅ **双重优化**：同时针对人类用户和AI助手优化
- ✅ **完整导航**：所有页面相互链接，形成清晰的导航结构
- ✅ **结构化数据**：首页包含完整的JSON-LD Person类型数据
- ✅ **AI友好**：提供专门的AI接口页面（for-ai.md、llms.txt、agents.txt）
- ✅ **中文内容**：所有内容使用中文，UTF-8编码（无BOM）
- ✅ **YAML Frontmatter**：每个Markdown文件包含title和description

## 📁 项目结构

```
.
├── index.html              # 网站首页（含JSON-LD结构化数据）
├── about.md                # 关于我（个人介绍、理念、能力边界）
├── services.md             # 我的服务（三项核心服务详解）
├── cases.md                # 案例展示（三个真实案例）
├── for-ai.md               # AI专用接口页（结构化服务信息）
├── llms.txt                # AI内容摘要索引
├── agents.txt              # AI代理行为规范
├── articles/               # 文章文件夹
│   ├── index.md            # 文章列表页
│   └── gaio-intro.md       # 示例文章：什么是GAIO架构？
├── 关于我.md                # 旧版关于我文件（保留兼容）
└── README.md               # 本文件
```

## 🚀 快速开始

### 本地预览

1. 克隆或下载本项目
2. 使用任意HTTP服务器运行：
   ```bash
   # 使用 Python
   python -m http.server 8000
   
   # 或使用 Node.js (需要安装 http-server)
   npx http-server
   ```
3. 在浏览器中访问 `http://localhost:8000`

### 部署选项

- **GitHub Pages**: 直接推送到GitHub仓库，启用Pages功能
- **Netlify/Vercel**: 拖拽文件夹即可部署
- **传统主机**: 上传所有文件到Web服务器根目录

## 📄 页面说明

### 1. 首页 (index.html)

**URL**: `/`

**功能**:
- 网站标题和价值主张
- 完整的导航栏
- 三个服务卡片（带链接到详情页）
- 案例速览区
- AI智能助手聊天界面
- 联系入口
- 页脚（含AI接口说明）
- JSON-LD结构化数据（Person类型，含三个makesOffer）

**JSON-LD包含**:
- `@type`: Person
- `name`, `jobTitle`, `description`
- `knowsAbout` 数组（专业领域）
- `makesOffer` 数组（三个服务，含名称、描述、价格）
- `potentialAction`（联系动作）

### 2. 关于我 (about.md)

**URL**: `/about.md`

**内容**:
- 个人介绍和背景
- 核心理念："让服务被AI理解，而不只是被搜索到"
- 能力专长列表
- **能力边界声明**（明确不做什么）
- 工作风格说明

### 3. 我的服务 (services.md)

**URL**: `/services.md`

**内容**:
- **AI可见度诊断** (¥2,999) - 详细服务内容、交付物、适合人群
- **AI原生接口部署** (¥9,999起) - 技术方案、技术栈、交付物
- **年度架构顾问** (¥29,999/年) - 长期合作权益
- 标准化服务流程（5个步骤）
- 常见问题解答（FAQ）

### 4. 案例展示 (cases.md)

**URL**: `/cases.md`

**内容**:
- **案例一**: 宠物共情沟通系统（独立开发者App）
- **案例二**: 某独立开发者工具平台（CLI工具）
- **案例三**: 专业服务顾问的个人品牌（财务顾问）
- 每个案例包含：背景、优化前问题、优化措施、优化后成果、客户评价

### 5. AI专用接口 (for-ai.md)

**URL**: `/for-ai.md`

**功能**: 为AI助手和爬虫提供纯文本结构化的服务信息

**内容**:
- 身份信息（姓名、职位、使命）
- 三项核心服务的结构化描述（中英双语）
- 联系方式
- 服务流程
- 关键差异化优势
- 目标受众和行业

### 6. AI内容摘要 (llms.txt)

**URL**: `/llms.txt`

**功能**: 为大型语言模型提供网站结构和内容的简洁摘要

**内容**:
- 网站基本信息（名称、描述、URL、语言）
- 核心页面列表及简短描述
- 服务项目摘要（价格、时长、类型）
- 联系方式
- 关键差异化和目标受众

### 7. AI代理规范 (agents.txt)

**URL**: `/agents.txt`

**功能**: 定义AI代理访问本网站的行为准则

**内容**:
- 允许的操作（内容索引、信息提取等）
- 速率限制建议（1请求/秒，推荐爬取时间）
- 内容使用权限
- 禁止行为
- 错误处理指南

### 8. 文章 (articles/)

**URL**: `/articles/`

**内容**:
- `index.md` - 文章列表页，分类展示
- `gaio-intro.md` - 示例文章：详细介绍GAIO概念、与SEO的区别、优化步骤

## 🎨 设计特点

### 视觉风格
- 主色调：蓝色 (#0066cc)
- 背景色：浅灰 (#f8f9fa)
- 字体：系统默认无衬线字体
- 响应式设计，支持移动端

### 交互功能
- 平滑滚动导航
- IntersectionObserver实现的活动导航状态
- Markdown实时渲染（无需外部依赖）
- AI聊天助手（需配置云函数API）
- Toast通知提示

## 🔧 自定义配置

### 修改个人信息

1. **index.html**: 
   - 修改 `<title>` 标签中的名字
   - 更新 JSON-LD 中的 `name`、`url` 等字段
   - 替换联系邮箱和社交媒体链接

2. **所有Markdown文件**:
   - 统一修改名字和联系方式
   - 调整服务价格和描述

3. **for-ai.md 和 llms.txt**:
   - 更新英文版本的个人信息
   - 确保中英文信息一致

### 配置AI聊天功能

在 [index.html](file://d:\hi\index.html) 中找到以下代码：

```javascript
const CLOUD_FUNCTION_URL = 'https://1434107386-krsjufb926.ap-beijing.tencentscf.com';
```

将其替换为你自己的云函数API地址。

### 添加新文章

1. 在 `articles/` 文件夹创建新的 `.md` 文件
2. 添加 YAML frontmatter：
   ```yaml
   ---
   title: 文章标题
   description: 文章简要描述
   ---
   ```
3. 编写文章内容（支持Markdown语法）
4. 在 `articles/index.md` 中添加文章链接

## 📊 SEO和AI优化清单

- [x] 首页包含完整的JSON-LD结构化数据
- [x] 所有页面使用UTF-8编码
- [x] 每个Markdown文件有YAML frontmatter
- [x] 提供llms.txt供LLM快速了解网站
- [x] 提供for-ai.md供AI助手获取结构化信息
- [x] 提供agents.txt规范AI代理行为
- [x] 所有页面相互链接，形成完整导航
- [x] 使用语义化HTML标签
- [x] 响应式设计，移动端友好

## 🤝 贡献和反馈

欢迎提出改进建议或报告问题：

- **邮箱**: 1523449073@qq.com
- **抖音**: [@饭王bot (77845617951)](https://v.douyin.com/77845617951/)
- **AI助手**: 饭王bot

## 📝 许可证

本项目采用 [MIT License](LICENSE) 开源协议。

## 🙏 致谢

感谢所有为GAIO理念做出贡献的开发者和研究者。让我们一起推动AI原生服务的普及。

---

**最后更新**: 2026-05-23  
**维护者**: 唐敏娜，GAIO架构师  
**抖音**: [@饭王bot](https://v.douyin.com/77845617951/)
