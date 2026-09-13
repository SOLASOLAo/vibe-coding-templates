# vibe-coding-templates — 模板使用说明

模板版本：0.2，2026-09-13。

这是个人维护的 AI 编码项目骨架，适用于 Codex 等工具。四文档提供分工明确的资料入口，流程根据实际任务触发；它不是 OpenAI 指定的标准框架，也不绑定某个模型或代理 SDK。

## 创建新项目

### GitHub 模板入口

1. 在 [模板仓库](https://github.com/SOLASOLAo/vibe-coding-templates) 点击 **Use this template → Create a new repository**。
2. 克隆新建的项目仓库。AGENTS、README、HANDOVER、TODO 和六类目录直接位于项目根。
3. 填写四文档中的占位内容，写清真实环境命令、本次范围与验收条件，删除不适用的条目。
4. 按技术栈完善源码布局和 .gitignore；有已核对的变更且在授权范围内时再提交、推送。

GitHub 会保留模板的目录结构，详见 [GitHub 官方说明](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)。新仓库拥有自己的历史，不需要再次 git init。

### 本地复制入口

复制仓库根内容，排除模板自身的 .git。下面的命令要求目标目录尚不存在，避免覆盖已有项目；源副本保留以供检查。

Windows PowerShell：

```powershell
$templateSource = Join-Path $env:TEMP ('vibe-template-' + [guid]::NewGuid().ToString('N'))
$projectPath = 'C:\Projects\MyProject' # 改成自己的新项目路径
if (Test-Path -LiteralPath $projectPath) { throw '目标目录已存在，请选择一个新目录。' }
git clone --depth 1 -- https://github.com/SOLASOLAo/vibe-coding-templates.git $templateSource
if ($LASTEXITCODE -ne 0) { throw '克隆模板失败。' }
New-Item -ItemType Directory -Path $projectPath -ErrorAction Stop | Out-Null
Get-ChildItem -LiteralPath $templateSource -Force |
    Where-Object { $_.Name -ne '.git' } |
    Copy-Item -Destination $projectPath -Recurse -ErrorAction Stop
git -C $projectPath init
if ($LASTEXITCODE -ne 0) { throw '初始化项目 Git 失败。' }
```

Linux：

```sh
set -e
template_source=$(mktemp -d)
project_path=/path/to/MyProject # 改成自己的新项目路径
if [ -e "$project_path" ]; then
  echo 'Target already exists; choose a new directory.' >&2
  exit 1
fi
git clone --depth 1 -- https://github.com/SOLASOLAo/vibe-coding-templates.git "$template_source"
mkdir -p -- "$project_path"
find "$template_source" -mindepth 1 -maxdepth 1 ! -name .git -exec cp -R -- {} "$project_path/" \;
git -C "$project_path" init
```

完成复制后同样填写占位符。若需要远程，设置自己的项目仓库；不要把模板的 origin 当作项目发布目标。

## 四文档如何使用

| 文件 | 职责 | 读取 / 更新时机 |
| --- | --- | --- |
| AGENTS.md | 长期规则、环境事实、按需资料入口 | 遵守适用规则；约束或环境事实变化时更新 |
| README.md | 项目定位、范围与使用方法 | 了解项目或使用方式时读；对外行为变化时更新 |
| HANDOVER.md | 当前有效状态、验证证据与阻塞 | 续作或状态不明时读；状态变化时更新 |
| TODO.md | 已确定的任务与完成条件 | 选择任务时读；任务或验收状态变化时更新 |

常规修改在授权范围内完成实现与必要验证；只读问答无需制造文档变更或 Git 提交。需要停止时说明真实阻塞，继续不依赖该阻塞的工作。项目可以使用既有框架的目录惯例，六类目录是默认布局。

## 已有项目采用 0.2

1. 检查现有差异，保留未提交工作、环境事实、许可证边界、数据兼容与备份约定。
2. 合并 AGENTS 中的按需阅读、持续执行、适量验证与 Git 范围规则。按项目实际授权调整，不把占位符写入已有事实。
3. 将动态状态集中到 HANDOVER；让 TODO 保留可检验的完成条件。纯资料问答或无状态变化的任务可直接回答。
4. 将指向 ai-repo-skeleton 的模板来源链接更新到仓库根。0.1 的子目录已在 0.2 提升到根，勿继续使用旧复制路径。
5. 对照文件差异与链接检查结果确认升级范围。文档调整本身不证明项目代码、引擎运行或性能通过验证。

已有工程直接合并这些规则即可。无需重新初始化仓库、移动源码、替换现有四文档或改变引擎，也无需接入 Agents SDK。该升级不会自动传播到之前派生的项目。

## 验证建议

- GitHub 模板派生与本地复制应得到一致的项目根布局；本地复制不得带入模板的 .git 或远程配置。
- 从项目根核对适用的 AGENTS，避免把根规则放在未进入的子目录；参见 [Codex 的指令发现规则](https://learn.chatgpt.com/docs/agent-configuration/agents-md)。
- 检查纯问答、小修改、跨会话续作和真实外部阻塞几类任务的行为。文件检查与实际模型行为检查分别记录，不互相替代。

## 版本与来源

0.2 将骨架提升到根目录，去除固定会话读写和无条件提交推送，集中进度来源，并补充执行边界与验证规则。0.1 的来源和项目登记保留在 Git 历史中。

最初摘取自 SOLASOLAo/ctrlx-ai-coding 的实践；此前使用此骨架的项目包括 FreePLCDemo（软 PLC 原型，2026-08-12）及 VsCodeIDE_OpenPlc（ST 编程、监控和本地仿真，2026-08-13）。

模型相关依据与适用边界见 [模型协作说明](MODEL_GUIDANCE.md)。
