# Knowledge Base — Twitter 账号分析知识库

> 本目录存储历史分析记录和可复用模式。

## 目录结构

```
knowledge-base/
├── README.md              ← 你在这里
├── accounts/              # 按账号存储分析记录
│   └── HANDLE/            # 每个被分析过的账号一个目录
│       ├── profile.json   # 基础画像快照
│       └── latest.md      # 最近一次分析报告
├── patterns/              # 可复用的增长模式
│   ├── content_formulas.md     # 高互动内容公式库
│   ├── growth_tactics.md       # 涨粉策略库
│   └── engagement_hooks.md     # 互动钩子库
└── reviews/              # 按日期存档的复盘记录
    └── YYYY-MM-DD_HANDLE.md
```

## 使用规则

1. **分析前**：先检索 `accounts/HANDLE/` 是否有历史记录
2. **分析后**：将报告存入 `accounts/HANDLE/latest.md`
3. **模式沉淀**：每次分析后，将可复用模式提取到 `patterns/`
4. **复盘存档**：完整报告按日期存入 `reviews/YYYY-MM-DD_HANDLE.md`

## 文件命名规范

- 账号目录名：使用小写 handle（如 `naval`、`elonmusk`）
- 复盘文件：`YYYY-MM-DD_handle.md`（如 `2026-04-22_naval.md`）
- 模式文件：使用英文蛇形命名（如 `content_formulas.md`）
