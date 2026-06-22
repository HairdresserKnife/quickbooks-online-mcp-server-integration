<div align="center">

# QuickBooks Online MCP Server

A local Model Context Protocol (MCP) server that exposes QuickBooks Online data and actions as callable tools for MCP-compatible AI assistants.

</div>

## 📌 Overview

QuickBooks Online MCP Server provides a standard MCP interface for working with QuickBooks Online data. It runs locally as a stdio subprocess and authenticates to a QuickBooks Online company through Intuit OAuth 2.0.

The server is designed for AI-assisted workflows where an MCP-compatible client needs to read or manage QuickBooks Online entities, search accounting records, or retrieve financial reports through structured tools.

## ✨ Features

- MCP server for QuickBooks Online
- OAuth 2.0 authentication with Intuit
- Support for sandbox and production QuickBooks Online environments
- Tools for QuickBooks entities such as customers, invoices, bills, vendors, payments, accounts, items, and more
- Financial report tools including Balance Sheet, Profit and Loss, Cash Flow, Trial Balance, and General Ledger
- Optional environment flags to disable write, update, or delete tool categories
- TypeScript implementation with Zod validation
- Jest test suite referenced by the upstream documentation

## 🧰 Tool Coverage

The server exposes QuickBooks Online functionality through MCP tools.

### Entities

Supported entity categories include:

- Customer
- Invoice
- Estimate
- Bill
- Vendor
- Employee
- Account
- Item
- Journal Entry
- Bill Payment
- Purchase
- Payment
- Sales Receipt
- Credit Memo
- Refund Receipt
- Purchase Order
- Vendor Credit
- Deposit
- Transfer
- Time Activity
- Class
- Department
- Term
- Payment Method
- Tax Code
- Tax Rate
- Tax Agency
- Company Info
- Attachable

Most entities support create, get, update, delete, and search operations where QuickBooks Online supports those actions. Some entities are read-only or do not support deletion.

### Reports

Available report tools include:

| Report | Tool |
|---|---|
| Balance Sheet | `get_balance_sheet` |
| Profit and Loss | `get_profit_and_loss` |
| Cash Flow | `get_cash_flow` |
| Trial Balance | `get_trial_balance` |
| General Ledger | `get_general_ledger` |
| Customer Sales | `get_customer_sales` |
| Aged Receivables | `get_aged_receivables` |
| Aged Receivables Detail | `get_aged_receivables_detail` |
| Customer Balance | `get_customer_balance` |
| Aged Payables | `get_aged_payables` |
| Vendor Expenses | `get_vendor_expenses` |

## 🔐 Authentication

This server uses Intuit OAuth 2.0. To connect it to QuickBooks Online, you need an app in the Intuit Developer Portal and a completed OAuth authorization flow.

Required environment values include:

```env
QUICKBOOKS_CLIENT_ID=your_client_id
QUICKBOOKS_CLIENT_SECRET=your_client_secret
QUICKBOOKS_ENVIRONMENT=sandbox
QUICKBOOKS_REFRESH_TOKEN=your_refresh_token
QUICKBOOKS_REALM_ID=your_realm_id
```

Optional flags can restrict registered tool categories:

```env
QUICKBOOKS_DISABLE_WRITE=true
QUICKBOOKS_DISABLE_UPDATE=true
QUICKBOOKS_DISABLE_DELETE=true
```

Read tools such as `get_*` and `search_*` remain available.

### Sandbox and production notes

- Sandbox usage can use a local callback such as `http://localhost:8000/callback`.
- Production OAuth setup requires a public HTTPS redirect URI for the initial authorization.
- After the initial OAuth handshake, the server runs locally using the stored refresh token until reauthorization is required.

## ⚙️ Basic Setup

```bash
git clone https://github.com/your-username/mcp-quickbooks-online.git
cd mcp-quickbooks-online
npm install
npm run build
```

Create a `.env` file from the provided environment template and fill in the QuickBooks Online OAuth values.

```bash
cp .env.example .env
```

To run the OAuth helper described by the upstream documentation:

```bash
npm run auth
```

## 🤖 MCP Client Configuration

Example MCP configuration:

```json
{
  "mcpServers": {
    "quickbooks": {
      "command": "node",
      "args": ["path/to/mcp-quickbooks-online/dist/index.js"],
      "env": {
        "QUICKBOOKS_CLIENT_ID": "your_client_id",
        "QUICKBOOKS_CLIENT_SECRET": "your_client_secret",
        "QUICKBOOKS_REFRESH_TOKEN": "your_refresh_token",
        "QUICKBOOKS_REALM_ID": "your_realm_id",
        "QUICKBOOKS_ENVIRONMENT": "sandbox",
        "QUICKBOOKS_DISABLE_WRITE": "false",
        "QUICKBOOKS_DISABLE_UPDATE": "false",
        "QUICKBOOKS_DISABLE_DELETE": "false"
      }
    }
  }
}
```

## 🧪 Development

Build the project:

```bash
npm run build
```

Run tests:

```bash
npm test
```

The upstream README references a Jest test suite and TypeScript implementation.

## 🧭 Tool Naming

Tool names follow a `{verb}_{entity}` convention.

| Prefix | Category | Restriction flag |
|---|---|---|
| `create_` | Write | `QUICKBOOKS_DISABLE_WRITE=true` |
| `update_` | Update | `QUICKBOOKS_DISABLE_UPDATE=true` |
| `delete_` | Delete | `QUICKBOOKS_DISABLE_DELETE=true` |
| `get_`, `search_`, `read_` | Read | Always available |

## ❓ FAQ

### What is this project used for?

It provides an MCP interface for QuickBooks Online so AI assistants and other MCP-compatible clients can call QuickBooks-related tools in a structured way.

### Does it run as a hosted service?

No. The documented behavior is for a local MCP server that runs as a stdio subprocess on the user’s machine.

### Is OAuth required?

Yes. QuickBooks Online access requires Intuit OAuth 2.0 through an app created in the Intuit Developer Portal.

### Can write operations be disabled?

Yes. Environment flags can suppress create, update, and delete tools while leaving read tools available.

### Does it support reports?

Yes. The documented report tools include Balance Sheet, Profit and Loss, Cash Flow, Trial Balance, General Ledger, customer reports, receivables reports, payables reports, and vendor expenses.

## 📄 License

MIT License. See `LICENSE` for details.

## 🙏 Acknowledgments

This project is based on Intuit’s QuickBooks Online MCP Server and uses the Model Context Protocol.
