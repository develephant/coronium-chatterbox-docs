To connect to your __Coronium Chatterbox__ instance, you will first need your IP address (see [Installation](/#installation)) then add the following:

!!! note
    The code below shows the minimum parameters needed to connect to a fresh __Coronium Chatterbox__ instance.

```lua
cb:connect({
  host = '<your-ip-address>',
  name = 'Sandy'
})
```

## Example

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

## Parameters

|Parameter|Description|Required|
|---------|-----------|--------|
|__host__|The instance address.|__Y__|
|__name__|The client display name.|__Y__|
|__port__|The instance port.|N|
|__key__|The authentication key.|N|
|__room__|The initial room to connect to.|N|
|__debug__|Output client-side debugging info.|N|

_See also:_ __[Server Configuration](/server/config)__.