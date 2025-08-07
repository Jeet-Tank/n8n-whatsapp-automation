
# WhatsApp-GoogleDrive Automation (n8n + Gemini AI)

This project integrates Twilio's WhatsApp API, Google Drive, Gemini API, and Google Sheets via n8n to create a natural-language-powered file assistant. Users can interact with their Google Drive through WhatsApp, sending commands like listing folders, moving files, or summarizing PDF content.

---

## 🔧 Setup

### 1. Twilio Sandbox for WhatsApp

1. Sign up or log into Twilio.
2. Go to the [Twilio Console](https://www.twilio.com/console/sms/whatsapp/learn).
3. Follow the steps to join the sandbox (scan QR code / send code from WhatsApp).
4. Note the Twilio SID, Auth Token, and sandbox number to use in your n8n credentials.

### 2. Google Credentials

1. Go to [Google Cloud Console](https://console.cloud.google.com/).
2. Create a new project.
3. Enable the following APIs:
   - Google Drive API
   - Google Sheets API
4. Go to “OAuth consent screen” → configure app (select "External").
5. Create **OAuth 2.0 credentials**.
6. In the **redirect URI**, paste the static URL of your n8n instance (e.g., `https://n8n.io/oauth2-redirect`).
   - You must use a static URL for production.
   - If you are testing locally or on a dynamic host, re-authentication is required each time.
7. Download the credentials JSON and use it to configure **Google Drive OAuth2 API** and **Google Sheets OAuth2 API** in n8n.

---

## 💬 Commands

You can send the following messages to the WhatsApp bot:

```
help
```
→ Displays help instructions.

```
list <foldername>
```
→ Lists contents of a folder in your root Drive.

```
move <file/folder> to <destination-folder>
```
→ Moves a file or folder to another **root-level** folder.

```
summary <file/folder>
```
→ Summarizes the content of a file (usually PDF or doc).

📝 **Note:**
- Commands are now NLP-driven (powered by Gemini API), so exact syntax is **not required**.
- File/folder **paths must not exceed two levels**. e.g., `folder/file` is allowed but `folder/folder/file` is **not**.
- The destination for `move` must be a root-level folder.

---

## 🧠 Gemini API Integration

Gemini API is used in two ways:

1. **NLP Query Understanding**: It analyzes the intent behind user messages and maps them to valid command actions.
2. **Document Summarization**: Extracted content from PDFs or text is sent to Gemini for summarization.

---

## 📄 Logging System

Move and Delete action (command) is logged into a **Google Sheet** with:

- Timestamp
- Original WhatsApp message
- Extracted intent/command
- Status: `Success` or `Failure`
- Additional info: error messages or result links

This requires:
- Google Sheets API to be enabled in your project

---

## ✅ Example Flow

1. User sends: `summarize my resume`
2. System:
   - Extracts intent using Gemini
   - Matches to `summary` command
   - Extracts content from the file
   - Sends to Gemini for summarization
   - Replies with the summary


---

## 🧪 Testing Tips

- Always test each flow manually in n8n before going live.
- If you're using a dynamic hosting environment, reauthorize Google OAuth each session.
- Use pinned test data in n8n for debugging logic branches.

---

## 📌 Requirements

- Twilio Account (WhatsApp sandbox)
- Google Cloud Project with:
  - Drive API
  - Sheets API
- Gemini API Key
- n8n hosted with webhook support (local or cloud)

---
