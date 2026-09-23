# 简历仓库

朱宇杰的简历源码。LaTeX 编写，基于 Awesome-CV 模板，XeLaTeX 编译，中文字体 Noto Sans CJK SC。

---

## 主要简历

**`general-full` 分支是主要简历，也是本仓库事实内容的唯一权威来源。**

- **事实内容**（经历、项目、技能、获奖、数字）以 `general-full` 为准，其他分支不得出现 `general-full` 里没有的事实
- 其他分支都是 `general-full` 的**按岗位裁剪版**：调整取舍与措辞，不新增事实
- 若某个分支出现了 `general-full` 里没有的事实内容，必须回填到 `general-full`

### 项目经历写法（主简历标准）

`general-full` 的项目经历统一采用**「项目内容 / 项目职责」两段式**，面向 HR 阅读：

- **项目内容**：项目是什么、范围与背景
- **项目职责**：自己做了什么、有哪些可核验的产出

其他分支沿用各自原有结构；若要把某分支的结构改动带到主简历，按上方「同步规则」第 2 步处理。

### 已知的结构性差异（非事实差异，属有意为之）

以下差异只在特定分支存在，**不要求**回填到 `general-full`：

| 分支 | 差异 |
| --- | --- |
| `desktop-support-en` | 额外提供英文版（`resume-en.tex` + `sections/*-en.tex`） |
| `devops-v2`、`mihoyo-8663` | 含「游戏经历」章节（`sections/gaming.tex`），仅在投递游戏行业时启用 |

> 「游戏经历」为 `devops-v2`、`mihoyo-8663` 的岗位定向内容，仅在投递游戏行业时启用，不回填 `general-full`。

### 同步规则

**此后所有分支的改动都必须同步进通用简历（`general-full`）。**

流程：

1. 在目标分支上做内容修改
2. 把同一处**内容**修改同步应用到 `general-full`
3. 两个分支各自提交、推送

**不允许只改专项分支而不回填通用简历。** 通用简历是主简历，事实内容必须始终最全、最新。

---

## 分支说明

| 分支 | 定位（取自 `\position`） | 用途 |
| --- | --- | --- |
| `general-full` | 开源软件工程师 / Linux 系统工程师 | **主简历**，内容最全，其余分支的基准 |
| `master` | 开源软件工程师 · Linux 内核开发 | 内核方向早期版本 |
| `it-support` | IT 技术支持 / 网络运维工程师 | 网络与终端支持岗 |
| `devops-cicd` | DevOps 工程师 / SRE · 云原生基础设施 | DevOps / CI-CD 方向 |
| `devops-v2` | 云原生运维 / DevOps（求职方向） | DevOps 方向第二版 |
| `k8s-ops` | 云原生运维开发工程师 · Linux 系统专家 | Kubernetes / 云原生运维 |
| `desktop-support` | 桌面运维 / IT 技术支持工程师 | 桌面运维岗（中文） |
| `desktop-support-en` | Desktop Support / IT Support Engineer | 桌面运维岗（英文），见下方说明 |
| `mihoyo-8663` | Linux 系统构建 / 基础设施自动化工程师 | 米哈游社招投递 |
| `full-projects` | 开源软件工程师 / Linux 系统工程师 | 两段式项目结构的试验分支，**该结构已并入 `general-full`** |

### desktop-support-en 的特殊说明

该分支有**两份主文件**，不是只有 `resume.tex`：

- `resume.tex` —— 中文简历（`sections/*.tex`）
- `resume-en.tex` —— 英文简历（`sections/*-en.tex`）

编译英文简历要指定 `resume-en.tex`，否则产出的是中文版。

---

## 构建

```bash
xelatex -interaction=nonstopmode resume.tex
xelatex -interaction=nonstopmode resume.tex   # 跑第二遍，收敛页码与引用
```

产物为 `resume.pdf`。英文简历把 `resume.tex` 换成 `resume-en.tex`。

---

## 注意事项

### 中文断行（所有分支已修复）

`awesome-cv.cls` **没有加载 `xeCJK`**，中文是当作主字体设置的：

```latex
\setmainfont{Noto Sans CJK SC}[ UprightFont=*, BoldFont=* Bold, ]
```

没有 `xeCJK` / `ctex`，XeTeX 就不会设置 `\XeTeXlinebreaklocale`，**中文之间没有断行点，只能靠空格断行**。结果是长中文句子放不下时直接冲出右边界。

这个问题此前一直潜伏，因为旧条目都够短（塞得进一行）或带够空格。实测最严重的一行**超出右边界约 119pt（≈4.2cm）**；`k8s-ops` 分支合并前有 4 处大溢出。

修复方式是在 `resume.tex` 加入：

```latex
\XeTeXlinebreaklocale "zh"
\XeTeXlinebreakskip = 0pt plus 1pt minus 0.1pt
```

**新增分支或从模板重建时，务必带上这两行。**

### 字体依赖

需要 `Noto Sans CJK SC`（含 Bold）与 HarfBuzz renderer；缺字体则编译失败。

### 排版检查

编译日志里出现 `Overfull \hbox` 时看数值：

- 超过约 10pt —— 真溢出，必须处理（通常是上面那条断行配置缺失）
- 1～2pt —— 正常排版噪声，可忽略

同时确认页数符合预期（当前各版本均为 2 页 A4）。
