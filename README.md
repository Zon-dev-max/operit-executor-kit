# Operit 执行器套件

面向 [Operit](https://github.com/AAswordman/Operit) 的通用执行器角色卡 + 三个配套技能。让 AI 按「规划 → 执行 → 验证 → 归档」的流程干活，交付可验证、可回滚的结果。

> **个人自用声明**：这套配置是我个人在自己机器上用的，不保证通用，也不保证适配所有人。你可以按自己的实际情况改造，也可以直接照用我这一套。配置里用到的目录（工作区 `active/`、归档 `archive/`、`skills/` 等）如果你本地还没有，可以让 AI 按规范跑一遍自动创建，不用手动一个个建。

## 一键下载

| 想要什么 | 怎么拿 |
|---|---|
| **最新版下载页（推荐）** | [Releases → v1.0](https://github.com/Zon-dev-max/operit-executor-kit/releases/latest)：角色卡 `.json` 与三技能 `zip` 都在这里，点附件名即下载 |
| **全部文件（zip）** | 点这里直接下载 → [operit-executor-kit-main.zip](https://github.com/Zon-dev-max/operit-executor-kit/archive/refs/heads/main.zip) |
| **只要角色卡** | 打开 [通用执行器-v3.7.json](https://github.com/Zon-dev-max/operit-executor-kit/blob/main/character-cards/通用执行器-v3.7.json)，点右上角 **Download raw file** → 保存为 `.json` 文件 |
| 只要某个技能 | 进 `skills/<技能名>/` 目录，点 `SKILL.md` 后同样点 **Download raw file** |

> 不熟悉 GitHub 的话：仓库首页点绿色 **Code → Download ZIP**，解压后 `character-cards/通用执行器-v3.7.json` 就是角色卡文件。

## 仓库内容

| 文件 | 用途 |
|---|---|
| `character-cards/通用执行器-v3.7.json` | 角色卡：铁律、工作流主链、输出格式、敏感任务边界 |
| `skills/通用执行器·细则/SKILL.md` | 细则分片：三验模板、交接单模板、回滚凭据、动作分级 |
| `skills/任务五步走·落盘规范/SKILL.md` | 单项目内部流程：复用→Refine→Plan→Act→沉淀 + 三验 |
| `skills/工作区管理·索引与归档规范/SKILL.md` | 多项目组织：active/done/archive 分层、INDEX 索引、子任务截断 |

## 导入方法

### 角色卡
1. 下载 `character-cards/通用执行器-v3.7.json`
2. Operit → 角色卡 → 导入 → 选择该文件

### 技能（三个分别导入）
每个技能一个独立目录，Operit 从仓库导入时指向对应目录：

- 通用执行器·细则：https://github.com/Zon-dev-max/operit-executor-kit/tree/main/skills/通用执行器·细则
- 任务五步走·落盘规范：https://github.com/Zon-dev-max/operit-executor-kit/tree/main/skills/任务五步走·落盘规范
- 工作区管理·索引与归档规范：https://github.com/Zon-dev-max/operit-executor-kit/tree/main/skills/工作区管理·索引与归档规范

技能可独立使用，也可以按需只装其中一两个。角色卡即使不装任何技能也能独立使用。

## 注意

- 技能里的路径（如 `/sdcard/Download/Operit/workspace/active/`）是默认值，请按自己的 Operit 工作区调整。
- 本地编写，非酒馆导入产物。反馈请开 Issue。

## 许可

MIT，见 LICENSE。