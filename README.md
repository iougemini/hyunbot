

---

# Lightweight Telegram Private Message Bot (Group-Based)

## 📖 Project Overview

A two-way Telegram private message bot built on Cloudflare Workers. It allows users to contact you privately while you manage all conversations as organized **Topics** within a single Telegram Group.

---

## 🏗️ Deployment Guide

### 1. Prerequisites

| Requirement | How to Get |
| --- | --- |
| Cloudflare Account | [Sign Up](https://dash.cloudflare.com/sign-up) |
| Bot Token | Send `/newbot` to [@BotFather](https://t.me/BotFather) |
| Your User ID | Send `/start` to [@userinfobot](https://t.me/userinfobot) |
| A Topic Group | Create a Telegram Group and enable "Topics" |

### 2. Set Up the Topic Group

1. Create a new **Telegram Group**.
2. Go to **Group Settings** → **Topics** → Toggle **On**.
3. Add your bot to the group and promote it to **Administrator**.
4. **Required Permissions**: *Manage Topics, Send Messages, Delete Messages*.

### 3. Get Group ID

1. Add the bot to your group.
2. Visit: `[https://api.telegram.org/bot](https://api.telegram.org/bot)<YOUR_BOT_TOKEN>/getUpdates`
3. Look for `"chat":{"id":-100xxxxxxxxx}` in the JSON response.

### 4. Cloudflare Deployment

#### 4.1 Create KV Namespace

* Go to **Workers & Pages** → **KV** → **Create Namespace**. Name it `nfd`.

#### 4.2 Create Worker

* Create a new Worker, paste the provided code, and **Deploy**.

#### 4.3 Bind KV & Set Variables

Navigate to **Settings** → **Variables** for your Worker:

**A. KV Namespace Bindings**

* Variable name: `nfd` | KV namespace: `nfd`

**B. Environment Variables**

| Variable | Type | Description |
| --- | --- | --- |
| `BOT_TOKEN` | Secret | Your Telegram Bot Token |
| `BOT_SECRET` | Secret | Random string for Webhook security |
| `ADMIN_PATH` | Variable | **Hidden management path (e.g., `my_private_admin`)** |
| `SUPERGROUP_ID` | Variable | Your Group ID (starts with `-100`) |
| `ADMIN_UID` | Variable | Your personal Telegram User ID |

---

## 🚀 Activation (Crucial Step)

Because the management endpoints are hidden, you must visit these URLs manually to start the bot:

1. **Register Webhook**:
`https://<your-worker-domain>/<ADMIN_PATH>/registerWebhook`
2. **Initialize Admin Topic**:
`https://<your-worker-domain>/<ADMIN_PATH>/init`

---

## 👤 User Guide

1. **Start**: Search for the bot and send `/start`.
2. **Verify**: Click the **"I am human"** button (Valid for 30 days).
3. **Chat**: Send messages; they are forwarded to the admin group.
4. **Rate Limit**: Users are limited to **45 messages per minute**.

---

## 👨‍💼 Administrator Guide

### Group Structure

```text
Group Topic List:
├── 📋 Instructions (System Topic for global commands)
├── [U0001] Alice (@alice)  ← Topic for User 1
└── [U0002] Bob             ← Topic for User 2

```

### Commands

| Location | Command | Function |
| --- | --- | --- |
| **Instructions Topic** | `/reloadblock` | Refresh the remote spam filter |
|  | `/help` | View help documentation |
| **User Topic** | `/info` | View detailed user ID and status |
|  | `/block` / `/unblock` | Ban or unban the user |
|  | `/close` / `/open` | Disable/Enable user messaging |
|  | `/reset` | Force user to re-verify |
|  | `/retopic` | Delete and recreate the topic |

### Replying to Users

Simply **type a message** inside a specific User Topic. The bot automatically forwards your text, stickers, or media to that user.

---

## ⚙️ Maintenance & URLs

To maintain your bot, use your unique `ADMIN_PATH`:

* **Update Webhook**: `.../<ADMIN_PATH>/registerWebhook`
* **Stop Bot**: `.../<ADMIN_PATH>/unRegisterWebhook`
* **Reset Admin Topic**: `.../<ADMIN_PATH>/init`

---

## ❓ FAQ

**Q: Why does it say "Unauthorized" when I visit /registerWebhook?**

A: You likely didn't include your `ADMIN_PATH` in the URL. Access it via `/<your_admin_path>/registerWebhook`.

**Q: Can anyone stop my bot if they know the URL?**

A: No. By setting a complex `ADMIN_PATH` (e.g., `manage_77a2b`), the management endpoints are hidden from scanners and unauthorized users.

**Q: How do I change the welcome message?**

A: Edit the `START_MSG_ZH_URL` or `START_MSG_EN_URL` in the code to point to your own raw Markdown files on GitHub.</ADMIN_PATH></ADMIN_PATH></ADMIN_PATH></ADMIN_PATH></ADMIN_PATH></YOUR_BOT_TOKEN>
