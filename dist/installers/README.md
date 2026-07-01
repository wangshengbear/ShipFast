# ShipFast

> 写代码是 IDEA 的事，上线是 ShipFast 的事。

ShipFast 是一款面向专业开发者的本机命令行部署工具，通过一行命令将本地项目或 Git 仓库部署到任意远程服务器，自动完成环境识别、容器构建、HTTPS 配置、域名绑定。

## 架构

```
开发者本机 (shipfast CLI)  ──HTTPS──►  远程服务器 (shipfast-agent)
```

| 组件 | 技术 | 说明 |
|------|------|------|
| shipfast CLI | Go | 本机命令行工具 |
| shipfast-agent | Python FastAPI | 远程部署执行器 |

## 快速开始

### 1. 构建 CLI

```bash
make build
# 或
go build -o bin/shipfast ./cmd/shipfast
```

### 2. 启动 Agent（开发模式）

```bash
cd shipfast-agent
pip install -r requirements.txt
python main.py
```

Agent 启动后会输出 Bearer Token，默认监听 `0.0.0.0:8443`。

### 3. 配置服务器

```bash
shipfast server add
# 别名: default
# 主机: 127.0.0.1
# 端口: 8443
# 协议: http
# Token: <Agent 输出的 Token>
```

### 4. 初始化并部署项目

```bash
cd your-project
shipfast init
shipfast deploy
```

## CLI 命令

| 命令 | 别名 | 说明 |
|------|------|------|
| `shipfast init` | | 生成 shipfast.yaml |
| `shipfast deploy` | `d` | 部署项目 |
| `shipfast list` | `ls` | 列出项目 |
| `shipfast info` | `i` | 项目详情 |
| `shipfast logs` | `l` | 查看日志 |
| `shipfast restart` | `r` | 重启项目 |
| `shipfast stop` | | 停止项目 |
| `shipfast start` | | 启动项目 |
| `shipfast delete` | `rm` | 删除项目 |
| `shipfast rollback` | | 回滚版本 |
| `shipfast history` | `hist` | 部署历史 |
| `shipfast env` | | 多环境管理 |
| `shipfast server` | | 管理服务器 |
| `shipfast template` | | 管理模板 / 市场 |
| `shipfast cloud` | | ShipFast Cloud |
| `shipfast approve` | | Teams 部署审批 |
| `shipfast secrets` | | 密钥管理 |
| `shipfast audit` | | Cloud 操作审计 |
| `shipfast metrics` | | Agent 部署指标 |
| `shipfast canary` | | Canary 灰度管理 |
| `shipfast schedule` | | 定时部署 |
| `shipfast team` | | Cloud 团队 RBAC |
| `shipfast k8s` | | Kubernetes 状态 |
| `shipfast dash` | | TUI 仪表板 |
| `shipfast web` | | Web 仪表板 (Vue 3) |

## 配置文件 shipfast.yaml

```yaml
name: my-api
type: node
git: https://github.com/user/repo.git
build:
  command: npm run build
run:
  command: node dist/index.js
  port: 3000
env:
  NODE_ENV: production
domain:
  primary: api.example.com
  ssl: auto
servers:
  - default
```

## 远程 Agent 安装

**推荐（Docker，闭源发行）：**

```bash
curl -fsSL https://shipfast.app/install-agent.sh | sudo bash
```

**CLI 安装：**

```bash
curl -fsSL https://shipfast.app/install.sh | bash
# Windows: iwr -useb https://shipfast.app/install.ps1 | iex
```

用户文档：[docs/quick-start.md](docs/quick-start.md)  
闭源发布清单：[docs/closed-source-release.md](docs/closed-source-release.md)

## 开发路线图

