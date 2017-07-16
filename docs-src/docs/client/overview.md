Being a real-time client, you will use event listeners to respond to events sent from the __Coronium Chatterbox__ instance. You can learn more about the events received in [Client Events](/events).

## Rooms

__Coronium Chatterbox__ allows you to group clients into "rooms" of your choosing. Using the [joinRoom](/actions#joinroom) action you can switch between rooms. By default, the client is connected to the "Lobby" room. If you want to join a different room when connecting, supply the __room__ parameter to the __cs:connect__ method (see [Connecting The Client](/client/connect)).

When a client joins a new room, they are automatically removed from the previous room they were in. A client can only be a member of one room at any given time.

At anytime a client joins a room , a [OnJoined](/events#onjoined) event will be triggered in the room with the newly joined members information. ___The joining client also receives this message___.

If a user leaves a room, the [OnLeft](/events#onleft) event is triggered in the room. The leaving client will not receive this message.

!!! tip
    Whenever a client joins or leaves a room (or changes their name), an [OnClientList](/events#onclientlist) event will be triggered. This is a good time to store and update your client display list.

To retrieve a list of the active rooms on your __Coronium Chatterbox__ instance use the [getRooms](/actions#getrooms) action along with the [OnRoomList](/events#onroomlist) event. 

## Client List

The client list is broadcast to the room on any [OnJoined](/events#onjoined), [OnLeft](/events#onleft), or [OnNameChange](/events#onnamechange) event. You can use this list to create a user display component, and to also gather the ID needed for the [sendWhisper](/actions#sendwhisper) action. To listen for the client list use the [OnClientList](/events#onclientlist) event.

## Messages

The client can send (and receive) three different types of messages; a "room" message, a "private" message, and a "system" message.

__Room Message__

A room message is sent using the [sendMessage](/actions#sendmessage) action. This message type is broadcast to all members of the current room the client is in. You can receive this message type using the [OnMessage](/events#onmessage) event. This message type works well for chat messaging.

!!! important
    All clients in the room receive this message type, including the client sending the message.

__Private Message__

You can send a message directly to any user in the room using the [sendWhisper](/actions#sendwhisper) action. This message type is sent directly to the recipient. It is not broadcast into the room. To send a "whisper" you need to supply the recipient ID. You can get ahold of these ids within the [OnClientList](/events#onclientlist) event.

__System Message__

To send control type messages within the room, you can use a "system" message. This message type is broadcast to the room, and can be captured outside of your "chat" layer. To receive these use the [OnSystemMessage](/events#onsystemmessage) event.

System messages can be used for controlling aspects of your application state. For example, opening a webview with a passed url.

!!! important
    All clients in the room receive this message type, including the client sending the message.

## Status Events

To listen for error events from both the __Coronium Chatterbox__ instance, as well as, the local client use the [OnError](/events#onerror) event.

To listen for client timeout (if any) use the [OnTimeout](/events#ontimeout) event.

To listen for the client disconnecting from the instance, use the [OnClosed](/events#onclosed) event.