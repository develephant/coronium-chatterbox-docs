# Event Table

|Event Name|Description|Scope|
|----------|-----------|-----|
|__[OnConnect](#onconnect)__|The client has connected to the server.|_client_|
|__[OnJoined](#onjoined)__|A client has joined the current room.|_room_|
|__[OnLeft](#onleft)__|A client has left the current room.|_room_|
|__[OnMessage](#onmessage)__|A room message has been received.|_room_|
|__[OnWhisper](#onwhisper)__|A private message has been received.|_client_|
|__[OnSystemMessage](#onsystemmessage)__|A system message has been received.|_room_|
|__[OnClientList](#onclientlist)__|The client list of the current room.|_room_|
|__[OnRoomList](#onroomlist)__|A list of rooms active on the server.|_client_|
|__[OnNameChange](#onnamechange)__|A client has changed their name.|_room_|
|__[OnUnknownEvent](#onunknownevent)__|An unknown event has been received.|_client_|
|__[OnClosed](#onclosed)__|The client has disconnected from the server.|_client_|
|__[OnTimeout](#ontimeout)__|The client connection has timed out.|_client_|
|__[OnError](#onerror)__|The client has received an error message.|_client_|

---

# Event Listeners

To listen for events, add an event listener:

```lua
local function onMessage( evt )
  print(evt.data.name, evt.data.msg)
end

cb.events:addEventListener('OnMessage', onMessage)
```

!!! note
    All event properties can be found on the __data__ object of the event (__evt.data.<prop\>__). See the following event details for their available properties.

---

### OnClientList

The client has received the client list for the current room.

__Data Properties__

|Property|Description|Type|
|--------|-----------|----|
|__clients__|A table array of clients.|Table|
|__cnt__|The client list count.|Number|
|__room__|The current room name.|String|

__Example__

```lua
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
local function onConnect()
  print('Client has successfully connected to the server')
end

cb.events:addEventListener('OnConnect', onConnect)
```

---

### OnError

The client has received an error message.

!!! note
    The __OnError__ event triggers on both local and server-side errors.

!!! important
    The __OnError__ event does not contain a __data__ property. To access the error, use the __error__ key directly on the event: __evt.error__.

__Example__

```lua
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
|__name__|Name of the joining client.|String|
|__id__|Unique ID of the client.|String|
|__room__|The current room name.|String|

__Example__

```lua
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
|__name__|Name of the joining client.|String|
|__id__|Unique ID of the client.|String|
|__room__|The current room name.|String|

__Example__

```lua
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
|__name__|Name of the sender.|String|
|__id__|Unique ID of the sender.|String|
|__msg__|The message content.|String|
|__room__|The current room name.|String|

__Example__

```lua
local function onMessage( evt )
  print(evt.data.name, evt.data.msg)
end

cb.events:addEventListener('OnMessage', onMessage)
```

---

### OnNameChange

A client has changed their display name.

!!! note
    After this event is received an __OnClientList__ event will follow with the updated name, allowing for easy updating of the client display list.

__Data Properties__

|Property|Description|Type|
|--------|-----------|----|
|__name__|New name of the client.|String|
|__old_name__|Previous name of the client.|String|
|__id__|Unique ID of the client.|String|
|__room__|The current room name.|String|

__Example__

```lua
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
|__rooms__|A table array of rooms names.|String|
|__cnt__|The client list count.|String|
|__room__|The current room name.|String|

__Example__

```lua
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
|__action__|The system event action ID.|String|
|__payload__|The system event payload.|Table|
|__room__|The current room name.|String|

__Example__

```lua
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
    The timeout event is only a status message and does not disconnect the client. When the client receives this message you should use the __disconnect__ action to close the connection, if wanted.

__Data Properties__

_This event has no available properties._

__Example__

```lua
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
|__name__|Name of the sender.|String|
|__id__|Unique ID of the sender.|String|
|__msg__|The message content.|String|
|__room__|The current room name.|String|

__Example__

```lua
local function onWhisper( evt )
  print(evt.data.name, evt.data.msg)
end

cb.events:addEventListener('OnWhisper', onWhisper)
```
