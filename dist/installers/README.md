# ShipFast

> **写代码是 IDEA 的事，上线是 ShipFast 的事。**  
> **IDEA writes the code. ShipFast ships it.**

**文档说明 · About this document**  
本文档为**中英双语合订**（先中文、后 English），适用于 **Gitee / GitHub** 发布的离线安装包目录。安装步骤见同目录 `QUICKSTART.md`。

This is a **bilingual** README (Chinese first, then English) for offline installer packages on **Gitee / GitHub**. See `QUICKSTART.md` in the same folder for setup steps.

---

# 中文

## ShipFast 是做什么的？

ShipFast 是面向开发者与团队的**部署平台**：在本机用一条命令把项目部署到远程服务器；可选接入 **ShipFast Cloud** 管理账号、License、团队、策略与计费。

| 场景 | 能力 |
|------|------|
| 个人开发者 | `shipfast init` → `shipfast deploy`，自动识别 Node / Python / Go 等 |
| 小团队 | 本地 **Web 控制台**（仪表板、部署向导、运维、密钥） |
| 企业 / 服务商 | Cloud 运营后台：用户、Policy-as-Code、定价、License、审计 |

## 系统架构

```
本机 shipfast CLI (+ Web GUI)  ──HTTPS──►  远程 shipfast-agent (Docker)
         │
         └── 可选 ──►  ShipFast Cloud（账号 / License / 策略 / 计费）
```

| 组件 | 安装位置 | 作用 |
|------|----------|------|
| **shipfast CLI** | Windows / Linux / macOS | 部署指令、服务器管理、**内置 Web GUI** |
| **shipfast-agent** | Linux VPS | 构建镜像、运行容器、Nginx、HTTPS |
| **shipfast-cloud** | 自托管或 SaaS（**不在 CLI 包内**） | 账号、License、运营后台 `/admin` |

## 本目录安装包

| 文件 | 说明 |
|------|------|
| `ShipFast-<版本>-windows-amd64.zip` / `.msi` | Windows CLI（含 `shipfast-mcp`） |
| `ShipFast-<版本>-linux-*.tar.gz` | Linux CLI |
| `ShipFast-<版本>-darwin-*.tar.gz` | macOS CLI |
| `ShipFast-Agent-<版本>-linux-*.tar.gz` | Linux Agent |
| `SHA256SUMS` | 校验清单 |
| `README.md` / `QUICKSTART.md` | 中英双语说明 |

## 当前版本主要功能（v0.20+）

### CLI

- 部署、回滚、日志、多环境、GitOps、密钥、流水线、Terraform 导出
- `shipfast web` — 本地 Web 控制台（默认 `http://127.0.0.1:19900`）
- `shipfast upgrade` — 检查新版本（CDN + Gitee + GitHub）
- `shipfast doctor` — 检查 CDN / Cloud 连通性

### Web GUI（内嵌于 CLI）

- **中英界面切换**：顶栏右侧语言按钮（English / 中文）
- **帮助中心**：GUI 使用文档 + 完整 CLI 命令参考（双语）
- **新版本提示**：有更新时顶栏下方显示横幅；**设置 → License** 可检查 / 一键升级
- 仪表板、部署向导、Cloud、运维、密钥、模板市场等

### 版本更新来源（自动探测）

客户端会同时检查以下来源，取**最高版本号**：

| 来源 | 默认地址 |
|------|----------|
| CDN | `https://releases.shipfast.app/latest/version.txt` |
| Gitee | `wang-sheng-ordersystem/shipfast`（tag，如 `v0.20.0`） |
| GitHub | `wangshengbear/ShipFast`（tag） |

安装包直链示例（macOS ARM）：

- Gitee：`.../raw/master/dist/installers/ShipFast-0.20.0-darwin-arm64.tar.gz`
- GitHub：`.../raw/main/dist/installers/ShipFast-0.20.0-darwin-arm64.tar.gz`

自定义源（可选）：

```bash
export SHIPFAST_DOWNLOAD_BASE=https://your-cdn.example
export SHIPFAST_GITEE_REPO=owner/repo
export SHIPFAST_GITHUB_REPO=owner/repo
export SHIPFAST_UPDATE_SOURCES=cdn,gitee,github
```

## 5 分钟流程

```
1. 安装 CLI        →  shipfast version
2. 打开 Web GUI    →  shipfast web
3. 登录 Cloud      →  shipfast cloud login（可选）
4. VPS 装 Agent    →  解压 Agent 包，运行 install.sh
5. 登记服务器      →  shipfast server add
6. 首次部署        →  shipfast init && shipfast deploy
```

## 下载与校验

**Linux / macOS：**

```bash
sha256sum -c SHA256SUMS
```

**Windows（PowerShell）：**

```powershell
Get-Content .\SHA256SUMS | ForEach-Object {
  $p = $_ -split '\s+', 2; $e = $p[0].ToLower()
  $a = (Get-FileHash $p[1] -Algorithm SHA256).Hash.ToLower()
  if ($a -eq $e) { "OK $($p[1])" } else { "FAIL $($p[1])" }
}
```

## 发布仓库

| 平台 | 地址 |
|------|------|
| Gitee | https://gitee.com/wang-sheng-ordersystem/shipfast/tree/master/dist/installers |
| GitHub | https://github.com/wangshengbear/ShipFast/tree/main/dist/installers |