- **Phase 1**: CLI + Agent 核心闭环 ✅
- **Phase 2**: TUI 仪表板、部署历史、多环境、健康检查 ✅
- **Phase 3**: Web GUI (Vue 3 + Naive UI) ✅
- **Phase 4**: 模板市场、ShipFast Cloud、VS Code 插件 ✅
- **Phase 5**: CI 集成、Secrets、通知、自动回滚、Teams 审批、Cursor MCP ✅
- **Phase 6**: 审计日志、并行部署、Agent 指标、SMTP STARTTLS ✅
- **Phase 7**: 蓝绿/Canary、Prometheus、定时部署 ✅
- **Phase 8**: RBAC 多租户、K8s 适配、Web 策略 UI ✅
- **Phase 9**: Helm Chart、OIDC SSO、K8s 多集群、告警规则 ✅
- **Phase 10**: GitOps Webhook、Cloud 工作区、配置漂移、SLA 监控 ✅
- **Phase 11**: GitOps 自动部署、备份恢复、故障管理、可靠性 Web UI ✅
- **Phase 12**: 密钥轮换、部署流水线、Terraform 导出、舰队运维 ✅
- **Phase 13**: Web 服务器管理、托管 Web、Cloud 远程控制台、Phase 12 Web UI ✅
- **Phase 14**: License 校验、CLI 升级、发行配置 ✅
- **Phase 15**: Cloud 套餐、计费 Demo、功能限制 ✅
- **Phase 16**: Stripe Checkout、Webhook、定价页、计费邮件 ✅
- **Phase 17**: Customer Portal、订阅取消、发票历史 ✅
- **Phase 18**: 用量计量、团队席位、Admin 运营 ✅
- **Phase 19**: Homebrew/winget、Cloud 自托管、状态页、doctor ✅
- **Phase 20**: 注册验证、离线 license.json、状态事件、Open VSX、CDN 同步 ✅

## Phase 20 — 生产就绪

```bash
shipfast cloud register              # 自助注册 + 邮件验证
shipfast cloud verify --token <T>
shipfast license import license.json # 企业离线 License
shipfast cloud license issue --offline
shipfast cloud admin status create   # 状态页事件
bash scripts/publish-extension.sh    # Open VSX 发布
```

- **生产**：`SHIPFAST_ENABLE_DEMO=0` + SMTP + `SHIPFAST_LICENSE_SECRET`
- **状态站**：`packaging/status-site/index.html` → `status.shipfast.app`
- **CDN**：CI `mirror-latest` + `scripts/sync-releases.sh`（S3/R2）
- 文档：[docs/registration.md](docs/registration.md) · [docs/offline-license.md](docs/offline-license.md)

## Phase 19 — 分发与状态

```bash
shipfast doctor                        # CDN / Cloud 连通性
curl -fsSL https://shipfast.app/install-cloud.sh | bash   # 自托管 Cloud
brew tap shipfast/tap && brew install shipfast            # macOS/Linux
winget install ShipFast.ShipFast                          # Windows
```

- **状态页**：Cloud `/status` · JSON `/api/v1/status`
- **打包**：`scripts/generate-packaging.sh` → Homebrew / winget manifest
- 文档：[docs/distribution.md](docs/distribution.md)

## Phase 18 — 用量与运营

```bash
shipfast cloud usage                 # 组织用量 / 席位
shipfast cloud team invite --email new@co.app --password *** --role deployer
shipfast cloud admin overview        # admin 运营总览
shipfast cloud admin users
```

- **用量**：API 请求、同步、部署（按月）
- **席位**：trial 1 · starter 3 · teams 10 · enterprise 999
- **Admin**：用户/订阅/MRR/审计总览

## Phase 17 — 订阅生命周期

```bash
shipfast cloud billing              # 用量与订阅状态
shipfast cloud billing portal       # Stripe Customer Portal
shipfast cloud billing cancel       # 周期末取消（--immediate 立即）
shipfast cloud billing invoices     # 账单历史
```

Webhook 新增：`customer.subscription.deleted/updated`、`invoice.paid`

## Phase 16 — Stripe 与商业化

```bash
# 生产 Cloud 环境
export STRIPE_SECRET_KEY=sk_live_...
export STRIPE_WEBHOOK_SECRET=whsec_...
export SHIPFAST_CLOUD_URL=https://cloud.shipfast.app

shipfast cloud upgrade starter    # 打开 Stripe Checkout
```

