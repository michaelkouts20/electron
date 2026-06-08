# YouTube Scheduled Download — Android (YouTube Premium)

Automatically taps the **Save offline** button inside the YouTube app on your Android phone at **6:40 AM**, downloading the 5 most recent videos from any channel — using your YouTube Premium account for full quality.

---

## Options

| App | Cost | File to use |
|-----|------|-------------|
| **MacroDroid** | Free (5 macros limit, enough for this) | Manual setup — see below |
| **Tasker** | $3.49 one-time (or free with Google Play Pass) | `tasker-youtube-download.xml` — import and done |

Both need the **Accessibility Service** enabled so they can tap buttons inside other apps.

---

## Option A — MacroDroid (Free)

### 1. Install & grant permission
Install **MacroDroid** from Play Store → open it → go to **MacroDroid Settings → Accessibility** → enable MacroDroid.

### 2. Create the macro
Tap **+** (Add Macro) → name it `YouTube Morning Download`.

### 3. Add the trigger
Tap **Triggers +** → **Timer** → **Specific time** → set **06:40** → tick **Every day** → OK.

### 4. Add actions

Tap **Actions +** for each action below, in order.

**Open YouTube to your channel:**
> Applications → Open URL → enter your channel URL:
> `https://www.youtube.com/@thegametimehighlights/videos`
> Open with: YouTube

**Wait for it to load:**
> Control Flow → Wait → **6 seconds**

Then repeat this block **5 times** (once per video), changing only the **Instance** number each time (0, 1, 2, 3, 4):

---

**── Video 1 (Instance: 0) ──**

> Applications → Interact with UI → Find by: **Content Description** → Text: `Watch video` → Instance: **0** → Action: Click

> Control Flow → Wait → **5 seconds**

> Applications → Interact with UI → Find by: **Content Description** → Text: `Save video` → Instance: 0 → Action: Click

> Control Flow → Wait → **3 seconds**

> Applications → Interact with UI → Find by: **Text** → Text: `Full HD` → Instance: 0 → Action: Click

> Control Flow → Wait → **3 seconds**

> Device Actions → Press Back

> Control Flow → Wait → **4 seconds**

---

**── Video 2 (Instance: 1) ──**

Repeat the same 7 actions above but change the first "Watch video" tap to Instance: **1**.

**── Video 3 → Instance: 2 ── Video 4 → Instance: 3 ── Video 5 → Instance: 4 ──**

Same pattern, incrementing the instance number each time. The Save/Full HD/Back steps keep instance 0 — only the "Watch video" tap changes.

---

**Finish:**
> Notifications → Popup notification → Message: `YouTube videos saved!`

Tap the **tick ✓** to save the macro and make sure it's **enabled** (toggle on).

### 5. Disable battery optimization
Settings → Battery → App battery usage → MacroDroid → **Unrestricted**

### 6. Test it now
Open the macro → tap the **▶ Run** button. YouTube should open and start saving the first video automatically.

---

## What you need (both options)

---

| Requirement | Notes |
|-------------|-------|
| Android phone | Any version ≥ 8.0 |
| YouTube app with Premium | Already signed in |
| Phone left on / charging | Must be on at 6:40 AM with internet |

---

## Option B — Tasker ($3.49, importable file included)

### 1. Install Tasker
Download **Tasker** from the Google Play Store.

### 2. Grant Accessibility Service
This is what allows Tasker to tap buttons inside other apps (like YouTube's Save button).

> Settings → Accessibility → Downloaded apps → Tasker → Enable

### 3. Download the automation file
From this repository, download **`tasker-youtube-download.xml`** to your phone's Downloads folder.

On your phone: tap the file link on GitHub → tap the download icon → save to Downloads.

### 4. Import into Tasker
Open Tasker → tap the **≡** (three dots) top-right → **Data** → **Restore** → **Local Backup** → pick `tasker-youtube-download.xml`.

You should now see:
- A **Profile** called "YouTube 06:40 Download"
- A **Task** called "Download Latest 5 Videos"

### 5. Channel URL — already set
The channel `@thegametimehighlights` is pre-configured in the file. No edits needed after import.

### 6. Enable the profile
Back on the main Tasker screen, make sure the **"YouTube 06:40 Download"** profile has its checkbox ticked (green).

---

## How it works

At 6:40 AM, Tasker:
1. Fetches the channel's public RSS feed to get the 5 newest video IDs (no login needed for this step — it's a public feed)
2. Opens each video directly in the YouTube app
3. Taps the **Save** button (the download icon in the action row under the video)
4. Selects **Full HD** quality
5. Goes back and repeats for the next video
6. Shows a notification when all 5 are queued

The downloads appear in YouTube → Library → Downloads — exactly where they'd be if you tapped Save manually.

---

## Test before 6:40 AM

To verify it works without waiting until morning:

1. In Tasker, long-press the task **"Download Latest 5 Videos"**
2. Tap **Run** (play icon)
3. Watch your phone — YouTube should open and start saving videos automatically

If it works manually, it will work at 6:40 AM.

---

## Troubleshooting

### "Save video" button not found
YouTube occasionally changes the content description (accessibility label) of the download button. If Tasker can't find it:

1. Open the task → tap **Action [10]**
2. Change the text from `Save video` to one of:
   - `Download video`
   - `Save offline`
   - `Download`
3. To find the exact label on your device: enable **Accessibility** → **TalkBack**, hover over the save button, and it will read out the label. Then disable TalkBack.

### "Full HD" option not shown
Some videos are only available in lower quality. If Tasker can't find "Full HD", it will time out and the video will download at whatever quality YouTube selects by default. This is fine.

### Channel ID not found
If the JavaScript step can't find the channel ID from your URL, open the task → tap **Action [1]** (the JavaScript) and verify `%CHANNEL_URL` is set to a valid YouTube channel URL with `/videos` at the end.

### Profile not running at 6:40 AM
- Make sure the profile is **enabled** (checkbox ticked in Tasker)
- Disable battery optimization for Tasker: Settings → Battery → App battery usage → Tasker → Unrestricted
- Check that your phone is not in airplane mode at 6:40 AM
- Keep the phone plugged in so Android doesn't kill background tasks

---

## Changing the time

To change from 6:40 AM to a different time:
1. In Tasker, tap the **"YouTube 06:40 Download"** profile
2. Tap the **Time** trigger
3. Change the hour and minute

---

## Changing the number of videos

To download more or fewer than 5 videos, edit the JavaScript in **Action [1]** and change:
```javascript
while ((m = pattern.exec(rss)) !== null && videoIds.length < 5) {
```
Replace `5` with your desired number.

---

## Files

| File | Purpose |
|------|---------|
| `tasker-youtube-download.xml` | Import this into Tasker (Option B only) |
| `README.md` | This setup guide |
