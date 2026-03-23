Tutorials
Introduction
Installation
Basics
Macros
Hotkeys
Schedule
Delay
Storage
Icons
Basic UI
Advanced UI
Synchronization
Callbacks
onKeyDown
onKeyPress
onKeyUp
onTalk
onTextMessage
onPlayerPositionChange
onUse
onUseWith
onContainerOpen
onPlayerHealthChange
listen
onContainerClose
onAddThing
onRemoveThing
onMissle
onCreatureAppear
onCreatureDisappear
onChannelList
onOpenChannel
onCloseChannel
onChannelEvent
onContainerUpdateItem
Functions
Player
Map
NPC
Tools
Sound
getSpectators()
Cavebotting
Anti Bot Systems
Introduction
I don't know what to write here yet

Installation
Download vBot Otimizado
To download the client you will have to go to the GitHub website:
https://github.com/vBot Otimizado/vBot Otimizado

Over here you will face roughly this layout:



To actually download the client, click on the green button "Clone or download" (as indicated on the picture above).
A small window will appear, choose "Download ZIP" to start the download.

 

Installation vBot Otimizado
Once you've downloaded the ZIP file open it with your preferred program. The content should look similar to the picture below.



Unpack/Extract/Drag the whole map to a preferred location on your computer.
From this point the client is basically installed and ready to use.
The inside of the "vBot Otimizado-master" folder should look like this (v1.7):



Before we can start playing Tibia we need to put the right sources in the right places.
First you need to download the right Tibia version, this can be done through various different sources such as Otservlist.org
Once you've downloaded the right Tibia version create a map called the version name in ...\vBot Otimizado-master\data\things then drag the tibia.dat and tibia.spr file into the newly created folder.

See GIF below for a visual explanation:
client-location.gif

Once the client is installed and the right Tibia client is placed in the correct folder location, it is time to run it. You can do this by clicking either "otclient_dx" or "otclient_gl". Both have exactly the same functionalities but one uses DirectX for the graphics and the other uses OpenGL.


Configuration vBot Otimizado
Once you have opened the vBot Otimizado the screen should look something like this (v1.7):



The following screen is the login screen. This is where you connect to the server and start playing. It's pretty straight forward you enter your login details of your account and specify the ip and port of the server (PS don't forget to choose the correct tibia version). The ip and port has to be written in the format IP:PORT as in the example above megatibia.com:7171.

When logged in the Client should look like this:

In the top right of the client you see a small robot head. That's the included vBot Otimizado bot. Read more about that here.

