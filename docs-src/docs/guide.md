## Get The Plugin

If you don't already have it, get the __Coronium Chatterbox Plugin__ from the __[Corona Marketplace](https://marketplace.coronalabs.com/)__.


## Adding The Plugin

Add the plugin by adding an entry to the __plugins__ table of __build.settings__ file:

```
settings =
{
    plugins =
    {
        ["plugin.chatterbox"] =
        {
            publisherId = "com.develephant"
        },
    },
}
```

Open your __main.lua__ file and add the following:

```lua
local cb = require('plugin.chatterbox')
```

!!! note
    If you are using Composer you may want to add this elsewhere.

## Connecting The Client

To connect to your __Coronium Chatterbox__ instance, you will first need your IP address (see [Installation](/#installation)) then add the following:

!!! note
    The code below shows the minimum parameters needed to connect to a fresh __Coronium Chatterbox__ instance.

```lua
cb:connect({
  host = '<your-ip-address>',
  name = 'Sandy'
})
```

### Connection Example

```lua
local cb = require('plugin.chatterbox')

local function onConnect()
  print('connected')
end

local function onClosed()
  print('disconnected')
end

local function onError(evt)
  if evt.error then
    print(evt.error)
  end
end

cb.events:addEventListener('OnConnect', onConnect)
cb.events:addEventListener('OnClosed', onClosed)
cb.events:addEventListener('OnError', onError)

cb:connect({
  host = '<your-ip-address>'
  name = 'Sandy'
})
```

### Connection Parameters

|Parameter|Description|Required|
|---------|-----------|--------|
|__host__|The instance address.|__Y__|
|__name__|The client display name.|__Y__|
|__port__|The instance port.|N|
|__key__|The authentication key.|N|
|__room__|The initial room to connect to.|N|
|__debug__|Output client-side debugging info.|N|

!!! tip
    See [Configuration](server/#configuration) in the Server Guide for more details.

## Client Overview

Being a real-time client, you will use event listeners to respond to events sent from the __Coronium Chatterbox__ instance. You can learn more about the events received in [Client Events](events).

### Rooms

__Coronium Chatterbox__ allows you to group clients into "rooms" of your choosing. Using the [joinRoom](actions#joinroom) action you can switch between rooms. By default, the client is connected to the "Lobby" room. If you want to join a different room when connecting, supply the __room__ parameter to the __cs:connect__ method (see [Connecting The Client](#connecting-the-client)).

When a client joins a new room, they are automatically removed from the previous room they were in. A client can only be a member of one room at any given time.

At anytime a client joins a room , a [OnJoined](events#onjoined) event will be triggered in the room with the newly joined members information. ___The joining client also receives this message___.

If a user leaves a room, the [OnLeft](events#onleft) event is triggered in the room. The leaving client will not receive this message.

!!! tip
    Whenever a client joins or leaves a room (or changes their name), an [OnClientList](events#onclientlist) event will be triggered. This is a good time to store and update your client display list.

To retrieve a list of the active rooms on your __Coronium Chatterbox__ instance use the [getRooms](actions#getrooms) action along with the [OnRoomList](events#onroomlist) event. 

### Client List

The client list is broadcast to the room on any [OnJoined](events#onjoined), [OnLeft](events#onleft), or [OnNameChange](events#onnamechange) event. You can use this list to create a user display component, and to also gather the ID needed for the [sendWhisper](actions#sendwhisper) action. To listen for the client list use the [OnClientList](events#onclientlist) event.

### Messages

The client can send (and receive) three different types of messages; a "room" message, a "private" message, and a "system" message.

__Room Message__

A room message is sent using the [sendMessage](actions#sendmessage) action. This message type is broadcast to all members of the current room the client is in. You can receive this message type using the [OnMessage](events#onmessage) event. This message type works well for chat messaging.

!!! important
    All clients in the room receive this message type, including the client sending the message.

__Private Message__

You can send a message directly to any user in the room using the [sendWhisper](actions#sendwhisper) action. This message type is sent directly to the recipient. It is not broadcast into the room. To send a "whisper" you need to supply the recipient ID. You can get ahold of these ids within the [OnClientList](events#onclientlist) event.

__System Message__

To send control type messages within the room, you can use a "system" message. This message type is broadcast to the room, and can be captured outside of your "chat" layer. To receive these use the [OnSystemMessage](events#onsystemmessage) event.

System messages can be used for controlling aspects of your application state. For example, opening a webview with a passed url.

!!! important
    All clients in the room receive this message type, including the client sending the message.

### Status Events

To listen for error events from both the __Coronium Chatterbox__ instance, as well as, the local client use the [OnError](events#onerror) event.

To listen for client timeout (if any) use the [OnTimeout](events#ontimeout) event.

To listen for the client disconnecting from the instance, use the [OnClosed](events#onclosed) event.