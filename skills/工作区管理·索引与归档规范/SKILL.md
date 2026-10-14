---
name: 工作区管理·索引与归档规范
description: 当工作区项目变多（≥5 个）、单项目 step 超 20、文件膨胀，或要建/更新全局索引、执行归档或巡检时使用。横切规范：涉及多项目或长任务文件管理即适用，始终生效。
---

# 工作区管理·索引与归档规范

> 读我的人＝执行项目的 AI。开工前读完，全程照做。
> **定位：横切规范，始终生效**——不依附任何单一任务，凡涉及多项目、长任务文件管理、索引维护、归档操作都适用。

## 0. 何时用（触发；与 description 同一口径）

- 工作区项目 ≥5 个，或单项目 step >20
- 要快速知道「哪些在进行 / 暂挂 / 已完成 / 可归档」
- 用户要求建立/更新索引、执行归档、清理膨胀文件
- 扫描后发现目录混乱、空项目、断层、垃圾堆积
- 长任务跨多天/多周，需要保持可回溯性

## 0.1 核心原则（写死）

1. **一个项目一个独立工作区**，绝不混放不相关任务。
2. **状态用目录体现，不写进项目名**（active / paused / done / archive）。
3. **索引是轻量总览，不是第二套日志**：只记状态、路径、一句话摘要。
4. **归档是移动整目录**，不是复制或删除内部文件。
5. **成功交付物永不主动删除**；临时物必须清零。
6. **先建索引再动手**，避免找不到东西。
7. **任何 `mv` 前先 `mkdir -p` 目标目录**——目标不存在时 `mv` 是改名，不是移动。
8. **一个对象只允许一个活跃项目**（canonical）：同一对象的后续优化一律**续作**已有项目（§3.6），不新建；确需新开时 `plan/step_1.md` 必须写 `承接: <旧项目>`，并在 INDEX 该项目行标 `重复→<canonical项目>`。**禁止链式**（A→B→C）——每个重复项直指最终项。

## 1. 标准分层（唯一允许的布局）

```text
/sdcard/Download/Operit/workspace/
├── active/          # 正在进行（唯一允许新建的地方）
│   ├── <项目名A>/
│   └── ...
├── paused/          # 已开工、主动搁置、未完成（随时可恢复）
├── done/            # 已完成、验收通过
│   ├── 2026-10/
│   └── 2026-09/
├── archive/         # 彻底封存
│   └── 2026-Q3/
├── INDEX.md         # 全局轻量索引（必须存在）
└── INSPECT_LOG.md   # 巡检历史（INDEX 只留最近一次）
```

固定规则：

- 新项目**必须**建在 `active/` 下。
- 项目内部结构固定为`{plan,src,tmp,logs}`，不得新增第五个子目录。
- `done/` `archive/` 下按「年月」或「季度」分子目录；状态目录下不得再自创层级。
- 禁止在 `workspace/` 根直接放项目文件夹。
- **项目名规则**：**按对象命名**（同一对象的每轮优化复用同一个名字，查重才命得中），不是按任务措辞；允许中英文、数字、下划线、连字符（≤30 字符）；不含空格、日期、状态词、版本号。
- **active ↔ paused 的区别**：active＝当前在推进；paused＝有未完成的下一步但主动搁置。两者用整目录 `mv` 互转（状态由目录体现，项目名不改）。
- **根目录残留**：根下只允许上面 6 项。发现其他条目（含隐藏目录）→ 巡检如实报告；属他方或来源不明 → 只报不动；能定归属 → 移入对应项目。

## 2. 全局索引（INDEX.md）

位置：`/sdcard/Download/Operit/workspace/INDEX.md`（固定，不可移动）

### 2.1 必须包含的字段

