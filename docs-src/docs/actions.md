# Action Table

|Action Name|Description|Scope|
|-----------|-----------|-----|
|[`joinRoom`](#joinroom)|Join a room.|_client_|
|[`sendMessage`](#sendmessage)|Send a message to the room.|_room_|
|[`sendWhisper`](#sendwhisper)|Send a private message to a client.|_client_|
|[`sendSystemMessage`](#sendsystemmessage)|Send a system message to the room.|_room_|
|[`changeName`](#changename)|Change the client name.|_client_|
|[`getRoomList`](#getroomlist)|Get a list of active rooms from the server.|_client_|
|[`getId`](#getid)|Get the client unique ID.|_client_|
|[`getName`](#getname)|Get the client name.|_client_|
|[`disconnect`](#disconnect)|Disconnect the client from the server.|_client_|

---

### changeName

Change the client name.

__Action Parameters__

|Parameters|Description|Type|
|----------|-----------|----|
|`name`|The new client name.|String|

__Example__

```lua
local cb = require('chatterbox.client')

cb:changeName( 'Marco' )
```

__See also:__ [`OnNameChange`](events/#onnamechange) event.

---

### disconnect

Disconnect the client from the server.

__Action Parameters__

_This action has no parameters._

__Example__

```lua
local cb = require('chatterbox.client')

cb:disconnect()
```

__See also:__ [`OnClosed`](events/#onclosed) event.

---

### getId

Get the clients unique identifier.

__Action Parameters__

_This action has no parameters._

__Example__

```lua
local cb = require('chatterbox.client')

local id = cb:getId()
```

---

### getRoomList

Get a list of active rooms from the server.

__Action Parameters__

_This action has no parameters._

__Example__

```lua
local cb = require('chatterbox.client')

local name = cb:getRoomList()
```

__See also:__ [`OnRoomList`](events/#onroomlist) event.

---

### getName

Get the clients name.

__Action Parameters__

_This action has no parameters._

__Example__

```lua
local cb = require('chatterbox.client')

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
|`room`|The room ID to join.|String|

__Example__

```lua
local cb = require('chatterbox.client')

cb:joinRoom( 'GameOne' )
```

__See also:__ [`OnJoined`](events/#onjoined) event.

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
|`message`|The message content.|String|

__Example__

```lua
local cb = require('chatterbox.client')

cb:sendMessage( 'Hello people!' )
```

__See also:__ [`OnMessage`](events/#onmessage) event.

---

### sendSystemMessage

Send a system message to the room.

!!! warning
    Payload data cannot contain __\n__ (newline) characters.

__Action Parameters__

|Parameters|Description|Type|
|----------|-----------|----|
|`action`|The system action ID.|String|
|`payload`|The action payload.|Table|

__Example__

```lua
local cb = require('chatterbox.client')

cb:sendSystemMessage( 'player_turn', {turn = 1} )
```

__See also:__ [`OnSystemMessage`](events/#onsystemmessage) event.

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
|`message`|The message content.|String|
|`to_id`|The recipient ID.|String|

__Example__

```lua
local cb = require('chatterbox.client')

cb:sendWhisper( 'Hello you!', 'b697869e-5e07-4a91-89bd-1c737123cae2' )
```

!!! note
    You can obtain a recipient ID from the client list. See the [OnClientList](events/#onclientlist) event.

__See also:__ [`OnWhisper`](events/#onwhisper) event.
