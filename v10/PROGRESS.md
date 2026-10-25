# v10 — Alaric: fix leg holes + sword gripped in hand when drawn  [pt482u] "Face is fine. Fix holes in his legs. Fix that he holds his sword when its equipped"
Rules: v9 face approved (do not change face), never delete, no Meshy, no BotCampus, no self-PASS (Chumi judges). v9 stays at /v9/, root = v10. (v9 log kept as PROGRESS_v9.md)
## 08:09 MST — workspace
- Copied /workspace/seraphina_fortnite_v9 → /workspace/seraphina_fortnite_v10 (v9 untouched).

## 08:43 MST — step 1: leg holes diagnosis (v9 GLB/blend, nothing changed yet)
- Materials are already double-sided (GLB doubleSided=true on all 5 + game styleModel sets DoubleSide) → NOT backface culling.
- Open boundary loops below the belt (legs/holes_probe.py, legs/runs_probe.py; red = open edges in legs/v9h_grid.png): 415 open edges. Two big loops are the v8b "web face" cut (427 cloak↔leg faces deleted): Meshy had no pants surface on the BACK/OUTER side of both thighs (the cloak's inner face was the thigh's back), so after the cut each thigh is an open half-shell from the hip (z 1.0) down to above the knee (z 0.54), outer side round to the inner/back side. Plus small leg holes: left shin outer (25 verts, z .24–.45), left inner thigh (23 verts), right thigh 4+3 verts; one 16-vert hole in the cloak itself.
- In-game baseline (v9, 8 angles × idle/walk/run/attack, test/legshot10.py → shots/legs/v9_*.png, g_v9_*.png): you can see into the hollow backs of the thighs through the cloak gaps when the legs swing.

## 08:59 MST — step 1: leg fix built (legs/legs_v10.py → glb/alaric_fortnite_v10.glb/.blend/_info.json)
- Caps the holes with NEW leg-only geometry (never joins cloak to leg, so the v8b black slabs can't return): each thigh's open half-shell is closed with a cylindrical patch around the UpLeg bone (front run + back run of the hole boundary, Delaunay-filled interior grid at 2.2 cm, radius interpolated from the hole rim), weights = leg/hip groups only (IDW from rim, cloak groups excluded), dark-pants texture.
- thigh_L 613 faces / thigh_R 593, left inner-thigh hole 105, left shin hole 71, tiny right hole 1, 16-vert cloak hole 14, crotch-back strip 2. Open edges below belt 415 → 225 (remaining = cloak hem/inner edges, which are supposed to be open). Verts above z 1.1 moved: 0 (face untouched).
- Trial t1 rendered the patches WHITE (all patch UVs on one texel → degenerate tangents break normal-map shading). Fixed (t2): patch UVs spread ±0.6% inside a uniform dark-pants texel block (checked: std <1/255 over ±32 px). legs/t2h_grid.png = dark, closed.
- GLB structure same as v9: 93 nodes, 40 clips, 5 double-sided materials; 31250 verts (v9 30290).
## 08:59 MST — step 2: sword code (web/src/_tail.js; v9 copy kept as _tail_v9.js)
- swordWant(): sword is in the hand whenever weapon==='sword' (v9 only drew it during the combo + 0.9 s, sheathed it when walking/running/jumping/crouching → "doesn't hold it when equipped"). Sheathes only when switching to fists.
- switchChar: arrives with it in hand (SWD.u=1). SWGRIP.a tunable slide along the grip. ALARIC_URL → v10 GLB, title v10, build.py paths fixed to v10. Serving on :8820.
## 09:17 MST — step 2 tests (test/sword10.py → shots/sword/v10.json, v10s_*.png; test/legshot10.py → shots/legs/v10_*, g_v10_*.png)
- Sword in hand (h=1, grip centre locked to the hand frame, 0 cm / 0° offset by construction, finger pose 'grip') in idle, walk, run, jump(+land), roll, crouch, crouch-walk, all 4 combo steps, kick, wave. Middle finger joints sit 2.5–3.6 cm from the grip axis (grip half-thickness ~1.5 cm + finger thickness), along-grip −3.7..+3.8 cm (grip is ±9.5 cm) → hand fully on the navy grip, guard above, pommel below. Switch to fists: 1 sheathe, back to sword: 1 draw. No page errors.
- Legs in game: thigh backs solid in all 8 angles × idle/walk/run/attack/jump; no black slabs between the legs.
## 09:22 MST — step 3: out/compare_v9_v10.png (= out/sheet_v10.png; rest-pose open-edge views v9|v10, in-game legs v9 vs v10 same frame/camera ×6, sword close-ups ×6) and out/motion.gif (98 frames, walk/run/jump/roll/crouch-walk/4-hit combo/sheathe/draw, test/motion10.py). Pushed as root sheet.png + motion.gif, v10/sheet.png, v10/motion.gif, v10/compare_v9_v10.png; v9/ copies untouched.
