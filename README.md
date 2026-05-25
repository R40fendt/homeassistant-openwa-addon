# OpenWA Add-on

Run a self-hosted OpenWA WhatsApp API gateway inside Home Assistant.

This add-on exposes:

- OpenWA native API on port `2785`
- Helper API and status UI on port `2786`

## First start

1. Install and start the add-on.
2. Open the add-on web UI.
3. Check that OpenWA health is OK.
4. Open OpenWA API docs at `/api/docs` on port `2785`.
5. Create/start a WhatsApp session.
6. Scan the QR code.
7. Copy the generated OpenWA API key and session ID into the add-on options.
8. Restart the add-on.

## Helper endpoints

All helper endpoints require the `api_master_key` to be passed in the `X-API-Key` header.

- `GET /`
- `GET /health`
- `GET /sessions`
- `GET /qr`
- `POST /send`
- `POST /send/primary`
- `POST /send/secondary`