# Smart Display

**IoT project course 236333, Technion (ICST), Group 5**
Shada Nujeidat · Yuval Carmelli · Liat Rokach

## Overview

A smart display for the home. It shows the time, the weather, Google Calendar events and two
lists (shopping and to-do) on a touchscreen. You add or remove items from your phone by voice
or by typing, in English or Hebrew; Gemini understands the command and the display updates
within seconds. It keeps working without internet, and everything is set up from the phone,
with no code.

## How it works

```
 Phone app (PWA) ──voice / text──▶ Server (Node.js, Railway) ──audio──▶ Gemini
                                     │  checks the command   ◀── JSON {action, item, list}
                                     ▼
                       Firebase Realtime Database
          /commands/<id> (queue)   /status/<id> (reply)   /lists   /device/lastSeen
                         │                 ▲
                         ▼ every 2 s       │
                  ESP32 Smart Display (touchscreen, LVGL)
```

1. The user holds the button in the app and says a command ("add milk to my shopping list"), or types it.
2. The server sends it to Gemini, which returns JSON. The server checks it (an explicit add/remove,
   one item that was really said, a named list) and rejects anything unclear with a message.
3. A valid command goes into the `/commands` queue in Firebase.
4. The display checks the queue every 2 seconds, applies the command, saves the list in flash
   and writes its reply to `/status/<id>`. The app shows the reply ("Added milk to your shopping list").
5. The display reports in every 10 seconds (`/device/lastSeen`). If it is offline, the server
   answers the app at once and the command waits in the queue until the display is back.

## Features

| Feature | Where |
|---|---|
| Voice and typed commands in English and Hebrew, with Undo | App + display |
| Shopping list and to-do list: tick items off, clear a list, synced with the phone | Display + app |
| Time, date and greeting; World Clock with 6 cities | Display |
| Weather and 4-day forecast for 5 cities | Display |
| Google Calendar events (Calendar screen and Home widget) | Display |
| Offline mode: lists, settings and last data kept in flash; changes synced when back online | Display |
| Wi-Fi and account setup from the phone (SmartDisplay-Setup), no code | Display |
| Customizable home screen, dark/light theme, brightness, power saving | Display |
| Connection status, device status and automatic recovery (watchdog) | Display |
| See both lists in the app; display online/offline status | App |

## Repositories

| Repo | Contents |
|---|---|
| [dispaly](https://github.com/IOT-Project-SUM2026/dispaly) | ESP32 display firmware (Arduino + LVGL), documentation, unit tests, course documents (user stories, edge cases) |
| [app](https://github.com/IOT-Project-SUM2026/app) | Phone app (PWA) and server (Node.js, Gemini, Firebase) |

## Hardware

- **ESP32-2432S028R** "Cheap Yellow Display": ESP32 with a built-in 2.8" 320×240 touchscreen.
  Everything is on one board: no external wiring, only a USB cable (5 V, 1 A).
- Display UI built with **LVGL 9.6** (on top of TFT_eSPI 2.5.43), ESP32 core 3.3.12.
  Full library versions are in the [dispaly README](https://github.com/IOT-Project-SUM2026/dispaly#versions).

---

This project is part of the **ICST - The Interdisciplinary Center for Smart Technologies**,
Taub Faculty of Computer Science, Technion.
