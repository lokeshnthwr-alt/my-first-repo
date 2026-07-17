# WhatsApp Scheduler

Schedule a text, photo, or video for a contact, at a chosen date and time.
When that moment arrives — even if your phone is locked — it wakes the
screen and shows the message fully prepared inside a WhatsApp chat. You
press **Send** once. That last tap can't be automated by any app (WhatsApp
itself blocks it, for everyone's protection against spam), but everything
before it is 100% automatic.

## What actually happens at trigger time
1. An exact alarm fires (works even if the app is closed, phone is idle, screen is off).
2. A full-screen alert appears over the lock screen showing who it's for and a preview.
3. You tap **"Open WhatsApp & Send"** → WhatsApp opens directly on that contact's
   chat with your text/photo/video already attached.
4. You tap WhatsApp's own Send arrow. Done.

This uses only official Android APIs (AlarmManager, Notifications, standard
Share/View intents) — no Accessibility Service abuse, no automated tapping.
That matters because Play Store actively rejects and removes apps that fake
auto-clicking into other apps; this one doesn't do that, so it's a legitimate
submission.

## Fastest way to get an installable .apk (no computer setup needed)
This project includes a GitHub Actions workflow that compiles the app in the
cloud and hands you a ready-to-install `.apk`. Takes about 5 minutes, free,
no Android Studio required.

1. Go to https://github.com and sign in (create a free account if needed).
2. Click **New repository**, name it anything (e.g. `whatsapp-scheduler`), click **Create repository**.
3. On the new repo's page: **Add file → Upload files**, then drag in the
   *entire contents* of this unzipped folder — including the hidden
   `.github` folder (turn on "show hidden files" in your file browser if
   you don't see it) — and commit.
4. Click the **Actions** tab. A run called "Build debug APK" starts
   automatically (or click **Run workflow** if it doesn't). Wait for the green checkmark (~3-5 min).
5. Open that finished run → **Artifacts** → download `WhatsAppScheduler-debug-apk`.
   It's a `.zip` containing `app-debug.apk` inside.
6. Get that `.apk` onto your phone any way you like (email to yourself,
   Google Drive, USB cable) and tap it to install. Android will warn
   "install from unknown sources" the first time — normal for any app not
   yet on the Play Store; allow it just for this file.

This debug build is fully functional for trying the app. Play Store
requires a separately *signed release* build — see below when you're ready for that.

## Building it yourself in Android Studio (needed eventually anyway, for Play Store signing)
1. Install **Android Studio** (free): https://developer.android.com/studio
2. Open Android Studio → **Open** → select this `WhatsAppScheduler` folder.
3. Let it sync (it will download Gradle + the Android libraries automatically
   the first time — needs internet, takes a few minutes).
4. Plug in your phone (enable *Developer Options → USB debugging*), or use an
   emulator, and press the green **Run ▶** button.
5. On first launch, the app will ask for:
   - **Contacts** permission (to let you pick who to message)
   - **Notifications** permission (Android 13+)
   - **"Alarms & reminders"** — it'll open a settings page for you, just switch it on
   Grant all three, or scheduling won't fire reliably.
6. Also worth doing once: go to your phone's **Settings → Apps → WhatsApp
   Scheduler → Battery → Unrestricted**. Some phones (Xiaomi, Oppo, Vivo,
   Samsung especially) aggressively kill background apps' alarms otherwise.

## Using the app
- Tap **+**, pick a contact, choose Text / Image / Video, write your message
  (or caption), pick a date and time, tap **Schedule message**.
- The home screen lists everything upcoming; swipe/tap **Cancel** to remove one.

## Publishing to the Play Store (this part has to be done by you)
I can't submit on your behalf — Google requires the developer account to
belong to a verified real person/company. Steps on your end:
1. Create a Google Play Console account: https://play.google.com/console (one-time $25 fee).
2. In Android Studio: **Build → Generate Signed App Bundle**, create a new
   signing key (keep it safe — you'll need the *same* key for every future update).
3. Upload the resulting `.aab` file in Play Console, fill in the store listing
   (description, screenshots, privacy policy URL — required even for simple apps).
4. Submit for review (usually a few days).

## Known limitation to know about up front
For photo/video messages, the app tries to jump straight into the right
contact's chat (using a parameter some WhatsApp versions honor). On WhatsApp
versions where that isn't honored, it'll open WhatsApp's own share screen
with the photo/video attached and ask *you* to pick the contact — one extra
tap, not a bug in the app, just WhatsApp's side varying by version. Text
messages always go straight to the correct chat.
