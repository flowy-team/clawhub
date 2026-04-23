# FlowyTeam MCP

> Connect Claude Code (or any MCP-compatible AI agent) to your FlowyTeam workspace and manage your entire business via natural language.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![MCP Protocol](https://img.shields.io/badge/MCP-2024--11--05-blue.svg)](https://modelcontextprotocol.io)
[![Tools](https://img.shields.io/badge/Tools-33-orange.svg)](SKILL.md)
[![Platform](https://img.shields.io/badge/Platform-FlowyTeam-teal.svg)](https://flowyteam.com)
[![ClawhHub](https://img.shields.io/badge/ClawhHub-flowyteam--mcp-purple.svg)](https://clawhub.ai/agungksidik/flowyteam-mcp)

---

## What is FlowyTeam?

[FlowyTeam](https://flowyteam.com) is an all-in-one SaaS platform for team productivity and performance management — OKRs, KPIs, projects, HR, CRM, finance, and more. Trusted by 7,000+ organizations across 140+ countries.

---

## What does this MCP do?

This skill connects Claude Code (or any MCP client) to your FlowyTeam workspace via a remote HTTP MCP server. Once connected, you can manage your entire workspace through natural language — no need to open the app.

**33 tools. Full CRUD. No extra software required.**

---

## Endpoints

| Endpoint | Auth | Purpose |
|---|---|---|
| `POST /api/mcp/gateway` | None required | **Gateway** — public endpoint; exposes `auth_register` & `auth_login` plus all authenticated tools (token passed per-call in header) |
| `POST /api/v2/mcp/rpc` | Bearer token | **RPC** — authenticated endpoint; all 31 workspace tools |

Use the **Gateway** if you want a single URL for everything (bootstrap + use).
Use the **RPC** endpoint if you already have a token.

---

## Install via ClawhHub

The easiest way to add FlowyTeam MCP is directly from [ClawhHub](https://clawhub.ai/agungksidik/flowyteam-mcp):

```bash
# Install from ClawhHub registry
clawhub install agungksidik/flowyteam-mcp
```

Or add manually using the steps below.

---

## Quick Start

### Option A — You already have an account & token

#### 1. Get your API Token

Log in to FlowyTeam → **Settings → MCP & AI Integration** → Copy your token.

#### 2. Connect with Claude Code (CLI)

```bash
claude mcp add flowyteam \
  --transport http \
  --url https://flowyteam.com/api/v2/mcp/rpc \
  --header "Authorization: Bearer YOUR_API_TOKEN"
```

#### 3. Or use Claude Desktop / Cursor

Add this to your `mcp.json`:

```json
{
  "mcpServers": {
    "flowyteam": {
      "transport": "http",
      "url": "https://flowyteam.com/api/v2/mcp/rpc",
      "headers": {
        "Authorization": "Bearer YOUR_API_TOKEN"
      }
    }
  }
}
```

---

### Option B — Zero-token bootstrap (no account yet)

If MCP registration is enabled on the platform, an AI agent can self-register and obtain a token without any prior account or manual web signup.

#### 1. Connect to the Gateway without a token

```bash
claude mcp add flowyteam \
  --transport http \
  --url https://flowyteam.com/api/mcp/gateway
```

#### 2. Ask Claude to register or login

```
"Register a new FlowyTeam account — company Acme Corp, name Alice Smith,
 email alice@acme.com, password secret123"
```

Claude calls `auth_register` → receives `api_token` immediately. No email verification required.

Or if you already have an account:

```
"Login to FlowyTeam with email alice@acme.com and password secret123"
```

#### 3. Reconnect with the token

```bash
claude mcp add flowyteam \
  --transport http \
  --url https://flowyteam.com/api/mcp/gateway \
  --header "Authorization: Bearer TOKEN_FROM_LOGIN"
```

#### 4. Full access — 33 tools available

```
"Create a task 'Review Q2 Report' in Marketing project, assign to Sarah, due April 30"
"Show me all open high-priority tickets"
"Who is on leave this week?"
"What are our company OKRs for Q2 2026?"
"Log 8 hours on the Mobile App project for today"
"Create an invoice for Acme Corp, total $5,000, due May 31"
```

---

## Tools (33)

### Authentication Tools (public — no token required)

| # | Tool | Description | Methods |
|---|---|---|---|
| 1 | `auth_register` | Register a new company + admin user account. Returns `api_token` immediately. Email verification is skipped. Only available when the platform admin has enabled MCP registration. | POST |
| 2 | `auth_login` | Login with email + password. Returns `api_token` for use as Bearer token. | POST |

### Workspace Tools (require Bearer token)

| # | Tool | Description | Methods |
|---|---|---|---|
| 3 | `tasks` | Manage tasks and assignments | GET POST PUT DELETE |
| 4 | `projects` | Manage projects and workflow | GET POST PUT DELETE |
| 5 | `employees` | Manage employees and team members | GET POST PUT DELETE |
| 6 | `objectives` | Manage OKR objectives | GET POST PUT DELETE |
| 7 | `key-result` | Manage OKR key results | GET POST PUT DELETE |
| 8 | `indicators` | Manage KPIs and performance indicators | GET POST PUT DELETE |
| 9 | `indicator-record` | Log KPI actual values per period | GET POST DELETE |
| 10 | `leads` | Manage sales leads and prospects | GET POST PUT DELETE |
| 11 | `clients` | Manage clients and relationships | GET POST PUT DELETE |
| 12 | `tickets` | Manage support tickets | GET POST PUT DELETE |
| 13 | `attendance` | Clock in / clock out / attendance history | GET POST PUT |
| 14 | `leave` | Manage leave requests and approvals | GET POST PUT DELETE |
| 15 | `department` | Manage departments and teams | GET POST PUT DELETE |
| 16 | `designation` | Manage job designations | GET POST PUT DELETE |
| 17 | `performance-cycle` | Manage performance / OKR cycles | GET POST PUT DELETE |
| 18 | `holiday` | Manage company public holidays | GET POST PUT DELETE |
| 19 | `project-category` | Manage project categories | GET POST PUT DELETE |
| 20 | `task-category` | Manage task categories | GET POST PUT DELETE |
| 21 | `ticket-type` | Manage ticket types | GET POST PUT DELETE |
| 22 | `ticket-channel` | Manage ticket channels | GET POST PUT DELETE |
| 23 | `ticket-agent` | List ticket agents and groups | GET |
| 24 | `indicator-category` | Manage KPI categories | GET POST PUT DELETE |
| 25 | `leave-type` | Manage leave types (Annual, Sick, etc.) | GET POST PUT DELETE |
| 26 | `invoices` | Manage client invoices | GET POST PUT DELETE |
| 27 | `estimates` | Manage client estimates and quotes | GET POST PUT DELETE |
| 28 | `contracts` | Manage client contracts | GET POST PUT DELETE |
| 29 | `events` | Manage company calendar events | GET POST PUT DELETE |
| 30 | `expenses` | Manage expenses and claims | GET POST PUT DELETE |
| 31 | `expense-category` | Manage expense categories | GET POST PUT DELETE |
| 32 | `notices` | Manage company notice board | GET POST PUT DELETE |
| 33 | `timelogs` | Start/stop timers, log project hours | GET POST PUT DELETE |

---

## How It Works

This MCP uses **Streamable HTTP transport (JSON-RPC 2.0)**. Every workspace tool accepts a `method` parameter to select the operation:

| `method` | Operation |
|---|---|
| `GET` | Read / list records |
| `POST` | Create a new record |
| `PUT` | Update an existing record |
| `DELETE` | Delete a record |

Auth tools (`auth_register`, `auth_login`) only support `POST` and do not require a `method` field.

### Gateway vs RPC

```
Gateway  POST /api/mcp/gateway     ← single URL for everything
  ├── initialize / tools/list      ← no auth needed
  ├── auth_register / auth_login   ← no auth needed (public tools)
  └── all workspace tools          ← reads Bearer token from header

RPC      POST /api/v2/mcp/rpc      ← authenticated only
  └── all workspace tools          ← requires Bearer token (middleware)
```

**MCP Protocol Version:** `2024-11-05`

---

## Smart Features

Several tools support **name-based resolution** — you don't need to look up IDs manually:

- `department` — lookup by `name` string (auto-finds the ID for PUT/DELETE)
- `invoices` — lookup by `invoice_number` (e.g. `INV#0001`)
- `estimates` — lookup by `estimate_number` (e.g. `EST#0003`)
- `contracts` — lookup by `subject` (partial match)
- `events` — lookup by `event_name` (partial match)
- `expenses` — lookup by `item_name` (partial match)
- `timelogs` — resolve `project_name` → ID, `task_name` → ID, `employee_name` → ID
- `timelogs` PUT — without `id`, finds the currently running timer automatically

---

## Permissions

| Role | Capabilities |
|---|---|
| **Admin** | Full read/write access to all tools |
| **Employee** | Read + self-service (own tasks, attendance, leave, expenses, time logs) |

Some modules (`invoices`, `estimates`, `contracts`, `events`, `expenses`, `notices`, `timelogs`) require the corresponding module to be enabled in your FlowyTeam plan.

---

## Security (Gateway)

The Gateway endpoint applies the following protections:

| Layer | Details |
|---|---|
| **Rate limiting** | 60 requests/minute per IP (throttle:60,1) |
| **Suspicious input check** | Blocks bot-like names and company names |
| **Duplicate email check** | Prevents duplicate account creation |
| **SuperAdmin notification** | Every new MCP registration triggers an email to all SuperAdmins |
| **Platform toggle** | `allow_mcp_registration` must be enabled by SuperAdmin (default: off) |

---

## Full Documentation

- **ClawhHub listing:** [clawhub.ai/agungksidik/flowyteam-mcp](https://clawhub.ai/agungksidik/flowyteam-mcp)
- **MCP Server page:** [flowyteam.com/get/mcp-server](https://flowyteam.com/get/mcp-server)
- **API Reference:** [flowyteam.com/get/mcp-docs](https://flowyteam.com/get/mcp-docs)
- **Tool parameters:** See [SKILL.md](SKILL.md)

---

## Sign Up

Don't have a FlowyTeam account yet? [Sign up free](https://flowyteam.com/register) or use the MCP Gateway with `auth_register` if enabled.

---

## License

MIT — free to use, modify, and distribute.
