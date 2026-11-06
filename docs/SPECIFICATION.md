# WeatherPal specification

**Product:** WeatherPal, your personal weather companion
**Platforms:** Windows desktop and Android (macOS and Linux later)
**Style:** Cozy pixel art mixed with a retro desktop interface
**Status:** Working HTML prototype complete. Flutter app not started.

Items marked **(prototype)** already work in the HTML prototype in this repository.

---

## 1. Core concept

WeatherPal is a small, interactive weather companion. It shows live weather through an animated pixel-art world and a customizable character. The goal is to make checking the weather feel like visiting a tiny virtual world instead of reading numbers on a screen.

The differentiating idea: **a tiny virtual pet that happens to understand the weather.**

## 2. Platforms and technology

| Component | Technology |
|---|---|
| Framework | Flutter |
| Language | Dart |
| State management | Riverpod |
| Architecture | Clean Architecture with a feature-based structure |
| Weather data | Open-Meteo Forecast API **(prototype)** |
| Location search | Open-Meteo Geocoding API **(prototype)** |
| Local database | Hive or Isar |
| Simple preferences | SharedPreferences |
| Animation | AnimationController with sprite sheets or Rive |
| Desktop integration | Windows system tray and window management |
| Mobile integration | Android notifications and a home-screen widget |
| Testing | flutter_test and mocktail |
| Version control | Git and GitHub |

**Platform plan**

- **Windows:** a compact desktop widget, a full application window, and optional system-tray access.
- **Android:** a mobile app with a compact companion screen, plus a native home-screen widget in a later milestone.
- **Later:** macOS and Linux.

The first version should not depend on permanent floating overlays. Windows supports a small always-on-top window, but Android restricts background overlays and needs special permissions. Build the normal app first, then add those integrations.

## 3. Visual identity

### Color palette **(prototype)**

| Name | Hex | Usage |
|---|---|---|
| Midnight Navy | `#191D2B` | Main dark background |
| Soft Lavender | `#B8B1E8` | Accent elements |
| Sky Blue | `#A9C9F4` | Main widget frame |
| Cloud White | `#F4F3F0` | Text and light surfaces |
| Sunshine Yellow | `#FFD45C` | Sunny weather |
| Mint Green | `#A8D8B0` | Positive indicators |
| Rain Blue | `#829DBB` | Rainy environments |
| Storm Purple | `#756C9B` | Thunderstorms |
| Snow White | `#E8F1FA` | Snowy environments |
| Sunset Pink | `#F2A9B8` | Sunset and special accents |

### Typography **(prototype)**

- Main interface: Nunito
- Weather information: Nunito Sans
- Pixel-style headings: Press Start 2P
- Numbers: a clear monospaced font (JetBrains Mono in the prototype)

Pixel fonts are for headings only; they become hard to read at small sizes.

### Widget dimensions

**Desktop compact widget**
- Default 360 × 430 px, resizable from 300 to 500 px wide
- 16 px rounded corners and a 3 px border **(prototype)**
- The character takes up about 25–35 % of the weather scene **(prototype)**
- The background adapts to the weather **(prototype)**

**Mobile**
- Full-screen responsive layout
- Compact character and weather card
- Character customization, detailed weather and settings screens

## 4. Main sections

| # | Section | Contents |
|---|---|---|
| 1 | **Weather Home** | Current temperature, condition, feels-like, humidity, wind, UV index, air quality (optional), today's high and low, tomorrow's forecast, last-updated time **(prototype, except air quality)** |
| 2 | **Character Studio** | Skin tone, hair style and color, eyes, clothing, shoes, hats, glasses, accessories, poses, name, saved presets **(prototype: skin, hair style, hair color, top, name)** |
| 3 | **Weather World** | Background, animated effects, lighting, color atmosphere, particles and character animation for each weather **(prototype)** |
| 4 | **Forecast** | Hourly and seven-day forecast, temperature trend, rain probability, sunrise and sunset, weather alerts **(prototype: six hours and tomorrow)** |
| 5 | **Settings** | Locations, °C/°F **(prototype)**, wind units **(prototype)**, widget size, theme, animation intensity, refresh frequency, notifications, start with Windows, privacy and location permissions |

