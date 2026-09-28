# WH Infinite Canvas

WH Infinite Canvas 是一个本地运行的 AI 图片创作工作台，提供无限画布、AI 图片生成、参考图编辑、对话助手、提示词、素材库、工作流、ComfyUI、RunningHub、ModelScope 以及浏览器/Photoshop 素材导入工具。

## 本地运行

首次使用可运行 `安装依赖.bat` 安装依赖。本机的自带 Python 已配置好依赖。Windows 下可直接运行 `run.bat`，也可以使用当前 Python 环境执行：

```powershell
python main.py
```

启动后打开 `http://127.0.0.1:3001`。如端口被占用，可设置 `WH_CANVAS_PORT` 使用其他端口。API Key 和画布素材默认保存在本机。

## 面试验收模式

启动服务后打开 `http://127.0.0.1:3001/interview`，进入不依赖 API Key 的本地 Mock 画布。该入口与生产画布共用仓库，但使用独立的版本化本地文档，便于在面试现场稳定演示题目 P0 闭环。

- 从界面创建图片、提示词和图片生成三类节点，节点位置使用世界坐标保存。
- 图片和提示词可以连接到生成节点；生成节点显示当前输入和比例参数。
- Mock 任务真实经历排队、生成中、成功或失败状态，运行中会阻止重复提交。
- 提示词包含 `#fail` 时确定性失败，保留失败原因、输入快照和参数；修改输入后可重试，历史任务仍可追踪。
- 成功结果会创建独立的图片资产和图片节点，不覆盖原始图片，并可继续作为下游输入。
- 文档包含 `version`、`viewport`、`nodes`、`edges`、`assets` 和 `tasks`，刷新后恢复；刷新时未完成的任务会转为可重试的 `interrupted` 状态。

现场验收建议：加载验收示例，拖动节点并滚轮缩放；运行一次生成查看状态和独立结果图片；将提示词改为包含 `#fail` 的内容验证失败；删除 `#fail` 后重试；最后刷新页面检查节点、连接、结果和任务记录恢复。

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

本项目保留原始仓库的许可证文本和第三方依赖声明；使用和再发布请遵守 `LICENSE` 中的条款，来源见 `NOTICE.md`。

更换端口示例（PowerShell）：

```powershell
$env:WH_CANVAS_PORT = "3002"
.\run.bat
```

插件的连接地址也需同步修改。当前版本未配置应用内在线更新源。

## 服务器部署

腾讯云轻量应用服务器的部署配置和步骤见 [deploy/README.md](deploy/README.md)。当前线上实例公开访问，服务仍仅监听服务器回环地址；如配置付费 API 或私有素材，建议在 Caddy 增加认证。