- **Checkout**：`POST /api/v1/billing/checkout` → `checkout_url`
- **Webhook**：`POST /hooks/stripe` → 自动升级 + License
- **落地页**：`/pricing`、`/billing/success`、`/billing/cancel`
- **邮件**：SMTP 配置后发送 License Key

详见 [docs/stripe-setup.md](docs/stripe-setup.md)。

## Phase 15 — 套餐与计费

### 套餐

| 计划 | 价格 | 服务器 | 远程代理 | GitOps | 团队 RBAC |
|------|------|--------|----------|--------|-----------|
| trial | 免费 | 1 | — | — | — |
| starter | $29/月 | 3 | ✓ | — | — |
| teams | $99/月 | 10 | ✓ | ✓ | ✓ |
| enterprise | 定制 | 999 | ✓ | ✓ | ✓ |

```bash
shipfast cloud plans
shipfast cloud billing
shipfast cloud upgrade starter --demo   # Demo 模式直接升级
```

Cloud 同步时会校验服务器数量；`sync --with-credentials`、Webhook、工作区、团队 RBAC 按套餐限制。

Web：**Cloud 页 → 定价** 可一键 Demo 升级。

Stripe 集成：设置 `STRIPE_SECRET_KEY` 后启用 Checkout；详见 [docs/stripe-setup.md](docs/stripe-setup.md)。

详见 [docs/billing.md](docs/billing.md)。

## Phase 14 — License 与发行闭环

### License

```bash
shipfast cloud login
shipfast license activate sf_trial_welcome
shipfast license status
shipfast cloud license issue    # admin 签发
```

强制校验：`SHIPFAST_REQUIRE_LICENSE=1 shipfast deploy`

Web 控制台：**设置 → License** 可查看/激活/检查更新。

MCP 工具：`shipfast_license_status`、`shipfast_upgrade_check`

详见 [docs/licensing.md](docs/licensing.md)。

### CLI 升级

```bash
shipfast upgrade
```

### 发行配置

- CDN 环境变量示例：`config/releases.env.example`
- CI 发布：`git tag v0.14.0 && git push origin v0.14.0`
- 用户文档：[docs/quick-start.md](docs/quick-start.md) · [docs/download.md](docs/download.md)

## Phase 13 — 远程控制平面与 Web 完整性

### Web 服务器管理

设置页可直接 **添加 / 编辑 / 删除 / 测试** 远程 Agent，无需 CLI：

- API: `POST/PUT/DELETE /api/gui/servers`, `POST .../test`
- Token 加密存储于 `~/.shipfast/servers.yaml`

### 托管 Web 模式

局域网或团队共享控制台：

```bash
shipfast web --host 0.0.0.0 --port 19900 --token $SHIPFAST_WEB_TOKEN
```

浏览器在 **设置 → Web 认证** 填写相同 Token。未配置 Token 时行为与 Phase 3 一致（仅本机）。

### Cloud 远程控制台

同步 Agent 凭证（Cloud 端 AES 加密）后，Cloud 可实时探测 Agent：

```bash
shipfast cloud sync --with-credentials
shipfast cloud serve --with-ui   # 嵌入 Web UI + OIDC/RBAC
```

- 实时仪表板：`EnrichDashboard` 使用凭证库
- Agent 代理：`GET /api/v1/agents/{alias}/status|projects`

### Phase 12 Web UI

侧栏新增：

- **密钥** `/secrets` — 设置 / 轮换 / 删除 / 审计
- **流水线** `/pipeline` — 校验与查看阶段
- **Terraform** `/terraform` — HCL 预览与下载

## Phase 12 — 生产运维、IaC、舰队报告

### 密钥生命周期

密钥存储于 `~/.shipfast/secrets.yaml`，操作写入 `~/.shipfast/secrets-audit.json`：

```bash
shipfast secrets set DB_URL
shipfast secrets rotate DB_URL
shipfast secrets rotate DB_URL --redeploy   # 轮换后重新部署
shipfast secrets delete OLD_KEY
shipfast secrets audit
```

