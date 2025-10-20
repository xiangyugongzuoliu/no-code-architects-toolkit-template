# NCA Toolkit - Zeabur 部署指南

本文档说明如何使用此公开模板仓库部署 NCA Toolkit 到 Zeabur 平台。

## 📦 仓库结构

```
nca-toolkit-zeabur-template/
├── template.yaml        # Zeabur 模板配置文件
├── README.md           # 用户部署说明
├── docker-compose.yml  # Docker Compose 参考配置
├── .env.example        # 环境变量模板
└── DEPLOYMENT_GUIDE.md # 本文档
```

## 🔑 关键说明

### 1. 本仓库是公开的配置仓库

- **仅包含**：配置文件、文档、部署说明
- **不包含**：任何源代码、敏感信息
- **目的**：满足 Zeabur 要求的公开仓库条件

### 2. 实际应用来自 Docker 镜像

- **镜像地址**: `xiangyugongzuoliu/nca-toolkit:latest`
- **镜像源**：私有仓库通过 GitHub Actions 自动构建
- **更新方式**：推送新代码 → 自动构建 → 推送镜像 → Zeabur 自动拉取

### 3. 模板提交流程

#### 步骤 1: 安装 Zeabur CLI

```bash
# macOS (arm64)
wget https://github.com/zeabur/cli/releases/download/v0.5.4/zeabur_Darwin_arm64.tar.gz
tar -xzf zeabur_Darwin_arm64.tar.gz
mkdir -p ~/.local/bin
mv zeabur ~/.local/bin/
chmod +x ~/.local/bin/zeabur
```

#### 步骤 2: 登录 Zeabur

```bash
zeabur auth login
# 按提示在浏览器完成登录
```

#### 步骤 3: 提交模板

首次提交：
```bash
zeabur template create -f template.yaml
```

输出示例：
```
Template "nca-toolkit" (https://zeabur.com/templates/ABC123) created
```

记录模板 CODE（如 `ABC123`），用于更新模板和生成部署链接。

#### 步骤 4: 更新 README.md 中的部署按钮

将 README.md 中的 `TEMPLATE_CODE` 替换为实际的模板 CODE：

```markdown
[![Deploy on Zeabur](https://zeabur.com/button.svg)](https://zeabur.com/templates/ABC123?referralCode=xiangyugongzuoliu)
```

#### 步骤 5: 提交更改到 GitHub

```bash
git add README.md
git commit -m "更新 Zeabur 部署链接"
git push origin main
```

## 🔄 更新模板

当需要修改模板配置时：

```bash
# 1. 修改 template.yaml
vim template.yaml

# 2. 更新到 Zeabur
zeabur template update -c ABC123 -f template.yaml

# 3. 提交到 Git
git add template.yaml
git commit -m "更新模板配置"
git push origin main
```

## 🐳 更新 Docker 镜像

镜像更新通过私有仓库的 GitHub Actions 自动完成：

```bash
# 在私有仓库中
git add .
git commit -m "更新代码"
git push origin main

# GitHub Actions 会自动：
# 1. 构建多平台 Docker 镜像
# 2. 推送到 Docker Hub
# 3. Zeabur 会自动拉取最新镜像
```

## 🌐 部署架构

```
┌─────────────────────────┐
│  私有源码仓库            │
│  no-code-architects-    │
│  toolkit (Private)      │
└───────────┬─────────────┘
            │
            │ GitHub Actions
            │ 自动构建
            ↓
┌─────────────────────────┐
│  Docker Hub (Public)    │
│  xiangyugongzuoliu/     │
│  nca-toolkit:latest     │
└───────────┬─────────────┘
            │
            │ 镜像引用
            ↓
┌─────────────────────────┐
│  公开模板仓库 (Public)   │
│  nca-toolkit-zeabur-    │
│  template               │
│  └─ template.yaml       │
└───────────┬─────────────┘
            │
            │ Zeabur CLI
            │ 提交模板
            ↓
┌─────────────────────────┐
│  Zeabur 平台            │
│  用户一键部署            │
└─────────────────────────┘
```

## ✅ 验证清单

部署前检查：

- [ ] template.yaml 中 `expose: true` 已添加到需要用户配置的变量
- [ ] README.md 中部署按钮包含 `?referralCode=xiangyugongzuoliu`
- [ ] Docker 镜像 `xiangyugongzuoliu/nca-toolkit:latest` 已发布且支持多平台
- [ ] template.yaml 使用 `PREBUILT` 模板类型
- [ ] 健康检查路径 `/v1/toolkit/test` 在应用中已实现
- [ ] 环境变量默认值合理（在 env 部分）

## 🆘 常见问题

### Q: 为什么需要公开仓库？
A: Zeabur 要求模板引用的 Git 仓库必须公开，但我们通过公开配置仓库 + 私有源码的方式保护了代码。

### Q: 如何更新已部署的服务？
A: 更新私有仓库代码 → GitHub Actions 自动构建新镜像 → Zeabur 自动拉取 → 用户手动重启服务。

### Q: 模板更新后现有部署会受影响吗？
A: 不会。模板更新只影响新的部署，已部署的服务需要手动重新部署才能应用新配置。

### Q: 如何删除模板？
A: `zeabur template delete -c <template-code>`（不影响已部署的服务）

## 📚 相关文档

- [Zeabur 官方文档](https://zeabur.com/docs)
- [Zeabur CLI 文档](https://zeabur.com/docs/deploy/cli)
- [项目主仓库](https://github.com/xiangyugongzuoliu/no-code-architects-toolkit)

---

**维护者**: 翔宇工作流
**最后更新**: 2025-10-20
