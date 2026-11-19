# Setup Guide — HULLO AI Reels (₹0, no laptop needed to run)

Total time: ~60–90 min, once. Keep a **Notepad / Notes file open** and paste every key you collect there, with its name.
Never share that note or post keys in screenshots.

```
GEMINI_API_KEY      = 
GEMINI_MODEL        = 
ACCOUNT_NAME        = 
IG_HANDLE           = 
AZURE_TTS_KEY       = 
AZURE_TTS_REGION    = 
PEXELS_API_KEY      = 
TELEGRAM_BOT_TOKEN  = 
TELEGRAM_CHAT_ID    = 
SHEET_URL           = 
SHEET_TOKEN         = 
FTP_HOST            = 
FTP_USER            = 
FTP_PASS            = 
FTP_DIR             = /
PUBLIC_BASE_URL     = 
```

---

## PART 1 — Collect keys (≈40 min)

### 1. Gemini (AI brain)
1. Open **aistudio.google.com** → sign in with Google.
2. Click **Get API key** → **Create API key** → copy → `GEMINI_API_KEY`.
3. In AI Studio's model list, pick the newest free **Flash** model. Copy its exact model ID (looks like `gemini-x.x-flash`) → `GEMINI_MODEL`.

### 2. Account name
- `ACCOUNT_NAME` = `HULLO AI`
- `IG_HANDLE` = your Instagram handle with @ (`@hulloai`). Shown on the "Follow" end card.

### 3. Azure (Malayalam voice — free tier)
1. Open **azure.microsoft.com/free** → create a free account (card is for verification only).
2. In **portal.azure.com**, search **"Speech service"** → **Create**.
3. Fill: Resource group → **Create new** → `reels` · Region → **East US** · Name → `reels-voice` · Pricing tier → **Free F0** → **Review + create** → **Create**.
4. Open the resource → **Keys and Endpoint** → copy **KEY 1** → `AZURE_TTS_KEY`, and **Location/Region** (e.g. `eastus`) → `AZURE_TTS_REGION`.
5. Safety: search **"Cost Management"** → **Budgets** → add a budget of ₹100 with an email alert.

### 4. Pexels (free stock video)
1. Open **pexels.com/api** → sign up → request API key (fill "personal project").
2. Copy the key → `PEXELS_API_KEY`.

