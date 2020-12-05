---
title: Empires Mod 2.30.1 Released!
author: Smithy
date: "2020-12-05"
post_type: Source
post_type_future: ["Source", "Changelog"]
---


We're happy to announce version 2.30.1 which is live on Steam right now! 

Join us on Discord! https://discord.gg/EXwY2X7

Changelog:

## Features 

- Added a new particle effect for turrets disabled by concussion grenades.
- Changes to how re-equip functions:
- Re-equip will now restore the previously active weapon, provided the player still has the same weapon.
    - NOTE: Previously the active weapon would be changed to the weapon in slot 2 regardless of what the player was previously holding.
- Re-equip now saves the reload state of a grenadier's RPG & Mortar and restores the original reload state of the weapon, provided the player isn't swapping to be a grenadier. In which case they would both be provided requiring a reload before use.
    - NOTE: Spawning as grenadier is unchanged, both weapons will be provided already loaded.
    - Re-equip now saves the engineer kit charge and restores the original charge, provided the player isn't swapping to be a engineer. In which case the engineer kit would be provided empty.
    - NOTE: Spawning as engineer is unchanged, the engineer kit will still be provided with full charge.
- Sabotage progress will now reset after 2 minutes.
    - NOTE: Previously sabotage progress would persist forever, this wasn't ideal behaviour given some rounds can last over an hour.


## Bug fixes 

- Fixed some issues with burst fire weapons that prevented us from implementing them.
    - NOTE: Previously you had to hold the primary fire button to fire all of the burst. This is no longer the case, burst fire should now work exactly as expected.
- Fixed a mismatch between the client & server relating to vehicle bullets effects. Vehicle MG tracers on the client had different angles than on the server for vehicles with offset MG model attachments.
- Fixed a small mismatch between the client & server relating to player bullet effects. This was an issue that affected bullet spread, essentially there could be a slight variance which was caused by differing player velocity values on the client/server. This mismatch should now be significantly reduced.
- Fixed a small mismatch in bullet spread caused by the Rifleman's squad aura accuracy increase. Previously the spread reduction was only applied on the server, this is now fixed.
- Fixed a visual bug where bullet tracers were slightly offset from where they were supposed to emit from.
- Fixed a client/server mismatch relating to player angles whilst in vehicles. (Hitboxes on server would differ from what was represented on the client)
- Fixed a bug with weapon spread, where if you tried to attack while out of ammo it would still increase your weapon spread.
- Fixed a bug where squad arty could collide with the map's skybox if the ray cast was exactly 4000 units.
- Fixed refinery starting health not using the value defined in scripts.
- Fixed the bounding box being larger than intended for BE AFV model. This would cause mines to trigger even if the afv wasn't visually over the mine.
- Fixed the research description for the top icon not updating properly.
- Fixed some particle effects being assigned to the wrong model attachment. (Previously particles could only be correctly spawned from model attachments 1-32 due to a network restriction, however models can have up to 512 attachments and some of our vehicles have ~40)
- Fixed some occasions where the Hit Tick HUD element could get stuck on screen.
- Fixed the width of HudMessage HUD element not spanning the whole screen. This is sometimes used by server addons. (Sourcemod plugins)
- Fixed a crash when trying to apply damage to tanks using server addons. (Sourcemod plugins)
- Fixed a bug relating to engineer charge not regenerating correctly whilst holstered.
- Dead players no longer emit the resupply sound.
- Fixed an issue with missile having the wrong death notice icon.
- Commander placed turrets no longer float in the air slightly.
- Fixed an exploit caused by wall placement:
- Walls are no longer allowed to be placed if the preview model intersects with the local player.
- emp_unstuck is now only called if the player is actually stuck inside a building. Previously an unstuck would be scheduled regardless of if the player was stuck. In some circumstances this was being abused.
    - This change is primarily aimed to combat an exploit that was frequently being used by some players to reach areas of the map that weren't accessible through other means.
    - NOTE: These changes can be a little restrictive in the heat of battle. Depending on how these changes work in the short-term this behaviour may be tweaked in the future if the placement becomes a bigger problem than the exploit it resolves.


## Other changes

- Changed turret and wall gibs to last for 8 seconds instead of 15. They now fade out in the last 2 seconds instead of 3.
- Re-enabled the HUD hint element that is sometimes used by server addons. (Sourcemod plugins)


### Changes to player collision 
_This deserved a section all to itself this patch since a lot has changed._

