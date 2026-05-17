 
### 1. 项目目标与核心流程

**一句话目标**

> 通过一个本地 Web API，将用户粘贴的聊天记录（可选整理指令）自动转为结构化的 Obsidian 笔记。

**核心流程图（文字版）**

text

[用户通过前端网页/curl 发送 POST 请求到 /process_chat]
    │
    ├─ 请求体: { "chat_text": "...", "instruction": "..." }
    ↓
[main.py 接收请求，校验参数]
    ↓
[call_deepseek_api() 构造元提示词，调用 DeepSeek API]
    │
    ├─ 输入: 原始对话 + 用户指令（或默认指令）
    ├─ 输出: 包含 YAML Frontmatter 的 Markdown 字符串
    ↓
[save_to_obsidian() 处理 Markdown]
    │
    ├─ extract_title_from_frontmatter() 提取 title
    ├─ inject_date_to_frontmatter() 插入当前日期
    ├─ 生成文件名: {safe_title}_{timestamp}.md
    ├─ 通过 Obsidian Local REST API 的 PUT 请求写入文件到 vault 的 "学习笔记/" 目录
    ↓
[返回响应: note_preview, saved_to_obsidian, obsidian_file_path]

---

### 2. 环境与配置模块

#### 2.1 环境变量 (`.env`)

|变量名|作用|示例值|是否必须用户修改|
|---|---|---|---|
|`DEEPSEEK_API_KEY`|DeepSeek API 密钥|`sk-xxxx`|✅ 必须|
|`DEEPSEEK_BASE_URL`|DeepSeek API 端点（可选）|`https://api.deepseek.com/v1`|❌ 有默认值|
|`OBSIDIAN_API_KEY`|Obsidian Local REST API 的授权密钥|从插件中获取|✅ 如需自动保存则必须|
|`OBSIDIAN_HOST`|Obsidian API 主机地址|`http://localhost`|❌ 有默认值|
|`OBSIDIAN_PORT`|Obsidian API 端口|`27123`（插件默认）|❌ 有默认值|

#### 2.2 配置文件

此项目没有独立的 `config.yaml`，所有配置通过环境变量 + 代码中的常量实现。  
**可配置的代码常量**（在 `main.py` 顶部附近）：

python

# DeepSeek 模型参数
model="deepseek-chat"
temperature=0.7
max_tokens=3000
timeout=90
# Obsidian 子目录
file_path = f"学习笔记/{filename}"   # 硬编码为 "学习笔记/"

如需要，未来可以抽离为 `config.py`。

---

### 3. 核心代码模块（`main.py` 结构）

|函数/类|职责|输入|输出|依赖|
|---|---|---|---|---|
|`extract_title_from_frontmatter(content)`|从 Markdown 的 YAML Frontmatter 中提取 `title` 字段|Markdown 字符串|标题字符串（若未找到则返回时间戳）|`re`, `datetime`|
|`inject_date_to_frontmatter(content, date)`|在 Frontmatter 的 `type` 字段后插入 `date: YYYY-MM-DD`|Markdown 字符串, 日期字符串|修改后的 Markdown 字符串|`re`|
|`save_to_obsidian(note_content)`|将笔记保存到 Obsidian 仓库|Markdown 内容|dict: `{success, file_path, error}`|`requests`, `re`, `datetime`|
|`call_deepseek_api(chat_text, instruction)`|调用 DeepSeek API 生成结构化笔记|对话原文, 整理指令（可选）|Markdown 字符串（含 Frontmatter）|`OpenAI` 库（DeepSeek 兼容）|
|`process_chat(request)`|FastAPI 路由处理函数|`ChatRequest` 对象|`NoteResponse`|上述所有函数|
|`test_obsidian()`|测试 Obsidian 连接|无|JSON 结果|`save_to_obsidian`|

#### 关键数据流示例

text

HTTP POST 请求体
    │
    ▼
ChatRequest.chat_text + instruction
    │
    ▼
call_deepseek_api()
    ├─ 内部生成 meta_prompt
    ├─ 调用 client.chat.completions.create()
    └─ 返回 raw_markdown (含 Frontmatter)
    │
    ▼
