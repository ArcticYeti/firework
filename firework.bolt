# 
# 
#    ________                                                         ___       
#    `MMMMMMM 68b                                                     `MM       
#     MM    \ Y89                                                      MM       
#     MM      ___ ___  __   ____   ____    _    ___   _____   ___  __  MM   __  
#     MM   ,  `MM `MM 6MM  6MMMMb  `MM'   ,M.   'M'  6MMMMMb  `MM 6MM  MM   d'  
#     MMMMMM   MM  MM69 " 6M'  `Mb  `Mb   dMb   d'  6M'   `Mb  MM69 "  MM  d'   
#     MM   `   MM  MM'    MM    MM   YM. ,PYM. ,P   MM     MM  MM'     MM d'    
#     MM       MM  MM     MMMMMMMM   `Mb d'`Mb d'   MM     MM  MM      MMdM.    
#     MM       MM  MM     MM          YM,P  YM,P    MM     MM  MM      MMPYM.   
#     MM       MM  MM     YM    d9    `MM'  `MM'    YM.   ,M9  MM      MM  YM.  
#    _MM_     _MM__MM_     YMMMM9      YP    YP      YMMMMM9  _MM_    _MM_  YM._
# 
# 
# 
#       A lightweight event library designed to speed up datapack development.
# 
#       Include desired events using the firework.yml file in the root directory.
#       Unused events will not be included in the built datapack.
# 
# 
#       The '.c' events fire continously while the condition is met.
#       All else fires on the first tick of an action.
# 
# 
#       Usage example (tick function):
# 
#           as @a:
# 
#              if predicate event:player.jump:
#                   say Hello World!
# 
#               if predicate event:player.offhand.bow.c:
#                   effect give @s speed 1 0 true
#                   
#                   if predicate event:player.item.bow.use:
#                       say I shot the bow from my offhand!
#                       give @s diamond 2
#               
# 
# 
#       Event list:
# 
#           player.jump
#           player.sneak
#           player.sneak.c
#           player.sprint
#           player.sprint.c
#           player.swim
#           player.swim.c
# 
#           NOT IMPLEMENTED YET:
#            player.mainhand.ender_eye
#            player.mainhand.warped_rod
#            player.mainhand.bow
#            player.mainhand.trident
#            player.offhand.ender_eye
#            player.offhand.warped_rod
#            player.offhand.bow
#            player.offhand.trident
#            player.anyhand.ender_eye
#            player.anyhand.warped_rod
#            player.anyhand.bow
#            player.anyhand.trident
#            player.item.warped_rod.use
#            player.item.ender_eye.use
#            player.item.bow.idle
#            player.item.bow.charge
#            player.item.bow.charge.c
#            player.item.bow.use
#            player.item.trident.idle
#            player.item.trident.charge
#            player.item.trident.charge.c
#            player.item.trident.use
# 
# 
# 
# 
# 
# 
# 
# 
# 
# ==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--=



from beet import Context
from bolt_expressions import Scoreboard, Data
from bolt import runtime
import yaml
import logging
import os


config = yaml.load((ctx.directory / 'firework.yml').read_text(),Loader=yaml.FullLoader)
included = []
print(./firework)


if 'include' in config:
    included = config['include']

merge function_tag minecraft:load {"values": ["firework:load"]}



function ./load:
    scoreboard objectives add firework dummy
    scoreboard objectives add firework.temp dummy
    function #firework:load
    function ./unload
    function ./tick
    
    
function ./tick:
    schedule function ./tick 1
    function #firework:tick

    as @a at @s:
        function ./tick_players

function ./tick_players:
    function #firework:tick_players



# ==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==     
                                                     

loaded_deps = []

def load_dep(name):
    global loaded_deps

    if name == 'is_sneaking':

        predicate ./deps/is_sneaking {
                "condition": "minecraft:entity_properties",
                "entity": "this",
                "predicate": {
                    "flags": {
                    "is_sneaking": true
                    }
                }
        }
        
    elif name == 'is_sprinting':

        predicate ./deps/is_sprinting {
                "condition": "minecraft:entity_properties",
                "entity": "this",
                "predicate": {
                    "flags": {
                    "is_sprinting": true
                    }
                }
        }

    elif name == 'is_swimming':

        predicate ./deps/is_swimming {
                "condition": "minecraft:entity_properties",
                "entity": "this",
                "predicate": {
                    "flags": {
                    "is_swimming": true
                    }
                }
        }

    elif name == 'mainhand.ender_eye':
        pass

    # this ensures invalid dependencies won't be loaded
    else:
        name = 'invalid'



    if name != 'invalid':
        append_dep_list(name)