- Set default "sv_turbophysics" to 0 (This is the default in games like TF2 and shouldn't be changed back to 1 as it may cause unexpected behaviour)
    - This changes physical interactions between players and objects in the game.
    - Players can no longer push objects with the use button. (This was rarely used in Empires anyway.)
    - Players now have physical mass which will influence other physics objects. (For example standing on tanks will affect the suspension)
- In a previous update the player "surrounding bounds" were changed to be automatically updated based on player hitbox positions.
    - This had positives and negatives
    - The biggest positive is that it fixed a problem where bullets would miss hitboxes entirely.
    - However because sv_turbophysics was previously set as 1 by default for Empires, this meant the surrounding box determined player collision. As a result, players often got stuck in each other. (For example if you jumped on top of someone while they were moving)
- So instead, with sv_turbophysics 0; the player collisions are separated from the "surrounding bounds" and resolves the player getting stuck. 
    - As a result of this we can go back to using a static size for the player "surrounding bounds". Which resolves bullets hitting hitboxes, without the additional calculations for determining the size. (Prone is still automatically determined by hitbox sizes because of the large width of the player in that stance.)


### Other changes made as a result of the above 
- Implemented various improvements to prevent players from getting stuck.
    - This includes an automatic check to fix a player stuck in a building.
    - New and improved system which should prevent players from getting stuck inside each other
- Some small changes to player movement to make things a little smoother.
- Eye offset positions for the player stances now transition a bit better and should feel a bit more polished.
- Prone now triggers on button press rather than release.
- Transitioning to prone is now slightly slower from the crouched position (Roughly 10%). (This is because it now maintains the same speed as transitioning from standing)
- Fixed some issues where eye angles would not be at the correct height. (An example of this is spamming crouch whilst transitioning to prone)
- Holding prone whilst in the air will cause you to start proning when you land.
- All projectiles now have a 0.25s grace period where they will not collide with players of the same team. (This includes grenades/missiles/shells for both players and vehicles)
- Fixed a potential client crash caused by console command "physicsshadowupdate_render"
- Tweaked player physical bounding box for the prone stance (Slightly taller than before)
- Tweaked player eye positions for the standing & prone stances to better reflect the actual position of the players head on the model.
    - The idea behind this is to prevent situations where enemies could shoot you but you couldn't see their head. (Like shooting over walls)
- Implemented a new system to prevent players from going prone under vehicles that are low to the ground.
- Vehicle road kills are now driven by the game engine's VPhysics system instead of the previous system which use raycasts to check road kills based on an timer/interval.
    - Using this new system any vehicle collision that occurred with the vehicle travelling over 15MPH will be a guaranteed road kill. If less than this the physics system will determine the damage. (Slightly scaled up for vehicles compared to other physics objects)
- Shooting physics objects previously didn't apply any force to them, this is no longer the case.
- Lowered force applied to players from taking damage (bullets/explosions etc)
- MG Turrets now apply a small amount of damage force but this shouldn't be game altering.
- Melee now applies a small amount of damage force, essentially the same as bullets already do.


## Script/Game Balance 

- Scout hide is no longer removed when taking damage.
    - This was added as part of a previous bug-fix but the behaviour isn't really ideal.
- Bullet spread distribution for players & vehicles is now circular rather than square.
- Tweaked fall damage distances to be more forgiving. (HL2 values were in use but Empires uses a higher gravity amount by default)
- Also tweaked the distance that the landing recoil effect triggers as this was too low and was being triggered when crouch jumping.
- Infantry Weapons' **Kick** has been changed to **push the player's view up** instead of to the top right.

## Hardcore Empires Scripts

Hardcore Empires changes have been merged into the base game. They will be used as a new foundation to build upon.
The changes below are a comparison with pre-2.30 Empires.

### Economy 

- Reduced **Wages** rewarded for performing various actions.
- Disabled **Player Refinery Multiplier**.
- Adjusted **Resource Node** values for several maps.


### Research 

- Researching now cost **resources**.
- Adjusted the time it takes to research new **chassis**.


### Vehicle Engines 

- Implemented **Horsepower Health Penalty**, your vehicle will now lose horsepower as your chassis gets damaged.
- Several changes to **Heat Dissipation** have been made to give each engine a more defined identity.
- Adjusted the engine prices for all chassis.
- Adjusted the weight of **Armored Personnel Carrier** engines to be in line with the other chassis.


### Vehicle Armors 

- All Vehice Armors now have a small **Angle Modifier**.
- All Vehice Armors now have a small **Damage to Heat Absorbed**.
- **Plain Armor** health has been significantly increased, it shall henceforth be known as **Cardboard**, **Paper** no longer!


### Vehicle Weapons

- **Missiles** have had their speed increased.
- Buffed **High Explosive Cannon**.
- **Upgraded Missile Launcher** is back to its original form.
- **Standard Artillery Cannons** are back to their original form.
- **Anti-Tank Machine Guns** have been adjusted to be better balanced.
- **Biological Machine Gun's Damage Over Time** has been reduced significantly.
- **Grenade Launchers** now have their own **Damage Type** and have had their damage against **Buildings** increased.

### Vehicle Chassis 

- **Jeeps** can now have a single plate of armor. Their cost has been increased.
- **Armored Personnel Carriers (APC)**:
- **Cost** has been increased to better match their impact on the game.
- **Weight** has also been slightly increased to compensate for the engines' weight.
- The **8th seat** is back!

### Buildings 

- Increased **Buildings** resistance against **Biological Cannons**.
- Increased **Buildings** resistance against **Plasma Cannons**.
- New Resistance against **Grenade Launchers** (see Vehicle Weapons).
- Increased **Turrets** resistance against **Biological Cannons**.
- Increased **Turrets** resistance against **Plasma Cannons**.
- Decreased **Turrets** resistance against **Concussion Grenades**.
- **Missile Launcher Turret** have had their Missiles' speed and accuracy increased.
- **Walls** cost has been reduced.
- **Walls** resistance against **Seismic Grenades** has been reduced.
- **Walls** resistance against **Infantry Missile** has been reduced.


### Infantry Weapons 

- **Headshot Multiplier** has been significantly increased across the board.
- **Leg and Arm Multipliers** have been increased.
- Falloff Damage has been tweaked.
- Fixed multiple inconsistencies with weapons' accuracy.


### Infantry Resists 

- Reduced Rifleman's Infantry Bullet Resist.
- Decreased resistance against Depleted Uranium Machine Gun.
- Increased resistance against High Explosive Machine Gun.


