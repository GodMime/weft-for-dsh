# 目录规划与提交范围

产品交付内容与内部工作资料由两个独立 Git 仓库管理。当前仓库公开产品源码、对外文档与正式品牌资产；可选的私有资料仓库放在 `.local/private/`，由当前仓库的 `.gitignore` 排除。两个仓库分别维护提交历史和远程地址。

私有资料用于设计和开发，不作为产品安装、构建、运行或发布的必要依赖。公开仓库应包含完整产品源码及所需的依赖清单、锁文件和构建配置。

## 公开目录

尚无内容的目录使用空 `.gitkeep` 文件纳入 Git，不代表对应组件已经实现。源码采用同一仓库中的独立包组织，便于 dsh 插件与辅助 CLI 分别构建和发布。

```text
weft-for-dsh/                 # 公开仓库根目录
├── .git/
├── packages/                # 正式产品源码
│   ├── dsh-plugin/
│   │   ├── src/             # 插件后端、宿主接入
│   │   └── client/          # 插件 UI、样式、前端资源
│   ├── core/
│   │   └── src/             # 编排逻辑、领域模型、执行器适配
│   └── cli/
│       └── src/             # 独立诊断、恢复等辅助工具
├── scripts/                 # 构建、发布、安装和迁移脚本
├── config/examples/         # 无凭据的 JSON/YAML 配置示例
├── examples/                # 使用示例与示例工作流
├── assets/brand/            # 正式图标、可编辑 SVG、PNG、品牌板
├── docs/                    # 面向用户与贡献者的定稿说明
│   ├── repository-layout.md # 本文
│   ├── brand/               # 当前品牌使用规范
│   ├── guides/              # 安装、配置、使用、升级指南
│   ├── architecture/        # 已实现架构与接口说明
│   └── development/         # 开发、构建、贡献说明
├── .github/                 # CI、Issue/PR 模板等协作配置
├── README.md                # 项目介绍与文档入口
├── LICENSE                  # 空占位，正式许可证在发布前补齐
├── CONTRIBUTING.md          # 空占位，开始接收贡献时补齐
├── CHANGELOG.md             # 空占位，发布时维护版本变更
├── .gitignore               # 当前公开仓库的排除规则
└── .local/                  # 整体忽略，不随公开仓库分发
    ├── private/             # 可选：独立私有资料仓库及其 .git/
    └── runtime/             # 本机临时数据、日志与迁移校验记录
```

`node_modules`、编译输出和工具缓存由安装或构建生成。目录加入实际文件后，可移除其中的 `.gitkeep`；空 `LICENSE` 仅用于占位，不表示已经授予开源许可。

## 内容归属

| 内容 | Git 管理范围 | 放置方式 |
|---|---|---|
| 正式产品源码 | 公开仓库 | `packages/`、`scripts/` 及必要配置、依赖清单、锁文件；产品运行所需代码全部公开 |
| 对外文档 | 公开仓库 | 根目录项目说明，以及 `docs/` 中面向用户、开发者的定稿使用、架构和接口说明 |
| 正式品牌成果 | 公开仓库 | `assets/brand/` 和 `docs/brand/README.md` |
| 内部需求、技术方案与术语 | 私有资料仓库 | 私有仓库内的 `docs/` 与 `CONTEXT.md` |
| AI 过程记录与调研 | 私有资料仓库 | 私有仓库内的 `.local/ai/`、`.local/research/` 与历史 `research/` |
| 本地测试与验证证据 | 私有资料仓库 | 私有仓库内的 `.local/tests/`，包括脚本、fixture、报告和截图 |
| 设计过程、交互原型与历史提案 | 私有资料仓库 | 私有仓库内的 `.local/design/` 与历史品牌提案目录 |
| Agent 工作约定 | 私有资料仓库 | 私有仓库内的 `AGENTS.md`；公开工作区通过本地兼容链接读取 |
| 环境变量、凭据、依赖缓存与运行数据 | 仅本机，不进入任一仓库 | dotenv 文件、凭据文件、缓存与 `.local/runtime/`；公开配置示例使用 JSON/YAML 占位值 |

是否由 AI 生成不决定文件是否公开：AI 编写的产品代码和对外说明属于交付内容；对话、推理草稿、任务计划、内部审核与验证记录属于私有工作资料。

测试文件与产品源码按用途区分。正式构建、迁移、诊断和恢复代码属于产品交付，即使曾用于验证，也应整理到公开源码目录；一次性探针与本机测试 fixture 放入私有资料仓库。

## 私有资料与旧路径

私有仓库保留既有资料的相对目录结构，以维持内部文档、原型资源和验证脚本之间的引用。公开工作区中的下列历史路径可通过本机符号链接指向私有仓库中的同名路径，链接本身也被公开仓库忽略：

- `AGENTS.md`、`CONTEXT.md` 与 `research/`。
- `docs/requirements.md`、`docs/technical-design.md`、需求问题与复审记录。
- `docs/brand/logo-brief-*.md` 与 `docs/brand/logo-v*/`。
- `.local/ai/`、`.local/research/`、`.local/tests/` 与 `.local/design/`。

克隆公开仓库不会包含私有资料或这些兼容链接。私有资料的克隆、链接恢复与提交方式由私有仓库自身说明维护。公开仓库的 Git worktree 同样不会自动携带被忽略的私有仓库，内部开发环境需单独接入相应资料。

公开说明只使用随公开仓库提交的文件链接或公开网站链接。从工作稿提炼说明时，一并去掉本机路径、真实配置和指向私有过程资料的链接。

## 提交前检查

在公开仓库根目录检查待提交文件和忽略规则：

```bash
git status --short --untracked-files=all
git check-ignore -v .env .local/private/README.md
git diff --cached --name-only
git diff --cached
```

需要管理私有资料时，显式指定私有仓库：

```bash
git -C .local/private status --short --untracked-files=all
git -C .local/private diff --cached
```

提交和推送前核对所在仓库及其远程地址。公开仓库的暂存列表应包含完整产品源码和需要发布的说明、资产；私有资料、兼容链接与环境文件继续由其排除规则保护。私有仓库也使用自己的 `.gitignore` 排除凭据、环境文件与运行数据。

`.gitignore` 作用于尚未跟踪的文件，不会清除已经提交的内容或 Git 历史。两个仓库拥有独立历史，不能依靠为同一仓库添加多个 remote 实现按目录隔离。