`shipfast.yaml` 中 `env` 仍支持 `{{ secrets.KEY }}` 占位符。

### 部署流水线

`pipeline` 段定义 build → deploy → smoke 阶段（见 `examples/pipeline.yaml`）：

```yaml
pipeline:
  - name: lint
    command: npm run lint
  - name: deploy
    deploy: true
  - name: smoke
    smoke: true
```

```bash
shipfast pipeline validate
shipfast pipeline run
shipfast pipeline run --env production
```

### Terraform 导出

从 `shipfast.yaml` 生成 Terraform HCL（K8s 项目生成 Deployment/Service）：

```bash
shipfast terraform export
shipfast terraform export -c shipfast.yaml -o main.tf
```

参考 `examples/terraform/README.md`。

### 舰队运维

跨服务器汇总 SLA、漂移、故障与成本：

```bash
shipfast ops report
shipfast ops report --hours 168
```

Web GUI **运维** 页 (`/ops`) 提供可视化舰队报告。

## Phase 11 — GitOps 闭环、备份、故障、Web 可靠性

### GitOps 自动部署

`shipfast.yaml` 支持 `gitops` 段（见 `examples/gitops.yaml`）：

```yaml
gitops:
  enabled: true
  branch: main
  auto_deploy: true
  poll_interval: 30s
```

```bash
shipfast gitops status
shipfast gitops deploy <event-id>
shipfast gitops daemon --interval 30s
# 或
shipfast schedule daemon --gitops --gitops-every 30s
```

设置 `SHIPFAST_GITOPS_CONFIG` 或 `SHIPFAST_GITOPS_ROOTS` 帮助 daemon 定位项目配置。

### 备份与恢复

```bash
shipfast backup export -o ./backup
shipfast backup restore -i ./backup
shipfast backup cloud-export -o cloud.json
```

包含 Agent SQLite/projects 与 Cloud `cloud.json`。

### 故障管理 (Incidents)

告警触发时自动创建 incident（可 ack/resolve）：

```bash
shipfast incidents list
shipfast incidents ack inc-xxxx
shipfast incidents resolve inc-xxxx
```

### Web 可靠性 UI

- 项目详情：漂移、SLA、故障卡片
- Cloud 页：Webhook / GitOps 事件、工作区

## Phase 10 — GitOps、工作区、漂移、SLA

### GitOps 入站 Webhook

```bash
shipfast cloud serve
shipfast cloud login
shipfast webhook create --project my-api --branch main
shipfast webhook list
shipfast webhook events    # 待处理 push 事件
shipfast webhook ack <event-id>
```

GitHub 仓库 Webhook URL 指向 Cloud 返回的 `URL`，Secret 使用创建时的 `secret`。推送匹配分支后产生 GitOps 事件，可配合 `shipfast deploy` 或 CI 消费。

参考 `examples/gitops.yaml`。

### Cloud 工作区与持久化

- 审计、审批、Webhook、工作区写入 `~/.shipfast/cloud-data/cloud.json`（重启不丢失）
- 同步服务器时探测 Agent 在线状态与项目数

```bash
shipfast workspace list
shipfast workspace create my-team
shipfast workspace use ws-xxxx
```

### 配置漂移检测

```bash
shipfast drift status my-api
shipfast drift check
shipfast drift check my-api --fix   # 漂移时重新部署
```

Agent 比较 `desired_state.config_hash` 与当前配置、健康与 K8s 副本。

### SLA 与告警守护进程

```bash
shipfast sla status my-api --hours 24
shipfast sla report --hours 168
shipfast schedule daemon --alerts --alert-every 5m
```

健康检查历史计算可用性；资源限制估算月度成本（演示）。

## Phase 9 — Helm、OIDC、多集群、告警

### Helm Chart 安装 Agent

```bash
shipfast helm template --release shipfast -n shipfast
shipfast helm install --release shipfast -n shipfast
shipfast helm uninstall --release shipfast -n shipfast
```

Chart 位于 `helm/shipfast/`。安装后从 Secret 读取 Agent Token（见 `helm template` 输出的 NOTES）。

