# Kudosity SMS Plugin for Claude Code

Send SMS, MMS, manage contact lists, and configure webhooks using the [Kudosity](https://kudosity.com) messaging platform — directly from Claude Code.

## Features

| Skill | Description |
|-------|-------------|
| `/kudosity-sms:create-list` | Create contact lists and add recipients (V1 API) |
| `/kudosity-sms:send-sms` | Send SMS to individuals or contact lists (V1 + V2 API) |
| `/kudosity-sms:send-mms` | Send multimedia messages with images, GIFs, video, or audio (V2 API) |
| `/kudosity-sms:setup-webhook` | Configure delivery status notifications, inbound messages, and more (V2 API) |

The plugin also includes a `/kudosity-sms:setup` command to walk you through credential configuration and verification.

## Prerequisites

1. **Kudosity account** — Sign up at [kudosity.com](https://kudosity.com)
2. **API credentials** — Found in your account under **Settings → API Settings**
3. **Claude Code v1.0.33+** — Run `claude --version` to check

## Installation

### From the Kudosity marketplace

```bash
/plugin marketplace add kudosity/kudosity-claude-sms-plugin
/plugin install kudosity-sms@kudosity-claude-sms-plugin
```

### For local development

```bash
git clone https://github.com/kudosity/kudosity-claude-sms-plugin.git
claude --plugin-dir ./kudosity-claude-sms-plugin
```

## Setup

### 1. Set your environment variables

Add these to your shell profile (`~/.zshrc` or `~/.bashrc`):

```bash
export KUDOSITY_API_KEY="your_v1_api_key"
export KUDOSITY_API_SECRET="your_v1_api_secret"
export KUDOSITY_V2_API_KEY="your_v2_api_key"
```

Then reload your shell:

```bash
source ~/.zshrc
```

### 2. Verify your credentials

Run the setup command in Claude Code:

```
/kudosity-sms:setup
```

This will verify both your V1 and V2 API credentials and show your account balance.

## Usage Examples

### Create a contact list

> "Create a contact list called 'March Campaign' with fields for email and postcode, then add John Doe (0491570156) and Jane Smith (0435795809)"

### Send an SMS

> "Send an SMS to 0491570156 from 61481074185 saying 'Your order has shipped!'"

> "Send an SMS to my 'March Campaign' list saying 'Sale starts tomorrow!'"

### Send an MMS

> "Send an MMS to 0435795809 with the image at https://example.com/product.jpg, subject 'New Arrival'"

### Set up a webhook

> "Set up a webhook at https://myapp.com/sms-status to receive SMS delivery status and inbound messages"

## API Coverage

| Operation | API Version | Auth Method |
|-----------|------------|-------------|
| Create list | V1 | Basic Auth |
| Add contact to list | V1 | Basic Auth |
| Send SMS (single recipient) | V2 | x-api-key |
| Send SMS (to list / multiple) | V1 | Basic Auth |
| Send MMS | V2 | x-api-key |
| Create/manage webhooks | V2 | x-api-key |

## Architecture

This plugin connects to the public Kudosity MCP server at `developers.kudosity.com/mcp`. No authentication is needed for the MCP connection itself — credentials are only required when executing API requests.

The MCP provides:
- **Documentation tools** (`list-specs`, `list-endpoints`, `get-endpoint`, `search-endpoints`) — no auth needed
- **Routing helper** (`route-kudosity-operations`) — no auth needed
- **API execution** (`execute-request`) — requires your API credentials in the request headers

## Plugin Structure

```
├── .claude-plugin/
│   ├── plugin.json              # Plugin manifest
│   └── marketplace.json         # Marketplace catalog
├── .mcp.json                    # MCP server connection
├── skills/
│   ├── create-list/SKILL.md     # Contact list management
│   ├── send-sms/SKILL.md        # SMS sending
│   ├── send-mms/SKILL.md        # MMS sending
│   └── setup-webhook/SKILL.md   # Webhook configuration
├── agents/
│   └── kudosity-assistant.md    # Specialized messaging agent
├── commands/
│   └── setup.md                 # Credential setup wizard
└── README.md
```

## Support

- **API Documentation**: [developers.kudosity.com](https://developers.kudosity.com)
- **Kudosity Support**: [kudosity.com](https://kudosity.com)

## License

MIT