save_to_obsidian(raw_markdown)
    ├─ extract_title_from_frontmatter() → title
    ├─ inject_date_to_frontmatter() → 添加 date
    ├─ 生成文件名 → 本地文件路径字符串
    ├─ requests.put() 到 Obsidian API
    └─ 返回保存结果
    │
    ▼
返回 NoteResponse 给客户端

---

### 4. 外部服务/API 模块

- **DeepSeek API**（兼容 OpenAI SDK）
    
    - 调用方式：`OpenAI(api_key, base_url).chat.completions.create`
        
    - 模型：`deepseek-chat`
        
    - 参数：`temperature=0.7, max_tokens=3000`
        
    - 返回结构：`response.choices[0].message.content`
        
- **Obsidian Local REST API**（社区插件）
    
    - 端点：`PUT /vault/{file_path}`
        
    - 鉴权：`Authorization: Bearer {OBSIDIAN_API_KEY}`
        
    - 返回：HTTP 200/204 表示成功
        

---

### 5. 触发方式/前端模块

目前项目提供：

- **本地 Web 界面**（未在代码中提供，但用户提到通过启动服务器后访问 `http://localhost:8000` 并使用 `/process_chat` 端点）
    
- 尚未提供内置的 HTML 页面，用户需要自己用 `curl`、Postman 或编写简单前端调用 API。
    

**启动命令**：

bash

python main.py   # 内部调用 uvicorn.run("main:app", host="0.0.0.0", port=8000)

---

### 6. 数据本地化与隐私说明（自检清单）

|检查项|状态/说明|
|---|---|
|API Key 存储|✅ 仅存在 `.env` 文件，代码用 `load_dotenv()` 读取，未硬编码|
|`.gitignore`|✅ 用户已配置忽略 `.env`|
|API 调用记录|⚠️ 代码中有 `print` 输出调用过程（如 `"🔄 开始调用 DeepSeek API..."`），不含密钥或完整对话，但可能打印长度信息。**建议**：生产环境关闭 print 或改用 logging。|
|剪贴板临时文件|❌ 不涉及剪贴板，用户需手动复制对话并通过 HTTP 发送。|
|Obsidian API 传输|✅ 使用 HTTP（`OBSIDIAN_HOST` 默认 localhost），未加密但仅在本地；若暴露到公网应启用 HTTPS。|
|笔记内容存储|✅ 仅存于本地 Obsidian 仓库，不上传任何外部服务器。|
|前端表单历史|⚠️ 用户如果有自己的前端页面，需在 HTML 中添加 `autocomplete="off"`。|
|请求日志|FastAPI 默认会输出访问日志到控制台，内容不含请求体。|

---

### 7. 已知问题 & 待改进点（技术债务）

- 依赖 Obsidian Local REST API 插件手动安装和配置（对普通用户不友好）。
    
- 没有对 `instruction` 做长度校验，过长的指令可能使 prompt 超过 token 限制。
    
- 标题提取仅依赖 Frontmatter 中的 `title`，若 DeepSeek 未生成合法 Frontmatter 则降级为时间戳，但无告警。
    
- 未处理 DeepSeek API 返回内容不符合 Frontmatter 格式的情况（例如漏掉 `---`）。
    
- 目前没有提供前端界面，影响开箱即用体验。
    
- `save_to_obsidian` 中的 `requests.put` 没有重试机制，网络波动可能导致保存失败。
    

---

### 8. 扩展思路（未来可能加入的模块）

- 提供简单的 HTML 表单页面，集成在 FastAPI 中，用户访问根路径即可粘贴对话和指令。
    
- 支持本地 Ollama 模型作为备选（无需 API Key）。
    
- 增加对话历史管理：在 Obsidian 中自动建立 MOC（Map of Content）索引。
    
- 支持从浏览器扩展直接捕获对话页面的内容。
    
- 将配置部分抽离为 `config.yaml`，允许用户自定义 Frontmatter 模板和保存路径。
    
- 增加日志系统，分级记录 API 调用和保存结果。