### Cloud OIDC / SSO

```bash
export SHIPFAST_OIDC_MOCK=1   # 本地演示
shipfast cloud serve

# 浏览器登录
shipfast cloud login --oidc
# 或使用回调页显示的兑换码
shipfast cloud login --oidc-code <code>
```

生产环境配置：

| 环境变量 | 说明 |
|----------|------|
| `SHIPFAST_OIDC_ISSUER` | IdP Issuer URL |
| `SHIPFAST_OIDC_CLIENT_ID` | OAuth Client ID |
| `SHIPFAST_OIDC_CLIENT_SECRET` | Client Secret |
| `SHIPFAST_OIDC_REDIRECT_URL` | 回调 URL（默认 `http://localhost:19000/api/v1/auth/oidc/callback`） |
| `SHIPFAST_OIDC_SCOPES` | 空格分隔 scope（默认 `openid email profile`） |

### Kubernetes 多集群

```yaml
kubernetes:
  enabled: true
  cluster: prod          # 默认集群
  clusters:
    prod:
      context: prod-ctx
      namespace: apps
      replicas: 3
    staging:
      context: staging-ctx
      namespace: staging
      replicas: 1
```

```bash
shipfast k8s clusters
shipfast k8s status my-api
```

参考 `examples/kubernetes-multicluster.yaml`。

### 告警规则

```yaml
alerts:
  - name: health-down
    condition: health_error
    severity: critical
    cooldown: 300
    notify: true
  - name: stopped
    condition: status_stopped
    severity: warning
```

条件：`health_error` | `status_stopped` | `status_error`。部署后自动评估；也可手动触发：

```bash
shipfast alerts list
shipfast alerts check
```

Web GUI 项目详情页展示最近告警。参考 `examples/alerts.yaml`。

## Phase 8 — RBAC、K8s、策略 Web UI

### 多租户 RBAC (Cloud)

| 角色 | 权限 |
|------|------|
| `admin` | 全部：审批、团队管理、同步、部署审计 |
| `deployer` | 同步服务器、提交部署/审批请求、写审计 |
| `viewer` | 只读：仪表板、审计列表 |

```bash
shipfast cloud serve
shipfast cloud login    # demo@shipfast.app / demo (admin)
shipfast team list
shipfast team set-role deployer@shipfast.app deployer
```

演示账户（密码均为 `demo`）：
- `demo@shipfast.app` — admin
- `deployer@shipfast.app` — deployer
- `viewer@shipfast.app` — viewer

Web GUI → **Cloud** 页显示角色、团队列表；**admin** 可审批。

### Kubernetes 部署

```yaml
kubernetes:
  enabled: true
  namespace: default
  replicas: 2
  manifest: k8s/deployment.yaml   # 可选
```

Agent 自动生成 Deployment + Service 并 `kubectl apply`；无 kubectl 时模拟模式。

```bash
shipfast deploy
shipfast k8s status my-api
```

参考 `examples/kubernetes.yaml`。

### Web 策略 UI

项目详情页 → **部署策略** 卡片：
- 显示 rolling / blue_green / canary / kubernetes
- Canary 活跃时一键 **Promote** / **Abort**

## Phase 7 — 蓝绿、Canary、Prometheus、定时部署

### 部署策略 (shipfast.yaml)

```yaml
strategy:
  type: canary          # rolling | blue_green | canary
  canary:
    percent: 10
    port_offset: 1001
  blue_green:
    port_offset: 1000
```

| 策略 | 行为 |
|------|------|
| `rolling` | 默认，直接替换容器 |
| `blue_green` | 候选容器健康检查后切换 |
| `canary` | 按 `%` 分流，手动或自动 promote |

```bash
shipfast deploy --env production
shipfast canary promote my-api    # 全量发布
shipfast canary abort my-api      # 终止灰度
```

参考 `examples/strategy.yaml`。

### Prometheus 指标

Agent 暴露 Prometheus 文本格式：

```bash
shipfast metrics --prometheus
# 或 curl http://agent:8443/metrics/prometheus
```