### 5. Telegram bot
1. In Telegram, search **@BotFather** → send `/newbot` → give a name and a username ending in `bot`.
2. Copy the token → `TELEGRAM_BOT_TOKEN`.
3. Open your new bot → press **Start** → send `hi`.
4. In a browser open: `https://api.telegram.org/bot<TOKEN>/getUpdates` (replace `<TOKEN>`).
5. Find `"chat":{"id":123456789` → that number → `TELEGRAM_CHAT_ID`.
   (Do this now — after Part 4 this page stops showing messages. That's normal.)

### 6. Google Sheet (log + memory)
1. Open **sheets.new** → name it `AI Reels Log`.
2. Menu **Extensions → Apps Script**. Delete everything in the editor.
3. Paste the full contents of `extras/apps-script.gs` (from the zip).
4. In line 5, replace `CHANGE_ME_TO_A_LONG_RANDOM_PASSWORD` with your own random text (20+ letters/numbers, no spaces) → also put it in `SHEET_TOKEN`.
5. Click 💾 **Save**.
6. **Deploy → New deployment** → ⚙️ **Select type → Web app**. Execute as: **Me** · Who has access: **Anyone** → **Deploy**.
7. Google asks permission → **Authorize access** → pick your account → **Advanced → Go to project (unsafe)** → **Allow**. (It's your own script; "unsafe" just means Google hasn't reviewed it.)
8. Copy the **Web app URL** → `SHEET_URL`.
9. Test: paste that URL in a new tab → you should see `{"ok":true,"msg":"alive"}`.

### 7. Hostinger (temporary video parking)
Instagram needs a public link to download each video. Videos are deleted automatically after posting.
1. hPanel → **Websites → theuncoder.com → Files → File Manager** → open `public_html` → **New folder** `reels`.
2. hPanel → **Files → FTP Accounts** → **Create FTP account**:
   Directory → `public_html/reels` · set a username + strong password → **Create**.
3. Copy from the FTP details on that page: **FTP IP / Hostname** → `FTP_HOST`, the **full username** shown (often has a prefix like `u123456789.`) → `FTP_USER`, password → `FTP_PASS`.
4. `FTP_DIR` = `/` · `PUBLIC_BASE_URL` = `https://theuncoder.com/reels`

---

## PART 2 — GitHub repo (≈15 min)

### 8. Create the repo
1. On **github.com** → top-right **+** → **New repository**.
2. Name: `malayalam-ai-reels` · Select **Public** · tick **Add a README file** → **Create repository**.
   (Public = unlimited free running time. Your keys stay private in Secrets.)

### 9. Upload the files
1. Unzip `malayalam-ai-reels.zip` on your computer.
2. In the repo click **Add file → Upload files**.
3. Open the unzipped folder, press **Ctrl+A** (Mac: Cmd+A) to select **everything inside** (including the `.github` folder), drag it into the GitHub page.
   - Mac users: the `.github` folder is hidden — press **Cmd+Shift+.** in Finder to show it.
4. Click **Commit changes**.
5. Check: the repo must now show a `.github` folder. Click it → `workflows` → you should see `build.yml` and `publish.yml`.
   **If `.github` is missing:** click **Add file → Create new file**, type the name `.github/workflows/build.yml`, paste the contents of that file from the zip, **Commit**. Repeat for `.github/workflows/publish.yml`.

### 10. Add the secrets
1. Repo → **Settings** → left menu **Secrets and variables → Actions** → **New repository secret**.
2. Add each line from your note: **Name** exactly as written (capital letters), **Secret** = the value → **Add secret**. Repeat for all 16.
3. Voice is male by default (`ml-IN-MidhunNeural`). Optional: `TTS_RATE` = `+0%` (slower) or `+10%` (faster); default `+5%`.

### 11. First test run 🎬
1. Repo → **Actions** tab → if asked, click **I understand my workflows, go ahead and enable them**.
2. Left side → **Build Daily Reel** → **Run workflow** → **Run workflow**.
3. Wait 6–10 min. A green ✅ = success → the video + caption arrive on Telegram with **✅ Post / 🔁 New story / ⏭️ Skip** buttons.
4. A red ❌ = you also get a Telegram message with the reason + a link to the log. Send me that message.
   The buttons won't do anything until Part 3.

---

## PART 3 — Approval buttons (≈15 min)

### 12. GitHub access key for the Worker
1. github.com → your photo (top-right) → **Settings** → bottom-left **Developer settings** → **Personal access tokens → Fine-grained tokens** → **Generate new token**.
2. Name `reels-worker` · Expiration **1 year** (set a calendar reminder) · Repository access → **Only select repositories** → `malayalam-ai-reels`.
3. **Permissions → Repository permissions → Actions → Read and write**.
4. **Generate token** → copy → keep as `GH_PAT`.

### 13. Cloudflare Worker
1. Sign up at **dash.cloudflare.com** (free, no card).
2. Left menu **Workers & Pages** → **Create** → **Create Worker** (Hello World) → name `reels-approve` → **Deploy**.
3. Click **Edit code** → delete everything → paste the full `extras/cloudflare-worker.js` → **Deploy**.
4. Copy your Worker URL (looks like `https://reels-approve.yourname.workers.dev`).
5. Back on the Worker → **Settings → Variables and Secrets** → **Add** each as type **Secret**:
   - `TG_TOKEN` = your bot token
   - `TG_CHAT_ID` = your chat id
   - `TG_SECRET` = any new random text (letters/numbers only, e.g. `reelsKey8x2m9q`)
   - `GH_REPO` = `yourGitHubUsername/malayalam-ai-reels`
   - `GH_PAT` = the token from step 12
   - `SHEET_URL` and `SHEET_TOKEN` = same as before
   → **Deploy**.

### 14. Connect Telegram → Worker
Open this in your browser (replace the 3 parts):
```
https://api.telegram.org/bot<TOKEN>/setWebhook?url=<WORKER_URL>&secret_token=<TG_SECRET>&allowed_updates=["callback_query"]
```
You should see `"Webhook was set"`.

### 15. Test the buttons
On the test preview in Telegram, tap **⏭️ Skip** → you should get "⏭️ Skipped". ✅
(Don't tap ✅ Post yet — Instagram is connected in Stage 4.)

---

## How it runs every day
5:00 PM IST → GitHub builds the reel → Telegram preview → you tap:
- **✅ Post** → live on Instagram in 2–5 min, logged in the Sheet
- **🔁 New story** → rebuilds on a different story
- **⏭️ Skip** → nothing posted

Change the time: edit `.github/workflows/build.yml` → `cron: '30 11 * * *'` is in UTC (IST − 5:30).
Optional sound: upload `music.mp3` (royalty-free) and/or `whoosh.mp3` into the `public` folder of the repo.
