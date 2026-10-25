# v8 [pt480u] — add Alaric — started 9:09 PM Phoenix
Chumi: "Awesome now add alaric". Folder /workspace/seraphina_fortnite_v8 (copied from v7; v7 PROGRESS kept as PROGRESS_v7.md). Alaric work in alaric/.
Plan: concept (GenerateImage, ref alaric-character-sheet-main-style.png, Fortnite style like Seraphina, T-pose, no sword) -> Meshy i2t3d+rig (<=~30cr, check balance) -> Blender (cloak weights spine/chest, sword separate/free, hip+hand) -> game switch button, sword+fists, 3-4 move fixed combo -> live verify 390x844 -> sheet/gif/PC copy.
Rules: never delete, Meshy only once, no self-PASS, no BotCampus, keep Seraphina = v7, root = v8, v7 at /v7/.
## Log
- 9:10 PM — concept r1 alaric/concept_r1.jpg: face/hair/skin/colors match sheet, T-pose, no sword. Drift: eyes read brown not amber-gold; small brown sword-frog strap on belt. Trying r2.
- 9:11 PM — concept r2 ACCEPTED (alaric/concept_r2.jpg): amber-gold eyes, curly dark-brown hair, tan skin, black/gold plate, navy gold-trim cloak, winged clasp+chest emblem, brown belt, black boots, no sword/strap. Meshy balance BEFORE = 2677 credits. Plan cost: image-to-3D meshy-7.1 (same params as Seraphina) ~30 + auto-rig 5 = ~35.
- 9:13 PM — Meshy image-to-3D task 01a17141… submitted (meshy-7.1, 4k PBR, remesh 40k tris, t-pose; same params as Seraphina) — task reports 30 credits. alaric/meshy_poll.py polls, downloads, then auto-rigs (height 1.8 m).
  Design: one GLB per hero; game loads both and swaps globals (model/mixer/BONE/W/cape chains/fingers/POSE0) on a switch button. Seraphina GLB untouched (v5 glb). Alaric: Sword (navy grip, gold guard) + Scabbard on a 'Scabbard' bone at left hip; modes sword + fist.
  Sword combo candidates (KayKit, CC0): 1H_Slice_Diagonal, 1H_Slice_Horizontal, 1H_Stab, 2H_Chop finisher (two-handed longsword). Blender = /home/box/blender/blender-4.2.3-linux-x64/blender.