设置 `SHIPFAST_PROMETHEUS_PUBLIC=1` 可免 Bearer 认证（供 Prometheus scrape）。

指标：`shipfast_projects_total`、`shipfast_deployments_total`、`shipfast_canary_active`、`shipfast_system_cpu_percent` 等。

### 定时部署

```bash
shipfast schedule add --name nightly --cron "0 2 * * *" --env production
shipfast schedule list
shipfast schedule run sched-xxx      # 立即执行
shipfast schedule daemon             # 守护进程（Cron 触发 deploy）
shipfast schedule remove sched-xxx
```

任务存储于 `~/.shipfast/schedules.yaml`。

## Phase 6 — 审计、并行部署、指标

### 操作审计

Cloud 自动记录部署、审批、同步等操作；CLI 已登录 Cloud 时部署结果也会写入审计。

```bash
shipfast audit list
shipfast audit list --limit 100
```

Web GUI → **Cloud** 页面底部 **操作审计** 列表。

### 并行多服务器部署

```bash
shipfast deploy --servers prod-a,prod-b --parallel
# 或 shipfast.yaml environments.production.servers 配置多台后：
shipfast deploy --env production --parallel
```

### Agent 指标

```bash
shipfast metrics              # 当前 -s 服务器
shipfast metrics -s default   # 指定 Agent
```

返回：项目数、运行/异常数、累计部署、CPU/内存/磁盘、Docker 状态。

Cursor MCP 工具：`shipfast_metrics`、`shipfast_audit`。

### Agent 安装脚本

```bash
curl -fsSL https://shipfast.app/install-agent.sh | bash
# 或本地
bash scripts/install-agent.sh
```

## Phase 5 — CI、审批、MCP

### Secrets 管理

部署前自动解析 `shipfast.yaml` 中的 `{{ secrets.KEY }}`：

```bash
shipfast secrets set DATABASE_URL postgres://...
shipfast secrets list
# 存储于 ~/.shipfast/secrets.yaml
```

### 通知与自动回滚

在 `shipfast.yaml` 中配置：

```yaml
notifications:
  webhook: https://hooks.example.com/shipfast
  slack_webhook: https://hooks.slack.com/services/XXX
  on_success: true
  on_failure: true
rollback:
  auto_on_health_fail: true
```

健康检查失败时 Agent 自动回滚到上一版本，并发送 Webhook/Slack 通知。

### GitHub Actions

```yaml
# .github/workflows/deploy.yml
- uses: ./actions/deploy
  with:
    server: production
    env: production
    skip-approval: false
```

参考 `actions/deploy/action.yml` 与 `.github/workflows/deploy.example.yml`。

### Teams 部署审批

生产环境部署需 Cloud 审批（staging/dev 自动通过）：

```bash
shipfast cloud serve              # 启动 Cloud
shipfast deploy --env production  # 自动提交审批并等待
shipfast approve list             # 查看待审批
shipfast approve accept <id>      # 批准
shipfast approve reject <id>      # 拒绝
shipfast deploy --skip-approval   # 跳过审批（CI 管理员）
```

Web GUI → **Cloud** 页面支持审批队列。

审批邮件（可选，配置 SMTP 后自动发送）：

```bash
export SHIPFAST_SMTP_HOST=smtp.example.com
export SHIPFAST_SMTP_PORT=587
export SHIPFAST_SMTP_USER=shipfast@example.com
export SHIPFAST_SMTP_PASS=secret
export SHIPFAST_SMTP_FROM=shipfast@example.com
export SHIPFAST_APPROVAL_EMAIL=lead@example.com,ops@example.com
shipfast cloud serve
```

未配置 SMTP 时，Cloud 会在日志中输出 `[email stub]` 便于本地开发。

### Cursor MCP 集成

```bash
make mcp-build   # → bin/shipfast-mcp
```

复制 `.cursor/mcp.json.example` 为 `.cursor/mcp.json`，在 Cursor 中启用 ShipFast MCP。

可用工具：`list_projects`、`deploy`、`project_info`、`project_logs`、`restart_project`、`rollback`、`history`、`template_search`、`list_servers`。

