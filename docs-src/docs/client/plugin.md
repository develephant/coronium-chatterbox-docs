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