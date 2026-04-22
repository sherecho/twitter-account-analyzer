# Twitter Account Analyzer Skill 🐦📊

> **一句话定位**：给我一个 @handle，还你一份「为什么他能涨粉 + 你怎么复制」的全链路分析报告。

一个面向 AI Agent（OpenClaw / Claude Code / Codex）的 **Twitter/X 账号逆向运营分析技能**。输入任意 Twitter 用户名，自动采集推文数据、分析内容性质与涨粉规律、归因增长驱动力、输出可复制的增长经验报告。

**内含 Twitter CLI 自动安装与认证引导**，用户安装本 Skill 后无需额外安装任何工具。

---

## ✨ 核心能力

| 能力 | 说明 |
|------|------|
| 🔍 **基础画像采集** | 自动获取目标账号的粉丝数、简介、注册时间、发帖频率等 |
| 📝 **内容性质分类** | AI 驱动的 12 种内容类型 + 6 种情绪基调 + 6 种互动钩子分类 |
| 📈 **涨粉规律分析** | 发帖时间分布、频率变化、内容策略演变时间线 |
| 🎯 **涨粉归因** | 四因素归因模型（内容 / 行为 / 关系 / 策略），带权重和证据 |
| 📋 **可复制经验提取** | 内容公式、发帖节奏、互动策略、话题选择、视觉风格 |
| 📅 **30 天复制计划** | 按周拆解的可执行增长计划 |
| 🆚 **竞品对比分析** | 多账号横向对比 |
| 🔁 **长期追踪** | 定期重复分析，输出变化趋势 |

---

## 🔧 多数据源融合

不是只依赖单一 API，而是融合多个数据源，最大化分析深度：

| 数据源 | 工具 | 获取什么 | 必须？ |
|--------|------|----------|--------|
| 推文内容 + 互动数 | `twitter-cli` | 所有推文及 likes/retweets/replies | ✅ 必须 |
| 用户基础画像 | `twitter-cli` | 粉丝数、简介、注册时间等 | ✅ 必须 |
| 历史粉丝增长 | `fch` (Wayback Machine) | 历史各时间点的粉丝数快照 | 💡 强烈推荐 |
| 全网传播数据 | `Exa` 搜索 | 被广泛转发/讨论的帖文 | 💡 推荐 |
| 第三方分析 | `Jina Reader` / `web_fetch` | SocialBlade/SuperX 公开数据 | 💡 推荐 |
| 竞品数据 | `twitter-cli` × N 个账号 | 多账号对比 | 按需 |

### 降级容错

即使只有 `twitter-cli` 可用，也能输出完整分析报告（粉丝增长曲线用推算法代替）。数据源越多，分析越精准。

---

## 📦 安装方式

### 方式一：OpenClaw 安装（推荐）

```bash
# 1. 克隆仓库到 Skills 目录
git clone https://github.com/YOUR_USERNAME/twitter-account-analyzer.git ~/.openclaw/skills/twitter-account-analyzer

# 2. 重启 OpenClaw 或在对话中直接使用
openclaw tui
```

安装完成后，在对话中说"帮我分析 @elonmusk 的涨粉策略"即可自动触发。

### 方式二：Claude Code 安装

```bash
# 1. 克隆仓库到 Claude Code 的 Skills 目录
git clone https://github.com/YOUR_USERNAME/twitter-account-analyzer.git ~/.claude/skills/twitter-account-analyzer

# 2. 在 Claude Code 中直接使用
claude
# > 帮我分析一下 @naval 的运营策略
```

### 方式三：Codex 安装

```bash
# 1. 克隆仓库到 Codex 的 Skills 目录
git clone https://github.com/YOUR_USERNAME/twitter-account-analyzer.git ~/.codex/skills/twitter-account-analyzer

# 2. 在 Codex 中直接使用
codex
# > 分析 @jimmy_jinglv 的涨粉规律
```

### 方式四：手动安装（适用于所有平台）

```bash
# 1. 克隆仓库到任意位置
git clone https://github.com/YOUR_USERNAME/twitter-account-analyzer.git

# 2. 创建符号链接到你的 Agent 的 Skills 目录
# OpenClaw:
ln -s $(pwd)/twitter-account-analyzer ~/.openclaw/skills/twitter-account-analyzer

# Claude Code:
ln -s $(pwd)/twitter-account-analyzer ~/.claude/skills/twitter-account-analyzer

# Codex:
ln -s $(pwd)/twitter-account-analyzer ~/.codex/skills/twitter-account-analyzer
```

---

## 🚀 首次使用引导

安装 Skill 后，**第一次使用时会自动引导你完成以下配置**：

### Step 1：自动安装依赖工具

Skill 会自动检测并安装以下工具：

```bash
# twitter-cli（核心推文采集）
pipx install twitter-cli

# fch（历史粉丝数追踪）
pipx install fch

# mcporter（Exa 搜索，可选）
npm i -g mcporter
```

### Step 2：Twitter 认证配置

Skill 会自动检测认证状态，如果未认证，会输出图文引导：

```
📋 Twitter 认证配置引导（约 2 分钟完成）

步骤 1：在浏览器中登录 https://x.com
步骤 2：安装 Cookie-Editor 浏览器扩展
步骤 3：在 x.com 页面上打开 Cookie-Editor
步骤 4：找到并复制以下两个 Cookie 值：
  • auth_token
  • ct0
步骤 5：将这两个值告诉我
```

