# Templeton Academy Display — Smart TV Deployment Guide

This guide covers how to run the campus display on boot across Fire TV, Roku, and Samsung smart TVs. The short version: **Fire TV is the easiest and most reliable, Samsung is doable, and Roku is the most limited.**

---

## Fire TV (Recommended)

Fire TV runs Android under the hood, which makes it the most flexible option. There are two solid approaches — a free one and a low-cost "set it and forget it" one.

### Option A: Fully Kiosk Browser (Best for Permanent Displays)

**Fully Kiosk Browser** is purpose-built for this. It locks the device to a single URL, auto-launches on boot, and has built-in features like scheduled on/off and remote management. It's free to try, $7.90 one-time per device for the full version.

**Setup:**

1. On your Fire TV, go to **Settings → My Fire TV → Developer Options**
2. Enable **Apps from Unknown Sources** (may say "Install unknown apps")
3. Install **Downloader** from the Amazon Appstore (it's free)
4. Open Downloader and enter: `https://www.intravision.com/download/fully-kiosk-browser-for-fire-tv/`
   - Alternatively, search for the APK URL at `fully-kiosk.com`
5. Install the APK when prompted
6. Open Fully Kiosk Browser
7. Set the **Start URL** to your GitHub Pages URL:
   ```
   https://YOUR-USERNAME.github.io/templeton-display/
   ```
8. In Fully Kiosk settings, enable:
   - **Web Auto Reload** → set to a long interval (e.g., every 12 hours) to keep things fresh
   - **Launch on Boot** → ON
   - **Keep Screen On** → ON
   - **Prevent Sleep** → ON
   - **Kiosk Mode** → ON (locks the device to the browser — no one can exit to the Fire TV home screen without a PIN)

That's it. Unplug and replug the Fire TV and the display will come right back up.

### Option B: Amazon Silk Browser + Boot Automation (Free)

This is fully free but a bit more fragile since Silk isn't designed for kiosk use.

1. Open **Silk Browser** (pre-installed on Fire TV)
2. Navigate to your display URL and bookmark it
3. Install **AutoStart - No Root** via sideloading (use Downloader):
   - URL: search for `AutoStart No Root APK`
4. Configure AutoStart to launch Silk Browser on boot
5. In Fire TV **Settings → Display & Sounds → Screen Saver**, set the timeout to "Never"

The downside is Silk may occasionally show its toolbar or address bar. Fully Kiosk is much cleaner for a permanent display.

### Fire TV Tips

- **CEC (HDMI-CEC):** Most TVs will power on automatically when the Fire TV Stick boots. Enable CEC in your TV settings so that plugging in the Fire TV's power (via a wall outlet timer or smart plug) turns the whole TV on.
- **Smart plug scheduling:** Use a simple outlet timer or smart plug to cut power at 6 PM and restore at 7 AM. The Fire TV Stick boots automatically when power is restored, and Fully Kiosk auto-launches the URL.
- **Remote management:** Fully Kiosk Pro supports remote admin via a web panel — you can change the URL, restart the browser, or take screenshots from any computer on the same network.

---

## Samsung Smart TVs (Tizen)

Samsung TVs have a built-in web browser, but keeping it locked to one page on boot takes a bit of work. There are two approaches depending on the TV model.

### Option A: Samsung Smart Signage (Business Displays)

If your school is purchasing displays, Samsung's commercial signage TVs (the "Samsung Business" line) have a built-in **URL Launcher** mode designed exactly for this:

1. Enter the TV's service menu or use the **MagicInfo** management tool
2. Set **URL Launcher** as the startup app
3. Enter your display URL
4. The TV boots directly into your webpage every time

This is the cleanest solution, but only available on commercial-grade Samsung displays.

### Option B: Consumer Samsung TVs (Workaround)

Consumer Samsung TVs don't have a native kiosk mode, so the process is more manual:

1. Open the **Samsung Internet Browser** from the app tray
2. Navigate to your display URL
3. Bookmark it or set it as the homepage
4. In **Settings → General → Smart Features**, configure:
   - **Autorun Last App** → ON (this will reopen the browser on boot)
   - **Screen Saver** → OFF or set to a very long timeout

**Limitations:**
- The browser address bar may appear briefly on launch
- Occasional firmware updates can reset the "last app" state
- Samsung's browser handles auto-refreshing JavaScript well, but may occasionally prompt about "unresponsive page" after many hours — the 12-hour page reload in the display code helps prevent this

### Samsung Tips

- **Tizen browser quirks:** Samsung's browser is Chromium-based and handles the display well, but test it on your specific TV model first.
- **CEC + smart plug** works here too for power scheduling.
- If you have multiple Samsung TVs, look into **Samsung MagicInfo Express** — it's free software for managing URLs across multiple Samsung displays.

---

## Roku (Most Limited)

Roku is the most locked-down platform. There is no built-in web browser, and Roku doesn't allow sideloading apps the way Fire TV does. Here are your realistic options:

### Option A: Screen Mirroring / Casting

The simplest approach — don't run the display on the Roku itself, run it on another device and cast to the TV.

**From a Raspberry Pi or old laptop on the same network:**
1. Open the display URL in Chromium on the Pi/laptop
2. Use **Miracast** or a wired HDMI connection to the TV

**From a phone/tablet (quick demo, not ideal for permanent use):**
1. Open the URL in Chrome on an Android phone
2. Cast to Roku via **Settings → Screen Mirroring** on the Roku

### Option B: Roku Developer Mode (Advanced)

Roku supports custom "channels" built in BrightScript, but creating one that renders a live webpage is a significant development effort — essentially rebuilding the display as a native Roku app. This isn't practical for your use case.

### Option C: Replace the Roku

Honestly, the most cost-effective solution for Rokus is to plug a **Fire TV Stick Lite ($20–30)** into the TV's HDMI port and follow the Fire TV instructions above. The Roku can stay connected to a different HDMI input for regular use.

### Roku Recommendation

Unless you have a strong reason to use the Roku directly, swapping in a Fire TV Stick is faster, cheaper, and far more reliable than trying to work around Roku's limitations. A Fire TV Stick Lite costs about the same as the time you'd spend trying to get Roku to cooperate.

---

## Summary: What to Buy

| Platform | Best Approach | Cost per TV | Reliability |
|----------|--------------|-------------|-------------|
| **Fire TV Stick** | Fully Kiosk Browser | $7.90 (app) + ~$25 (stick if needed) | ★★★★★ |
| **Samsung (Commercial)** | URL Launcher | $0 (built-in) | ★★★★★ |
| **Samsung (Consumer)** | Autorun Last App | $0 | ★★★☆☆ |
| **Roku** | Plug in a Fire TV Stick | ~$25 | ★★★★★ (via Fire TV) |
| **Raspberry Pi** | Chromium Kiosk | ~$50 (Pi + case) | ★★★★★ |

### For a Multi-Display Campus Rollout

If you're putting displays in multiple locations around the building, here's what I'd suggest:

1. **Standardize on Fire TV Sticks** — they're cheap, reliable, and easy to manage
2. **Use Fully Kiosk Browser** on all of them ($7.90 each, one-time)
3. **Smart plugs on each TV** for power scheduling (school hours only)
4. **One GitHub Pages URL** serves all displays — push an update once, every screen refreshes

For a 5-display rollout, you're looking at roughly $175 total (5 sticks at ~$25 + 5 Fully Kiosk licenses at ~$8) — significantly less than any commercial digital signage solution.

---

## Power Scheduling (All Platforms)

Since you probably don't want TVs running 24/7, the simplest universal solution is a smart plug or outlet timer on each TV:

- **On:** 7:00 AM weekdays
- **Off:** 6:00 PM weekdays (or whenever the building closes)
- **Off:** Weekends

When power is restored, Fire TV Sticks auto-boot → Fully Kiosk auto-launches the URL → CEC turns the TV on. Fully hands-free.

---

## Updating Announcements Across All Displays

Since every display points to the same GitHub Pages URL, updating the marquee or any content is a single `git push`:

```bash
# Edit index.html (update marqueeMessages or any content)
git add index.html
git commit -m "Update Monday announcements"
git push
```

All displays pick up the change within minutes. No touching any TV or stick.
