# WH Infinite Canvas

WH Infinite Canvas 是一个本地运行的 AI 图片创作工作台，提供无限画布、AI 图片生成、参考图编辑、对话助手、提示词、素材库、工作流、ComfyUI、RunningHub、ModelScope 以及浏览器/Photoshop 素材导入工具。

面试提交仓库：[`wh-infinite-canvas-interview`](https://github.com/whdj/wh-infinite-canvas-interview)。本仓库保留原有产品能力，并按面试题要求补充了验收说明；题目验收报告见 [`docs/面试验收报告.md`](docs/面试验收报告.md)。

## 本地运行

首次使用可运行 `安装依赖.bat` 安装依赖。本机的自带 Python 已配置好依赖。Windows 下可直接运行 `run.bat`，也可以使用当前 Python 环境执行：

```powershell
python main.py
```

启动后打开 `http://127.0.0.1:3001`。如端口被占用，可设置 `WH_CANVAS_PORT` 使用其他端口。API Key 和画布素材默认保存在本机。

macOS/Linux 也可以使用：

```bash
python3 -m pip install -r requirements.txt
python3 main.py
```

## 项目结构

- `main.py`：本地服务与 API
- `static/`：网页界面
- `workflows/`：内置工作流
- `tools/`：浏览器素材导入与 Photoshop 连接工具
- `CLI/`：可选的 CLI 集成脚本

## 面试题验收说明

### 已具备的能力

- 从画布界面创建图片、提示词和生成节点。
- 世界坐标画布、鼠标中键平移、滚轮缩放、节点拖动和节点删除。
- 图片/提示词到生成节点的有向连接、输入预览和连接清理。
- 真实 API 生成任务的提交、pending 输出、轮询、日志和结果保存。
- 画布节点、连接、视口和输出记录的本地服务端保存与刷新恢复。

### 与本题 P0 的边界

当前生成节点是原产品的真实 API 链路，并非题目要求的无 API Key 本地 mock；真实生成结果默认进入 `Output` 节点或生成节点的输出引用，也不是自动创建的独立图片结果节点。普通单节点失败主要通过错误提示处理，尚未提供面试专用的确定性失败开关和统一重试任务记录。

因此，现场演示时不应把真实 API 的 pending 状态描述成本题 mock 已通过。应演示已有的画布、连接、输入读取和保存恢复能力，并主动说明上述边界。完整矩阵、可执行演示脚本、技术追问回答和后续优先级见 [`docs/面试验收报告.md`](docs/面试验收报告.md)。

### 主要依赖和 AI 使用说明

- 后端：FastAPI、Uvicorn、Pydantic、Requests、HTTPX、Pillow、WebSockets。
- 前端：原生 HTML/CSS/JavaScript；画布渲染和交互逻辑位于 `static/js/canvas.js`，图标使用仓库内 Lucide 资源。
- AI 工具：本次使用 Codex 进行题目拆解、现有实现核对、验收报告和 README 整理；本次文档更新没有向画布实现新增功能代码。
- 现有代码、第三方声明和许可证以 `LICENSE`、`NOTICE.md` 以及仓库历史为准。

本项目保留原始仓库的许可证文本和第三方依赖声明；使用和再发布请遵守 `LICENSE` 中的条款，来源见 `NOTICE.md`。

更换端口示例（PowerShell）：

```powershell
$env:WH_CANVAS_PORT = "3002"
.\run.bat
```

插件的连接地址也需同步修改。当前版本未配置应用内在线更新源。

## 服务器部署

腾讯云轻量应用服务器的部署配置和步骤见 [deploy/README.md](deploy/README.md)。当前线上实例公开访问，服务仍仅监听服务器回环地址；如配置付费 API 或私有素材，建议在 Caddy 增加认证。
