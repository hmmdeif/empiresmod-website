---
title: Empires Mod 2.30.2 Released!
author: Smithy
date: "2020-12-11"
post_type: Source
post_type_future: ["Source", "Changelog"]
---


We're happy to announce version 2.30.2 which is live on Steam right now! 

{{< rawhtml >}}
<iframe src="https://store.steampowered.com/widget/17740/" frameborder="0" width="646" height="190"></iframe>
{{< /rawhtml >}}

Join us on Discord! https://discord.gg/EXwY2X7

Changelog:

## Bug fixes
- Fixed a rare crash that was caused by weapons
- Fixed a networking issue related to rotating the command view
- Fixed a bug causing spawns to appear blocked by team mates
- Fixed a bug that caused player movement to be slowed down when leaving the command vehicle
- Fixed an issue where players could be launched into the air by vehicles
- Players that get stuck inside enemy vehicles are now killed instead of attempting to apply force to get them out (Fixes roadkill not working 100% of the time)
- Player ragdolls now have some initial force applied to them when the player dies, previously the ragdoll would just fall to the ground.
    - Since these are client-side, you would actually see your own ragdoll have some initial force in some situations (Like roadkills); this just makes the behaviour more consistent for everyone

## Script/Game Balance
- Reduced the total explosion force applied to vehicles
- Vehicle wheel road-kills are now triggered when the vehicle is going over 5MPH, previously 2MPH. (2MPH turned out to be too low)
- Seat 2 of BE medium is now standing stance instead of crouch
- Reduction in cost for AFV/LT (-50) and APC (-100)
- Increase in cost for commander turrets(+25)
- Reduction in cost for Armouries (-25)
- Re-introduction of the BE Burst Rifle and Machine Pistol, damage and magazine sizes increased to compensate
- Head-shot multiplier on all weapons reduced to 2x (3x for Scout rifles)
- Bullet damage falloff distance on all weapons changed from 5000 units to 10000 units, minimum damage changed from approximately 75% of max damage to 50% max damage
- Concussion grenade damage increased from 20 to 40 
- Shotgun damage per pellet increased from 15 to 20 (all 3 shotguns)


