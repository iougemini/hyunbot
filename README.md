


---

# Lightweight Telegram Private Message Bot (Group-Based)

## 📖 Project Overview

A two-way Telegram private message bot built on Cloudflare Workers. Users send messages to the bot privately, and admins reply one-on-one within specific topics in a Telegram group.

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
4. Required Permissions: *Manage Topics, Send Messages, Delete Messages*.

### 3. Get Group ID

1. Add the bot to your group.
2. Visit the following URL in your browser:
`[https://api.telegram.org/bot](https://api.telegram.org/bot)<YOUR_BOT_TOKEN>/getUpdates`
3. Look for `"chat":{"id":-100xxxxxxxxx}` in the JSON response.

### 4. Cloudflare Deployment

#### 4.1 Create KV Namespace

* Go to **Workers & Pages** → **KV** → **Create Namespace**.
* Name it: `nfd`.

#### 4.2 Create Worker

* Go to **Workers & Pages** → **Create** → **Create Worker**.
* Give it a name, paste the code, and click **Deploy**.

#### 4.3 Bind KV

* Navigate to **Worker Settings** → **Variables** → **KV Namespace Bindings**.
* Variable name: `nfd`
* Namespace: `nfd`

#### 4.4 Set Environment Variables

* Navigate to **Worker Settings** → **Variables** → **Environment Variables**.

| Variable | Description |
| --- | --- |
| `BOT_TOKEN` | Your Telegram Bot Token |
| `BOT_SECRET` | Custom Secret (Alphanumeric, e.g., `tsaihyun`) |
| `SUPERGROUP_ID` | Your Group ID (starts with `-100`) |
| `ADMIN_UID` | Your personal Telegram User ID |

#### 4.5 Register Webhook

* Visit the following URL in your browser:
`https://<your-worker-domain>/registerWebhook`
* Success if you see: `{"ok":true,"result":true}`

---

## 👤 User Guide

### Getting Started

1. Search for the bot and send `/start`.
2. View the welcome message.
3. Click the **Verification Button** to complete the process.
4. Send your message; the admin will reply shortly.

### Important Notes

* Verification is required for the first message.
* Verification is valid for **30 days**.
* Rate limit: **45 messages per minute**.
* You cannot send messages if you are blocked.

---

## 👨‍💼 Administrator Guide

### Group Structure

```text
Group Topic List:
├── 📋 Instructions (System Topic)
├── [U0001] User A  ← Chat with User 1
├── [U0002] User B  ← Chat with User 2

```

### Global Commands (Execute in 📋 Instructions topic)

| Command | Function |
| --- | --- |
| `/reloadblock` | Refresh the remote sensitive word filter |
| `/help` | View help documentation |

### User-Specific Commands (Execute within a User's topic)

| Command | Function |
| --- | --- |
| `/close` | Close the chat (User cannot send messages) |
| `/open` | Reopen the chat |
| `/block` | Ban the user |
| `/unblock` | Unban the user |
| `/reset` | Reset user's verification status |
| `/retopic` | Recreate the user's topic |
| `/info` | View detailed user information |

### Replying to Users

Simply enter the specific **User Topic** and send a message. The bot will automatically forward it to the user.

---

## ⚙️ Configuration Details

### Welcome Messages

Update the code with your own message URLs:

* `START_MSG_ZH_URL`: Chinese welcome message URL.
* `START_MSG_EN_URL`: English welcome message URL.

### Sensitive Word Filter

Modify the following URL in the code:

* `DEFAULT_BLOCKLIST_URL`: Your wordlist URL.
* **Format:** One word per line (e.g., `spam`, `scam`, `ad`).

### Expiration & Intervals

* `VERIFIED_EXPIRE_SECONDS`: Default is `2592000` (30 days).
* `NOTIFY_INTERVAL`: Default is `3600000` (1 hour).

---

## 🔄 Maintenance

| Task | Action |
| --- | --- |
| Re-register Webhook | Visit `/registerWebhook` |
| Unregister Webhook | Visit `/unRegisterWebhook` |
| Initialize Topics | Visit `/init` |
| Check Logs | Cloudflare → Worker → Logs |
| Update Code | Edit Worker Code → Deploy |

---

## ❓ FAQ

**Q: Admin commands are not working?**
Global commands (`/help`) must be sent in the **Instructions** topic. User commands (`/block`) must be sent inside the specific **User Topic**.

**Q: Bot is not responding?**
Re-register the Webhook and double-check your environment variables (especially the `BOT_TOKEN`).

**Q: How do I add more admins?**
Update the `ADMIN_UID` in environment variables or manually modify the `admin-list` key in the KV namespace.

**Q: User messages are being blocked?**
Check your remote wordlist or run `/reloadblock` in the Instructions topic to refresh the filter.

---

## 
