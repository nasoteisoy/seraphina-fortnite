# v9 — Alaric face: shorter jaw/chin + confident smirk  [pt481u] "Alaric face is too long and he isnt smerking"
Rules: never delete, no Meshy (credits 2642 untouched), no BotCampus, no self-PASS (Chumi judges). v8 stays at /v8/, root = v9. (v8 log kept as PROGRESS_v8.md)

## 23:25 MST — workspace
- Copied /workspace/seraphina_fortnite_v8 → /workspace/seraphina_fortnite_v9 (all v8 files kept in both).

## 23:33 MST — step 1: measurements (free) + where the length comes from
- Refs cropped: face/ref_front_head.png (main sheet FRONT), face/ref_smirk.png ('CONFIDENT SMIRK'), face/ref_main_expr.png.
- v8b close-ups (Blender, tools/faceview.py, ortho 0.40 m / 1000 px = 0.4 mm/px): face/v8_front.png, v8_q3.png, v8_side.png (+ v8_front_grid.png).
- v8b landmarks (z, m): hairline 1.672, brow 1.624, eye line 1.601, nose tip 1.546, mouth 1.524, chin 1.471.
  eye→chin 130 mm vs eye→hairline 72 mm = 1.81 (sheet front ≈ 1.42); mouth→chin 53 mm = 2.4× nose→mouth 22 mm (sheet front ≈ 1.3×); face W/H (cheek width 160 mm / hairline→chin 202 mm) = 0.79 (sheet front ≈ 0.88).
  → The length is the chin/jaw below the mouth (mouth→chin), not the neck or forehead. Side view: chin hangs low and forward.
- Mouth: symmetric gentle smile line painted in the texture; mesh mouth is nearly flat. Texture density only ~2 px/mm at 4096 (≈1 px/mm in the 2048 GLB).

## 23:36 MST — step 2/3: free mesh edit + texture tweak (tools/face_v9.py on v8b.blend; only face vertex positions + a new texture file)
- Lower face: verts below the mouth line pulled up toward it (s=0.31 → chin up 16.7 mm), fading out under the chin into the neck (35 mm), toward the jaw angles (|x| 45→95 mm) and on non-Head-weighted verts (hair/collar/cloak untouched: moved_nonface 0, 345 verts moved).
- Smirk on HIS LEFT (+x): mouth corner up 5.7 mm/out 1.2 mm, cheek lift up 3 mm/forward 3 mm, brow up 2.5 mm; his right corner down 2–3 mm. Texture: short tapered crease continuing the left corner upward (painted at 4096 via 3D→UV ray lookup; first try was misplaced on quads — fixed with per-triangle lookup), saved as NEW alaric/tex_base_v9.png (tex_base_final.png untouched).
- Trial renders face/try/t1..t4 (+ c1/c2 compare, mouth2/3/4 strips).

## 23:46 MST — step 4: rebuilt GLB + game
- glb/alaric_fortnite_v9.glb (5,461,056 B; v8b 5,460,940 B): same 93 nodes / 40 clips / skin / 5 materials / accessor count as v8b; only face verts + base texture (tex_base_v9) differ. Final params: cup .0065 csx .012 csz .009 rdn .003 kup .003 kfw .003 bup .0025 cdark .72 (face/build_v9.log, glb/alaric_fortnite_v9_info.json). v8/v8b GLBs + .blend kept.
- After (same Blender close-up, face/v9_front.png, v9_front_grid.png): chin z 1.471→1.454 (-16.7 mm); eye→chin 130→114 mm; eye→chin / eye→hairline 1.81→1.58 (sheet ≈1.42); mouth→chin 53→38 mm; face W/H 0.79→0.86 (sheet ≈0.88). Smirk: his-left corner +5.7 mm with upward crease, his-right corner -3 mm, left cheek +3 mm, left brow +2.5 mm. Compare: face/try/c9.png.
- Game: ALARIC_URL → glb/alaric_fortnite_v9.glb, title "Seraphina & Alaric v9"; build.py/tests repointed to v9 paths (v8 copies kept: src/_tail_v8.js, _head_v8.js, template_v8.html). node --check OK, index 0.81 MB.
- Local (8819) live8 Alaric 390x844 touch: switch both ways OK, sword combo diag/back/thrust/cleave, walk 13.3 cm/m (v8 local 12.1, v8 live 14.3), run 0.2, crouch 33.0 (v8 30.5–33.7), 0 page errors.
- In-game close-ups test/faceshot9.py → shots/face/cmp_full.png (v8 | v9 front, v8 | v9 3/4): shorter chin + smirk read at phone size.
- 23:46 MST — step 5: early push (v9/ + root index + glb/alaric_fortnite_v9.glb; v8/ untouched, nothing deleted).
- 23:47 MST — face/face_compare.png (sheet | v8 | v9, front / 3/4 / in-game + numbers) and out/sheet_v9.png (face strip on top of the v8 sheet) → repo root sheet.png + v9/sheet.png + v9/face_compare.png; v8/sheet.png untouched. motion.gif unchanged (body motion identical; v9/motion.gif = v8 copy).

## 23:56 MST — step 6: live verification (https://nasoteisoy.github.io/seraphina-fortnite/, Pages built 01e85d2)
- Live MD5 = local: index.html/v9/index.html ba3fa95c…, glb/alaric_fortnite_v9.glb 6d28380b…, sheet.png 1d81978c…, v9/face_compare.png eaa2a0e5…; v8/index.html 42e68ad1… and v8/sheet.png 748b2c19… unchanged.
- Live 390x844 touch, title "Seraphina & Alaric v9", 0 page errors. Alaric: switch both ways OK, combo diag/back/thrust/cleave, walk 11.8 cm/m (v8 live 14.3), run 0.4, crouch 33.1 (v8 33.7). Seraphina: combo cross/spin/dash/xfinish root 160 cm (v8 160), walk 11.6 (v8 11.8), crouch 30.7 (v8 30.3) → not regressed.
- Live face close-ups: shots/face/live9_close.png, live9_q3.png (live9_pair.png) — shorter chin + left-corner smirk visible at phone size.
- Meshy: NOT called (balance still 2642). A Meshy regen from a new smirk concept would cost ≈35 credits (image-to-3D 30 + auto-rig 5, same as v8) + the free rebuild pipeline (cloak/curls/fingers/scabbard re-fit) — only if Chumi asks.
- PC copies: alaric\fortnite_v9\ (glb, blend, info, tex_base_v9.png, sheet.png, face_compare.png, face_v9.py, PROGRESS.md); seraphina\fortnite_v9\ (index.html, glb\alaric_fortnite_v9.glb, glb\seraphina_fortnite_v5.glb, PROGRESS.md). Not self-passed — Chumi judges.