```markdown
# 工作区全局索引

最后更新：YYYY-MM-DD HH:MM
统计：active N ｜ paused N ｜ done N ｜ archive N

## 进行中（active）
| 项目名 | 对象 | 当前 step | 一句话状态 | 路径 | 最后活动 |
|--------|------|-----------|------------|------|----------|
| 项目A  | <对象> | step_N | <一句话状态 ≤20 字> | active/项目A/ | <日期> |

## 暂挂（paused）
| 项目名 | 对象 | 停在哪一步 | 一句话状态 | 路径 | 最后活动 |
|--------|------|------------|------------|------|----------|

## 已完成（done）
| 项目名 | 对象 | 完成日期 | 最终产物摘要 | 路径 |
|--------|------|----------|--------------|------|

## 归档（archive）
| 项目名 | 对象 | 归档日期 | 备注 | 路径 |
|--------|------|----------|------|------|

## 巡检记录（最近一次）
- 时间：
- 发现的问题：
- 已处理：
- 历史见 INSPECT_LOG.md
```

字段口径（**查重键**）：

- `对象`＝稳定对象名（如 `规范与执行器·四件套`、`MiMo-cache_opt`、`手机Edge本地闭环`）。它**不随任务措辞变**，是查重（§3.1）与续作（§3.6）的键。
- 同一 `对象` 的项目，在 `active/` 里**只允许一行**。重复 / 续作关系写在「一句话状态」里：`重复→<canonical项目>` 或 `承接 <旧项目>`（**禁止链式**，定义见 §0.1-8）。
- `done` / `archive` 的 `对象` 列必须保留——续作要按对象从旧项目里找人。
- §3.5 的子项目：`对象` 与主项目**相同**，状态列写 `承接 <主项目> step_N`。

### 2.2 更新规则

- 新建项目、状态迁移、归档、巡检、发现异常后，必须更新 INDEX.md。
- 只改对应表格行，不重写整个文件；「一句话状态」≤20 字，只写阻塞点或下一步。
- 禁止把 plan/logs 内容复制进 INDEX.md；巡检历史写 `INSPECT_LOG.md`，INDEX 只留最近一次。
- 「最后活动」：每次 update plan/logs 时顺手刷新，不要等归档时才刷。
- **统计行**：与 §4 的机检输出对不上即为不一致，必须当场核对修正。
- **大改前留备份**（重排表格 / 批量迁移）：`cp "$D/INDEX.md" "$D/active/<当前项目>/src/父版-INDEX-$(date +%Y%m%d_%H%M).bak"`；日常单行更新不留。**不要放 workspace 根**（§1 根只允许 6 项，放根会被巡检报成残留）。

## 3. 生命周期操作

### 3.1 新建项目（**先查重，再建**）

**第 0 步 · 查重（不跑这一步不得新建）**——在已有项目里搜对象词：

```bash
D=/sdcard/Download/Operit/workspace
OBJ='<对象词1>|<对象词2>'   # 例：'规范|角色卡|技能'
grep -nE "$OBJ" "$D/INDEX.md"
ls -d "$D"/active/*/ "$D"/paused/*/ "$D"/done/*/*/ "$D"/archive/*/*/ 2>/dev/null | grep -E "$OBJ"
```

- **命中 → 改走续作（§3.6），不新建。**
- 未命中 → 继续；`plan/step_1.md` 首部必须写 `对象: <稳定对象名>`。
- 确需为同一对象开第二个项目（目标确实不同）→ 首部改写 `承接: <旧项目>`，并在 INDEX 该项目行状态里标 `重复→<canonical>`。

```bash
# POSIX 写法：**不要**写 <项目名>/{plan,src,tmp,logs} —— brace expansion 只在 bash/zsh/mksh 展开，
# dash（Ubuntu 的 sh）下会静默建成一个字面目录 `{plan,src,tmp,logs}`，退出码仍为 0
mkdir -p "$D/active/<项目名>"/plan "$D/active/<项目名>"/src "$D/active/<项目名>"/tmp "$D/active/<项目名>"/logs
ls "$D/active/<项目名>"    # 自检：必须恰好列出 logs plan src tmp
```

