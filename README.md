<img width="1535" height="1024" alt="88f13f1f-42ce-4760-abaf-684810e075e2" src="https://github.com/user-attachments/assets/7ca0ae17-1057-4608-8d28-c5303ce17b35" />
BTR-And-Heli-Total-War

BTR and Heli Total War
The BTR's autocannon and the Helicopter's rocket pods now hunt every NPC on the map. Bandits, Guards, Military, the Punisher — none of them are safe. Anyone caught in the open eats steel.
Tired of watching enemy patrols stroll past the heli while it lazily circles waiting for you to make noise? Sick of the BTR ignoring an entire squad of bandits ten meters from its turret because they're not on the Player team? This mod turns both of them into equal-opportunity area-denial weapons.
---
Features
Total War on the AI
BTR turret scans for the nearest hostile AI within 120 m every second. If the AI is in the turret's firing cone and there's clear line of sight, the tower tracks and the cannon opens fire. Direct hits deal 50 damage to the Torso hitbox via the AI's own `WeaponDamage` damage pipeline — so wounds, bleeding, and death animations work normally.
Helicopter scans for AI within 350 m every second, with a 25% engage chance per tick. When it spots a target, it transitions into its full vanilla Attack state: bank toward the target, line up the rockets, fire the salvo.
The player always takes priority — NPCs are only engaged when the player isn't in the BTR's cone or the heli's sensor.
Helicopter Patrols Forever
Vanilla heli does one attack run and flies off. This version returns to Patrol after the attack, picks a fresh waypoint, then becomes available for another engagement after a configurable cooldown.
The rhythm: Patrol → Spot AI → Attack → Rockets → Patrol → (cooldown) → Spot AI → Attack → ...
It only despawns naturally when it drifts 2 km from the map origin (same as vanilla `DistanceClear()`).
F5 — Spawn Helicopter On Demand
Press F5 (rebindable in MCM) to immediately spawn a helicopter using the game's own `EventSystem.Helicopter()` event. Shows the vanilla red "Helicopter Spawned" message at the bottom of the screen, just like other dynamic events. Great for testing or just wanting to see the heli wreck a bandit camp on demand.
MCM Integration (Optional)
If you have the Mod Configuration Menu installed, you get three in-game settings:
Helicopter Patrol Length — float multiplier from `0.25` to `5.0` (step `0.25`). `1.0` is vanilla (10-second rotations). Crank it up if you want the heli to loiter longer between waypoints.
Helicopter Engagement Cooldown — float from `0` to `60` seconds (step `5`, default `15`). How long the heli must patrol between attacks before it can lock onto another NPC. Set to `0` for instant re-engagement, `30+` for a calmer "patrol with occasional engagements" feel.
Spawn Helicopter Hotkey — rebind the spawn key to whatever you want.
If you don't have MCM, the defaults (F5 hotkey, 1.0 patrol multiplier, 15s cooldown) work without any setup.
---
Requirements
Metro Mod Loader (RTVModLib-compatible) v3.0.1 or newer — required
Mod Configuration Menu (MCM) — optional, for in-game settings
---
Installation
Install the Metro Mod Loader.
Drop `BTR and Heli Total War.vmz` into your `<game>/mods/` folder.
(Optional) Install MCM to configure patrol length, engagement cooldown, and rebind the spawn key.
Launch the game and enable the mod from the loader UI.
---
Technical Notes (For Mod Authors)
This mod uses the RTVModLib hook API exclusively — no `[script_extend]`, no file replacement, no monkey-patching. A single autoload registers 7 hooks against vanilla `BTR.gd` and `Helicopter.gd` methods:
Hook	Type	Purpose
`btr-sensor-post`	aspect	Hijack `playerVisible` to make vanilla Fire engage NPCs
`btr-turret`	replace	Aim tower at NPC; falls through to vanilla otherwise
`btr-raycast`	replace	Route damage to AI on hit; falls through to vanilla otherwise
`helicopter-_ready-post`	aspect	Apply MCM patrol-length multiplier
`helicopter-_physics_process-pre`	aspect	NPC sensor + cooldown + state transition
`helicopter-attack`	replace	NPC-aware Attack phases with clean Patrol return
`helicopter-firerockets`	replace	Aim rockets at NPC when locked
Per-instance state is stored via `Node.set_meta()` so multiple BTRs / helicopters work independently. All replace hooks fall through to vanilla when no NPC is locked, so vanilla behavior against the player is preserved exactly. Should compose cleanly with any other mod that hooks the same scripts via `-pre` / `-post`.
The Attack-to-Patrol transition skips vanilla's phase 4 pull-up animation because lerping `rotation_degrees.x` near Euler gimbal lock (which is where `look_at` lands when firing on a ground target) produces unstable orientations. Instead we hard-reset orientation: clamp Y back to `flyHeight`, pick a new horizontal waypoint, snap to look at it level, restart Patrol.
---
Credits
Author: Dead Nasty
Built with: Metro Mod Loader (ametrocavich), Mod Configuration Menu (Doink Oink)
Thanks to: the Road to Vostok modding community
---
Changelog
v1.0.0
BTR and Helicopter engage hostile NPCs (Bandits, Guards, Military, Punisher)
Helicopter returns to Patrol after Attack for repeated engagements
Configurable post-attack cooldown (default 15s) keeps the rhythm calm
F5 hotkey spawns helicopter with on-screen confirmation
MCM-configurable patrol-length multiplier, engagement cooldown, and hotkey rebinding

<img width="400" height="225" alt="Timeline 1" src="https://github.com/user-attachments/assets/897b54e9-2b62-4e36-823c-1513afd628db" />
