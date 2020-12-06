---
title: Empires UE4 Progress, November 2020
author: RoyAwesome
date: "2020-12-05"
post_type: UE4
---

## Project Update

The project is humming along nicely!  After a brief slowdown back in September, we’ve made huge amounts of progress in November, and even hit a major game development milestone… Empires UE4 is playable from start to finish!  That’s right, you can hop into a game, play it, and then once a victory condition is achieved, the match ends and the server goes to the next map.  

Building the game loop was a huge part of the current milestone, and we are in the final stretch of closing out Milestone 2.  To recap what Milestone 2 contains:



*   Players are able to be placed into squads, and all the classes are created.  **(DONE)**
*   Players can select a spawn point (although the spawns can be manually placed at this point).  **(DONE)**
*   Many of the easy skills, utilities, and grenades are created.  **(MOSTLY DONE)**
*   Players can sprint and crouch.  **(DONE)**
*   Players can interact with ammo containers to get ammo.  **(NOT DONE)**
*   Teams have tickets, and when a team hits 0 tickets, the game ends. **(DONE)**

As you can see, our task list for this milestone is getting pretty small.  Here is a snapshot of all that remains in our project board, hacknplan:

{{< image-proc hacknplan.png Resize "400x" >}}



Once this phase is complete, I think we’ll take a short period of time to polish up what we have and make it playable, and then may begin some playtests with larger numbers of people!  That will probably be sometime early next year, but it’s on our radar!

Outside of just doing every task, we’ve spent a lot of time discussing gameplay features and how to modernize not just the look of the game, but the feel of it as well.  We’ll discuss both of these aspects in the Programming and Art updates, but keep in mind that all of this is early development, and everything is being produced with the ability to change it all if it doesn’t end up fun or Empires-y.  I ask that you all keep an open mind toward these changes, and get excited about the possibilities that exist within UE4!


#### Programming


### Gameplay Programming - Roy Awesome

November was a banner month for development.  I finished up Teams and Squads around the middle of the month, and launched straight into getting the gameplay loop working.  The Gameplay loop is one of the elements that we stand to gain the most flexibility with the UE4 port.  In the Source version of Empires, the objectives of the game (Gather resources, Drain the other team’s tickets, kill their command vehicle) is very hard coded into the very fabric of the mod.  It is very difficult to change what happens when a team runs out of tickets, or when the command vehicle dies.  

<video controls>
  <source src="./squadnames.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video> 

_Creating a squad with a custom name_

For the UE4 port, I decided that hard coding the gamemode into every portion of the game would be a poor way to execute this project.  It took quite a bit of time to design out a technical system that didn’t fall into that trap, but I feel that I have figured out a good method for it.  Empires game modes are now entirely event driven, and both Teams and the Game Mode now listen to events and react to them, and can even send events.  

For demonstration and early development, I have implemented “Conquest”, the Battlefield gamemode.  The design is simple, Both teams have a certain number of tickets (I set it to 100 for now), and when one team hits 0 tickets, the other team wins.  A ticket is subtracted from the team when a member of that team respawns.  It’s a simple design and easy enough to prove the underlying event driven gamemode system.  To implement this, as I mentioned last month, I gave the Teams an Ability System Component, or ASC.  To each team, I gave them an Attribute Set that contained that team’s Tickets.  I created an Ability to give to each team that passively waits for a respawn event, and watches that Attribute set for a change in Tickets.  When the team gets the Respawn event, it checks if the player respawned after being killed, ignoring revives and first-time spawns.  If it's a valid event for subtracting a ticket, it does so.  When the Tickets change, I fire off an event to all listeners (in this case, Teams and Gamemode) that says “Hey, my tickets changed”.  The gamemode has an ability that listens for this Ticket Changed event, and watches the value.  When it hits 10 tickets, it sends a chat message to the server with “{Team} nears defeat!”, and when that team hits 0 tickets, the game ends and a winner is determined.  For conquest, the winner is determined by whichever team has the highest tickets.  

This system proved to be extremely successful in implementing game events and allowing for the flexibility and ease of modification that is one of the core pillars of this project.  I am sure many of you are starting to imagine how the Empires Classic gamemode would be created, listening for a “Vehicle Destroyed” event and ending the game if that vehicle is a Command Vehicle.  I really hope to ship Empires with a variety of gamemodes for you all to enjoy, and for modders to create even more interesting things with this system.

<video controls>
  <source src="./roundend.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video> 

_Game round flow, including a work in progress end sequence.  It even plays music!_

While that was a big task, I got even more things done this month.  In November, the plugin Easy Ballistics was released for free on the Unreal Engine Marketplace.  I had heard good things about this plugin and how flexible and feature complete it was, so it becoming free was a great opportunity to try to implement it into Empires, so that way we have ballistics features for our infantry and vehicle combat.  It took me about 3 hours to integrate it into our gun system, so now we have the ability to create fully physically simulated bullets, with travel time, bullet drop, ricochet, wind/atmosphere pressure, and more.  Empires is an extremely arcade-y shooter, so we will likely not take advantage of many of these features, it’s nice to have them for modders and maybe we can experiment with them in the future.  Travel Time and Bullet Drop will definitely be used though, as Empires having hitscan guns is one of the bigger issues with infantry balance today.  

