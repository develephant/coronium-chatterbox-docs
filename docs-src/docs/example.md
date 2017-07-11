# main.lua

What follows is a complete set up of all __Coronium ChatterBox__ events and associated listeners.

To learn more about the properties contained in each event see [Client Events](events).

---

```lua
local cb = require('chatterbox.client')

--#############################################################
--# Listeners
--#############################################################

local function onConnect()
  print('Connected')
end

local function onJoined( evt )
  print('Client Joined')
end

local function onLeft( evt )
  print('Client Left')
end

local function onMessage( evt )
  print('Got Message')
end

local function onWhisper( evt )
  print('Got Whisper')
end

local function onSystemMessage( evt )
  print('Got System Message')
end

local function onClientList( evt )
  print('Got Client List')
end

local function onRoomList( evt )
  print('Got Room List')
end

local function onNameChange( evt )
  print('Name Changed')
end

local function onUnknownEvent( evt )
  print('Unknown Event')
end

local function onTimeout()
  print('Timed Out')
end

local function onClosed()
  print('Disconnected')
end

local function onError( evt )
  print('Error')
end

--#############################################################
--# Events
--#############################################################

cb.events:addEventListener('OnConnect', onConnect)
cb.events:addEventListener('OnJoined', onJoined)
cb.events:addEventListener('OnLeft', onLeft)
cb.events:addEventListener('OnMessage', onMessage)
cb.events:addEventListener('OnWhisper', onWhisper)
cb.events:addEventListener('OnSystemMessage', onSystemMessage)
cb.events:addEventListener('OnClientList', onClientList)
cb.events:addEventListener('OnRoomList', onRoomList)
cb.events:addEventListener('OnNameChange', onNameChange)
cb.events:addEventListener('OnUnknownEvent', onUnknownEvent)
cb.events:addEventListener('OnClosed', onClosed)
cb.events:addEventListener('OnTimeout', onTimeout)
cb.events:addEventListener('OnError', onError)

cb:start({
  host  = '<your-instance-ip>',
  key   = '<your-instance-key>', 
  name  = "Timmy"
})

```