def unload_unused_deps():
    global loaded_deps

    # if 'example_dependency' not in loaded_deps:
    #     scoreboard objectives remove my_dependency


def append_dep_list(name):
    global loaded_deps
    
    if name not in loaded_deps:
        loaded_deps.append(name)
#

# ==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==





# DONE
if 'player.jump' in included:

    predicate event:player.jump {
        "condition": "minecraft:value_check",
        "value": {
            "type": "minecraft:score",
            "target": "this",
            "score": "firework.event.player.jump"
            },
        "range": -1
    }

    jump = Scoreboard('firework.event.player.jump')


    append function ./load:
        scoreboard objectives add firework.event.player.jump custom:jump


    append function ./tick_players:
        if score @s jump matches -1:
            jump['@s'] = 0
        if score @s jump matches 1..:
            jump['@s'] = -1

else:
    append function ./unload:
        scoreboard objectives remove firework.event.player.jump
#

# DONE
if 'player.sneak' in included:

    predicate event:player.sneak {
        "condition": "minecraft:value_check",
        "value": {
            "type": "minecraft:score",
            "target": "this",
            "score": "firework.event.player.sneak"
            },
        "range": 1
    }

    load_dep('is_sneaking')
    sneak = Scoreboard('firework.event.player.sneak')


    append function ./load:
        scoreboard objectives add firework.event.player.sneak dummy
        

    append function ./tick_players:
        if predicate ./deps/is_sneaking:
            sneak['@s'] += 1
        unless predicate ./deps/is_sneaking:
            sneak['@s'] = 0

else:
    append function ./unload:
        scoreboard objectives remove firework.event.player.sneak
#

# DONE
if 'player.sneak.c' in included:
    predicate event:player.sneak.c {
        "condition": "minecraft:reference",
        "name": "firework:deps/is_sneaking"
    }

    load_dep('is_sneaking')

#

# DONE
if 'player.sprint' in included:

    predicate event:player.sprint {
        "condition": "minecraft:value_check",
        "value": {
            "type": "minecraft:score",
            "target": "this",
            "score": "firework.event.player.sprint"
            },
        "range": 1
    }

    load_dep('is_sprinting')
    sprint = Scoreboard('firework.event.player.sprint')


    append function ./load:
        scoreboard objectives add firework.event.player.sprint dummy
        

    append function ./tick_players:
        if predicate ./deps/is_sprinting:
            sprint['@s'] += 1
        unless predicate ./deps/is_sprinting:
            sprint['@s'] = 0

else:
    append function ./unload:
        scoreboard objectives remove firework.event.player.sprint
#

# DONE
if 'player.sprint.c' in included:
    predicate event:player.sprint.c {
        "condition": "minecraft:reference",
        "name": "firework:deps/is_sprinting"
    }

    append function ./load_deps:
        load_dep('is_sprinting')

#
    
# DONE
if 'player.swim' in included:

    predicate event:player.swim {
        "condition": "minecraft:value_check",
        "value": {
            "type": "minecraft:score",
            "target": "this",
            "score": "firework.event.player.swim"
            },
        "range": 1
    }

    load_dep('is_swimming')
    swim = Scoreboard('firework.event.player.swim')


    append function ./load:
        scoreboard objectives add firework.event.player.swim dummy


    append function ./tick_players:
        if predicate ./deps/is_swimming:
            swim['@s'] += 1
        unless predicate ./deps/is_swimming:
            swim['@s'] = 0

else:
    append function ./unload:
        scoreboard objectives remove firework.event.player.swim
#

# DONE
if 'player.swim.c' in included:
    predicate event:player.swim.c {
        "condition": "minecraft:reference",
        "name": "firework:deps/is_swimming"
    }

    append function ./load_deps:
        load_dep('is_swimming')

#
   

# ==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==--==


# automatically unloads unused dependencies
# checks the loaded_deps list and unloads unused dependencies (mostly removes scoreboard objectives)
unload_unused_deps()








