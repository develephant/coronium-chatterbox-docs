# Action Table

|Action Name|Description|Scope|
|-----------|-----------|-----|
|__[joinRoom](#joinroom)__|Join a room.|_client_|
|__[sendMessage](#sendmessage)__|Send a message to the room.|_room_|
|__[sendWhisper](#sendwhisper)__|Send a private message to a client.|_client_|
|__[sendSystemMessage](#sendsystemmessage)__|Send a system message to the room.|_room_|
|__[changeName](#changename)__|Change the client name.|_client_|
|__[getRoomList](#getroomlist)__|Get a list of active rooms from the server.|_client_|
|__[getId](#getid)__|Get the client unique ID.|_client_|
|__[getName](#getname)__|Get the client name.|_client_|
|__[disconnect](#disconnect)__|Disconnect the client from the server.|_client_|

---

### changeName

Change the client name.

__Action Parameters__

|Parameters|Description|Type|
|----------|-----------|----|
|__name__|The new client name.|String|

__Example__

```lua
cb:changeName( 'Marco' )
```

_See also:_ __[OnNameChange](events/#onnamechange)__ event.

---

### disconnect

Disconnect the client from the server.

__Action Parameters__

_This action has no parameters._

__Example__

```lua
cb:disconnect()
```

_See also:_ __[OnClosed](events/#onclosed)__ event.

---

### getId

Get the clients unique identifier.

__Action Parameters__

_This action has no parameters._

__Example__

```lua
local id = cb:getId()
```

---

### getRoomList

Get a list of active rooms from the server.

__Action Parameters__

_This action has no parameters._

__Example__

```lua
local name = cb:getRoomList()
```

_See also:_ __[OnRoomList](events/#onroomlist)__ event.

---

### getName

Get the clients name.

__Action Parameters__

_This action has no parameters._

__Example__

```lua
local name = cb:getName()
```

---

### joinRoom

Join a room.

!!! note
    A client can only be connected to one room at a time. When joining a new room, the client will automatically leave the previous room.

__Action Parameters__

|Parameters|Description|Type|
|----------|-----------|----|
|__room__|The room ID to join.|String|

__Example__

```lua
cb:joinRoom( 'GameOne' )
```

_See also:_ __[OnJoined](events/#onjoined)__ event.

---

### sendMessage

Send a message to the room.

!!! warning
    Do not use the reserved sequence __//__ (two forward slashes) in your message body.

!!! important
    The message body must be of type __String__.

__Action Parameters__

|Parameters|Description|Type|
|----------|-----------|----|
|__message__|The message content.|String|

__Example__

```lua
cb:sendMessage( 'Hello people!' )
```

_See also:_ __[OnMessage](events/#onmessage)__ event.

---

### sendSystemMessage

Send a system message to the room.

!!! warning
    Payload data cannot contain __\n__ (newline) characters.

__Action Parameters__

|Parameters|Description|Type|
|----------|-----------|----|
|__action__|The system action ID.|String|
|__payload__|The action payload.|Table|

__Example__

```lua
cb:sendSystemMessage( 'player_turn', {turn = 1} )
```

_See also:_ __[OnSystemMessage](events/#onsystemmessage)__ event.

---

### sendWhisper

Send a private message to a client.

!!! warning
    Do not use the reserved sequence __//__ (two forward slashes) in your message body.

!!! important
    The message body must be of type __String__.

__Action Parameters__

|Parameters|Description|Type|
|----------|-----------|----|
|__message__|The message content.|String|
|__to_id__|The recipient ID.|String|

__Example__

```lua
cb:sendWhisper( 'Hello you!', 'b697869e-5e07-4a91-89bd-1c737123cae2' )
```

!!! note
    You can obtain a recipient ID from the client list. See the [OnClientList](events/#onclientlist) event.

_See also:_ __[OnWhisper](events/#onwhisper)__ event.
