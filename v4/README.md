# Seraphina — Fortnite style (v4)

Play: https://nasoteisoy.github.io/seraphina-fortnite/ (works on phone)
Older: v3 https://nasoteisoy.github.io/seraphina-fortnite/v3/ · v2 https://nasoteisoy.github.io/seraphina-fortnite/v2/

v4:
- No more gliding: feet stay planted during attacks, bow draw and idle (movement locked during attack clips,
  tiny eased lunge only, recovery cancels when you move, walk/run speed matched to the clip stride, foot-lock IK).
- Flowy hair and cape: ponytail bone chains + 5x4 cape bone grid driven by a verlet spring sim
  (gravity, inertia, wind, body collisions, anti-flip limits) — swings on run/turn/stop/jump/attack, settles at idle.
- Render pass: removed the two sticks over her right shoulder and loose slivers, sharper phone rendering (pixel ratio 2),
  tighter shadows, rim light, new ground + bladed grass, nicer training dummies, help bar no longer overlaps buttons.

Phone: left stick to move, icon buttons: 🤸 roll, 🦶 kick, ⬆️ jump, 🔄 switch weapon, 🏹/🗡️ attack (tap 3× for the combo).
Desktop: WASD move, Shift run, Space jump, F attack, X/Tab switch, K kick, C roll.

![sheet](sheet.png)

Before/after motion: [motion.gif](motion.gif) · flaw crops: [flaws.png](flaws.png)
