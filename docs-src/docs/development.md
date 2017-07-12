## Event Listeners

Events can be registered to different listeners to take action on them. In almost all cases, there should only be one listener for an event at any given time. This means you will need to be mindful of "removing" a listener when appropiate.

To remove an event listener use __removeEventListener__:

```lua
cb.events:removeEventListener('OnJoined', onJoined)
```

You should always be listening for the [OnClientList](events#onclientlist) and [OnClosed](events#onclosed) event throughout your application while connected to your instance. The [OnClientList](events#onclientlist) event provides a syncing point for the clients currently connected to the room. This event is trigged when a client joins, leaves, or makes a name change in the room. ___If you're not listening for this event, you will not know what the current state of the room is.___

The [OnClosed](events#onclosed) event is important for clean up associated with a client disconnecting from the server.

The [OnJoined](events#onjoined) and [OnLeft](events#onleft) events are helpful for cleaning up client specfic state.

!!! tip
    Don't try to manage your client list using the [OnJoined](events#onjoined) and [OnLeft](events#onleft) events. Instead listen for the [OnClientList](events#onclientlist) event to keep your client list current.

## Client Names

When a client connects to a room, it's possible for two (or more) clients to have the same name. A clients name can be changed with the [changeName](actions#changename) action. If you want to impose unique names, you can write logic in the [OnJoined](events#onjoined) event to check the client list for a matching name, and prompt the user to change their name.

## System Messages

System messages can facilitate more complex applications. A system message contains two properties, an __action__ and __payload__. Using this message type allows you to control your application without having to parse through "chat" based messages.

The __action__ property should be a unique key that you can check for on the [OnSystemMessage](events#onsystemmessage) event. The __payload__ is any data required for that action. It can be a simple value, or a complex data table.

By listening for system messages, and checking the __action__ key you can implement limitless functionality:

```lua
local cb = require('chatterbox.client')

local function onSystemMessage( evt )
  local action = evt.action
  local payload = evt.payload

  if action == 'open_broswer' then
    --possible function, payload is a data table
    openWebView(payload.url)
  elseif action == 'flash_screen' then
    --possible function, payload contains a number
    flashScreen({repeat=payload})
  end
end

cb.events:addEventListener('OnSystemMessage', onSystemMessage)
```

!!! important
    System messages are always broadcast to the entire room. If you want an event to only operate on a specific client, pass the clients ID along in the payload and check against it.

Acting on a specific client:

```lua
local cb = require('chatterbox.client')

local function onSystemMessage( evt )
  local action = evt.action
  local payload = evt.payload --a data table

  if action == 'set_score' then
    if payload.client_id == cs:getId() then
      client_score = payload.score
    end
  end
end

cb.events:addEventListener('OnSystemMessage', onSystemMessage)
```

You can also sync application state by passing a data table as the `payload`:

```lua
...

local function onSystemMessage( evt )
  if evt.action == 'state' then
    app_state = evt.payload
    --do stuff with the app_state
  end
end

...
```