### Step 3：网络代理配置（仅中国大陆用户）

```
⚠️ 如果你在中国大陆，Twitter API 需要代理访问。
请确认你的代理端口，我会帮你自动配置。
```

### Step 4：开始分析！

全部配置完成后，直接说：

> "帮我分析 @username 的涨粉策略"

---

## 📁 项目结构

```
twitter-account-analyzer/
├── SKILL.md                                    # 主技能文件（Agent 加载入口）
├── README.md                                   # 本文件
├── references/
│   ├── twitter-cli-setup.md                    # Twitter CLI 安装与认证引导
│   ├── content-classification.md               # 内容分类标准（12类型+6情绪+6钩子）
│   ├── growth-attribution.md                   # 涨粉归因分析方法论
│   └── experience-extraction.md                # 可复制经验提取方法论
├── examples/
│   └── sample-report.md                        # 完整分析报告示例
└── knowledge-base/
    ├── README.md                               # 知识库入口
    ├── accounts/                               # 账号分析记录（自动生成）
    ├── patterns/
    │   └── content_formulas.md                 # 高互动内容公式库
    └── reviews/                                # 复盘存档（自动生成）
```

---

## 📊 分析报告示例

分析完成后，Skill 会输出一份结构化报告：

```
# 🔍 Twitter 账号逆向分析报告

## 基本信息
- 账号：@Jimmy_JingLv
- 粉丝数：19,725
- 运营时长：11.5 年

## 内容性质分析
  产品展示/开发日志  40%  ← 绝对主力
  AI 工具体验分享    20%
  个人生活/带娃日常  13%
  ...

## 涨粉归因
  1. 🛠️ 独立开发产品展示  40% → "做出来就发"
  2. 📋 方法论沉淀型长文   25% → 收藏率极高
  3. 🔥 真实情绪表达       20% → "卧槽！" 拉近距离
  4. 👶 带娃开发者反差人设  15% → 精准打中目标群体

## 可复制经验
  🟢 直接复制：做出来就发、引用转发做互动、真实情绪表达
  🟡 需要适配：独立开发产品展示、方法论长文、反差人设

## 30 天复制计划
  第 1 周：基础建设 → 第 2 周：找到节奏 → ...
```

> 完整示例见 [examples/sample-report.md](examples/sample-report.md)

---

## 🧠 分析方法论

### 内容分类体系

| 维度 | 标签数 | 示例 |
|------|--------|------|
| 内容类型 | 12 种 | 技术干货、行业观点、个人故事、教程指南… |
| 内容形式 | 7 种 | 纯文字、图片、视频、Thread、引用转发… |
| 情绪基调 | 6 种 | 积极鼓励、中性客观、争议挑战、共情共鸣… |
| 互动钩子 | 6 种 | 提问式、观点站队、资源诱惑、悬念设计… |

### 涨粉归因模型

```
涨粉驱动力 = f(内容因素 40%, 行为因素 25%, 关系因素 20%, 策略因素 15%)
```

- **内容因素**：什么类型的内容最涨粉？Thread vs 短推效率对比
- **行为因素**：发帖频率、时间窗口、互动行为
- **关系因素**：被大V转发、社交网络效应
- **策略因素**：固定栏目、简介优化、系列活动

详见 [references/growth-attribution.md](references/growth-attribution.md)

---

## ⚙️ 兼容性

| Agent 平台 | 兼容方式 | 状态 |
|-----------|---------|------|
| **OpenClaw** | 放入 `~/.openclaw/skills/` | ✅ 完全兼容 |
| **Claude Code** | 放入 `~/.claude/skills/` | ✅ 完全兼容 |
| **Codex** | 放入 `~/.codex/skills/` | ✅ 完全兼容 |
| **其他 SKILL.md 兼容 Agent** | 放入对应 skills 目录 | ✅ 理论兼容 |

---

## 🛠️ 技术依赖

| 依赖 | 安装方式 | 必须？ | 说明 |
|------|---------|--------|------|
| Python 3.9+ | 系统自带 | ✅ | twitter-cli 和 fch 的运行环境 |
| pipx | `brew install pipx` | ✅ | Python CLI 工具包管理器 |
| twitter-cli | `pipx install twitter-cli` | ✅ | 核心推文采集工具 |
| fch | `pipx install fch` | 💡 推荐 | 历史粉丝增长曲线 |
| mcporter | `npm i -g mcporter` | 💡 可选 | Exa 全网搜索 |
| curl | 系统自带 | ✅ | 调用 Jina Reader 等服务 |

---

## 📝 使用示例

```
用户: 帮我分析 @naval 的涨粉策略
AI: [自动采集数据 → AI分析 → 输出完整报告]

用户: 对比一下 @elonmusk 和 @naval 的运营策略差异
AI: [双账号采集 → 对比分析 → 差异报告]

用户: 分析一下 @Jimmy_JingLv 最近一个月的内容策略
AI: [指定时间范围采集 → 内容分类 → 策略分析]

用户: 我该怎么复制 @xxx 的涨粉经验？
AI: [采集分析 → 经验提取 → 30天复制计划]
```

---

## 📜 License

MIT License

---

## 🤝 贡献

欢迎提交 Issue 和 PR！

- 发现分析不准确？提交 Issue 告诉我们
- 有新的分析维度想法？提交 PR
- 发现新的数据源？提交 PR

---

**Made with ❤️ for AI Agent Skill Ecosystem**
