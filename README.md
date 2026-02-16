# Templeton Academy — Campus Information Display

A live dashboard showing WMATA train arrivals, weather, clock, and school calendar — built for Templeton Academy and designed to run on any screen, kiosk, or browser.

![Templeton Display](https://img.shields.io/badge/Templeton-Academy-455be2?style=for-the-badge) ![WMATA Live](https://img.shields.io/badge/WMATA-Live%20Data-00B140?style=for-the-badge) ![GitHub Pages](https://img.shields.io/badge/Hosted-GitHub%20Pages-222?style=for-the-badge)

---

## Features

- **Real-time WMATA predictions** — Gallery Pl-Chinatown (Red, Green, Yellow lines)
- **Live weather** — via Open-Meteo (free, no API key needed)
- **School calendar** — Google Calendar integration (or demo events)
- **Live clock** with date
- **Scrolling marquee** for school announcements
- **Fully branded** to Templeton Academy Brand Guide v001

## Live URL

Once deployed to GitHub Pages, your display will be available at:

```
https://YOUR-USERNAME.github.io/templeton-display/
```

---

## Deployment — GitHub Pages

### 1. Create the Repository

1. Go to [github.com/new](https://github.com/new)
2. Name it `templeton-display` (or whatever you prefer)
3. Set it to **Public** (required for free GitHub Pages)
4. Click **Create repository**

### 2. Push the Files

From your local machine:

```bash
cd templeton-display
git init
git add .
git commit -m "Initial campus display"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/templeton-display.git
git push -u origin main
```

### 3. Enable GitHub Pages

1. Go to your repo → **Settings** → **Pages**
2. Under **Source**, select **Deploy from a branch**
3. Set branch to `main` and folder to `/ (root)`
4. Click **Save**
5. Wait 1–2 minutes, then visit `https://YOUR-USERNAME.github.io/templeton-display/`

That's it — your display is live.

---

## Configuration

All configuration is at the top of `index.html` inside the `CONFIG` object:

```javascript
const CONFIG = {
  wmataKey: "your-wmata-key",         // Get one at developer.wmata.com
  wmataStations: "F01,B01",           // Station codes (comma-separated)
  gcalApiKey: "",                      // Google Calendar API key
  gcalCalendarId: "",                  // Google Calendar ID
  marqueeMessages: [ ... ],           // Scrolling announcements
};
```

### WMATA Station Codes

Find your station(s) at the [WMATA API docs](https://developer.wmata.com/docs/services/547636a6f9182302184cda78/operations/547636a6f918230da855363f). Some common ones:

| Station | Code |
|---------|------|
| Gallery Pl-Chinatown (Green/Yellow) | F01 |
| Gallery Pl-Chinatown (Red) | B01 |
| Metro Center (Blue/Orange/Silver) | C01 |
| Metro Center (Red) | A01 |
| Union Station | B03 |
| Dupont Circle | A03 |

### Google Calendar Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a project → Enable **Google Calendar API**
3. Create an **API Key** (restrict to Calendar API)
4. Make your calendar **publicly readable** (Settings → Access permissions)
5. Copy the **Calendar ID** from Settings → Integrate calendar
6. Add both values to `CONFIG` in `index.html`

---

## Running on Displays

### Smart TVs (Fire TV, Samsung, Roku)

See **[SMART-TV-GUIDE.md](SMART-TV-GUIDE.md)** for detailed instructions on each platform. The short version: **Fire TV Sticks + Fully Kiosk Browser** is the best approach for most setups — cheap, reliable, and auto-launches on boot.

### Raspberry Pi Kiosk

Point your Pi's kiosk browser at the hosted URL instead of a local file. This way, any edits you push to GitHub auto-update every display.

In your kiosk startup script, change the last line to:

```bash
chromium-browser \
  --noerrdialogs --disable-infobars --kiosk --incognito \
  https://YOUR-USERNAME.github.io/templeton-display/
```

See **[SETUP-GUIDE.md](SETUP-GUIDE.md)** for full Raspberry Pi kiosk instructions.

---

## Updating Announcements

Edit the `marqueeMessages` array in `index.html`, commit, and push:

```bash
git add index.html
git commit -m "Update announcements"
git push
```

Changes go live within a minute or two.

---

## Data Refresh Rates

| Data | Interval |
|------|----------|
| Clock | 1 second |
| WMATA Trains | 20 seconds |
| Weather | 10 minutes |
| Calendar Events | 5 minutes |

---

## Security Note

Your WMATA API key is visible in the page source. For an internal school display this is fine — the free tier is rate-limited and the key is easily regenerated at [developer.wmata.com](https://developer.wmata.com). If you ever want to lock it down further, you could add a small Cloudflare Worker or similar proxy.

---

*Never Stop Being Curious.* 🟣🔵🟢🟡🔴
