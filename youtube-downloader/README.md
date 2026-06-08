# YouTube Scheduled Download — Android (YouTube Premium)

Automatically taps the **Save offline** button inside the YouTube app on your Android phone at **6:40 AM**, downloading the 5 most recent videos from any channel — using your YouTube Premium account for full quality.

---

## What you need

| Requirement | Notes |
|-------------|-------|
| Android phone | Any version ≥ 8.0 |
| YouTube app with Premium | Already signed in |
| **Tasker** | $3.49 on Play Store — one-time purchase. Also available free with Google Play Pass. |
| Phone left on / charging | Must be on at 6:40 AM with internet |

---

## Setup (one time, ~5 minutes)

### 1. Install Tasker
Download **Tasker** from the Google Play Store.

### 2. Grant Tasker the Accessibility Service
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

### 5. Set your channel URL
Tap the task **"Download Latest 5 Videos"** → tap **Action [0]** (Variable Set) → change `@CHANNEL_NAME_HERE` to your actual channel handle.

Examples:
```
https://www.youtube.com/@MrBeast/videos
https://www.youtube.com/@CNN/videos
https://www.youtube.com/@CHANNEL_HANDLE/videos
```

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
| `tasker-youtube-download.xml` | Import this into Tasker |
| `README.md` | This setup guide |
