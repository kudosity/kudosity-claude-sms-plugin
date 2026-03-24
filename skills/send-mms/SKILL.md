---
name: send-mms
description: Send MMS (multimedia) messages via the Kudosity platform. Use when the user wants to send a message with images, GIFs, videos, or audio to a recipient.
---

# Send MMS Messages

MMS messages are sent via the Kudosity V2 API and support images, GIFs, videos, and audio attachments.

## Authentication

- Header: `x-api-key: {KUDOSITY_V2_API_KEY}`
- If the user hasn't configured credentials, direct them to run `/kudosity:setup`

## API Details

- **Endpoint**: `POST https://api.transmitmessage.com/v2/mms`
- **Content-Type**: `application/json`
- Use the MCP `execute-request` tool with title `Transmit Message API`

## Parameters

Required:
- `sender` (string): The sender number (must be assigned to the account)
- `recipient` (string): Destination number (local or E.164 international format)
- `content_urls` (array of strings): URLs to the media files to attach

Optional:
- `subject` (string): Message subject — max 20 characters, ASCII only
- `message` (string): Body text — max 1,000 characters. Use `[opt-out-link]` for opt-out if required
- `message_ref` (string, max 500 chars): Your reference ID, returned in webhooks
- `track_links` (boolean): Enable link tracking with shortened URLs

## Example HAR Request

```json
{
  "method": "POST",
  "url": "https://api.transmitmessage.com/v2/mms",
  "headers": [
    {"name": "x-api-key", "value": "{KUDOSITY_V2_API_KEY}"},
    {"name": "Content-Type", "value": "application/json"}
  ],
  "postData": {
    "mimeType": "application/json",
    "text": "{\"subject\": \"New Arrival\", \"message\": \"Check out our latest product!\", \"sender\": \"61481074185\", \"recipient\": \"61435795809\", \"content_urls\": [\"https://example.com/product.jpg\"]}"
  }
}
```

## Supported Media

| Format | Type | Limit |
|--------|------|-------|
| JPG | Image | Up to 400 KB |
| GIF | Image | Up to 400 KB |
| PNG | Image | Up to 400 KB |
| MP3 | Audio | Up to 400 KB |
| MP4 | Video | Up to 400 KB |

## Important Notes

- **Only one image can be attached at a time**, up to 400 KB
- Media URLs **must be absolute paths** (e.g., `https://example.com/image.jpg`), not relative paths
- **Currently MMS can only be sent to Australia**
- The `subject` field is limited to 20 characters and ASCII characters only
- Always ask the user which sender number to use if not specified
- The sender must be a number assigned to their account