<video controls>
  <source src="./ballistics.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video> 

_Ballistics with ricochet turned way up to demonstrate it working_

<video controls>
  <source src="./ballistics2.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video> 

_Ballistics with bullet speed turned down to demonstrate_

Also, I began to integrate the plugin MoveIt, which is a semi-procedural animation framework for 3rd person meshes and movement.  It’s a powerful tool for accelerating our development, and it’s integration will allow us to do quite a few extremely good looking things, such as Turn In Place animations, sprint, crouch, and even a Sprint-into-Crouch slide.  It also lets us tweak the weight of the player, and implement momentum and deceleration into infantry movement.  Again, like Ballistics, we probably won't use many of these features, but I do plan on experimenting with them and playtesting the crap out of them to see if they would fit within the feel of Empires.  I strongly feel that Infantry movement is the most important thing to get right in Empires, and having a deep and interesting character movement simulation will make this game extremely fun.  I see you all posting gifs of highly mobile mortar duels and fights like that, and I want to make those more fun and more interesting.  

<video controls>
  <source src="./moveit_tip.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video> 

_Work in progress MoveIt integration, featuring Turn In Place animation, Crouching, and Aim Spaces_

<video controls>
  <source src="./moveit_slide.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video> 

_Work in progress MoveIt integration, featuring crouch slide.  It’s a long slide to demonstrate the feature._

It was a big month for the project, and next month is going to be even bigger.  To give a preview, as I write this section, I’m updating Empires to Unreal Engine 4.26, which if you take a gander at the patch notes, you will see massive new features such as a full Water and Ocean simulation, Volumetrics (clouds and fog volumes), some frankly amazing landscape tools, and geometry editing that blows Hammer out of the water.  I cannot wait to see what level designers do with this engine upgrade.  


#### Art


### f1r3w4rr10r

Both Mayama and me are currently chipping away and making character models for both Northern Faction and the Brenodi Empire. Mayama has taken on Northern Faction, I have taken the Brenodi Empire. We wanted to keep the overall spirit for NF mostly the same and Mayama managed that quite well I think, while also iterating on the ideas and going in new directions. On my side, I was and am still of the opinion that we should move away from the Wehrmacht looking Brenodi infantry towards something more unique. I have a rough idea on where this is going, but my models are still in a look development and block out phase and not quite ready to show in an update yet. You can however catch a glimpse of them whenever I stream myself working on them.


### Mayama

I have finally managed to finish 2 NF infantry high polys. It's a heavy armored soldier which will be the rifleman model and the less armored and agile scout. Both models still need a low poly, texturing and rigging before they can be put into the game but it is nice to see stuff moving forward. The basic idea behind the armors was that the Northern Faction uses everything they get their hands on to protect themself from the superior firepower that Brenodi forces have. It’s a mix of old industrial gear, metal plates strapped to the body and hidden military gear that was stashed away from the last war.

{{< image-proc NF_heavy_armor_1.png Resize "1100x" >}}

{{< triple-image-thumbnail "heavy_armor_1.png" "heavy_armor_2.png" "heavy_armor_3.png" >}}
{{< triple-image-thumbnail "heavy_armor_4.png" "heavy_armor_5.png" "heavy_armor_6.png" >}}

{{< image-proc NF_scout_armor_1.png Resize "1100x" >}}

{{< triple-image-thumbnail "scout_armor_1.png" "scout_armor_2.png" "scout_armor_3.png" >}}
{{< triple-image-thumbnail "scout_armor_4.png" "scout_armor_5.png" "scout_armor_6.png" >}}

I also made some NF tools like mines and the engineering tool. I'm not really happy with the camera but I post it here so you can have a look at it.

{{< image-proc NF_engi_tool_1.png Resize "1100x" >}}
{{< image-proc NF_camera_1.png Resize "1100x" >}}
{{< image-proc NF_Mine_1.png Resize "1100x" >}}

Here is an example to show you how that soft armor will most likely look ingame because in the pictures that I posted it kinda looks like glossy satin pillows. If I would have reduced the glossiness in the pics you would not be able to make out much detail.

{{< image-proc bags.png Resize "1200x" >}}


----

Play the original Empires Mod on Steam!

{{< rawhtml >}}
<iframe src="https://store.steampowered.com/widget/17740/" frameborder="0" width="646" height="190"></iframe>
{{< /rawhtml >}}

Join us on Discord!

{{< rawhtml >}}
<iframe src="https://discordapp.com/widget?id=227846801653366784&theme=dark" width="350" height="500" allowtransparency="true" frameborder="0" sandbox="allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts"></iframe>
{{< /rawhtml >}}