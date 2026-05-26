# OpenWA Home Assistant Add-on

A secure, persistent wrapper for the OpenWA WhatsApp API gateway, specifically designed for Home Assistant.

## Overview

This add-on bundles the [OpenWA](https://github.com/rmyndharis/OpenWA) API with a dedicated helper server that provides simplified endpoints for Home Assistant `rest_command` integrations and a web-based status UI.

- **Native OpenWA API (Port 2785)**: Full access to the WhatsApp gateway.
- **Helper Server (Port 2786)**: Simplified API for sending messages and a status dashboard.

---

## 🚀 Deployment & Setup Guide

### 1. Install and Configure
Install the add-on and enter the following in the **Options** tab:
- `openwa_api_key`: A secret key for the native API.
- `api_master_key`: A secret key for the helper API. (Optional: If left blank, a secure random key will be generated and printed in the logs on first boot).
- `session_id`: (Optional: The add-on will automatically create a session if this is left blank).

**Restart the add-on** after saving these options.

### 2. One-Time QR Linkage
If this is your first time using the add-on:
1. Wait for the add-on to start.
2. Visit the QR page: 👉 `http://homeassistant.local:2786/qr`
3. Scan the code using **WhatsApp $\rightarrow$ Linked Devices $\rightarrow$ Link a Device**.

The add-on handles all the technical session creation and starting in the background. Once you scan the QR code, you are connected!

---

## 🛠️ Helper API Usage

All helper endpoints require the `api_master_key` in the `X-API-Key` header.

### Send a Message
**Endpoint**: `POST /send`
**Payload**: `{"chat_id": "123456789@c.us", "message": "Hello!"}`

**Example `curl`**:
```bash
curl -X POST -H "X-API-Key: [MASTER_KEY]" -H "Content-Type: application/json" -d '{"chat_id": "123456789@c.us", "message": "Hello!"}' http://[YOUR_IP]:2786/send
```

### Home Assistant `rest_command` Example
Add this to your `configuration.yaml`:
```yaml
rest_command:
  openwa_send:
    url: "http://[YOUR_IP]:2786/send"
    method: POST
    headers:
      Content-Type: "application/json"
      X-API-Key: "YOUR_MASTER_API_KEY"
    payload: '{"chat_id": "{{ chat_id }}@c.us", "message": "{{ message }}"}'
```

---

## 📖 Troubleshooting

- **Blank API Docs Page**: If `http://[YOUR_IP]:2785/api/docs` is blank, ensure you have correctly set the `openwa_api_key` in options and restarted.
- **401 Unauthorized**: Ensure you are using the correct key. `dev-admin-key` is for the native API; your `api_master_key` is for the helper server.
- **QR Code Not Showing**: Ensure you have called the `/start` endpoint for your session.
- **Session Lost on Restart**: This add-on implements full persistence. If you are asked to scan the QR code after every restart, ensure you have a valid `session_id` configured in the options.
`