## 5. Weather behavior

This is the most important part of the project. The character is never a static image; it reacts to the weather.

| Weather | Character | Environment |
|---|---|---|
| Sunny / clear | Sunglasses, occasional wave, may hold a drink | Shining sun, floating sun particles, warm yellow light **(prototype)** |
| Rainy | Umbrella, occasional puddle jump | Rain behind the character, puddles on the ground **(prototype)** |
| Thunderstorm | Raincoat, umbrella, surprised by thunder | Dark purple and blue sky, lightning flashes **(prototype)** |
| Snow | Winter coat and scarf, catches snowflakes | Falling snow that piles up, icy blue and white **(prototype)** |
| Cloudy | Casual clothes, relaxed | Slow clouds, soft gray-blue sky, occasional wind **(prototype)** |
| Foggy | Hoodie, sleepy or curious | Fog moving across, reduced visibility **(prototype)** |

**Additional states to support:** drizzle, heavy rain, freezing rain, hail, windy, partly cloudy **(prototype)**, clear night **(prototype)**, rainy night **(prototype)**, extreme heat and extreme cold.

**Rule:** conditions must come from the real weather API. Outfits and animations are chosen by a weather-state mapping engine, never at random.

## 6. Avatar customization engine

The avatar is built from layers so that any combination works without drawing every combination separately. **(prototype)**

**Layer order**
1. Base body
2. Skin tone
3. Eyes and expression
4. Hairstyle
5. Shirt or top
6. Pants or skirt
7. Shoes
8. Outerwear
9. Weather accessories
10. Decorative accessories

| Feature | Examples |
|---|---|
| Hair | Short, long, curly, straight, buns |
| Hair color | Black, brown, blonde, red, pastel |
| Eyes | Different shapes and colors |
| Clothing | Shirts, hoodies, dresses |
| Outerwear | Raincoats, jackets, winter coats |
| Accessories | Glasses, hats, earrings |
| Weather items | Umbrella, sunglasses, scarf |
| Expressions | Happy, sleepy, surprised, excited |

**Outfit modes**
- **Automatic:** the system picks clothes for the weather. **(prototype)**
- **Manual:** the user's chosen outfit is kept in every weather. **(prototype)**

A manual outfit is respected unless the user turns automatic outfits back on.

## 7. Weather API integration