## 系统要求

| 项目 | 要求 |
|------|------|
| CLI 本机 | Windows 10+ / Linux / macOS 11+ |
| Agent | Linux + Docker（推荐），开放 8443 |
| Cloud（可选） | 端口 19000（需单独部署） |

## 文档与支持

| 内容 | 说明 |
|------|------|
| `QUICKSTART.md` | 分步安装（中英双语） |
| 在线文档 | https://shipfast.app/docs |
| Issues | 请附 `shipfast version` 与操作系统 |

---

# English

## What is ShipFast?

ShipFast is a **deployment platform** for developers and teams: ship projects from your machine to remote servers with one command. Optionally connect **ShipFast Cloud** for accounts, licenses, teams, policy, and billing.

| Audience | Capabilities |
|----------|--------------|
| Individual | `shipfast init` → `shipfast deploy`, stack auto-detection |
| Small teams | Local **Web console** (dashboard, deploy wizard, ops, secrets) |
| Enterprise | Cloud admin: users, Policy-as-Code, pricing, license, audit |

## Architecture

```
Local shipfast CLI (+ Web GUI)  ──HTTPS──►  Remote shipfast-agent (Docker)
         │
         └── optional ──►  ShipFast Cloud (account / license / policy / billing)
```

| Component | Where | Role |
|-----------|-------|------|
| **shipfast CLI** | Windows / Linux / macOS | Deploy, server management, **embedded Web GUI** |
| **shipfast-agent** | Linux VPS | Build, containers, Nginx, HTTPS |
| **shipfast-cloud** | Self-hosted or SaaS (**not in CLI package**) | Account, license, `/admin` console |

## Packages in this folder

| File | Description |
|------|-------------|
| `ShipFast-<ver>-windows-amd64.zip` / `.msi` | Windows CLI (includes `shipfast-mcp`) |
| `ShipFast-<ver>-linux-*.tar.gz` | Linux CLI |
| `ShipFast-<ver>-darwin-*.tar.gz` | macOS CLI |
| `ShipFast-Agent-<ver>-linux-*.tar.gz` | Linux Agent |
| `SHA256SUMS` | Checksums |
| `README.md` / `QUICKSTART.md` | Bilingual docs |

## Key features (v0.20+)

### CLI

- Deploy, rollback, logs, multi-env, GitOps, secrets, pipeline, Terraform export
- `shipfast web` — local Web UI (`http://127.0.0.1:19900`)
- `shipfast upgrade` — check for updates (CDN + Gitee + GitHub)
- `shipfast doctor` — CDN / Cloud connectivity

### Web GUI (embedded in CLI)

- **EN / 中文 UI** — language switch (top-right)
- **Help center** — GUI guide + full CLI reference (bilingual)
- **Update notifications** — banner when a new version exists; **Settings → License** to check or upgrade
- Dashboard, deploy wizard, Cloud, ops, secrets, marketplace, etc.

### Update sources (auto-probed)

The client checks all sources below and uses the **highest version**:

| Source | Default |
|--------|---------|
| CDN | `https://releases.shipfast.app/latest/version.txt` |
| Gitee | `wang-sheng-ordersystem/shipfast` (tags, e.g. `v0.20.0`) |
| GitHub | `wangshengbear/ShipFast` (tags) |

Installer URL example (macOS ARM):

- Gitee: `.../raw/master/dist/installers/ShipFast-0.20.0-darwin-arm64.tar.gz`
- GitHub: `.../raw/main/dist/installers/ShipFast-0.20.0-darwin-arm64.tar.gz`

Optional env vars:

```bash
export SHIPFAST_DOWNLOAD_BASE=https://your-cdn.example
export SHIPFAST_GITEE_REPO=owner/repo
export SHIPFAST_GITHUB_REPO=owner/repo
export SHIPFAST_UPDATE_SOURCES=cdn,gitee,github
```

## 5-minute workflow

```
1. Install CLI     →  shipfast version
2. Open Web GUI    →  shipfast web
3. Cloud login     →  shipfast cloud login (optional)
4. Install Agent   →  extract Agent tarball, run install.sh on VPS
5. Add server      →  shipfast server add
6. First deploy    →  shipfast init && shipfast deploy
```

## Verify downloads

**Linux / macOS:** `sha256sum -c SHA256SUMS`  
**Windows:** see PowerShell snippet in the Chinese section above.

## Release repositories

| Host | URL |
|------|-----|
| Gitee | https://gitee.com/wang-sheng-ordersystem/shipfast/tree/master/dist/installers |
| GitHub | https://github.com/wangshengbear/ShipFast/tree/main/dist/installers |

## Requirements

| Item | Requirement |
|------|-------------|
| CLI host | Windows 10+ / Linux / macOS 11+ |
| Agent | Linux + Docker (recommended), port 8443 open |
| Cloud (optional) | Port 19000 (separate install) |

## Documentation & support

| Item | Link |
|------|------|
| `QUICKSTART.md` | Step-by-step setup (bilingual) |
| Online docs | https://shipfast.app/docs |
| Issues | Include `shipfast version` and OS |

---

*ShipFast — Deploy like you code.*
