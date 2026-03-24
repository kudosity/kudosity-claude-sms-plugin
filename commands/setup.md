---
name: setup
description: Configure your Kudosity API credentials and verify they work correctly.
---

# Kudosity Setup

Help the user configure their Kudosity API credentials step by step.

## Step 1: Check Prerequisites

Ask the user if they have a Kudosity account. If not, direct them to:
- Sign up at **https://kudosity.com**
- Once signed in, go to **Settings → API Settings** to find their API credentials

## Step 2: Identify Required Credentials

The user needs three values from their Kudosity account:

| Credential | Where to Find | Environment Variable |
|-----------|--------------|---------------------|
| V1 API Key | Settings → API Settings → API Key | `KUDOSITY_API_KEY` |
| V1 API Secret | Settings → API Settings → API Secret | `KUDOSITY_API_SECRET` |
| V2 API Key | Settings → API Settings → API Key | `KUDOSITY_V2_API_KEY` |

## Step 3: Guide Environment Variable Setup

Provide the user with the shell commands to set their environment variables. For **zsh** (macOS default):

```bash
echo 'export KUDOSITY_API_KEY="your_v1_api_key_here"' >> ~/.zshrc
echo 'export KUDOSITY_API_SECRET="your_v1_api_secret_here"' >> ~/.zshrc
echo 'export KUDOSITY_V2_API_KEY="your_v2_api_key_here"' >> ~/.zshrc
source ~/.zshrc
```

For **bash**:

```bash
echo 'export KUDOSITY_API_KEY="your_v1_api_key_here"' >> ~/.bashrc
echo 'export KUDOSITY_API_SECRET="your_v1_api_secret_here"' >> ~/.bashrc
echo 'export KUDOSITY_V2_API_KEY="your_v2_api_key_here"' >> ~/.bashrc
source ~/.bashrc
```

Remind the user to replace the placeholder values with their actual credentials.

## Step 4: Verify V1 Credentials

After the user has set their environment variables, verify them by calling the V1 get-balance endpoint:

Use the MCP `execute-request` tool with title `Transmit SMS API`:
```json
{
  "method": "GET",
  "url": "https://api.transmitsms.com/get-balance.json",
  "headers": [
    {"name": "Authorization", "value": "Basic {base64(KUDOSITY_API_KEY:KUDOSITY_API_SECRET)}"}
  ]
}
```

If successful, show the user their account balance and currency.
If it fails, check the error and help them troubleshoot (wrong credentials, account not activated, etc.)

## Step 5: Verify V2 Credentials

Verify the V2 API key by listing SMS messages:

Use the MCP `execute-request` tool with title `Transmit Message API`:
```json
{
  "method": "GET",
  "url": "https://api.transmitmessage.com/v2/sms",
  "headers": [
    {"name": "x-api-key", "value": "{KUDOSITY_V2_API_KEY}"}
  ]
}
```

If successful, confirm to the user that their V2 credentials are working.
If it fails, help them troubleshoot.

## Step 6: Summary

Once both credentials are verified, show a summary:

```
✅ Kudosity Plugin Setup Complete

V1 API (contacts, lists, bulk SMS): Connected
V2 API (SMS, MMS, webhooks):        Connected
Account Balance: {balance} {currency}

You can now use:
- /kudosity:create-list  — Create contact lists and add recipients
- /kudosity:send-sms     — Send SMS to individuals or lists
- /kudosity:send-mms     — Send multimedia messages
- /kudosity:setup-webhook — Configure delivery notifications