# Smart Display

**IoT project course 236333, Technion (ICST), Group 5**
Shada Nujeidat · Yuval Carmelli · Liat Rokach

## Overview

A smart display that presents useful personal information such as weather, time, calendar
events and lists. You use it through a touchscreen interface, and you can send it
information or commands from a smartphone by voice, interpreted by Gemini.

## How it works

```
 Phone app ──voice──▶ Backend ──audio──▶ Gemini
 (PWA)                  │   ◀──JSON {action, item, list}──
                        ▼
                Firebase Realtime Database
                 /command  ▲      │  /status (confirmation)
                           │      ▼
                    ESP32 Smart Display (touchscreen)
```

1. The user holds the button in the phone app and speaks a command ("add milk to my shopping list").
2. The backend sends the audio to Gemini, which returns a structured JSON command.
3. The command is written to Firebase. The display reads it, applies it, and writes a confirmation back.
4. The phone app shows whether the display confirmed the command.

## Features

| Feature | Status |
|---|---|
| Weather + 4-day forecast, 5 Israeli cities | ✅ Display |
| Time & date (NTP) + world clock | ✅ Display |
| Wi-Fi setup (captive portal) | ✅ Display |
| Connection status icon | ✅ Display |
| Settings: brightness, 24-hour time, saved across reboots | ✅ Display |
| Idle / power saving mode | ✅ Display |
| Voice command → Gemini → Firebase | ✅ App · 🚧 Display side |
| Command confirmation in the app | ✅ App · 🚧 Display side |
| Shopping list / To-do list | 🚧 In progress |
| Calendar, display customization | 📋 Planned |

## Repositories

| Repo | Contents |
|---|---|
| [dispaly](https://github.com/IOT-Project-SUM2026/dispaly) | ESP32 display firmware (Arduino + LVGL), install instructions, pinout |
| [app](https://github.com/IOT-Project-SUM2026/app) | Phone app (PWA) and backend (Node.js, Gemini, Firebase) |

## Hardware

- **ESP32-2432S028R** "Cheap Yellow Display": ESP32 with a built-in 2.8" 320×240 touchscreen
- Display UI built with **LVGL 9** (on top of TFT_eSPI)

---

This project is part of the **ICST - The Interdisciplinary Center for Smart Technologies**,
Taub Faculty of Computer Science, Technion.