1. 项目名按 §1 规则自查：**对象名**（同一对象复用同名）、无日期 / 状态词 / 空格 / 版本号。
2. 建结构 → 立即更新 INDEX.md「进行中」+ 统计行（**`对象` 列必填**）→ 写 `plan/step_1.md`（首部含 `对象:`）。

### 3.2 项目完成（验收通过）

1. 确认 plan/logs 1:1 配对、tmp 已清零、正式产物在 src/ 或交付位。
2. **先建目标目录再移动**（目标不存在时 `mv` 会把项目改名成月份，项目名丢失）：

```bash
D=/sdcard/Download/Operit/workspace
mkdir -p "$D/done/$(date +%Y-%m)"          # ← 必做
mv "$D/active/<项目名>" "$D/done/$(date +%Y-%m)/"
ls "$D/done/$(date +%Y-%m)/<项目名>"       # 确认项目名完整、内容在位
```

3. 更新 INDEX.md：从「进行中」删除，加入「已完成」，刷新统计行。
4. 如有交付物，确认已复制到 `/sdcard/Download/<项目名>-交付/`。
5. 如该项目是子任务（见 §3.5），在其父项目最新交接单标注「已交付」并附新路径。

### 3.3 归档（彻底封存）

触发（任一即做）：进 done 超 30 天且用户确认不再需要 / 用户明确说「归档」/ 巡检发现长期无活动且无未关闭问题。

```bash
D=/sdcard/Download/Operit/workspace
Y=$(date +%Y)
Q=Q$(( ($(date +%-m)-1)/3+1 ))            # %-m 在 toybox 与 GNU 均可用
mkdir -p "$D/archive/$Y-$Q"                # ← 必做
mv "$D/done/YYYY-MM/<项目名>" "$D/archive/$Y-$Q/"
```

更新 INDEX.md（移入「归档」+ 统计行）。**不删除任何内部文件**（容器合并见 §3.4-B，属唯一例外）。

### 3.4 文件数膨胀

> 与 §3.5 是两条不同的轴：本节管**文件数**，§3.5 管**任务链长度**。

#### A. 未冻结项目（active / paused）：只折叠、不动原件

- 建 `logs/INDEX.md`（项目级轻量索引）：只列 step 编号 + 一句话。
- 原件 `plan/step_n.md`、`logs/step_n.md` 一律原位保留，**不得**合并或删除（保持 1:1 配对）。
- **不得**新建 `src/历史过程/` 之类子目录。
- 注：项目根不放任何文件，故项目级索引落 `logs/`。

#### B. 冻结项目（done / archive）：容器合并（**唯一允许动原件的场合**）

- 触发（任一）：`logs/` ≥20 个文件，或 `logs/` ≥200 KB。
- 范围：**只合 `logs/`**；`plan/` 契约原文一律不动一字。
- 做法：

```bash
P="/sdcard/Download/Operit/workspace/done/YYYY-MM/<项目名>"
cd "$P/logs"
find . -maxdepth 1 -type f | sed 's|^\./||' | sort > /tmp/l
sha256sum $(cat /tmp/l) > MANIFEST.sha256          # 卷头清单（校验用）
tar czf steps.tar.gz -T /tmp/l MANIFEST.sha256    # 体积 ≤256 KB，超则按 step 序号分卷 steps-01.tar.gz…
```

- 校验（**不过即回滚**）：`tar tzf` 条目数 = 原文件数 + 1；解到临时目录后 `sha256sum -c MANIFEST.sha256` 全 OK；逐文件 `cmp` 一致。
- 建 `logs/INDEX.md`：每步一行摘要 + 卷名 + 条目数 + 复原命令。
- 校验通过后才删除原 step 文件（内容已在卷内，**不是**丢数据）。
- 复原（一条命令，写进交接单）：
  `cd "$P/logs" && tar xzf steps.tar.gz && rm -f steps.tar.gz INDEX.md MANIFEST.sha256`
  原因：卷头清单必须**一起删**——卷已不在时 §4-2 断层自检走 `ls logs` 分支，残留 `MANIFEST.sha256` 会造成配对不齐的**假警**。

