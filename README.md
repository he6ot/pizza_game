**English** | [Русский](README.ru.md)

# Wolf Egg Catcher

> *Don't go broke!*

A browser arcade game in the spirit of the classic Soviet handheld game *Nu, pogodi!* Four chickens lay eggs, the eggs roll down chutes, and you catch them in a basket. There's a twist, though: your **Api Balance** drains every second, and the only way to refill it is to catch eggs. When the balance hits zero, the game is over, and your score is how many seconds you survived.

![Wolf Egg Catcher screenshot](screenshot.png)

## Features

- Four chutes and four player positions; play with **W A S D** or with on-screen buttons on mobile
- Golden eggs worth $100 and a difficulty that keeps ramping up
- The balance drains constantly, and faster and faster over time; below $20 the screen edges pulse red
- Nickname and custom avatar (any image from your device, cropped to a square)
- Your best survival time is saved in the browser
- Sounds are synthesized on the fly with the Web Audio API, so there are no audio files
- The field scales to the window size, and the game works on both desktop and mobile
- The whole game lives in a single `index.html` (plus sprite images), with no dependencies or build step

## How to Play

1. On the start screen, press **PLAY**, enter a nickname, optionally pick an avatar (**Change Avatar**), and press **START GAME**.
2. Eggs roll down the chutes from the chickens toward the center of the field. To catch an egg, you have to be standing at its position at the end of its path: on the same side and at the same level (top or bottom) as the chicken that laid it.
3. A caught egg refills your balance. A missed egg breaks and costs you **$30**.
4. When the balance reaches zero, the game is over. You'll see your survival time, the number of eggs caught (including golden ones), your peak balance, and the total amount earned.

| Parameter | Value |
|---|---|
| Starting balance | $150 |
| Balance drain | $2 per second at the start, increasing over time |
| Regular egg | $5–50 (worth less the longer the game lasts) |
| Golden egg | $100, about 9% of eggs |
| Missed egg | −$30 |

Difficulty ramps up over time: eggs spawn more often and roll faster, and up to 10 eggs can be rolling at once.

## Controls

The player stands in one of four positions (the corners of an imaginary square) and moves between them:

| Key | Action |
|:---:|---|
| `W` | move up |
| `S` | move down |
| `A` | move left |
| `D` | move right |
| `M` | toggle sound |

To reach any position, two keys are enough (in any order):

| | Left | Right |
|---|:---:|:---:|
| **Top** | `W` + `A` | `W` + `D` |
| **Bottom** | `S` + `A` | `S` + `D` |

The keys also work with a Russian keyboard layout (Ц, Ф, Ы, В). On touch screens (and in narrow browser windows), four buttons ↖ ↙ ↗ ↘ appear at the bottom of the screen and jump straight to the matching position.

## Getting Started

No installation needed:

1. Download the repository (**Code → Download ZIP**) or clone it.
2. Open `index.html` in any modern browser.

Optionally, you can serve it with a local web server:

```bash
python -m http.server 8000
```

and open http://localhost:8000.

## Configuration

The balance and difficulty settings live in the `CFG` object at the top of the script in `index.html`:

| Parameter | Default | Description |
|---|---|---|
| `startBalance` | `150` | starting balance, $ |
| `penalty` | `30` | penalty for a missed egg, $ |
| `goldChance` | `0.09` | chance of a golden egg |
| `goldValue` | `100` | value of a golden egg, $ |
| `catchFrom` | `0.88` | fraction of the path from which an egg can be caught (the last 12%) |
| `drain(t)` | `2 + 0.08 * t + 0.01 * t ** 1.5` | balance drain, $/sec (`t` is seconds since the game started) |
| `spawnMs(t)` | `max(400, 1800 - 15 * t)` | delay between eggs, ms |
| `travelMs(t)` | `max(1000, 2500 - 16 * t)` | time an egg takes to roll down a chute, ms |
| `maxEggs(t)` | `3 → 5 → 7 → 10` | maximum number of eggs rolling at once (steps at 20 s, 40 s and 60 s) |
| `eggValue(t)` | `5–50` | random value of a regular egg, $ (decreases over time) |

## Project Structure

```text
.
├── index.html       the whole game: markup, styles and script
├── README.md        project description (English)
├── README.ru.md     project description (Russian)
├── screenshot.png   screenshot for the README
├── 1.png            regular egg
├── 2.png            regular egg
├── 3.png            golden egg
├── 4.png            chicken
├── 5.png            default player avatar
└── 6.png            regular egg
```

Any sprite can be replaced with your own as long as you keep the file name. If an image fails to load, the game shows a letter placeholder instead and stays playable.

## Tech

Plain HTML, CSS and JavaScript, with no frameworks, dependencies or build step. The game loop runs on `requestAnimationFrame`, and the sound is produced with the Web Audio API. Your nickname, avatar and best time are stored in the browser's `localStorage` (keys `wec_nick`, `wec_avatar`, `wec_best`); to reset your record, clear the site data.
