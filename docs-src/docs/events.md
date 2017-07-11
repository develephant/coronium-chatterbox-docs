# Event Table

|Event Name|Description|Scope|
|----------|-----------|-----|
|[`OnConnect`](#onconnect)|The client has connected to the server.|_client_|
|[`OnJoined`](#onjoined)|A client has joined the current room.|_room_|
|[`OnLeft`](#onleft)|A client has left the current room.|_room_|
|[`OnMessage`](#onmessage)|A room message has been received.|_room_|
|[`OnWhisper`](#onwhisper)|A private message has been received.|_client_|
|[`OnSystemMessage`](#onsystemmessage)|A system message has been received.|_room_|
|[`OnClientList`](#onclientlist)|The client list of the current room.|_room_|
|[`OnRoomList`](#onroomlist)|A list of rooms active on the server.|_client_|
|[`OnNameChange`](#onnamechange)|A client has changed their name.|_room_|
|[`OnUnknownEvent`](#onunknownevent)|An unknown event has been received.|_client_|
|[`OnClosed`](#onclosed)|The client has disconnected from the server.|_client_|
|[`OnTimeout`](#ontimeout)|The client connection has timed out.|_client_|
|[`OnError`](#onerror)|The client has received an error message.|_client_|

---

# Event Listeners

To listen for events, add an event listener:

```lua
local cb = require('chatterbox.client')

local function onMessage( evt )
  print(evt.data.name, evt.data.msg)
end

cb.events:addEventListener('OnMessage', onMessage)
```

!!! note
    All event properties can be found on the `data` object of the event (`evt.data.<prop>`). See the following event details for their available properties.

---

### OnClientList

The client has received the client list for the current room.

__Data Properties__

|Property|Description|Type|
|--------|-----------|----|
|`clients`|A table array of clients.|Table|
|`cnt`|The client list count.|Number|
|`room`|The current room name.|String|

__Example__

```lua
local cb = require('chatterbox.client')

local function onClientList( evt )
  print('client count:'..evt.data.cnt)
  local clients = evt.data.clients
  for i=1, #clients do
    local client = clients[i]
    print(client.name, client.id)
  end
end

cb.events:addEventListener('OnClientList', onClientList)
```

---

### OnClosed

The client has been disconnected from the server.

__Data Properties__

_This event has no available properties._

```lua
local cb = require('chatterbox.client')

local function onClosed()
  print('Client has disconnected from the server')
end

cb.events:addEventListener('OnClosed', onClosed)
```

---

### OnConnect

The client has connected to the server.

__Data Properties__

_This event has no available properties._

__Example__

```lua
local cb = require('chatterbox.client')

local function onConnect()
  print('Client has successfully connected to the server')
end

cb.events:addEventListener('OnConnect', onConnect)
```

---

### OnError

The client has received an error message.

!!! note
    The `OnError` event triggers on both local and server-side errors.

!!! important
    The `OnError` event does not contain a `data` property. To access the error, use the `error` key directly on the event: `evt.error`.

__Example__

```lua
local cb = require('chatterbox.client')

local function OnError( evt )
  print('Client Error: '..evt.error)
end

cb.events:addEventListener('OnError', onError)
```

---

### OnJoined

A client has joined the current room.

__Data Properties__

|Property|Description|Type|
|--------|-----------|----|
|`name`|Name of the joining client.|String|
|`id`|Unique ID of the client.|String|
|`room`|The current room name.|String|

__Example__

```lua
local cb = require('chatterbox.client')

local function onJoined( evt )
  print(evt.data.name, evt.data.id, evt.data.room)
end

cb.events:addEventListener('OnJoined', onJoined)
```

---

### OnLeft

A client has left the current room.

__Data Properties__

|Property|Description|Type|
|--------|-----------|----|
|`name`|Name of the joining client.|String|
|`id`|Unique ID of the client.|String|
|`room`|The current room name.|String|

__Example__

```lua
local cb = require('chatterbox.client')

local function onLeft( evt )
  print(evt.data.name, evt.data.id, evt.data.room)
end

cb.events:addEventListener('OnLeft', onLeft)
```

---

### OnMessage

A room message has been received.

__Data Properties__

|Property|Description|Type|
|--------|-----------|----|
|`name`|Name of the sender.|String|
|`id`|Unique ID of the sender.|String|
|`msg`|The message content.|String|
|`room`|The current room name.|String|

__Example__

```lua
local cb = require('chatterbox.client')

local function onMessage( evt )
  print(evt.data.name, evt.data.msg)
end

cb.events:addEventListener('OnMessage', onMessage)
```

---

### OnNameChange

A client has changed their display name.

!!! note
    After this event is received an `OnClientList` event will follow with the updated name, allowing for easy updating of the client display list.

__Data Properties__

|Property|Description|Type|
|--------|-----------|----|
|`name`|New name of the client.|String|
|`old_name`|Previous name of the client.|String|
|`id`|Unique ID of the client.|String|
|`room`|The current room name.|String|

__Example__

```lua
local cb = require('chatterbox.client')

local function onNameChange( evt )
  print('Old name was: '..evt.data.old_name)
  print('New name is: '..evt.data.name)
end

cb.events:addEventListener('OnNameChange', onNameChange)
```

---

### OnRoomList

A list of active rooms on the server.

__Data Properties__

|Property|Description|Type|
|--------|-----------|----|
|`rooms`|A table array of rooms names.|String|
|`cnt`|The client list count.|String|
|`room`|The current room name.|String|

__Example__

```lua
local cb = require('chatterbox.client')

local function onRoomList( evt )
  print('room count:'..evt.data.cnt)
  local rooms = evt.data.rooms
  for i=1, #rooms do
    print(rooms[i].name)
  end
end

cb.events:addEventListener('onRoomList', onRoomList)
```

---

### OnSystemMessage

A system message has been received.

!!! note
    A system message can be used to broadcast a message for service related events outside of the "chat" layer.

__Data Properties__

|Property|Description|Type|
|--------|-----------|----|
|`action`|The system event action ID.|String|
|`payload`|The system event payload.|Table|
|`room`|The current room name.|String|

__Example__

```lua
local cb = require('chatterbox.client')

local function onSystemMessage( evt )
  if evt.data.action == 'player_turn' then
    print('players turn is: '..evt.data.payload)
  end
end

cb.events:addEventListener('OnSystemMessage', onSystemMessage)
```

---

### OnTimeout

The client has timed out.

!!! important
    The timeout event is only a status message and does not disconnect the client. When the client receives this message you should use the `disconnect` action to close the connection, if wanted.

__Data Properties__

_This event has no available properties._

__Example__

```lua
local cb = require('chatterbox.client')

local function onTimeout()
  print('The client has timed out')

  --if you'd like to disconnect on a timeout event:
  cs:disconnect()
end

cb.events:addEventListener('OnTimeout', onTimeout)
```

---

### OnUnknownEvent

An unknown event type was received by the client.

!!! caution
    You should only us this event for logging purposes. Do not perform any actions on it.

__Data Properties__

_This event has no available properties._

__Example__

```lua
local cb = require('chatterbox.client')

local function onUnkownEvent()
  print('An unknown event was received')
end

cb.events:addEventListener('OnUnknownEvent', onUnkownEvent)
```

---

### OnWhisper

A private message has been received.

!!! note
    This message will only be received by its intended recipient.

__Data Properties__

|Property|Description|Type|
|--------|-----------|----|
|`name`|Name of the sender.|String|
|`id`|Unique ID of the sender.|String|
|`msg`|The message content.|String|
|`room`|The current room name.|String|

__Example__

```lua
local cb = require('chatterbox.client')

local function onWhisper( evt )
  print(evt.data.name, evt.data.msg)
end

cb.events:addEventListener('OnWhisper', onWhisper)
```