Basics
How to edit, create and remove config, how to add scripts, how script are loaded (alphabetically), link to place where you can learn lua (or some tutorial), log functions (print, info, warn, error), main variables (now, time, player, storage), how to get item id, link to bot files and functions (https://github.com/vBot Otimizado/vBot Otimizado/tree/master/modules/game_bot)

Create Config
To create a config navigate to %appdata%\vBot Otimizado\vBot Otimizado\bot and create a folder with desired config name. Every folder in the bot directory is treated as a config.

Following images shows 2 configs. Default config and a config called config1 created by the user.

index.png

Inside of a config will be your .lua files containing the scripts you want to run. The client will run the files in alphabetical order. See next picture for a visual demonstration on which while runs first.

index2.png

You can also follow the quick in-client tutorial on how to setup your configs if you feel confident enough to tackle it on your own. Picture shown below is the in-client tutorial.

ingame_tutorial.png

Getting started with scripting
Before diving into the unknown world of vBot Otimizado scripting it's highly recommended to learn Lua and the basic principles of programming.

A few resources that can help you on your way can be found here and here.

Now that you feel more comfortable with the Lua language and how scripting / programming works lets dive right into it.

First off lets cover the built in debugging functions we can use in vBot Otimizado.

info(string)  -- Displays white text in the macro window
warn(string)  -- Displays yellow text in the macro window
error(string) -- Displays red text in the macro window
print(string) -- Prints to the built in terminal (ctrl+t to access terminal)
info: Great way to print information you're collecting to know exactly what you're working with
warn: Great to use when you meet a condition that could possibly cause an issue
error: Great to use when you meet a condition that WILL cause an error
print: A neat way to constantly log a lot of values that you want to monitor (terminal can display way more text than macro window)
Now that we're familiar with the logging features lets take a look at some built in variables we can access.

now  -- Time in milliseconds from when the application started
time -- Time in milliseconds from when the application started
player -- Local player object
storage -- A way to store variables even when client is restarted
now/time: Very useful for scripts that are dependant on time example: mwall timers
player: Contains a lot of information about the player such as position, health, outfit etc
storage: Great to use when you want to store values even when you close the client
Lets take a look at some examples on how to use these variables and logging functions.

-- Example how to use certain warning depending on a condition
macro(1000, "HP Tracker", function()
  if hppercent() > 90 then
  	info("Your hp is in a good condition")
  else if and hppercent() > 50 then
    warn("Your hp is getting low, please heal")
  else
    error("Your hp is ridiculously low, HEAL NOW!")
  end
end) -- Information about macro can be found here (http://bot.otclient.ovh/books/tutorials/page/macros)

-- Example how to log player position in terminal
macro(1000, "Log Player Pos", function()
  print("X: " .. posx() .. 
        " Y: " .. posy() .. 
        " Z: " .. posz())
end)
  
-- Warn if player has skull
macro(250, "Skull Tracker", function()
  if player:getSkull() ~= 0 then
    warn("You have a skull!!!")
  end
end)
As you can see in the examples above there's quite a few use cases above where logging information can be useful. In the scripts above I made sure to use built in functions to not confuse you with a lot of extra code.

posx(): Same as player:getPosition().x (returns the x position of player)
posy(): Same as player:getPosition().y (returns the y position of player)
posz(): Same as player:getPosition().z (returns the z position of player)
hppercent(): Same as player:getHealthPercent() (returns the health percentage of player)
I also made use of a player function, more specifically the getSkull() function which is accessed through a creature objects. It returns the ID of the skull (0 if there's no skull).

 Now that we know how to make use of the logging functions and some built in functions lets make a script that has some real value in terms of gameplay. Try to make sense of the following code before reading through the walkthrough guide below.

local healSpellWeak = "Exura"
local healSpellMedium = "Exura Gran"
local healSpellStrong = "Exura Vita"
macro(250, "Auto Heal", function()
  if hppercent() > 90 and hppercent() < 99 then
    say(healSpellWeak)
  else if hppercent() > 50 then
    say(healSpellMedium)
  else
    say(healSpellStrong)
  end
ene)
If you can't make sense of the code don't worry. Lets walk it through step by step. First we assign 3 variables to contain a string with the spell name of our choosing. A weaker healing spell, a medium and then a strong one depending on how critical our health is. We then create a macro, this macro will run every 250 milliseconds and the callback for the macro is our function that contains the logic for when we should heal ourselves. The functions works as following, if players hp is between 90 and 99 percent we will use the weaker healing spell however if player hp drops below 90 percent but is still above 50 percent we will use the medium strong healing spell but if all of those if statements fail we will use the strongest spell possible to make sure we don't die. Note that we're using a new function called say(), more info about that below.

say(string): Accepts a string as parameter and will make your character say that string in game.
Information about macros can be found here

 

 

Macros
Description
Macros are persistent Lua functions being called as long as the player is logged in. The persistence of the Lua function is determined by the timeout specified by the user. All Parameters are not required in order to create a macro.

Prototypes
macro(timeout, callback)
macro(timeout, name, callback)
macro(timeout, name, callback, parent)
macro(timeout, name, hotkey, callback)
macro(timeout, name, hotkey, callback, parent)
Parameters
timeout: the time in milliseconds after which the function is called.
name: macro name on the bot interface. Macro with name will keep it's status (on/off) after login/logout/restart. If the macro has no name it won't show up on the bot interface
hotkey: keyboard hotkey to disable/enable the macro. can be nil or empty string
parent: the tab to show the on/off macro button on. Default tab is main
callback: is the Lua function being called called.
the function can have macro object parameter function(macro) to toggle macro state.
Return
Macro return table with following functions:

isOn() - return true if macro is enabled
isOff() - return true if macro is disabled
setOn() - enables macro
setOff() - disables macro
toggle() - enables or disables macro
Examples
macro(timeout, callback)
Macro with no button on the bot interface.

macro(10000, function() -- run every 10 seconds
    turn(0) -- turn the character direction to north.
  end)

macro(timeout, name, callback)
Macro with a on/off button on bot main Tab.

onoff-macro.png

macro(1000, "exchange money", function()
    for i, container in pairs(getContainers()) do
      for j, item in ipairs(container:getItems()) do
        if item:isStackable() and (item:getId() == 3035 or item:getId() == 3031) and item:getCount() == 100 then
          return use(item)
        end
      end
    end
  end)

macro(timeout, name, callback, parent)
Macro with a on/off button on a tab

macro-on-tab.png

local warTab = addTab("War") -- Add new tab called War

local oldTarget
macro(500, "Hold Target", function()

    if g_game.isAttacking() then
      oldTarget = g_game.getAttackingCreature() -- save the target
    end
    if oldTarget and not g_game.isAttacking() and getDistanceBetween(pos(), oldTarget:getPosition()) <= 8 then -- if the player has no target and player is on distance
      g_game.attack(oldTarget)
    end

  end, warTab)
 
macro(timeout, name, hotkey, callback)
Macro with on/off keyboard hotkey on Main tab

hotkeymacro.png


macro(100, "hide useless tiles", "Ctrl+H", function()
    for i, tile in ipairs(g_map.getTiles(posz())) do
      if not tile:isWalkable(true) then
        tile:setFill('black')
      end
    end
  end)


macro(timeout, name, hotkey, callback, parent)
Macro with a keyboard hotkey on a tab

hotkeymacroontab.png


local warTab = addTab("War") -- Add new tab called War

macro(250, "Dance", "Ctrl+D", function()

    turn(math.random(0, 3)) -- turn to a random direction.

 end, warTab)
Toggling macro state
This feature is available from version 1.8, it adds ability to disable/enable the macro from the script.

macro(1000, "refill arrows", function(macro)
	if getAmmo() == nil then
		local arrows = findItem(3447) -- find arrows
		if arrows then
			moveToSlot(arrows, SlotAmmo, arrows:getCount()) -- the arrows was found move them to ammo slot
			return -- arrows was found and moved end the cycle.
		end
		-- if the script reach this point the arrows wasn't found on any of player backpacks.
		macro.setOff() -- disable the macro since we didn't find any arrows
	end
end)
available toggling functions is setOff()setOn()toggle()

you also assign the macro to a variable and change it's state

local refilArrows = macro(1000, "refill arrows", function(macro)
	if getAmmo() == nil then
---.......
      
-- using a hotkey for example to toggle between refilling arrow state
hotkey("f5", "toggle refilling", function()
	refilArrows.toggle() -- you can also use setOn(), setOff()
	info("refilling was disabled")
end)
Hotkeys
Hotkey is spammable as long as u hold the keyboard key, singlehotkey occurs once even if the keyboard key is held.

hotkey
Prototypes
hotkey(keys, callback)
hotkey(keys, name, callback)
hotkey(keys, name, callback, parent)
Parameters
keys: keys as string.
name: name on bot interface.
callback: function being called on hotkey.
parent: tab interface hotkey belongs to. default tab is main
Examples
hotkey(keys, callback)
hotkey with no interface button.

hotkey("f5", function()
    info("Wow, you clicked f5 hotkey")
  end)
-- simple dash
hotkey("u", function() g_game.walk(0) delay(50) end) -- north
hotkey("k", function() g_game.walk(1) delay(50) end) -- east
hotkey("j", function() g_game.walk(2) delay(50) end) -- south
hotkey("h", function() g_game.walk(3) delay(50) end) -- west
hotkey(keys, name, callback)
hotkey with button interface on Main tab

hotkey.png

hotkey("f5", "hello world", function()
    info("Hello World!")
  end)
hotkey(keys, name, callback, parent)
hotkey with button interface on a tab

hotkeywithbutton.png

local toolsTab = addTab("Tools")
hotkey("f5", "hello from tools", function()
    info("Hello World!")
  end, toolsTab)
 

singlehotkey
 

Prototypes
singlehotkey(keys, callback)
singlehotkey(keys, name, callback)
singlehotkey(keys, name, callback, parent)

Examples
singlehotkey(keys, callback)
single occurs hotkey with no interface button. happens once once keypress even it was held

singlehotkey("ctrl+f6", function()
    info("Wow, you clicked ctrl+f6 singlehotkey")
  end)

singlehotkey(keys, name, callback)
example with button interface button on main tab.

singlehotkeyonmain.png

singlehotkey("ctrl+f7", "singlehotkey", function()
    info("Wow, you clicked ctrl+f7 singlehotkey")
  end)
singlehotkey(keys, name, callback, parent)
example with button interface button on a tab

singlehotkeyontab.png

singlehotkey("ctrl+f7", "singlehotkey on tools", function()
    info("hello once from tools")
  end)
 

Schedule
Registers a function to be called after a certain timeout in milliseconds.

Prototypes
schedule(timeout, callback)
Parameters
timeout: the time in milliseconds after which the function is called.
callback: the function being called.
Usage
macro(10000, "Anti Kick",  function()
  local oldDir = direction()
  turn((oldDir + 1) % 4)
  schedule(1000, function() -- Schedule a function after 1000 milliseconds.
    turn(oldDir)
  end)
end)
singlehotkey("ctrl+1", "buy300Manas", function()
  NPC.say("hi")
  schedule(100, function() NPC.say("trade") end)
  schedule(200, function() NPC.buy(268, 100) end)
  schedule(300, function() NPC.buy(268, 100) end)
  schedule(400, function() NPC.buy(268, 100) end)
  schedule(500, function() NPC.say("bye") end)  
end)
Delay
Delay the next execution cycle for macro, hotkeys or callbacks

Prototypes
delay(duration)
Parameters
duration: delay time in milliseconds.
Usage
macro(500, "AdvertiseMsg", function()
	yell("Advertising message")
	delay(math.random(2000, 2500)) -- delay execution for the next macro cycle
  end)
Storage
How to use storage and storage.json file, some examples and what not to do (you can't store objects in storage, for eg. items, you must use itemid instead)

Icons
Icons is a cool GUI way to create on / off switches or clickable buttons that link to bot features, it was added in version 1.8.
Icons support macro and hotkeys as a callback function, the icon can be dragged anywhere on screen by holding CTRL.

e25ebb54b0b20cfe21fd2651e8ca3710.gif

Prototypes
addIcon(id, options, callback)
Parameters
id: icon id on client widget, must be unique for every icon. It is used to store icon status and position in storage.
options: icon controling options.
callback: the function being called on click event. can be function() or function(icon, isOn).
icon: icon widget object.
isOn: current icon state Boolean.
Available Options:

item: the item icon id can set item count by {id = ItemID, count = ItemCount}.
outfit: outfit object contains outfit type and colors
text: text to display below the icon
x: x location on client window (percent, 0.0-1.0 range)
y: y location on client window (percent, 0.0-1.0 range)
hotkey: hotkey to control the icon button instead of mouse clicking it.
switchable: if the logic is on/off button (like singlehotkey instead of hotkey). is it's true callback is automatically called once after icon creation (to report it's status: on/off). default is true
movable: can the item be dragged from it's place?
phantom: can the item float above game screen?

Usage
-- trash 9x9 tiles
addIcon("trash", {item={id = 3492, count = 100}, switchable=false}, function()
  for x=-3, 3 do
    for y=-3, 3 do
      local worms = findItem(3492) -- find some worms
      if worms then
        g_game.move(worms, {x = posx() + x, y = posy() + y, z = posz()}, 1) -- drop them around
      end
    end
  end
end)

Macro as Icon
addIcon("change gold", {item={id = 3043, count = 100}}--[[ icon options ]], macro(100, "change gold", function()
  local containers = getContainers()
  for i, container in pairs(containers) do
    for j, item in ipairs(container:getItems()) do
      if item:isStackable() and (item:getId() == 3035 or item:getId() == 3031) and item:getCount() == 100 then
        g_game.use(item)
        return
      end
    end
  end
end))
-- toggle macro using icon
local sdTarget = macro(1000, "SD Target", function() -- sd target macro
  local target = g_game.getAttackingCreature()
  if target then
    useWith(3155, target)
  end
end)

-- Item icon
addIcon("sdTarget", {item=3155, hotkey="F5"}--[[ icon options ]], function(icon, isOn)
  sdTarget.setOn(isOn)
end)

-- outfit icon
addIcon("sdTarget", {outfit={type = 130, head = 114, body = 114, legs = 114, feet = 114, addons = 3}, hotkey="F5"}--[[ icon options ]], function(icon, isOn)
  sdTarget.setOn(isOn)
end)
 

hotkey (from version 1.9)
-- dance when holding ctrl+D
addIcon("Dance", {item=6573}, hotkey("ctrl+D", function()
  turn(math.random(0,3))
end))
singlehotkey (from version 1.9)
addIcon("rainbow", {item=6578}, singlehotkey("f9", function()
  local outfit = outfit()
  outfit.head = (outfit.head + 1) % 133;
  outfit.body = (outfit.body + 1) % 133;
  outfit.legs = (outfit.legs + 1) % 133;
  outfit.feet = (outfit.feet + 1) % 133;
  setOutfit(outfit);
end))
 

Basic UI
Tabs
setDefaultTab("NAME HERE")
 The following code demonstrates how to use setDefaultTab to create a new tab and add macros / buttons to that tab.

setDefaultTab("Test")

macro(100, "Testing", function()
  info("test macro")
end)

addButton("id1", "Test Button", function()
  info("test button")
end)
addTab("NAME HERE")
The following code demonstrates how to use addTab to create a new tab and add macros / buttons to that tab.

local newTab = addTab("Test")

macro(100, "Testing", function()
  info("test macro")
end, newTab)

addButton("id1", "Test Button", function()
  info("test button")
end, newTab)
It's recommended to use setDefaultTab instead of addTab as that will make it way easier to organize tabs by files instead of having to keep track of tab variables within your code.

tabs, labels, buttons, separator - with examples -- ( WORK IN PROGRESS, WILL BE ADDED SOON TO THIS DOCUMENTATION PAGE )

Advanced UI
otml based ui, don't do it yet 

Synchronization
Tutorial how to use BotServer with some examples

Callbacks
List of every callback with examples (from game_bot/functions/callbacks.lua)

Callbacks
onKeyDown
Getting triggered once when keyboard key is down accepts key combos. (NOTE: triggered once even if the key is held)

Prototypes
onKeyDown(callback) -- callback = function(keys)
Parameters
function(keys): callback function

keys: string
Usage
onKeyDown(function(keys)
  if keys == "W" then
  	print("Move north!")
  elseif keys == "Ctrl+W" then
  	print("Look north!")
  elseif keys == "Alt+D" then
  	print("Look east?")
  end
end)
Callbacks
onKeyPress
Getting triggered as long as the keyboard key is pressed.

Prototypes
onKeyPress(callback) -- callback = function(keys)
Parameters
function(keys): callback function
keys: string
Usage
onKeyPress(function(keys)
  if keys == "W" then
  	print("Move north! and keep moving")
  elseif keys == "Ctrl+W" then
  	print("Ctrl+W is pressed")
  elseif keys == "Alt+W" then
  	print("Alt+W is pressed")
  end
end)
Callbacks
onKeyUp
Getting triggered once when keyboard key is up accepts key combos. (NOTE: triggered once even if the key was held)

Prototypes
onKeyUp(callback) -- callback = function(keys)
Parameters
function(keys): callback function
keys: string
Usage
onKeyUp(function(keys)
  if keys == "W" then
      print("W is up")
  elseif keys == "Ctrl+W" then
      print("Ctrl+W is up!")
  elseif keys == "Alt+W" then
      print("Alt+W is up!")
  end
end)
Callbacks
onTalk
Getting triggered when the client received a chat message.

Prototypes
onTalk(callback) -- callback = function(name, level, mode, text, channelId, pos)
Parameters
function(name, level, mode, text, channelId, pos): callback function
name: string
level: integer
mode: integer
text: string
channelId: integer
pos: position object (contains x, y, z)
Usage
onTalk(function(name, level, mode, text, channelId, pos)
  print(name .. " said " .. text)
end)
Callbacks
onTextMessage
Getting triggered when the client receives a game server message example: Server Log messages.

Prototypes
onTextMessage(callback) -- callback = function(mode, text)
Parameters
function(mode, text): callback function
mode: integer
text: string
Usage
onTextMessage(function(mode, text)
  if string.find(text, "Warning! The murder") then
  	print("Horray!")
  end
end)
 

Callbacks
onPlayerPositionChange
Getting triggered when the player position changes

Prototypes
onPlayerPositionChange(callback) -- callback = function(newPos, oldPos)
Parameters
function(newPos, oldPos): callback function
newPos: position object of new player position (contains x, y, z)
oldPos: position object of old player position (contains x, y, z)
Usage
onPlayerPositionChange(function(newPos, oldPos)
  info("New Pos: (X: " .. newPos.x .. " Y: " .. newPos.y .. " Z: " .. newPos.z) -- Displays newPos (as info text in the macro window)
  info("Old Pos: (X: " .. oldPos.x .. " Y: " .. oldPos.y .. " Z: " .. oldPos.z) -- Displays oldPos (^)
end)
Callbacks
onUse
Getting triggered when player uses an item.

Prototypes
onUse(callback) -- callback = function(pos, itemId, stackPos, subType)
Parameters
function(pos, itemId, stackPos, subType): callback function
pos: item position object x, y and z.
itemId: client side item id.
stackPos: current item position on stack
subType: integer
Usage
onUse(function(pos, itemId, stackPos, subType)
	print(itemId .. " was used")
end)
Callbacks
onUseWith
Getting triggered when player uses an item on target.

Prototypes
onUseWith(callback) -- callback = function(pos, itemId, target, subType)
Parameters
function(pos, itemId, target, subType): callback function
pos: item position object x, y and z.
itemId: client side item id.
target: the target for the item usage.
subType: integer
Usage
onUseWith(function(pos, itemId, target, subType)
	print(itemId .. " was used")
end)
Callbacks
onContainerOpen
Getting triggered when player opens a new container.

Prototypes
onContainerOpen(callback) -- callback = function(container, previousContainer)
Parameters
function(container, previousContainer): callback function
container: opened container object.
previousContainer: parent container object.
Usage
onContainerOpen(function(container, previousContainer)
	print(container:getName() .. " was opened.")
end)
Callbacks
onPlayerHealthChange
Getting triggered when player health changes

Prototypes
onPlayerHealthChange(callback) -- callback = function(healthPercent)
Parameters
function(healthPercent): callback function
healthPercent: healthPercent of player (integer value)
Usage
local spellName = "Exura"
onPlayerHealthChange(function(healthPercent)
  if healthPercent < 99 then
    say(spellName)
  end
end)
Callbacks
listen
Getting triggered when received a chat message from a certain player.

Prototypes
listen(name, callback) -- callback = function(text, channelId, pos)
Parameters
name: player name
function(text, channelId, pos): callback function
text: message (string value)
channelId: channel id (integer value)
pos: position object of sender (contains x, y, z)
Usage
-- Wait for a message from Vincent
listen("Vincent", function(text, channelId, pos)
	print("Vincent said " .. text)
end)
Callbacks
onContainerClose
Getting triggered when player closes a container.

onContainerClose(callback) -- callback = function(container, previousContainer)
Parameters
function(container, previousContainer): callback function
container: container object.
Usage

onContainerClose(function(container)
	print(container:getName() .. " was closed.")
end)
Callbacks
onAddThing
Getting triggered a thing is added to the tile.

Prototypes
onAddThing(callback) -- callback = function(tile, thing)
Parameters
function(tile, thing): callback function
tile: tile object.
thing: thing object. can be creature, item, effect or text.
Usage
local playerTile = g_map.getTile(pos()) -- get current position tile
onAddThing(function(tile, thing)
    if thing and thing:isStackable() then
      if thing:getId() == 3031 then -- found gold under character
        g_game.move(thing, { x = posx() + 1, y = posy(), z = posz() }, 1) -- push it away +1 x
      end
    end
end)
Callbacks
onRemoveThing
Getting triggered a thing is remove from the tile.

Prototypes
onRemoveThing(callback) -- callback = function(tile, thing)
Parameters
function(tile, thing):callback function
tile: tile object.
thing: thing object. can be creature, item, effect or text.
Usage
local playerTile = g_map.getTile(pos()) -- get current tile
onRemoveThing(function(tile, thing)
    local goldItem = findItem(3031) -- search for gold
    if goldItem then
      g_game.move(thing, pos(), 1) -- drop the gold
    end
  end)
Callbacks
onMissle
Description
Callback function getting triggered when the client received missile attack example: shooting arrow or rune
Prototypes
onMissle(callback) -- callback = function(missle)
Parameters
function(missle): callback function
getSource(): position object of source (contains x, y, z) 
getDestination(): position object of destination (contains x, y, z)
Usage
onMissle(function(missle)
  local src = missle:getSource()
  local shooterTile = g_map.getTile(src)
  if shooterTile then
    local creatures = shooterTile:getCreatures()
    local creatureName = creatures[1]:getName()
    info("Name: " .. creatureName) -- Show name as info label in macro window
  end
end)
Callbacks
onCreatureAppear
Triggered when a new creature added to Battle List.

Prototypes
onCreatureAppear(callback) -- callback = function(creature)
Parameters
function(creature): callback function
creature: creature object
Usage
onCreatureAppear(function(creature)
    info(creature:getName()) -- print the creature name
  end)
Callbacks
onCreatureDisappear
Triggered when creature removed from Battle List.

Prototypes
onCreatureDisappear(callback) -- callback = function(creature)
Parameters
function(creature): callback function
creature: object contains creature information
Usage
onCreatureDisappear(function(creature)
    info(creature:getName() .. " was removed")
  end)
Callbacks
onChannelList
Triggered when player logs in.

Parameters
channels: array contains channel id and channel name
Usage
onChannelList(function(channels)

    for k,v in pairs(channels) do
      local channelId = v[1]
      local channelName = v[2]
		info("channel name" .. channelName)
    end

  end)
Callbacks
onOpenChannel
Triggered when player or server opens a new channel.

Prototypes
onOpenChannel(callback) -- callback = function(channelId, name)
Parameters
function(channelId, name): callback function
channelId: client side channel id.
name: channel name
Usage
onOpenChannel(function(channelId, name)
    info("channel " .. name .. " with " .. channelId .. " id was opened")
  end)
Callbacks
onCloseChannel
Triggered when player or server closes a channel.

Prototypes
onCloseChannel(callback) -- callback = function(channelId)
Parameters
function(channelId): callback function
channelId: client-side channel id (integer value)
Usage
onCloseChannel(function(channelId)
    info("channel id " .. channelId .. " was closed")
  end)
Callbacks
onChannelEvent
Triggered when a channel event occurs can be someone joined or left the channel.

Prototypes
onChannelEvent(callback) -- callback = function(channelId, name, event)
Parameters
function(channelId, name, event): callback function
channelId: client side channel id
name: event player name.
event: channel event enum.
ChannelEvent.Join
ChannelEvent.Leave
ChannelEvent.Invite
ChannelEvent.Exclude
Usage
onChannelEvent(function(channelId, name, event)
	if event == ChannelEvent.Join then
		info(name .. " joined the room!")
	end
end)
Callbacks
onContainerUpdateItem
Getting triggered when an item in container updates

Prototypes
onContainerUpdateItem(callback) -- callback = function(container)
onContainerUpdateItem(callback) -- callback = function(container, slot, item)
Parameters
function(container, slot, item): callback function
container: container object
slot: position object
item: item object
Usage
onContainerUpdateItem(function(container, slot, item)
  if item then
    info("Item updated: " .. item:getId())
    info("In Container: " .. container:getName())
  end
end)
Functions
List of functions, don't edit it yet

Functions
Player
Player/Character information functions.

name()
returns character name as string.

hp()
return current character health.

mana()
return current character mana.

hppercent()
return current character health percent.

manapercent()
return current character mana parcent.

maxhp()
return current character max health.

maxmana()
return current character max mana.

hpmax()
return current character max health.

manamax()
return current character max mana.

cap()
return current character capacity.

freecap()
return current character free capacity.

maxcap()
return current character max capacity.

capmaxcap()
return current character max capacity.

exp()
return current character experience points.

lvl()
return current character level.

level()
return current character level.

mlev()
return current character magic level.

magic()
return current character magic level.

mlevel()
return current character magic level.

soul()
return current character souls.

stamina()
return current character stamina.

voc()
return current character vocation.

vocation()
return current character vocation.

bless()
return current character done blessings.

blesses()
return current character done blessings.

blessings()
return current character done blessings.

pos()
return current character position object.

posx()
return current character position X coordinate.

posy()
return current character position Y coordinate.

posz()
return current character floor.

direction()
return current character direction.

speed()
return current character speed.

skull()
return current character skulls.

outfit()
return current character outfit object.

Functions
Map
Functions
NPC
Functions
Tools
Functions
Sound
tutorial for sound, need to mention that it only accepts ogg files

Functions
getSpectators()
Returns table with spectators within specified range

Prototypes
getSpectators = function(param1, param2)
Parameters
Depending on the types of parameters, function behaviour will change:

if no parameters were used, function will return all creatures on current posz() withing the sight 
if param1 is boolean(true), function will return all creatures, on all floors withing the sight
if param1 is string(pattern), function will return all creatures withing the range of pattern* and player position is used as center it's center
if param1 is table(position), then the param2 should be string(pattern), function will return all creatures withing the range of pattern* and param1 is used as it's center
if param1 is creature, then param2 should be string(pattern), function will return all creatures withing the range of pattern* and creature pos is used as it's center. in this configuration it is possible to set patterns dependent on the look direction of creature
Pattern
Pattern is a string bitmap of tiles marked to be considered with getSpectators() calculations.
Anchor for the pattern is always it's center point.

Available markers for tiles:

1 - tile to consider
0 - tile to be ignored
N,W,S,E - tiles to consider depending on the turn direction
Example pattern for exori:

[[
  111
  101
  111
]]
Example pattern for GFB:

[[
  0011100
  0111110
  1111111
  1111111
  1111111
  0111110
  0011100
]]
Example pattern for wave:

[[
  0000NNN0000
  0000NNN0000
  0000NNN0000
  00000N00000
  WWW00N00EEE
  WWWWW0EEEEE
  WWW00S00EEE
  00000S00000
  0000SSS0000
  0000SSS0000
  0000SSS0000
]]
 
Usage
for _, spec in ipairs(getSpectators()) do
  if spec:isMonster() then
    info("Monster detected!")
  end
end
local waveArea = [[
  0000NNN0000
  0000NNN0000
  0000NNN0000
  00000N00000
  WWW00N00EEE
  WWWWW0EEEEE
  WWW00S00EEE
  00000S00000
  0000SSS0000
  0000SSS0000
  0000SSS0000
]]

local creatures = getSpectators(player, waveArea)
info("Creatures within wave range: " .. #creatures)
Cavebotting
What is cavebotting?
When people talk about cavebotting it means that the bot does everything for you while hunting. This can be as simple as only hunting in a cave, or with supplying, depositing, selling items all at once. The latter form is also called "full AFK botting." 
Within the vBot Otimizado BOT, cavebotting is possible from the get go. What you need for a basic script are the "Cave" and "HP" tab.
An example of a setup for Yalahar dragons is shown below.



Waypoints
For a cavebot to work successfully it needs waypoints. Waypoints will bring the character from one point to another. To do this with the vBot Otimizado BOT you simply start with the "Add" button. This will bring up a screen as shown below:



Here you can already see it got a name which is "Config name," you can adjust this to the name you prefer. In the example earlier it's called Yala-dragons. It's normal to name the waypoints after the location and what creature you are mainly going to hunt. This can also be extended with the vocation if you prefer. But because this bot has configuration sets, you can make a whole config for each vocation and therefor it's not needed to add the vocation. Also there is already a label called "start." This usability of labels will be explained later.

To know more about configs check the page "Basics Create Config."

 

Waypoint buttons
Once you made your waypoint config you can start with filling up the waypoints. The following buttons are available to use:

 This button will give you a popup like below, here you can see your current location in X, Y, Z format.In tibia all tiles are placed on a grid and each tile is addressable with the right combination of numbers.
The first number is for the horizontal axis. If you add 1 it means you will go 1 more tile to the east(right).
The second number is for the vertical axis. If you add 1 it means you will go 1 more tile to the south(down).
The third number is for the floor. If you add 1 it means you will go 1 floor up.

 This button will give you a popup like below, here you can adjust the time you want to wait.The time is in milliseconds this mean 1000 is 1 second. Wait can be useful if you talk and don't want to spam everything almost at once. Wait is also useful if your previous waypoint was a teleport tile.

 This button will give you a popup like below, here you can adjust the name of the label.A label can be useful in several ways, the main reason of using it is to make clear what is going on at that point of the waypoints.
For example a label to show that you are in depot is a good beginning of a waypointlist. Besides of using it as a clarification, it can also be used to let the cavebot return to that point. This can be done with the use of a function shown below.

waypoints.gotoLabel("atHuntspot")
To learn more about functions read it here.

 This button will give you a popup like below, here you can adjust the location (current location is shown).
This is the same popup as the goto button gives, the difference is that the bot will use that location.
It's common used to get up ladders, it is advised to make a goto waypoint before it.

 This button will give you a popup like below, here you can adjust the message.The text will be said in the default channel. It is advised to not use this in your (full AFK) cavebotting sessions. 

 This button will give you a popup like below, here you can adjust the character/creature you want to follow.This can be really useful if you need to talk to an NPC that moves around, in this way you will always be close enough to have a conversation with the NPC.

 This button will give you a popup like below, here you can adjust the location and item to use.

The first number will be the itemid of the item you want to use, this could be for example a rope. The other 3 numbers are the numbers for the location where you want to use the item. To find out what ID your item has, you can do Ctrl click on the item. Which will show the options of that item and at the bottom it states the ID. For the rope example it is 3003. 

 This button will give you the same popup as say, here you can adjust the message.
The text will be said in the the NPC channel. Which makes this very useful for your (full AFK) cavebotting sessions. You could use this to for example deposit the gold you gathered from your hunt. The outcome would look like this:

label:depositGold
npc:Hi
wait:500
npc:deposit all
wait:500
npc:yes
wait:500
npc:bye
 This button will give you a popup like below, here you can enter your function:This is probably the most difficult part of the cavebot, here you can address functions to execute them. It is important to follow what is said in the standard example. So the function will only be executed if the previous goto was successful or the function is after a label. It also has to return true otherwise it will be in an eternal loop. Here you could for example buy potions, this could be done as shown below:

function(waypoints)
  -- your lua code, function is executed if previous goto was successful or is just after label  
  schedule(100, function() NPC.say("hi") end)
  schedule(700, function() NPC.say("trade") end) 
  schedule(1400, function() NPC.buy(266, 100) end) 
  NPC.closeTrade()

  -- must return true to execute next command, otherwise will run in loop till correct return
  return true
end
To learn more about functions read it here.

Anti Bot Systems
One of many vBot Otimizado bot advantages it’s his capability to work around anti-bot systems.
Since this client is bot, basically everything that happens on screen can be detected and processed.

There was already many requests regarding this kind of scripts so we decided to make tutorial for them.

Whole process to create such script can be listed in two or three steps:

Finding anti-bot message (which contains requested value)
optional: decoding the message (ie. removing “:” between numbers)
Returning full expression
Let’s begin with detecting the message.

Based on our requests, we can assume that vast majority of anti-bot systems are text (server) messages. There are probably other ways or different protections against botting that we are not aware of - it that case just inform us about it and we will cover them also.

 

To get the idea, we will start off with simple example:

anti-bot-1.png

So like mentioned before, most anti bot systems are based server messages. To detect those we will use one of two callbacks:

onTextMessage
onTalk – this one will require additional protection to prevent it from being triggered by players messages
For this example we should use onTextMessage. But it will return every message user receives, so we have to narrow it to only those from anti bot system:

onTextMessage(function(mode, text)
  if string.find(text:lower(), "anti bot system") then 
  	print("OK now the rest")
  end
end) 
 

Since this is simple example, this system won’t require additional formatting the message, all we have to do is extract the value and return it. The value will be the last number within the string.

local returnValue
onTextMessage(function(mode, text)
  if string.find(text:lower(), "anti bot system") then 
  	returnValue = string.match(text, "%d+$")
  end
end) 
in this example returnValue will become 92558, so all that is left to do is to return the full expression:

local returnValue
onTextMessage(function(mode, text)
  if string.find(text:lower(), "anti bot system") then 
  	returnValue = string.match(text, "%d+$")
    say(“!antibot ” .. returnValue)
  end
end) 
Done.

Now let’s try with little more complicated example, it will require formatting.

anti-bot-2.png

This time simple string.find function wont work, due to colons between digits. We need to use regex.
*To test regex expressions you can use pages like https://regex101.com/

First we need to find our coded returnValue within the string:

local returnValue
local regex = "([0-9:]*)" -- use https://regex101.com/ to see how it will behave
Next thing to do is to remove colons from returnValue. To do so, we will prepare a simple table with value to replace and end value:

local replace = {
    {":", ""} -- value[1] = ":", value[2] = ""
}
Now we have everything we need to make our script:

local returnValue
local regex = "([0-9:]*)" -- use https://regex101.com/ 
local replace = {
    {":", ""} -- val[1] = ":", val[2] = ""
}

onTextMessage(function(mode, text)
    if string.find(text:lower(), "anti bot system") then       -- narrowing to only anti bot messages 
    	local re = regexMatch(text, regex)        			   -- finding regex within our string
    	if #re == 1 then 									   -- if found only one match
        	returnValue = re[1][1]                             -- save it
        	for key, val in ipairs(replace) do
            	returnValue = returnValue:gsub(val[1], val[2]) -- removing colons by replacing values (table)
        	end
        say("!antibot ".. returnValue)                         -- returning the full expression 
    	end
	end
end)
 Done.

 

That’s with the examples, for what we could see most of systems works in very similar way. If very different  request will show up. Good luck!

This Documentation was NOT developed by me (VictorNeox).
If you know who developed, please, message me on Discord: VictorNeox#4112
