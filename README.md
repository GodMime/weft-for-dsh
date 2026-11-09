# weft-for-dsh

面向 dsh 的多 Agent 编排插件，目标是在 dsh 中组织需求、工作流和多个编码 Agent 的协作。

项目目前处于设计与原型验证阶段，尚未发布可安装的产品版本。正式源码将随实现逐步加入本仓库。

## 两个工程的区别与职责

内部开发工作区采用两个独立 Git 仓库，分别管理产品交付内容和内部设计资料：

| 工程 | 可见性 | 本地位置（相对工作区根目录） | 职责与内容 |
|---|---|---|---|
| `weft-for-dsh`：产品工程 | 公开 | `.` | 实现并交付 dsh 多 Agent 编排插件；保存插件后端与 UI、编排核心、辅助 CLI，以及构建、安装、迁移、发布脚本、对外文档和正式品牌资产 |
| `weft-for-dsh-private`：设计与研究工程 | 私有 | `.local/private/` | 定义产品范围与验收标准、规划技术实现、验证交互与接入可行性；保存需求、技术方案、术语、交互设计稿、调研、内部验证脚本和证据 |

公开产品工程的主要目录为 `packages/`、`scripts/`、`docs/`、`config/examples/`、`examples/` 和 `assets/brand/`。产品安装、构建、运行和发布所需的本项目代码全部放入公开工程，私有资料用于内部研发，不作为产品交付的必要依赖。

两个工程在本地的关系如下；克隆公开仓库时，私有工程不会一并下载：

```text
工作区根目录/                 # weft-for-dsh：公开产品工程
├── .git/                    # 公开工程的 Git 历史与远程配置
├── README.md                # 当前说明
├── packages/                # 产品源码：插件、编排核心、辅助 CLI
├── scripts/                 # 产品构建、安装、迁移与发布
├── docs/                    # 对外文档
├── assets/brand/            # 正式品牌资产
└── .local/                  # 被公开工程整体忽略
    ├── private/             # weft-for-dsh-private：私有设计与研究工程
    │   ├── .git/            # 私有工程独立的 Git 历史与远程配置
    │   ├── README.md        # 内部资料入口及工作区恢复说明
    │   ├── docs/            # 内部需求与技术方案
    │   ├── CONTEXT.md       # 项目术语
    │   ├── research/        # 技术调研与接入探针
    │   └── .local/          # 交互设计稿、内部验证与工作记录
    └── runtime/             # 仅本机的临时数据，不进入任一仓库
```

在工作区根目录执行以下命令，分别查看两个工程：

```bash
# 公开产品工程
git status --short --untracked-files=all
git remote -v

# 私有设计与研究工程（已检出时）
git -C .local/private status --short --untracked-files=all
git -C .local/private remote -v
```

两个工程分别执行 `add`、`commit` 和 `push`；在工作区根目录直接执行 Git 命令，默认操作公开工程。私有工程使用 `git -C .local/private ...`。环境文件、凭据与本机运行数据保留本地，不提交到任一工程。

## 项目资料

- [目录规划与提交范围](docs/repository-layout.md)
- [品牌与图标使用说明](docs/brand/README.md)
- [品牌素材与可编辑 SVG](assets/brand/README.md)
