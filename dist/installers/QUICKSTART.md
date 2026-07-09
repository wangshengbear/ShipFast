# ShipFast 快速开始 · Quick Start

> **发行版本 Release:** `0.20.0`  
> **文档 · Document:** 中英双语合订 · Bilingual (中文 first, then English)

---

# 中文

## 本发布包包含

| 文件 | 说明 |
|------|------|
| `ShipFast-0.20.0-windows-amd64.zip` / `.msi` | Windows CLI |
| `ShipFast-0.20.0-linux-*.tar.gz` | Linux CLI（amd64 / arm64） |
| `ShipFast-0.20.0-darwin-*.tar.gz` | macOS CLI（Intel / Apple Silicon） |
| `ShipFast-Agent-0.20.0-linux-*.tar.gz` | Linux Agent（装在你的 VPS） |
| `SHA256SUMS` | 安装包校验 |
| `README.md` | 产品说明（中英双语） |

**架构：** 本机 **CLI + Web GUI** → 远程 **Agent** 执行部署；**Cloud**（可选）管账号与 License。

---

## 第 0 步：校验下载（推荐）

将安装包与 `SHA256SUMS` 放在同一目录。

**Linux / macOS：**

```bash
sha256sum -c SHA256SUMS
# macOS 若无 sha256sum：shasum -a 256 -c SHA256SUMS
```

**Windows（PowerShell）：**

```powershell
Get-Content .\SHA256SUMS | ForEach-Object {
  $p = $_ -split '\s+', 2; $e = $p[0].ToLower()
  $a = (Get-FileHash $p[1] -Algorithm SHA256).Hash.ToLower()
  if ($a -eq $e) { "OK $($p[1])" } else { "FAIL $($p[1])" }
}
```

---

## 第 1 步：安装 CLI（你的电脑）

### Windows

- **MSI（推荐）：** 双击 `ShipFast-0.20.0-windows-amd64.msi`
- **ZIP：** 解压后运行 `install.ps1`，或将 `shipfast.exe` 加入 PATH

```powershell
shipfast version
```

### Linux

```bash
tar -xzf ShipFast-0.20.0-linux-amd64.tar.gz
cd ShipFast-0.20.0-linux-amd64
chmod +x install.sh && ./install.sh
shipfast version
```

（arm64 服务器请选 `linux-arm64` 包。）

### macOS

```bash
tar -xzf ShipFast-0.20.0-darwin-arm64.tar.gz   # Apple Silicon
# Intel：ShipFast-0.20.0-darwin-amd64.tar.gz
cd ShipFast-0.20.0-darwin-arm64
chmod +x install.sh && ./install.sh
shipfast version
```

---

## 第 2 步：打开 Web 控制台（可选但推荐）

```bash
shipfast web
```

浏览器打开 **http://127.0.0.1:19900**

| 功能 | 说明 |
|------|------|
| 语言切换 | 顶栏右侧 **English / 中文** |
| 帮助中心 | 顶栏「帮助」— GUI 文档与 CLI 命令集（双语） |
| 检查更新 | 有新版本时顶栏横幅提示；或 **设置 → License → 检查更新 / 立即升级** |
| 服务器管理 | **设置 → 服务器** 添加 Agent |

团队访问可加 Token：

```bash
shipfast web --host 0.0.0.0 --port 19900 --token YOUR_SECRET
```

在 Web **设置 → Web 认证** 填入相同 Token。

---

## 第 3 步：登录 Cloud 并激活 License（如已开通）

```bash
shipfast cloud login
shipfast license activate <YOUR_LICENSE_KEY>
shipfast license status
```

也可在 Web **设置 → License** 中激活。  
推广期若未强制 License，可先完成 Agent 与首次部署。

---

## 第 4 步：在远程服务器安装 Agent

**要求：** Linux VPS、**Docker**（推荐）、开放 **8443**。

```bash
tar -xzf ShipFast-Agent-0.20.0-linux-amd64.tar.gz
cd shipfast-agent
chmod +x install.sh
sudo ./install.sh              # systemd
# 或: sudo ./install.sh --docker
```

记录 Token：

```bash
sudo cat /var/lib/shipfast/agent.token
curl -s http://127.0.0.1:8443/health
```

---

## 第 5 步：登记服务器（本机）

```bash
shipfast server add
```

| 字段 | 示例 |
|------|------|
| 别名 | `prod` |
| 主机 | 公网 IP 或域名 |
| 端口 | `8443` |
| 协议 | `http` / `https` |
| Token | Agent Token |

```bash
shipfast server test prod
```

---

## 第 6 步：部署第一个项目

```bash
cd your-project
shipfast init
shipfast deploy -s prod
```

---

## 检查与升级新版本

CLI 与 Web GUI **内嵌在同一二进制**中，升级 CLI 即升级 GUI。

```bash
shipfast upgrade              # 检查并显示升级命令
shipfast upgrade --check      # 仅检查
shipfast doctor               # 连通性（含 CDN 版本对比）
```

客户端自动探测：**CDN**、**Gitee**、**GitHub** 上的最新 tag（取最高版本）。

| 来源 | 仓库 |
|------|------|
| Gitee | https://gitee.com/wang-sheng-ordersystem/shipfast |
| GitHub | https://github.com/wangshengbear/ShipFast |

