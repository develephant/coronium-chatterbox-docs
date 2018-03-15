After the __Coronium ChatterBox__ server is installed, you can change the configuration which the plugin uses to connect to your instance.

The following configuration options are available to set on the server-side:

|Name|Description|Default|
|----|-----------|-------|
|__key__|The client connection key.|"8477ebc412386117059664d45637e397"
|__port__|The port that the client connects to.|7175
|__room__|The default room the client joins, if not provided.|"Lobby"|
|__timeout__|The global client timeout setting, in seconds.|900 (15 min)|

#### Key

To help protect from errant client connections, the server contains a key that must be matched with client connection call. If the key is incorrect or not provided, the connection will be closed. There is a default key baked into the client and the server, but you should change this at your convienence.

!!! important
    Once you change the server key, make sure to also update your client connection method by passing the __key__ parameter, or you won't be able to connect. See [Connecting The Client](guide/#connecting-the-client).

#### Port

By default the server will listen for new connections on port 7175, you can change this option as desired.

!!! important
    Once you change the server port, make sure to also update your client connection method by passing the __port__ parameter, or you won't be able to connect. See [Connecting The Client](guide/#connecting-the-client).

#### Room

When a client connection is confirmed, they will be added to the default room _Lobby_. You can change this value to make the default something different.

#### Timeout

Client connection timeouts are managed on the server-side. By default a client will be disconnected after being idle for 15 minutes. This option can be set to 0 to disable the timeout.

!!! warning
    While you can disable the client timeout by provding a 0 as a config value, you run the risk of have a client "hang" or in other cases an errant client that takes up one of the servers connections. ___Disabling the timeout is not recommended___.

---

### config.lua

To update the configuration file, log in with the __coronium__ user:

```
ssh coronium@<your-instance-ip>
```

Open the __chatterbox/config.lua__ using the __nano__ file editor:

```
nano ~/config.lua
```

You should see the following default content in the __chatterbox/config.lua__ file:

```
return 
{
  key = "8477ebc412386117059664d45637e397",
  port = 7175,
  room = "Lobby",
  timeout = 900 --secs
}
```

Use the arrow keys to move the cursor, and replace the values as needed:

```
return 
{
  key = "12345abcdef",
  port = 7175,
  room = "Lobby",
  timeout = 900 --secs
}
```

When your changes have been made, use __control-x__ , then press __y__, and then press enter, to save the changes.

Reload the __chatterbox__ process to pick up the new configuration:

```
sudo monit restart chatterbox
```

Close the shell connection with:

```
exit
```

!!! note
    Any changed values will need to be added/updated in the plugin __connect__ method as well. See __[Connecting](/client/connect)__.