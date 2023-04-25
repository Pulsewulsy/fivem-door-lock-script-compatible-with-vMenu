# fivem-door-lock-script-compatible-with-vMenu

 

```lua
-- Define the door objects
local door1 = {
    object = nil, -- The door object
    position = vector3(100.0, -200.0, 71.0), -- The position of the door
    rotation = vector3(0.0, 0.0, 0.0), -- The rotation of the door
    locked = true -- Whether the door is locked or not
}

local door2 = {
    object = nil,
    position = vector3(100.0, -202.0, 71.0),
    rotation = vector3(0.0, 0.0, 0.0),
    locked = true
}

-- Function to create doors
function createDoor(door)
    door.object = CreateObject(GetHashKey("prop_ld_bankdoors_02"), door.position.x, door.position.y, door.position.z, false, false, false)
    SetEntityRotation(door.object, door.rotation.x, door.rotation.y, door.rotation.z, 2, true)
    FreezeEntityPosition(door.object, door.locked)
end

-- Create the doors
createDoor(door1)
createDoor(door2)

-- Function to toggle door lock status
function toggleDoorLock(door)
    door.locked = not door.locked
    FreezeEntityPosition(door.object, door.locked)
    if door.locked then
        print("Door locked.")
    else
        print("Door unlocked.")
    end
end

-- Function to handle door interaction
function interactWithDoor(door)
    if not door.locked then
        DoorSystemSetDoorState(door.object, false, false)
    else
        print("Door is locked.")
    end
end

-- Register vMenu permissions for the doors
function registerVMenuPermissions()
    for doorIndex, door in ipairs({door1, door2}) do
        local doorPermission = "door" .. doorIndex
        AddResourceCommandPermission("vMenu", doorPermission)
        RegisterCommand(doorPermission, function(source, args, rawCommand)
            local player = source
            if IsPlayerAceAllowed(player, "command." .. doorPermission) then
                toggleDoorLock(door)
            else
                TriggerClientEvent('chat:addMessage', player, {args = {"^1Error: You don't have permission to use this command."}})
            end
        end, false)
    end
end

-- Start the vMenu permission registration
Citizen.CreateThread(registerVMenuPermissions)
```

This script uses vMenu's built-in permission system to control access to door commands. It registers custom commands for each door, such as `/door1` and `/door2`, and checks the player's vMenu permissions to determine whether they have access to the command. If the player has permission, they can toggle the lock status of the corresponding door. Otherwise, an error message is displayed in the player's chat. Note that you will need to configure the appropriate ACE permissions in vMenu's configuration file to grant or revoke access to these commands based on your desired access control logic.