Primary provider: [Open-Meteo](https://open-meteo.com/en/docs).

| Data | Purpose |
|---|---|
| `temperature_2m` | Current temperature **(prototype)** |
| `apparent_temperature` | Feels-like **(prototype)** |
| `relative_humidity_2m` | Humidity **(prototype)** |
| `wind_speed_10m` | Wind **(prototype)** |
| `wind_direction_10m` | Wind direction indicator |
| `weather_code` | Weather state **(prototype)** |
| `precipitation` | Rain and snow intensity |
| `precipitation_probability` | Rain probability **(prototype)** |
| `cloud_cover` | Cloudiness |
| `uv_index` | Sun exposure **(prototype)** |
| `sunrise`, `sunset` | Day and night transitions **(prototype)** |
| Daily max and min | Forecast **(prototype)** |

**Refresh rules**
- Refresh when the app opens and every 15 minutes while active. **(prototype)**
- Cache the last successful response and use it offline. **(prototype)**
- Retry failures with exponential backoff. **(prototype)**
- Show the last update time. **(prototype)**
- Avoid unnecessary requests and never crash on API errors.

**Location**
- GPS location with permission
- Manual city search **(prototype)**
- Several saved favorite locations and switching between them

Air quality comes from a separate, optional air-quality endpoint; it is not part of the forecast response.

## 8. Desktop experience (Windows)

| Mode | Behavior |
|---|---|
| **A. Compact companion** | Small window, optional always-on-top, draggable, remembers position, resizable, can hide details |
| **B. Expanded window** | Full details, character interaction, hourly and seven-day forecast, shortcut to Character Studio |
| **C. System tray** | Weather icon in the tray with a menu: open companion, refresh, change location, exit |

**Also:** optional start with Windows, minimize to tray, remember size and position, transparency where possible, multi-monitor support, a hide/show shortcut, and never stealing keyboard focus when the weather updates.

**Android:** a native home-screen widget in a later milestone, showing a small character, the temperature and the condition.

## 9. Personality

| Situation | Behavior |
|---|---|
| App opens | Waves hello **(prototype)** |
| Weather changes | Changes outfit **(prototype)** |
| Rain begins | Opens umbrella **(prototype)** |
| High temperature | Looks hot and fans itself |
| Low temperature | Shivers **(prototype)** |
| Thunderstorm | Looks surprised **(prototype)** |
| Snow begins | Plays with snowflakes **(prototype)** |
| Night | Becomes sleepy **(prototype)** |
| Morning | Wakes up and stretches **(prototype)** |
| User clicks the character | Plays an idle animation **(prototype)** |

**Speech bubbles (optional, rule-based)** **(prototype)**
- "Looks like a beautiful day!"
- "Don't forget your umbrella!"
- "It's getting chilly outside!"
- "Good night! See you tomorrow."

No AI service is needed to generate these.

## 10. Advanced features

| Feature | Description |
|---|---|
| Weather moods | Sunny: happy. Rainy: relaxed. Stormy: nervous. Snowy: excited. Very hot: tired. Very cold: sleepy. **(prototype, except heat and cold)** |
| Day and night cycle | Sunrise, bright afternoon, sunset colors, stars and moon at night **(prototype)** |
| Weather journal | A daily record of the weather and how the character looked |
| Outfit collection | Cosmetic outfits unlocked by achievements (seven rainy days, first snowfall, 30 days of use). Never paid. |
| Multiple characters | Create several characters and switch between them |
| Background themes | Default, minimal, nature, cozy bedroom, cityscape, seasonal |
| Notifications | Rain soon, temperature drop, strong wind, much warmer tomorrow, severe weather. Configurable and never annoying. |

**Creative extras**
- **Cozy bedroom scene:** rain falls outside the window; sunlight comes into the room on clear days.
- **Wardrobe collection:** collectible clothing and seasonal outfits.
- **A living world:** the scene changes through the day even when the weather doesn't.

## 11. Application architecture (Flutter)

```
weatherpal/
├── android/
├── windows/
├── assets/
│   ├── characters/      base, hairstyles, outfits, accessories, expressions, animations
│   ├── backgrounds/     sunny, cloudy, rainy, stormy, snowy, foggy, night
│   ├── weather_effects/
│   ├── icons/
│   └── fonts/
├── lib/
│   ├── core/            constants, errors, network, theme, utils, services
│   ├── features/
│   │   ├── weather/       data, domain, presentation
│   │   ├── character/     data, domain, presentation
│   │   ├── environment/
│   │   ├── forecast/
│   │   ├── customization/
│   │   ├── notifications/
│   │   ├── desktop_widget/
│   │   └── settings/
│   ├── shared/          widgets, models, providers
│   └── main.dart
├── test/                unit, widget, integration
├── docs/
├── README.md
├── CHANGELOG.md
├── LICENSE
└── pubspec.yaml
```

The architecture keeps these concerns separate:
1. Fetching weather data
2. Interpreting weather into a normalized state
3. Character appearance and clothing
4. Background selection
5. Animation and particle effects
6. User preferences
7. Platform-specific window behavior

The weather engine produces one normalized weather state. The character engine and the environment engine react to it independently. The prototype already follows this split.

## 12. Core data models

| Model | Main properties |
|---|---|
| `WeatherData` | temperature, humidity, wind, condition, timestamp |
| `DailyForecast` | date, maxTemp, minTemp, condition |
| `HourlyForecast` | time, temperature, precipitation |
| `UserLocation` | name, latitude, longitude, timezone |
| `CharacterProfile` | id, name, appearance, preferences |
| `CharacterOutfit` | clothing, accessories, shoes |
| `WeatherState` | condition, intensity, dayNight |
| `EnvironmentTheme` | background, effects, lighting |
| `UserSettings` | units, refresh interval, theme, notifications |

All saved models support serialization and database migration.

## 13. Technical rules

- No hardcoded or fake weather in the production app.
- No random character changes unrelated to the real conditions.
- No API calls inside UI widgets; no business logic in the UI.
- No unnecessary rebuilding of the whole interface.
- Animations never block while weather loads.
- No API credentials committed to GitHub.
- Proper loading, error and offline states.
- Light and dark interface themes.
- Consistent pixel-art rendering with nearest-neighbor filtering. **(prototype)**
- Transparent backgrounds on character assets, with consistent dimensions, anchor points and timing.
- High-DPI support on Windows.
- Text alternatives for weather information. **(prototype)**
- Respect location permissions, and never require location when a manual city is enough. **(prototype)**

## 14. Development roadmap

| Phase | Deliverable |
|---|---|
| 0 | Working HTML prototype **(done)** |
| 1 | Flutter project setup and design system |
| 2 | Live weather API and location selection |
| 3 | Weather state detection engine |
| 4 | Character assets and animations |
| 5 | Dynamic backgrounds and weather effects |
| 6 | Avatar customization |
| 7 | Hourly and weekly forecasts |
| 8 | Windows desktop companion |
| 9 | Local persistence and notifications |
| 10 | Android home-screen widget |
| 11 | Testing, optimization and documentation |

After every milestone: run the app, run the tests, fix compile errors, check the UI, verify the feature, and update the documentation. Unfinished features are never presented as done.

## 15. MVP

The first Flutter release includes:
- Live weather and automatic weather detection
- At least six weather environments
- One fully animated, customizable character
- Automatic weather-appropriate outfits and manual customization
- The Windows compact desktop widget
- The Android mobile interface
- Saved settings and offline cached weather

### First demo

1. Open WeatherPal.
2. It detects your location.
3. It loads the real weather.
4. The character appears in the right environment.
5. The character changes clothes when the weather changes.
6. Click the character and it animates.
7. Open Character Studio and change its look.
8. Close and reopen the app; the character and settings are still there.

## 16. Acceptance criteria

The project is complete when:
1. Real weather data loads.
2. The weather engine interprets conditions correctly.
3. The character's appearance changes with the weather.
4. The background changes with the weather.
5. Animations run smoothly.
6. Avatar customization works and is saved.
7. Locations can be switched.
8. Cached weather works offline.
9. The Windows companion can be moved, resized and minimized.
10. The Android app runs.
11. API and permission errors are handled.
12. Tests pass.
13. Documentation matches what is actually implemented.

## 17. Assets

- All pixel art is original. No copyrighted characters and no unlicensed commercial assets.
- Character assets have transparent backgrounds and consistent dimensions.
- Sprite sheets are used where they fit.
- Placeholder assets are documented and listed for replacement.
- The app never depends on missing assets or broken image paths.

## 18. GitHub presentation

- **Suggested repository name:** `weatherpal-pixel-weather-companion`
- **Description:** A cozy pixel-art weather companion for desktop and mobile, with live weather, customizable animated avatars and dynamic environments.
- **Topics:** `flutter` `dart` `weather-app` `pixel-art` `desktop-widget` `android` `open-meteo` `avatar-customization` `animations`
