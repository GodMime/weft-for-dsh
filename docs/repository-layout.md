# 目录规划与提交范围

仓库将产品交付内容与本地工作资料按目录分开管理。公开文件正常纳入 Git；本地资料由根目录 `.gitignore` 排除。

## 目录规划

以下公开目录已创建；尚无内容的目录使用空 `.gitkeep` 文件纳入 Git，不代表对应组件已经实现。源码采用同一仓库中的独立包组织，便于 dsh 插件与辅助 CLI 分别构建和发布。

```text
weft-for-dsh/
├── packages/                  # 公开：正式产品源码
│   ├── dsh-plugin/
│   │   ├── src/               # 插件后端、宿主接入
│   │   └── client/            # 插件 UI、样式、前端资源
│   ├── core/
│   │   └── src/               # 编排逻辑、领域模型、执行器适配
│   └── cli/
│       └── src/               # 独立诊断、恢复等辅助工具
├── scripts/                   # 公开：构建、发布、安装和迁移脚本
├── config/
│   └── examples/              # 公开：无凭据的 JSON/YAML 配置示例
├── examples/                  # 公开：使用示例与示例工作流
├── assets/
│   └── brand/                 # 公开：图标、SVG 编辑源、PNG、品牌板
├── docs/                      # 公开：面向用户与贡献者的定稿说明
│   ├── repository-layout.md   # 本文
│   ├── brand/                 # 当前品牌使用规范
│   ├── guides/                # 安装、配置、使用、升级指南
│   ├── architecture/          # 已实现架构与接口说明
│   └── development/           # 开发、构建、贡献说明
├── .github/                   # 公开：CI、Issue/PR 模板等协作配置
├── README.md                  # 公开：项目介绍与文档入口
├── LICENSE                    # 公开：空占位，正式许可证在发布前补齐
├── CONTRIBUTING.md            # 公开：空占位，开始接收贡献时补齐
├── CHANGELOG.md               # 公开：空占位，发布时维护版本变更
├── .gitignore                 # 公开：本地文件排除规则
├── .local/                    # 仅本地：整个目录忽略
│   ├── ai/                    # AI 计划、任务拆解、会话与审核记录
│   ├── research/              # 调研、实现方案草稿、验证笔记
│   ├── tests/                 # 本机测试脚本、数据、报告、截图、录屏
│   ├── design/                # 图标草案、提示词、设计过程与历史方案
│   └── runtime/               # 本机运行数据、临时文件、日志
├── AGENTS.md                  # 仅本地：供编码 Agent 读取的工作约定
└── .env                       # 仅本地：环境变量与凭据
```

包的依赖清单、锁文件和构建配置随相应源码提交。`node_modules`、编译输出和工具缓存由安装或构建生成。目录加入实际文件后，可移除其中的 `.gitkeep`；空 `LICENSE` 仅用于占位，不表示已经授予开源许可。

## 六类内容的处理规则

| 内容 | 提交范围 | 放置方式 |
|---|---|---|
| AI 编码过程资料 | 不提交 | 统一放 `.local/ai/`；调研及技术方案草稿放 `.local/research/` |
| `.env` 环境文件 | 不提交 | 任意层级的 `.env`、`.env.*`、`*.env`、`*.env.*` 均忽略，包含 `.env.example`；公开配置示例改用 JSON/YAML，值使用占位符 |
| 正式产品源码 | 提交 | `packages/`、`scripts/` 及必要的配置、依赖清单、锁文件；产品运行所需代码均放公开目录 |
| 开源描述性文档 | 提交 | 根目录项目说明，以及 `docs/` 中面向用户、开发者的定稿使用、架构和接口说明 |
| 本地测试相关 | 不提交 | 测试脚本、fixture、真实数据、验证结果及报告统一放 `.local/tests/` |
| 图标与设计资料 | 正式成果提交，过程不提交 | 当前图标、可编辑 SVG、PNG、品牌板、使用规范放 `assets/brand/` 和 `docs/brand/`；草案及设计过程放 `.local/design/` |

是否由 AI 生成不决定文件是否公开：AI 编写的产品代码和对外说明属于交付内容；对话、推理草稿、任务计划、内部审核与验证记录属于本地工作资料。

本地测试文件与产品源码按用途区分。正式构建、迁移、诊断和恢复代码属于产品交付，即使曾用于验证，也应整理到公开源码目录；一次性探针与本机测试 fixture 留在本地。

## 现有资料的兼容处理

为了保留现有文档之间的引用，以下历史路径暂时保持原位置并加入忽略规则。后续新增过程资料使用 `.local/`。

| 当前路径 | 处理 |
|---|---|
| `CONTEXT.md` | 仅本地，供设计与编码阶段统一术语 |
| `research/` | 仅本地，现有内容为调研、探针、实验 fixture 和验证结果；正式产品源码使用 `packages/` |
| `docs/requirements.md`、`docs/technical-design.md` | 仅本地，当前需求与实施方案工作稿；对外说明另行整理到公开文档目录 |
| `docs/requirements-issues.md`、`docs/requirements-review-*.md` | 仅本地，需求处理与复审记录 |
| `docs/brand/logo-brief-*.md`、`docs/brand/logo-v*/` | 仅本地，品牌需求、历史草案和探索版本 |
| `assets/brand/`、`docs/brand/README.md` | 公开，正式素材与当前使用规范 |

公开说明使用仓库内可访问的链接或公开网站链接；从工作稿提炼说明时，一并去掉本机路径、真实配置和指向本地过程资料的链接。

## 提交前检查

查看准备纳入 Git 的文件与忽略规则：

```bash
git status --short --untracked-files=all
git check-ignore -v .env .local/tests/report.json
git diff --cached --name-only
git diff --cached
```

`.gitignore` 作用于尚未跟踪的文件，不会清除已经提交的内容或 Git 历史。检查暂存列表时，确认包含完整产品源码和需要发布的说明、资产，且没有本地工作资料与环境文件。
