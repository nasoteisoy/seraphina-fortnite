# Seraphina v7 [pt479u] — started 3:49 PM Phoenix
Copied from v6 (web/test/tools/glb). Server 8817 = v7. Build: cd web && cat src/_head.js src/_mid.js src/_tail.js > src/game.js && python3 build.py
Chumi: "her dager attack movement is weird" / "WHAT HAPPEND TO HER WALK MOVEMENT looks so bad"
Tasks: 1 walk = exact v5 clip ual_Walk_Loop, faster timeScale (~1.25-1.35x v5), belt-matched, ~1.9-2.0 m/s; 2 dagger combo frame-by-frame audit + fixes, deterministic order; 3 keep all v6 fixes; repo: v6 -> /v6/, root = v7.
Rules: never delete anything (incl. duplicates), Meshy 0, no BotCampus edits, no self-PASS.

## Status log (Phoenix times)
- 4:40 PM — WORKING BUILD LIVE: repo commit 88f490c pushed, Pages built, live root index MD5 e0349e61… = web/site/index.html. v6 copied to repo /v6/ (index 9afcba5a, sheet 1880bebb, README 6022fb09, flaws ef620de5, motion.gif 4aab887a, glb 72818e74). README has v7 section + v6 link.

### WALK (done in code)
- CLIP.walk back to 'ual_Walk_Loop' (exact v5 clip). belt .775, MOVE.walk 2.0, lim.walk [.7,2.7] -> rate 2.58 vs v5 1.96 = 1.32x timeScale.
- belt sweep (test/belt7.py, 2.5 s straight walk): v5 1.55 m/s slide abs L28/R21.9 cm; v7 belt .79 L38/R23 (signed +10 drift); .775 -> 1.99 m/s L34.6/R26.1, signed -3.3/+3.1 (chosen); .76 L40.7/R18.2.
- Phone 390x844 local (test/walkcmp7.py): v5 1.55 m/s, cycle .697 s, 13.1 cm planted-foot slide per m; v7 2.00 m/s, cycle .530 s, 13.4 cm/m (same quality, 29% faster). Frames shots/phone/w5_*.png, w7_*.png. Looks natural (not frantic) in frames.

### DAGGER COMBO audit (v6, test/comboaudit7.py + clipprobe7.py + c6a frames)
What was weird: (1) cross used Dualwield_Slice whose real slash is a backward hop at clip .55-.85 but it was cut at .735 -> she froze mid back-hop while lunging forward; hit fired before the swing. (2) spin used 2H_Spinning: crouched greatsword spin, hands glued 32 cm apart; clip ended before step end -> idle pose flashed in for .04 s before the stab. (3) dash-stab slid 1.1 m during the crouch wind-up (7.3 m/s, feet planted). (4) leap X: clip already jumps 1.34 m, code added another .55 m hop -> ~1.9 m float. (5) slow-mo snapped to 30%. (6) leg pops 3000-7700 deg/s: foot lock released instantly when a foot lifted; head/feet IK piled up on a clip's held last frame (three.js does not rewrite unchanged bones) then snapped. (7) order varied: taps < .05 s into a step ignored + done-window logic.
### DAGGER fixes (done in code, _tail.js)
- New DCOMBO with clip start offsets, hit on measured fastest-hand frame, chain point, lock, 0.15 s crossfades, never through idle: cross=Dualwield_Chop (2 hits), spin=2H_Spin (real 360 w/ steps, reverse grip), dash=Dualwield_Stab + .7 m hop on the thrust, xfinish=1H_Jump_Chop with jump flattened 45% (LEAPY table) + .9 m travel only while airborne.
- Deterministic: every tap during a step buffered; always i -> i+1 (finisher -> cross); reset only after recovery + .35 s idle. WS.seq logs order.
- Eased slow-mo (sin envelope, .34 s @ 45%), hitstop .07, DREV blend 11/s, snapYaw 12x clamp 8 rad/s.
- Feet IK: eased release (F.last relative to root), re-plant at eased spot; POSE0 restore of clean anim pose before mixer.update each frame.
- Audit after (m7c): timeline cross>spin>dash>xfinish>cross, no idle flash; head/neck spikes gone; leg spikes only at leap landing (clip slam, 4075 deg/s 1 frame); max root speed 5.3 (was 7.3); planted slide 25 cm total (spin pivots 15).
### Regression (local, after fixes): bowprobe same as v6 (bottom tip in cape idle/walk; out only when cape flaps on run), jump 1 root peak key+touch, roll v=0 feet still after, handbow grip constant .105.
### NEXT: seqtest7 (touch tap patterns) live; live walkcmp v5 vs v7; combophone7 strip live; sheet.png; motion gif; PC copy; final commit.
- 4:52 PM — seqtest7 LIVE touch (#atk taps): mash .12s 25 taps, strip 6, slow .6s 7 -> always cross>spin>dash>xfinish>cross... (jitter/double patterns cut by 700s timeout). Deterministic.
- 4:52 PM — started live phone runs: walkcmp7 L5 (/v5/) & L7 (/) D=4.2 full body, combophone7 LC7 390x844 (logs /tmp/L5.log L7.log LC7.log; frames shots/phone/L5_*, L7_*, shots/motion/LC7_*)
- 5:02 PM — LIVE walk phone 390x844: v5 /v5/ 1.55 m/s cycle .697 s slide L25.0/R22.9 abs = 12.5 cm/m; v7 / 2.00 m/s cycle .530 s (timeScale 2.581 = 1.32x v5) slide L31.0/R23.8 = 11.1 cm/m (less slide per meter than v5). Both ual_Walk_Loop. 0 page errors.
- 5:05 PM — LIVE combo phone 390x844 (real #atk taps): seq cross>spin>dash>xfinish, planted slide L0.2/R8.3 cm (spin pivot 6.9), root travel 1.58 m, slow-mo 1, 0 errors. Frames shots/motion/LC7_00..39.
- 5:06 PM — built sheet.png (2612x1600: v5 vs v7 walk 16 frames each + 16-frame live combo strip + numbers) and motion.gif (72 frames: v7 walk x2 + full combo).