## Phase 4 — 模板市场、Cloud、IDE 插件

### 模板市场

```bash
shipfast template search              # 浏览内置市场
shipfast template search api --tag node
shipfast template info node-express    # 查看详情
shipfast template pull node-express    # 安装到 ~/.shipfast/templates/
shipfast template pull nextjs --init   # 同时写入 ./shipfast.yaml
```

内置模板：`node-express` · `python-fastapi` · `go-api` · `nextjs` · `static-site` · `docker-custom`

Web GUI → **模板市场** 页面支持搜索、预览、一键安装。

环境变量：
- `SHIPFAST_MARKETPLACE_URL` — 远程目录地址
- `SHIPFAST_MARKETPLACE_OFFLINE=1` — 仅使用内置模板

### ShipFast Cloud

中心化控制台，跨设备同步服务器元数据。

```bash
# 本地自托管 Cloud（开发）
shipfast cloud serve          # http://localhost:19000
# 演示账户: demo@shipfast.app / demo

shipfast cloud login          # 登录（Cloud 地址填 http://localhost:19000）
shipfast cloud sync           # 同步本地服务器到 Cloud
shipfast cloud dashboard      # 查看 Cloud 仪表板
shipfast cloud status
shipfast cloud logout
```

Web GUI → **Cloud** 页面支持登录、同步、仪表板。

独立二进制：`go build -o bin/shipfast-cloud ./shipfast-cloud`

### VS Code 插件

```
extensions/vscode-shipfast/
```

```bash
cd extensions/vscode-shipfast
npm install && npm run compile
# F5 调试，或 npm run package 生成 .vsix
```

功能：侧边栏项目树、Deploy、Logs、Init、Open Web Dashboard、Template Search、状态栏快捷部署。

配置项：`shipfast.cliPath` · `shipfast.server` · `shipfast.webPort`

## Phase 3 — Web GUI

### 启动

```bash
# 构建前端并编译 CLI（首次）
make web-build
go build -o bin/shipfast ./cmd/shipfast

# 或一步
make build

# 启动 Web 控制台
shipfast web
# → http://localhost:19900
```

开发模式（热更新）：

```bash
# 终端 1
shipfast web

# 终端 2
cd shipfast-web && npm run dev
# → http://localhost:5173 （API 代理到 19900）
```

### 功能页面

| 页面 | 路径 | 功能 |
|------|------|------|
| 仪表板 | `/` | 多服务器卡片、项目表格、批量重启/停止 |
| 项目详情 | `/projects/:server/:name` | 概览、部署历史时间线、资源图表、SSE 日志 |
| 部署向导 | `/deploy` | 选服务器 → 填配置 → 一键部署，支持模板 |
| 设置 | `/settings` | 服务器列表、模板列表 |

Web GUI 通过本地 Go 服务 (`/api/gui/*`) 代理到各 Agent，Token 不暴露给浏览器。

## Phase 2 功能

### TUI 仪表板 (`shipfast dash`)

```
左栏 — 项目列表（状态/版本）
右上 — 详情与部署历史
右下 — 日志尾部

快捷键: ↑/↓ 选择  r 重启  s 停止  b 回滚  R 刷新  q 退出
```

### 多环境 (`shipfast env`)

```bash
shipfast env list              # 列出 environments
shipfast env use staging       # 切换全局环境
shipfast env use production --local  # 仅当前项目 (.shipfast.env)
shipfast env show              # 预览合并后的配置
shipfast deploy --env production   # 或读取 env use 设置
```

`shipfast.yaml` 环境块支持覆盖：`servers`、`env`、`domain`、`resources`、`health_check`、`branch`。

### 部署历史与回滚

```bash
shipfast history my-api        # 查看部署历史
shipfast rollback my-api --list
shipfast rollback my-api --version v1.2.2
```

### 健康检查

`shipfast init` 会引导配置 `health_check.path/timeout/expected_status`；Agent 部署后轮询检查，失败标记为 `error`。

## License

MIT