硬约束：

- 卷内**必须**含 `MANIFEST.sha256`。
- **禁止「卷 + 原件」双份并存**（同一真相禁存两份，副本必须可自动校验）。
- 未冻结项目**禁止**容器合并；冻结项目的 `plan/` 侧**永不**合并。

### 3.5 子任务截断（长任务拆解）

触发（任一）：
- 单项目预估 step >20
- 剩余步骤 ≥10 且属独立可交付单元
- 单次对话上下文装不下剩余步骤

拆法：
1. 主项目在截断点 step_N 的交接单「下一步」写：`后续见子项目 <子项目名>，上游依赖 step_N`。
2. 建 `active/<主项目名>-<阶段名>/`（阶段名用短语义词，如 `-阶段2`、`-收尾`，不带日期）。
3. 子项目 step_1 上游依赖写：`主项目 <主项目名> / logs/step_N.md`。
4. INDEX.md「进行中」新增一行，状态注明 `承接 <主项目名> step_N`。
5. 主项目保留原 step_1..step_N，不搬走、不归档；**截断点后主项目冻结**。

禁止：用对话记忆当截断依据 / 子项目另起不相关名字 / 主项目与子项目同时改同一个文件。

### 3.6 续作（reopen：同一对象的后续优化）


触发：要改的对象已有项目（可能在 active / paused / done / archive）。

```bash
D=/sdcard/Download/Operit/workspace
mkdir -p "$D/active" && mv "$D/done/<YYYY-MM>/<项目名>" "$D/active/"
# 从 archive：mkdir -p "$D/active" && mv "$D/archive/<YYYY-Qx>/<项目名>" "$D/active/"
```

1. **整目录 `mv` 回 `active/`**（不改项目名）；INDEX 从「已完成」移回「进行中」。
2. 新 `plan/step_<n+1>.md` 写 `上游依赖：logs/step_<n>.md`（`n`＝该项目已有最大 step 号）。
3. **不重编号、不搬旧文件、不改旧 step 内容**（旧交接单是历史证据）。
4. paused → active 同法（§1：状态由目录体现）。
5. 若该项目此前被标过 `重复→<canonical>`：**续作 canonical，不续作重复项**。

### 3.7 体积治理（INDEX 与 INSPECT_LOG 的滚动 / 卷化 / 快照）

> 与 §3.4 是两条不同的轴：§3.4 管**项目内文件数**，本节管**全局那两个单文件**的体积。

阈值（**任一触发即滚动**）：

| 文件 | 体积上限 | 条目上限 |
|---|---|---|
| `INDEX.md` | 8 KB | — |
| `INSPECT_LOG.md` | 32 KB | **当前区 ≤5 条** |

#### A. 双区制（INSPECT_LOG 结构）

```markdown
# 巡检日志                 ← 固定头部（永不滚动）
## 快照                    ← 覆盖式重写：最近一次 / 当前区 n/5 / 段数 / 主要发现 Top3
## 段清单                  ← CURRENT 指针：段名 | 覆盖 | 条目 | 卷路径
## 当前区（最近 ≤5 次，全文）
## 历史区（一行一条：<日期> — <主题>｜全文见段 <段名>）
```

- **快照区、段清单、INDEX 的 active/paused 行＝固定前缀**，滚动时**逐字节不动**（保 prompt 缓存命中）。
- 记录单元＝**一个二级标题** `## <日期> — <主题>`（其后 `-` 行为正文）；机检计数按二级标题数，不按 `-` 行数。
- 只滚「已终态、不再变更」的内容：INDEX 的 `done`/`archive` 行、INSPECT_LOG 的旧明细。

