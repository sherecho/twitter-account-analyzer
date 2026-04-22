# Twitter CLI 自动安装与认证引导

> 本文档是 SKILL.md 第 0 节的详细参考，包含完整的安装、认证、代理配置流程。

## 1. 安装流程

### 1.1) 前置依赖检查

```bash
# 检查 Python3
python3 --version  # 需要 3.9+

# 检查 pipx
command -v pipx || echo "pipx_not_found"
```

### 1.2) 安装 pipx（如果未安装）

```bash
# macOS（推荐）
brew install pipx && pipx ensurepath

# Linux
python3 -m pip install --user pipx
python3 -m pipx ensurepath

# 通用
source ~/.zshrc 2>/dev/null || source ~/.bashrc 2>/dev/null
```

### 1.3) 安装 twitter-cli

```bash
pipx install twitter-cli
```

### 1.4) 验证安装

```bash
twitter --version  # 应输出 v0.8.5 或更高
```

### 1.5) 升级（如果已安装旧版本）

```bash
pipx upgrade twitter-cli
```

## 2. 认证流程

### 2.1) 认证方式对比

| 方式 | 稳定性 | 操作复杂度 | 适用场景 |
|------|--------|-----------|---------|
| Cookie 环境变量 | ⭐⭐⭐⭐⭐ | 中等 | 推荐，最稳定 |
| 浏览器自动提取 | ⭐⭐⭐ | 低 | 需要有浏览器环境 |
| API Key | ⭐⭐ | 高 | 开发者账号 |

### 2.2) Cookie 环境变量配置（推荐）

#### 获取 Cookie 步骤

1. 在浏览器中登录 https://x.com
2. 安装 Cookie-Editor 扩展（[Chrome](https://chrome.google.com/webstore/detail/cookie-editor/hlkenndednhfkekhgcdicdfddnkalmdm) / [Firefox](https://addons.mozilla.org/en-US/firefox/addon/cookie-editor/)）
3. 在 x.com 页面打开 Cookie-Editor
4. 搜索并复制 `auth_token` 的值
5. 搜索并复制 `ct0` 的值

#### 配置环境变量

```bash
# 临时生效（当前 session）
export TWITTER_AUTH_TOKEN="你的auth_token"
export TWITTER_CT0="你的ct0"

# 永久生效（写入 shell 配置）
cat >> ~/.zshrc << 'EOF'
export TWITTER_AUTH_TOKEN="你的auth_token"
export TWITTER_CT0="你的ct0"
EOF

# 如果使用 bash
# cat >> ~/.bashrc << 'EOF'
# export TWITTER_AUTH_TOKEN="你的auth_token"
# export TWITTER_CT0="你的ct0"
# EOF
```

#### 验证认证

```bash
twitter status
# 应输出类似：Authenticated as @username

twitter whoami
# 应输出当前登录用户信息
```

### 2.3) Cookie 过期处理

Twitter Cookie 的 `auth_token` 通常有效期较长（数月），但 `ct0` 可能会过期。

**症状**：
```bash
twitter status
# 返回错误或 "not authenticated"
```

**解决方案**：
重新按 2.2 步骤获取最新的 `ct0` 值（通常只需要更新 ct0，auth_token 不变）。

## 3. 网络代理配置

### 3.1) 判断是否需要代理

```bash
# 测试直连
twitter status --verbose 2>&1 | head -5

# 如果超时或连接失败，需要配置代理
```

### 3.2) 常见代理软件端口

| 代理软件 | 默认端口 |
|---------|---------|
| Clash | 7890 |
| Clash Verge | 7897 |
| V2rayU | 1087 |
| ShadowsocksX | 1080 |
| Surge | 6152 |

### 3.3) 配置代理

```bash
# 临时生效
export HTTP_PROXY="http://127.0.0.1:7897"
export HTTPS_PROXY="http://127.0.0.1:7897"

# 永久生效
cat >> ~/.zshrc << 'EOF'
export HTTP_PROXY="http://127.0.0.1:7897"
export HTTPS_PROXY="http://127.0.0.1:7897"
EOF
```

### 3.4) 验证代理

```bash
twitter status
# 如果通过代理能正常返回认证信息，说明代理配置成功
```

## 4. 完整自检脚本

```bash
#!/bin/bash
# Twitter CLI 完整自检

echo "=== Twitter CLI 环境自检 ==="

# 1. 检查命令
if command -v twitter &>/dev/null; then
    echo "✅ twitter-cli 已安装: $(twitter --version)"
else
    echo "❌ twitter-cli 未安装，尝试安装..."
    pipx install twitter-cli
fi

# 2. 检查认证
if twitter status 2>/dev/null | grep -q "authenticated\|Authenticated"; then
    echo "✅ Twitter 认证正常"
else
    echo "❌ Twitter 未认证"
    echo "请配置环境变量："
    echo "  export TWITTER_AUTH_TOKEN=\"你的auth_token\""
    echo "  export TWITTER_CT0=\"你的ct0\""
fi

# 3. 检查代理
if [[ -n "$HTTP_PROXY" ]]; then
    echo "✅ 代理已配置: $HTTP_PROXY"
else
    echo "⚠️ 未配置代理（如果在墙内可能需要）"
fi

echo "=== 自检完成 ==="
```
