# agent-log

## 2026.09.15 15:23

对LumosLaTeX生态做了完整调研，明确维护对象与现状。未做代码改动，仅记录基线。

### 维护对象

- 元仓库`LumosLaTeX`：`README.md`仓库索引、`Plan.md`文档规范、`AGENTS.md`与`agent-*.md`。
- `Class`（7个）：`ArticleArgon`、`BeamerBismuth`、`CheatsheetCarbon`、`IEEETran`、`NotebookNeon`、`SI200MiniReview`、`StandaloneSilicon`。
- `Package`（2个）：`Minimus`（9个`.sty`+`.md`，另含内部`semi-split.sty`）、`PyJool`。
- `Makefile`（1个）：`MakefileLaTeX`（`makefile-latex.mk`、`latex-std-dependence.mk`）。
- `Template`（6个）：`ArticleArgonTemplate`、`BeamerBismuthTemplate`、`CheatsheetCarbonTemplate`、`IEEETranTemplate`、`NotebookNeonTemplate`、`SI200MiniReviewTemplate`。

### 关键机制

- 所有仓库均有`origin`（Lithium）与`github`（GitHub）两个源，在`dev`开发，发布到`master`并打tag，同时推送两个源。
- `Template`三支模型：`dev`带动测试内容且子模块源指向Lithium的`dev`；`master`干净且子模块指向Lithium的`master`；`github`基于`master`但`.gitmodules`改为GitHub源且`branch=master`。
- `Template`通过`init.sh`一次性完成改名、子模块初始化、移除remote、转到`dev`。

### 发现的问题

高优先级

- `CheatsheetCarbonTemplate`的`github`分支未做GitHub转换，`.gitmodules`仍为`git@Lithium:...`，与`master`无差异，GitHub用户无法初始化子模块。
- `MakefileLaTeX`的`dev`分支有未推送提交`a4a99c1`（新增`CLSS_CHEATSHEET_CARBON`），且README的变量表未收录该变量，`github/master`也落后。
- `CheatsheetCarbonTemplate`的`Makefile`引用`${CLSS_CHEATSHEET_CARBON}`，但固定`makefile-latex`于`v2.1`未定义该变量，导致类文件依赖追踪失效。
- 5个模板固定`Minimus`于`v2.0`，落后于最新`v2.1.2`（新增`longdivision`、修复`Table`的`&`），仅`CheatsheetCarbonTemplate`为`v2.1.2`，版本不一致。

中优先级

- `Plan.md`未记录各README统一使用的"LumosLaTeX计划的一部分"链接规范。
- `IEEETran`的README缺`## 主要特点`小节（`Plan.md`规定的Class结构）。
- `PyJool`的README有笔误"PyJool了提供了"，且缺引入与用法说明（`Plan.md`无Python包规范）。
- 5个模板的`.gitignore`缺行末换行。
- `MakefileLaTeX`的README使用`### 编译器修改`等中文描述式三级标题，与`Plan.md`"三级标题仅包含名称"存在张力，需确认适用范围。

低优先级（清理）

- 大量`feature/*`、`fix/*`分支已合入`dev`（`Minimus`约40个、`MakefileLaTeX`约9个）；`Minimus`的`feature/longdivision`本地领先`origin`1个已合并提交。
- Class仓库（除`CheatsheetCarbon`）遗留`github`分支，内容为旧版README，现模板改用`branch=master`后已不需要；`MakefileLaTeX`的`github`分支停留在`v1.5.2`。
- 未设置upstream的分支：`Class/CheatsheetCarbon`的`dev`/`master`、`Class/SI200MiniReview`的`master`、`Template/CheatsheetCarbonTemplate`的`github`/`master`。
- 实验分支：`test-en-us`（3个模板）、`ieee`、`fix/use-new-document-command`。
- 根仓库`dev`领先`master`3个提交，`github/dev`落后`dev`4个提交。

### 验证

- 各仓库工作树干净，所有`feature/*`均已合入`dev`，各仓库`master`与最新tag一致（`MakefileLaTeX`因未推送提交除外）。
- 文档与代码交叉核对：Class文档选项与`.cls`一致；Minimus命令/环境文档覆盖完整（缺项均为带`@`的内部宏或计数器重定义）。
- 实测`ArticleArgonTemplate`（`dev`，`Minimus v2.0`）可正常`make -j`编译。

## 2026.09.15 15:35

按`agent-request.md`推进第一轮修复与发布。

### MakefileLaTeX

- README变量表补充`CLSS_CHEATSHEET_CARBON`，提交`e585c34`。
- 合并`dev`至`master`，打tag`v2.2`，推送`dev`、`master`、`v2.2`至`origin`与`github`。

### CheatsheetCarbonTemplate

- `dev`更新`makefile-latex`子模块至`v2.2`（`f21d39e`），`master`合并`dev`（`4345d7f`）。
- 修复`github`分支：合并`master`并将`.gitmodules`全部改为GitHub源（`c0df758`），此前该分支仍指向Lithium，GitHub用户无法初始化子模块。
- 打tag`v1.2`，推送`dev`、`master`、`github`、`v1.2`至`origin`与`github`。

### PyJool

- 修复README笔误“PyJool了提供了”为“PyJool提供了”（`ac33793`），同步`dev`、`master`、`github`。
- 打tag`v1.3`，推送两源；按指示不更新下游模板的`pyjool`子模块。

### 上游跟踪

- 为`Class/CheatsheetCarbon`的`dev`/`master`、`Class/SI200MiniReview`的`master`、`Template/CheatsheetCarbonTemplate`的`github`/`master`设置upstream。

### 未处理

- `Minimus`版本漂移（5个模板固定`v2.0`，最新`v2.1.2`）本轮未动，待确认。
- `PyJool`的“引入与用法说明”因无现成示例与规范，本轮未补。

## 2026.09.15 15:45

按指示清理残留分支（#10）。未清理任何`feature/*`、`fix/*`（#9），保持原样。

### 删除遗留`github`分支

根据说明，`Class`/`Package`/`Makefile`仓库的`github`分支已废弃（子模块统一跟踪`master`），`Template`仓库的`github`分支保留。

| 仓库 | 删除位置 | 原指向 |
|------|----------|--------|
| `Class/ArticleArgon` | github | `7f6df57` |
| `Class/BeamerBismuth` | github | `6c38d5b` |
| `Class/IEEETran` | github | `7a82e5b` |
| `Class/NotebookNeon` | github | `f331199` |
| `Class/SI200MiniReview` | github | `f0f289c` |
| `Class/StandaloneSilicon` | github | `5192022` |
| `Package/Minimus` | origin+github | `27fdcf4` |
| `Package/PyJool` | origin+github | `ac33793` |
| `Makefile/MakefileLaTeX` | github | `5e996b2` |

### 删除实验分支

| 仓库 | 分支 | 原指向 | 备注 |
|------|------|--------|------|
| `Class/NotebookNeon` | `fix/use-new-document-command` | `8aa7ca5` | 已合入`dev`/`master` |
| `Template/ArticleArgonTemplate` | `test-en-us` | `1fb5a02` | 已合入 |
| `Template/BeamerBismuthTemplate` | `test-en-us` | `004ee1f` | 未合入，仅测试提交 |
| `Template/NotebookNeonTemplate` | `test-en-us` | `e80fc97` | 未合入，仅测试提交 |
| `Template/IEEETranTemplate` | `ieee` | `44aa6c6` | 已合入 |

所有`github`分支的提交均可在`dev`/`master`中回溯，删除无内容损失；两个未合入的`test-en-us`仅为英语环境测试提交，SHA已存档于上表。
