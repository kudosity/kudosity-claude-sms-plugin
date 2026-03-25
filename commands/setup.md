---
name: setup
description: Configure your Kudosity API credentials and verify they work correctly.
---

# Kudosity Setup

Walk the user through configuring their Kudosity API credentials interactively. The goal is to collect their credentials, save them to their shell profile, and verify they work — all without the user needing to manually edit files.

## Step 1: Check Prerequisites

Ask the user if they have a Kudosity account. If not, direct them to:
- Sign up for your free account at **https://kudosity.com/signup**
- Once signed in, go to **Developers → API Settings** to find their API credentials

Show this table so they know what to look for:

| Credential | Where to Find | Environment Variable |
|-----------|--------------|---------------------|
| API Key | Developers → API Settings → API Key | `KUDOSITY_API_KEY` |
| API Secret | Developers → API Settings → API Secret | `KUDOSITY_API_SECRET` |

Explain how the credentials are used:
- **V1 API** (contacts, lists, bulk SMS): Uses Basic Auth with base64-encoded `API_KEY:API_SECRET`
- **V2 API** (single SMS, MMS, webhooks): Uses `x-api-key` header with just the API Key

## Step 2: Collect Credentials Interactively

Before asking for credentials, display this security notice:

> 🔒 **Your credentials are stored securely.** They will be saved as environment variables in your local shell profile (`~/.zshrc`) — they are never sent to Claude or stored in the cloud. They are only used locally on your machine when making API requests to Kudosity.

Ask the user for each value one at a time:

1. "What is your **API Key**?" (from Developers → API Settings → API Key)
2. "What is your **API Secret**?" (from Developers → API Settings → API Secret)

Store the values they provide for use in the next steps.

## Step 3: Auto-Save to Shell Profile

Detect the user's shell and determine the profile file:
- If `$SHELL` contains `zsh` → use `~/.zshrc`
- If `$SHELL` contains `bash` → use `~/.bashrc`
- Otherwise default to `~/.zshrc`

For each credential, check if it already exists in the profile file:
- If the line `export KUDOSITY_API_KEY=` already exists, **replace** the entire line using `sed`
- If it doesn't exist, **append** it

Execute these commands (replacing `{value}` with the actual user-provided values):

```bash
# For each variable, check and update or append
grep -q 'export KUDOSITY_API_KEY=' ~/.zshrc && \
  sed -i '' 's|^export KUDOSITY_API_KEY=.*|export KUDOSITY_API_KEY="{value}"|' ~/.zshrc || \
  echo 'export KUDOSITY_API_KEY="{value}"' >> ~/.zshrc

grep -q 'export KUDOSITY_API_SECRET=' ~/.zshrc && \
  sed -i '' 's|^export KUDOSITY_API_SECRET=.*|export KUDOSITY_API_SECRET="{value}"|' ~/.zshrc || \
  echo 'export KUDOSITY_API_SECRET="{value}"' >> ~/.zshrc

source ~/.zshrc
```

After executing, confirm to the user that credentials have been saved to their shell profile.

## Step 4: Verify V1 Credentials

Verify the V1 credentials by calling the get-balance endpoint.

Execute this curl command:
```bash
curl -s "https://api.transmitsms.com/get-balance.json" \
  -u "${KUDOSITY_API_KEY}:${KUDOSITY_API_SECRET}"
```

If successful, show the user their account balance and currency.
If it fails, check the error and help them troubleshoot (wrong credentials, account not activated, etc.)

## Step 5: Verify V2 Credentials

Verify the V2 API access by listing SMS messages. The V2 API uses the same API Key (not the secret).

Execute this curl command:

```bash
curl -s -w "\n%{http_code}" "https://api.transmitmessage.com/v2/sms?limit=1" \
  -H "x-api-key: ${KUDOSITY_API_KEY}"
```

If the HTTP status code is 200, confirm to the user that their V2 credentials are working.
If it returns 401 or 403, the API key is incorrect — help them troubleshoot.

## Step 6: Summary

Once both credentials are verified, show a summary:

```
✅ Kudosity Plugin Setup Complete

V1 API (contacts, lists, bulk SMS): Connected
V2 API (SMS, MMS, webhooks):        Connected
Account Balance: {balance} {currency}

Credentials saved to: ~/.zshrc

You can now use:
- /kudosity-sms:create-list      — Create contact lists and add recipients
- /kudosity-sms:send-sms         — Send SMS to individuals or lists
- /kudosity-sms:send-mms         — Send multimedia messages
- /kudosity-sms:setup-webhook    — Configure delivery notifications