#### B. 卷化归档（复用 §3.4-B 的容器模式）

待下沉明细按**月**收成一个段文件，再压成卷：

```bash
D=/sdcard/Download/Operit/workspace
Y=$(date +%Y); Q=Q$(( ($(date +%-m)-1)/3+1 ))
mkdir -p "$D/archive/$Y-$Q"          # ← 必做
cd /tmp && mkdir -p seg && cd seg && rm -f ./*
# …此处写入 INSPECT_LOG-YYYY-MM.md（下沉的旧明细）＋ INDEX.md（段内索引＋复原命令）…
find . -maxdepth 1 -type f | sed 's|^\./||' | sort > /tmp/l
sha256sum $(cat /tmp/l) > MANIFEST.sha256
tar czf "$D/archive/$Y-$Q/INSPECT_LOG-YYYY-MM.tar.gz" -T /tmp/l MANIFEST.sha256
```

- 校验（**不过即回滚**）：`tar tzf` 条目数 = 段文件数 + 1；解到临时目录后 `sha256sum -c MANIFEST.sha256` 全 OK；逐文件 `cmp` 一致。
- **校验通过后才删**段原件（内容已在卷内，**不是**丢数据）。
- 复原（一条命令，写进交接单）：`cd /tmp/seg && tar xzf "$D/archive/YYYY-Qx/INSPECT_LOG-YYYY-MM.tar.gz" && rm -f *.tar.gz`
- 硬约束：卷内**必须**含 `MANIFEST.sha256`；**禁止「卷＋段原件」并存**；**只压已封闭的段**，绝不压正在追加的活跃文件（对齐 logrotate `delaycompress`）。

#### C. 快照＋段指针（CURRENT）

- 快照区＝当前态摘要（**覆盖式重写**，不是追加）→ 主文件体积与历史条数**解耦**（O(1)），"读一次即知现状"。
- 段清单＝卷的 CURRENT 指针；恢复＝读快照 ＋ 按需取段。
- **双向链接**：主文件历史区 → 段名；段内 `INDEX.md` → 复原命令与主文件路径（照 Keep a Changelog：移走的历史必须两边都能找到）。

#### 自检（POSIX，随 §4 一起跑）

```bash
D=/sdcard/Download/Operit/workspace
for f in "$D/INDEX.md" "$D/INSPECT_LOG.md"; do printf '%s bytes  %s\n' "$(wc -c < "$f")" "$f"; done
awk '/^## 当前区/{f=1;next} f&&/^## (快照|段清单|历史区)/{f=0} f&&/^## /{n++} END{print "当前区条目: "n+0" (上限 5)"}' "$D/INSPECT_LOG.md"
ls "$D"/archive/*/INSPECT_LOG-*.tar.gz 2>/dev/null || echo "（暂无段卷）"
```

判据：`INDEX` ≤8KB；`INSPECT_LOG` ≤32KB 且当前区 ≤5 条；**任一超限 → 执行一次滚动**。

## 4. 巡检与自愈（定期执行）

用户要求「检查工作区」或「整理文件」时，按以下顺序执行。

> **环境前提（写死）**：本节的命令只用 POSIX 语法（**不含 `<(...)>` 进程替换**），Android `/system/bin/sh`（mksh）与 Ubuntu 的 sh 都能直接跑。

**1) 结构扫描**（错位、根残留、空项目、缺 plan/logs）

```bash
D=/sdcard/Download/Operit/workspace
# 1a 根残留（含隐藏项）
for e in "$D"/* "$D"/.[!.]*; do
  [ -e "$e" ] || continue
  case "$(basename "$e")" in
    active|paused|done|archive|INDEX.md|INSPECT_LOG.md) ;;
    *) echo "根残留: $e" ;;
  esac
done
# 1b 结构异常（进了状态目录但不是完整项目）
for d in "$D"/active/*/ "$D"/paused/*/ "$D"/done/*/*/ "$D"/archive/*/*/; do
  [ -d "$d" ] || continue
  [ -d "$d/plan" ] && [ -d "$d/logs" ] || echo "结构异常: $d"
done
```