安装包目录：`dist/installers/`（与本 `QUICKSTART.md` 同目录）。

---

## 常用命令

```bash
shipfast list -s prod
shipfast logs <name> -s prod
shipfast restart <name> -s prod
shipfast rollback <name> -s prod
shipfast web
shipfast upgrade
```

---

## 故障排查

| 现象 | 处理 |
|------|------|
| `server test` 失败 | 检查 8443 防火墙；Agent 是否运行 |
| Web 打不开 | 确认 `shipfast web` 已启动；端口 19900 未被占用 |
| 检查更新失败 | 确认能访问 Gitee/GitHub；或设置 `SHIPFAST_UPDATE_SOURCES=gitee,github` |
| Cloud 登录失败 | `shipfast cloud status` 检查 endpoint |

---

# English

## What's in this release

| File | Description |
|------|-------------|
| `ShipFast-0.20.0-windows-amd64.zip` / `.msi` | Windows CLI |
| `ShipFast-0.20.0-linux-*.tar.gz` | Linux CLI (amd64 / arm64) |
| `ShipFast-0.20.0-darwin-*.tar.gz` | macOS CLI |
| `ShipFast-Agent-0.20.0-linux-*.tar.gz` | Linux Agent (on your VPS) |
| `SHA256SUMS` | Checksums |
| `README.md` | Product overview (bilingual) |

**Flow:** local **CLI + Web GUI** → remote **Agent**; optional **Cloud** for account & license.

---

## Step 0: Verify downloads (recommended)

Place packages and `SHA256SUMS` in the same folder. Use the verification commands in the Chinese section above.

---

## Step 1: Install CLI (your machine)

### Windows

- **MSI (recommended):** run `ShipFast-0.20.0-windows-amd64.msi`
- **ZIP:** run `install.ps1` or add `shipfast.exe` to PATH

```powershell
shipfast version
```

### Linux

```bash
tar -xzf ShipFast-0.20.0-linux-amd64.tar.gz
cd ShipFast-0.20.0-linux-amd64
chmod +x install.sh && ./install.sh
shipfast version
```

### macOS

```bash
tar -xzf ShipFast-0.20.0-darwin-arm64.tar.gz
cd ShipFast-0.20.0-darwin-arm64
chmod +x install.sh && ./install.sh
shipfast version
```

---

## Step 2: Open the Web console (recommended)

```bash
shipfast web
```

Open **http://127.0.0.1:19900**

| Feature | Notes |
|---------|-------|
| Language | **English / 中文** switch (top-right) |
| Help | GUI guide + CLI reference (bilingual) |
| Updates | Banner when a new version exists; **Settings → License** to check or upgrade |
| Servers | **Settings → Servers** to add Agent |

Remote access with token:

```bash
shipfast web --host 0.0.0.0 --port 19900 --token YOUR_SECRET
```

Enter the same token under **Settings → Web auth**.

---

## Step 3: Cloud login & license (if enabled)

```bash
shipfast cloud login
shipfast license activate <YOUR_LICENSE_KEY>
shipfast license status
```

Or activate under **Settings → License** in the Web UI.

---

## Step 4: Install Agent on VPS

**Requirements:** Linux VPS, **Docker** (recommended), port **8443** open.

```bash
tar -xzf ShipFast-Agent-0.20.0-linux-amd64.tar.gz
cd shipfast-agent
chmod +x install.sh
sudo ./install.sh
```

Save the Agent token (see Chinese section for commands).

---

## Step 5: Register server (local machine)

```bash
shipfast server add
shipfast server test prod
```

Or use **Settings → Servers** in the Web UI.

---

## Step 6: First deploy

```bash
cd your-project
shipfast init
shipfast deploy -s prod
```

---

## Check for updates

CLI and Web GUI ship in **one binary** — upgrading CLI upgrades the GUI.

```bash
shipfast upgrade
shipfast upgrade --check
shipfast doctor
```

The client probes **CDN**, **Gitee**, and **GitHub** tags and uses the highest version.

| Host | Repository |
|------|------------|
| Gitee | https://gitee.com/wang-sheng-ordersystem/shipfast |
| GitHub | https://github.com/wangshengbear/ShipFast |

Packages live under `dist/installers/` (same folder as this file).

---

## Common commands

```bash
shipfast list -s prod
shipfast logs <name> -s prod
shipfast web
shipfast upgrade
```

---

## Troubleshooting

| Issue | Action |
|-------|--------|
| `server test` fails | Open port 8443; check Agent is running |
| Web UI won't load | Ensure `shipfast web` is running; port 19900 free |
| Update check fails | Ensure Gitee/GitHub reachable; set `SHIPFAST_UPDATE_SOURCES=gitee,github` |
| Cloud login fails | Run `shipfast cloud status` |

---

## More documentation

| Topic | URL |
|-------|-----|
| Quick start | https://shipfast.app/docs/quick-start |
| GUI guide | https://shipfast.app/docs/gui-guide |
| CLI reference | https://shipfast.app/docs/cli-reference |

---

*ShipFast 0.20.0 · 写代码是 IDEA 的事，上线是 ShipFast 的事。*

