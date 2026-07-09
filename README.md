# 资源站监控 - 影视资源站可用性监测工具

![GitHub Actions](https://img.shields.io/github/actions/workflow/status/mylazily/ziyuanzhan/monitor.yml?branch=main&style=flat-square)
![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=flat-square)
![Python 3.11](https://img.shields.io/badge/Python-3.11-green?style=flat-square)
![GitHub Stars](https://img.shields.io/github/stars/mylazily/ziyuanzhan?style=flat-square)
![GitHub Forks](https://img.shields.io/github/forks/mylazily/ziyuanzhan?style=flat-square)

自动监测 [ziyuanzu.com](https://www.ziyuanzu.com/) 影视资源站可用性的开源工具，通过 GitHub Actions 定时运行，生成静态监控页面并通过 GitHub Pages 发布。

支持 LibreTV、maccms、LunaTV、KatelyaTV、OrionTV、MoonTVPlus 等平台。

**MCP (Model Context Protocol) 服务器支持 - 让 AI 工具直接查询资源站数据！**

## 功能特性
- **定时监测**：每 2 小时自动抓取并检测所有资源站状态
- **实时数据**：HTTP 状态码、响应时间、在线/离线状态
- **静态页面**：自动生成美观的暗色主题监控面板
- **历史存档**：每次监测结果保存为 JSON，支持历史对比
- **GitHub Pages**：零成本部署，自动更新
- **MCP 服务器**：为 Claude/Cursor 等 AI 工具提供资源站查询能力

## 项目结构
```
ziyuanzhan/
├── .github/
│   └── workflows/
│       └── monitor.yml      # GitHub Actions 定时工作流
├── docs/
│   └── index.html           # 生成的监控页面（GitHub Pages 源）
├── data/
│   ├── latest.json          # 最新监测数据
│   └── monitor_*.json       # 历史数据存档
├── monitor.py               # 核心监测脚本
├── mcp_server.py            # MCP 服务器
├── mcp.json                 # MCP 配置示例
├── requirements.txt         # Python 依赖
└── README.md                # 项目说明
```

## 快速开始

### 1. Fork 本项目
点击右上角 **Fork** 按钮，将本项目复制到你的 GitHub 账号下。

### 2. 启用 GitHub Pages
1. 进入你 Fork 后的仓库
2. 点击 **Settings** → **Pages**
3. **Source** 选择 **Deploy from a branch**
4. **Branch** 选择 `main`，文件夹选择 `/docs`
5. 点击 **Save**

等待几分钟后，你的监控页面将发布在：
```
https://你的用户名.github.io/ziyuanzhan/
```

### 3. 启用 GitHub Actions
1. 进入仓库的 **Actions** 标签页
2. 点击 **I understand my workflows, go ahead and enable them**
3. 工作流将自动按照 cron 计划每 2 小时运行一次

### 4. 手动触发（可选）
进入 **Actions** → **Monitor ziyuanzu.com Resources** → **Run workflow**，可以立即手动运行一次监测。

## MCP 服务器使用

### 安装依赖
```bash
pip install -r requirements.txt
```

### 配置到 Claude Desktop

1. 打开 Claude Desktop 配置文件：
   - macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - Windows: `%APPDATA%\Claude\claude_desktop_config.json`

2. 添加以下配置（替换为实际路径）：
```json
{
  "mcpServers": {
    "ziyuanzhan-monitor": {
      "command": "python3",
      "args": [
        "/path/to/ziyuanzhan/mcp_server.py"
      ]
    }
  }
}
```

3. 重启 Claude Desktop

### 可用工具
- `get_all_resources` - 获取所有资源站列表及状态
- `get_online_resources` - 获取在线资源站
- `get_offline_resources` - 获取离线资源站及原因
- `get_statistics` - 获取整体统计数据
- `search_resource` - 按关键词搜索资源站
- `get_fastest_resources` - 获取响应最快的资源站

### 使用示例
在 Claude 中可以这样提问：
- "帮我看看当前有哪些资源站在线"
- "搜索一下包含'蓝光'的资源站"
- "给我看看整体统计数据"
- "响应最快的5个资源站是哪些？"

## 本地运行
```bash
# 克隆仓库
git clone https://github.com/mylazily/ziyuanzhan.git
cd ziyuanzhan

# 安装依赖
pip install -r requirements.txt

# 运行监测脚本
python monitor.py

# 启动 MCP 服务器
python mcp_server.py

# 查看生成的页面
open docs/index.html
```

## 自定义配置

### 修改监测频率
编辑 `.github/workflows/monitor.yml` 中的 cron 表达式：
```yaml
schedule:
  - cron: '0 */2 * * *'   # 每2小时（默认）
  - cron: '0 */6 * * *'   # 每6小时
  - cron: '0 0 * * *'     # 每天0点
```

### 修改超时时间
编辑 `monitor.py` 中的 `TIMEOUT` 变量：
```python
TIMEOUT = 15  # 秒
```

## 数据来源
- 资源站列表：[ziyuanzu.com](https://www.ziyuanzu.com/)
- 监测方式：HTTP 请求检测可用性

## 免责声明
本项目为第三方开源监测工具，与 ziyuanzu.com 官方无关。仅供学习和技术交流使用。

## License
[MIT](LICENSE)