**2) 断层检查**（覆盖 active/paused/done/archive；冻结区容器模式用**卷内清单**代替文件名配对）

```bash
D=/sdcard/Download/Operit/workspace
for d in "$D"/active/*/ "$D"/paused/*/ "$D"/done/*/*/ "$D"/archive/*/*/; do
  [ -d "$d" ] || continue
  a=$(ls "$d/plan" 2>/dev/null | sort | tr '\n' ' ')
  if [ -f "$d/logs/steps.tar.gz" ]; then
    # 容器模式（§3.4-B）：用卷内清单比对
    b=$(tar tzf "$d/logs/steps.tar.gz" 2>/dev/null | grep -v 'MANIFEST.sha256' | sort | tr '\n' ' ')
    [ "$a" = "$b" ] || echo "断层(容器): $d plan=[$a] 卷=[$b]"
  else
    b=$(ls "$d/logs" 2>/dev/null | sort | tr '\n' ' ')
    [ "$a" = "$b" ] || echo "断层: $d plan=[$a] logs=[$b]"
  fi
done
echo ok
```

只有 `ok` ＝全配对；有 `断层:` 行 ＝该步缺失，**补文件或如实登记为既成事实，不补造、不删除**。
容器模式另需完整性抽检：`tar tzf 条目数 = 原文件数 + 1`，解包后 `sha256sum -c MANIFEST.sha256` 全 OK。

**3) 索引一致性机检**（与 INDEX 统计行逐项比对）

```bash
D=/sdcard/Download/Operit/workspace
echo "active=$(ls -d "$D"/active/*/ 2>/dev/null | wc -l)"
echo "paused=$(ls -d "$D"/paused/*/ 2>/dev/null | wc -l)"
echo "done=$(ls -d "$D"/done/*/*/ 2>/dev/null | wc -l)"
echo "archive=$(ls -d "$D"/archive/*/*/ 2>/dev/null | wc -l)"
```

（done/archive 多一层年月目录，故用 `*/*/`；写 `done/*/` 只会得到 1 个年月夹，是错的。）

**4) 同对象多项目检测**（查「重复建项目」；数据源＝`INDEX.md` 的 `对象` 列，按状态分区计数）

```bash
D=/sdcard/Download/Operit/workspace
awk -F'|' '
/^## /{sec=$0; sub(/^## /,"",sec); next}
/^\|/{
  n1=$2; o=$3; gsub(/^ +| +$/,"",n1); gsub(/^ +| +$/,"",o)
  if(n1=="" || n1 ~ /^-+$/ || n1=="项目名" || n1=="（暂无）") next
  key=sec"#"o; c[key]++; p[key]=p[key]" "n1
}
END{for(k in c){split(k,a,"#");
  printf "%s | %s | x%d | %s\n", a[1], (a[2]==""?"（无对象）":a[2]), c[k], p[k]
  if(a[2]!="" && c[k]>1 && a[1] ~ /active|进行中|paused|暂挂/) print " ⚠ 同对象多项目: "a[1]" / "a[2]" → 合并或标 重复→（§3.6 / §0.1-8）"}}' "$D/INDEX.md"
```

全 `x1` ＝无重复；出现 `⚠` ＝**同一状态内**该对象有多个项目 → 按 §3.6 续作 canonical、其余标 `重复→`（**禁止链式**，定义见 §0.1-8）。
`active` 与 `done` 同对象属正常历史（旧项目封存 + 新项目续作），**不算重复**，故按状态分区计数。
`（无对象）` 属漏填：INDEX 是状态唯一来源，**当场补列**。