- 9:14 PM — Meshy model SUCCEEDED (alaric/meshy_raw.glb + 4k textures + thumb). Thumb matches concept (curly dark hair, black/gold plate, navy cloak, belt, boots). Rig submitted (rig_task.json).
- 9:15 PM — Meshy rig SUCCEEDED → alaric/rigged.glb. Balance 2677 → 2642 = **35 credits used** (30 model + 5 rig). No more Meshy calls.
- 9:21 PM — Rig inspected (Meshy names, 1.80 m, front = -Y, hands x±0.64 z1.33, fingertips x±0.83). Model skin read pale (tex mean sRGB 218,164,131) vs ref "tan skin" swatch (164,108,61) → recolored head/neck skin texels only (UV mask of head faces × skin hue) to ~(176,116,74): alaric/tex_base_tan.png. Cloak = navy-seeded flood (excl. legs/arms/front), hair = brown texels z>1.5 (curls 1.53–1.79 m). Back emblem missing on Meshy cloak → will add a decal (emblem cropped from his own chest texture).
- 9:23 PM — Texture done: tan skin + gold winged-sword emblem painted on the cloak back (cut from Chumi's own sheet back view → alaric/alaric_emblem.png; folds kept) → alaric/tex_base_final.png. Check render alaric/views/tan3.png (front/back/face: amber eyes, curls, tan skin, emblem). Next: tools/build_alaric.py.

## 9:28 PM MST — Alaric rig build (no-anim test) OK
- tools/build_alaric.py: 20527 verts; cloak 5259 verts on 5×5 cape bones weighted ONLY to Spine/Spine01/Spine02/neck (no leg/arm weights); curls 3×6 hair chains (2184 verts); finger bones 4×2 + thumb ×2 per hand (603 verts); hip Scabbard bone (non-deform) with modelled longsword (0.80 blade, 0.19 navy grip w/ gold bands, curved gold quillons, gold pommel) + black scabbard w/ gold throat/chape, on his left hip.
- Pose check renders: alaric/views/pc1t_grid.png (rest), pc1p_grid.png (legs/spine/cape/finger posed). Cloak follows chest, legs move free of cloak; fingers curl. Scabbard tip pokes slightly below cloak hem at back.
- Next: full build with clips → glb/alaric_fortnite_v8.glb

## 9:30 PM MST — Alaric full GLB built
- glb/alaric_fortnite_v8.glb 5.47 MB, 40 clips retargeted (same CC0 UAL + KayKit set as Seraphina + 5 extra KayKit sword clips: 1H Chop, 1H Stab, 2H Chop, 2H Slice, 2H Stab).
- Next: probe sword clip timings, then game two-character code.

- 2026-10-24 21:38 MST v8 game code: two-character kit swap (Seraphina/Alaric) + sword draw/sheathe + 4-move sword combo (diag > back > thrust > cleave) + top-centre character button. Built web/site/index.html (666,019 B). Local smoke test (390x844 touch, port 8818): Alaric loads in background, button switches both ways, walk uses ual_Walk_Loop, combo plays all 4 moves in order, sword drawn (u=1) in combo and sheathed after, no page errors. Shots: shots/s1_grid.png, k1_idle_grid.png, k1_cb_zoom.png.

- 2026-10-24 09:59 PM MST local audits (port 8818, 390x844 touch): seqtest8 Alaric sword, 5 tap patterns -> always diag > back > thrust > cleave (fixed order). comboaudit8 found a hit-stop knee pop in Alaric's cleave (legs 140998 deg/s for one frame: during the .07 s hit-stop the mixer re-applied the raw clip while foot IK was skipped). Fix: holdFeetIK() re-applies held foot IK during hit-stop, guarded CHAR.cur==='alaric' so Seraphina's code path is unchanged. Re-audit al_mash2: no leg spikes, root travel .31 m, max root speed 1.24 m/s, grounded slide L7.7/R16.2 cm over the whole 4-move combo. Note: Seraphina v7 has the same pre-existing hit-stop pop in Jump_Chop (RightUpLeg 25022 deg/s), left untouched per 'keep Seraphina exactly as v7'. Walk (loaded box, 4 browsers parallel): Alaric 2.0 m/s 15.6 cm/m, Seraphina v8 15.8 cm/m; v7 baseline 11.4 cm/m -> recheck live unloaded.

- 2026-10-24 10:00 PM MST EARLY PUSH LIVE: commit f4f17df. Root / and /v8/ index md5 79c0c8e3 (= local build), glb/alaric_fortnite_v8.glb 879c29f5 (= local), /v7/ still e0349e61 (v7 untouched). Added only (no deletes). https://nasoteisoy.github.io/seraphina-fortnite/

- 2026-10-24 10:17 PM MST LIVE check #1 (Alaric, 390x844 touch, live root): switch both ways OK (alaric/sword -> sera/bow), sword combo diag>back>thrust>cleave, combo foot slide L0/R0.2 cm, root .31 m (lunges), walk 2.0 m/s 12.7 cm/m, run 5.0 m/s 0.5 cm/m, 1 jump, 0 errors. FOUND (frames shots/phone/al_walk_strip.png): big black slabs between his legs when striding + scabbard tip at shin height. Cause (weight-colour renders alaric/views/skirt/dom_grid.png): cloak weighting had grown into the dark-navy boots/shins (cloak bones on boot verts) + Meshy web faces welding cloak hem to the legs. Fix in tools/build_alaric.py (backup build_alaric_v8a.py): exclude boot/foot verts and near-shin verts below the hem from the cloak; remove the 427 thin web faces (0.106 m2) joining cloak- and leg-weighted verts (alaric/an/capesplit_lib.py cape_leg_unweld); scabbard throat 0.975->1.0 m, angle (.16,.34,-1)->(.14,.5,-1) so tip sits ~0.24 m up near the hem instead of the shin. Test renders alaric/views/skirt/e_grid.png: slabs gone. Now: full rebuild -> glb/alaric_fortnite_v8b.glb (old v8 glb kept).

## 22:50 MST — v8b GLB wired in + scabbard sweep (local)
- full_b rebuild OK: no Traceback, alaric_fortnite_v8b.glb 5.46 MB, 40 clips, cape verts 4838, cape_leg_unweld removed 427 web faces (0.106 m²), 1232 boot verts kept out of the cloak. Old v8 GLB kept.
- Game now loads glb/alaric_fortnite_v8b.glb (build.py copies both).
- Walk strip (shots/phone/al_walkb_strip.png): the black slabs between his legs are gone. Walk 2.0 m/s, 13.9 cm/m slide.
- Scabbard tip was still 7–12 cm BELOW the cloak hem (tip .17–.24 m vs hem chain end .24–.34 m). Added a runtime 15° back-sweep of the Scabbard bone (about his side axis, re-applied after each mixer update, never stacked — some clips key that bone, some don't). Probe (test/scabprobe8.py, shots/scab/): idle tip .265 vs hem .228; walk tip .31–.35 vs hem .28–.36. Tried 8/12/22°: 22° stuck out behind the cloak, 12° still at hem.

## 22:55 MST — local check of v8b + 15° sweep
- Picked the 15° sweep (best from the 0/8/12/15/22° probe). Default probe run (no override): idle tip .265 m vs hem .228; walk tip .31–.34 vs hem .28–.36.
- close8 k3 combo frames (shots/k3_cb_grid.png): seq diag>back>thrust>cleave, no slabs, sword drawn for all 4 moves, errs [].
