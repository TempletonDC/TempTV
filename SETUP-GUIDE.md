# Templeton Academy Campus Display — Raspberry Pi Setup Guide

## What You Need

- Raspberry Pi 4 (2GB+ RAM recommended) or Pi 5
- MicroSD card (16GB+) with Raspberry Pi OS (Desktop version)
- HDMI cable + TV/monitor
- Power supply for Pi
- Wi-Fi or Ethernet connection

---

## Quick Setup (5 Steps)

### 1. Copy the Display File

Copy `templeton-display.html` to your Raspberry Pi. You can use a USB drive, SCP, or place it directly:

```bash
mkdir -p /home/pi/display
# Copy the file to /home/pi/display/templeton-display.html
```

### 2. Install Chromium (if not already installed)

```bash
sudo apt update
sudo apt install -y chromium-browser unclutter
```

`unclutter` hides the mouse cursor after inactivity — perfect for a display.

### 3. Create the Kiosk Startup Script

```bash
nano /home/pi/display/start-kiosk.sh
```

Paste this:

```bash
#!/bin/bash

# Wait for desktop to load
sleep 10

# Disable screen blanking / power saving
xset s off
xset -dpms
xset s noblank

# Hide mouse cursor
unclutter -idle 0.5 -root &

# Launch Chromium in kiosk mode
chromium-browser \
  --noerrdialogs \
  --disable-infobars \
  --kiosk \
  --incognito \
  --disable-translate \
  --disable-features=TranslateUI \
  --overscroll-history-navigation=0 \
  --disable-pinch \
  --check-for-update-interval=31536000 \
  --autoplay-policy=no-user-gesture-required \
  file:///home/pi/display/templeton-display.html
```

Make it executable:

```bash
chmod +x /home/pi/display/start-kiosk.sh
```

### 4. Auto-Start on Boot

Add the kiosk script to autostart:

```bash
mkdir -p /home/pi/.config/autostart
nano /home/pi/.config/autostart/kiosk.desktop
```

Paste:

```ini
[Desktop Entry]
Type=Application
Name=Templeton Display
Exec=/home/pi/display/start-kiosk.sh
X-GNOME-Autostart-enabled=true
```

### 5. Reboot and Test

```bash
sudo reboot
```

The display should launch automatically in fullscreen.

---

## Google Calendar Integration

To show live calendar events instead of demo data, you need a Google Calendar API key:

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (or use existing)
3. Enable the **Google Calendar API**
4. Create an **API Key** (restrict it to Calendar API only)
5. Make your school calendar **public** (or use a service account for private calendars)
6. Find your **Calendar ID** in Google Calendar Settings → Integrate Calendar

Then edit `templeton-display.html` and update these lines near the top of the `<script>`:

```javascript
gcalApiKey: "YOUR_API_KEY_HERE",
gcalCalendarId: "your_calendar_id@group.calendar.google.com",
```

---

## Useful Tips

### Prevent Screen Blanking (Belt & Suspenders)

Edit `/etc/xdg/lxsession/LXDE-pi/autostart`:

```bash
sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
```

Add these lines:

```
@xset s off
@xset -dpms
@xset s noblank
```

### Auto-Refresh the Page (Optional)

The display already auto-refreshes data, but to do a full page reload daily (clears memory), add a cron job:

```bash
crontab -e
```

Add:

```
0 4 * * * DISPLAY=:0 xdotool key F5
```

This refreshes the browser at 4 AM daily.

### Rotate Display (if mounted vertically)

Edit `/boot/config.txt`:

```
display_rotate=1   # 90 degrees
display_rotate=3   # 270 degrees
```

### Monitor On/Off Schedule

To turn the display off at night and on in the morning:

```bash
crontab -e
```

```
0 18 * * 1-5 vcgencmd display_power 0   # Off at 6 PM weekdays
0 7  * * 1-5 vcgencmd display_power 1   # On at 7 AM weekdays
```

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Trains not loading | Check Wi-Fi, verify WMATA API key is active |
| Weather not loading | Open-Meteo may be temporarily down, it will retry |
| Calendar shows demo events | Set up Google Calendar API key (see above) |
| Screen goes black | Apply screen blanking fixes above |
| Display too small/large | Adjust your TV's overscan settings or add `disable_overscan=1` to `/boot/config.txt` |

---

## Data Refresh Rates

| Data | Refresh Interval |
|------|-----------------|
| Clock | Every 1 second |
| WMATA Trains | Every 20 seconds |
| Weather | Every 10 minutes |
| Calendar | Every 5 minutes |

---

## Architecture Note

This pairs nicely with your Raspberry Pi bell/intercom system — you could potentially run both on the same Pi if it's a Pi 4 or 5 with sufficient resources. The display is just a Chromium browser tab, so it's very lightweight.