**5) 更新**：INDEX.md（统计行 + 各分区行）＋ `INSPECT_LOG.md` 追加本轮记录。

**6) 报告**：只报真实发现与已执行的动作，给可复现证据（命令 + 输出）；**不主动删除任何项目**，除非用户明确授权。

## 5. 禁止项（违反＝失败）

- 在 workspace 根或四个状态目录下自创额外层级
- 项目名带日期、状态词、版本号、空格
- 把 plan/logs/交接单内容复制进 INDEX.md
- 只复制项目而不移动（导致双份）
- 删除 done 或 archive 中的任何文件（**唯一例外**：§3.4-B 容器合并，内容已无损入卷且 `sha256sum -c` 全 OK）
- 对未冻结项目（active/paused）做容器合并，或合并冻结项目的 `plan/` 侧
- 让「卷 + 原件」双份并存（同一真相两份）
- 在 tmp/ 以外的地方堆积临时文件
- 不更新 INDEX.md 就新建 / 迁移 / 归档
- 用对话记忆代替 INDEX.md 作为项目状态来源
- 不 `mkdir -p` 就 `mv` 到不存在的完成 / 归档目录
- 用「重开看一眼」代替 §4 的机检

## 6. 收尾与验证

每次执行本规范相关操作后：

1. 跑 §4 的 4 条机检命令（结构扫描 / 断层检查 / 索引一致性 / 同对象多项目）。
2. 核对 INDEX.md 统计行与机检数字一致（不一致即当场修正）。
3. 汇报固定行：

```text
当前：工作区管理 / 已更新索引与归档状态 / 等待下一步指令
```
（有异常时在其后附 `异常：…`）

4. 证据（命令输出、文件列表）落盘到相关项目 logs/ 或直接给出路径。

## 7. 判据冲突

- 冲突时以更严者为准；**发现判据本身有问题 → 停、报告、等授权**。
- **项目层的查重与合并**口径：同一真相只留一处、重复项指向唯一项；落到 §3.1（查重）／§3.6（续作）／§4-4（机检）三处操作。

## 附录 · 命令速查

```bash
D=/sdcard/Download/Operit/workspace

# 生命周期命令（新建 / 查重 / 续作 / 暂挂 / 完成 / 归档）见 §3.1–§3.6 正文，此处不复制以免两处漂移

# 四条机检（POSIX，Android shell 可直接跑）
for e in "$D"/* "$D"/.[!.]*; do [ -e "$e" ] || continue; case "$(basename "$e")" in active|paused|done|archive|INDEX.md|INSPECT_LOG.md) ;; *) echo "根残留: $e" ;; esac; done
for d in "$D"/active/*/ "$D"/paused/*/ "$D"/done/*/*/ "$D"/archive/*/*/; do [ -d "$d" ] || continue; [ -d "$d/plan" ] && [ -d "$d/logs" ] || echo "结构异常: $d"; done
for d in "$D"/active/*/ "$D"/paused/*/ "$D"/done/*/*/ "$D"/archive/*/*/; do [ -d "$d" ] || continue; a=$(ls "$d/plan" 2>/dev/null|sort|tr '\n' ' '); if [ -f "$d/logs/steps.tar.gz" ]; then b=$(tar tzf "$d/logs/steps.tar.gz" 2>/dev/null|grep -v 'MANIFEST.sha256'|sort|tr '\n' ' '); else b=$(ls "$d/logs" 2>/dev/null|sort|tr '\n' ' '); fi; [ "$a" = "$b" ] || echo "断层: $d plan=[$a] logs=[$b]"; done; echo ok
for s in active paused; do echo "$s=$(ls -d "$D/$s"/*/ 2>/dev/null|wc -l)"; done; for s in done archive; do echo "$s=$(ls -d "$D/$s"/*/*/ 2>/dev/null|wc -l)"; done
# 第 4 条：同对象多项目 → 命令同 §4-4（此处不复制实现，避免两处漂移）
```