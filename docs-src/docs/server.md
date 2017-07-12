# System Service

When your __Coronium ChatterBox__ instance starts, its monitored by a utility called __Monit__, which makes sure that your instance keeps running. In the event that the server runs into an issue or crashes, it will be restarted shortly.

In the rare case where you need to manually stop or start the instance, log in using the __coronium__ user.

```
ssh coronium@<your-instance-ip>
```

To stop the instance, on the command line, enter:

```
sudo monit stop chatterbox
```

To start the instance, use:

```
sudo monit start chatterbox
```

_You will be prompted for your password. The default is __coroniumadmin__._

!!! caution
    You should rarely need to manually control the instance.

---

## Configuration

The following configuration options are available to set on the server-side:

|Name|Description|Default|
|----|-----------|-------|
|__key__|The client connection key.|"8477ebc412386117059664d45637e397"
|__port__|The port that the client connects to.|7175
|__room__|The default room the client joins, if not provided.|"Lobby"|
|__timeout__|The global client timeout setting, in seconds.|900 (15 min)|

To adjust the configuration options, you will need to log into the server via SFTP:

First log into your __Coronium ChatterBox__ instance with an SFTP program.

!!! tip
    The open source SFTP browser [FileZilla](https://filezilla-project.org/) works well for both OSX and Windows.

Log in with the SFTP program using the __coronium__ user.

Supply the password (if you didn't change it from the fresh install, it's __coroniumadmin__).

Once inside your instance, download the __config.lua__ file to your local machine.

Open the __config.lua__ file in a text editor and edit the config table, as needed.

_Default table in config.lua:_

```lua
return 
{
  key = "8477ebc412386117059664d45637e397",
  port = 7175,
  room = "Lobby",
  timeout = 900 --secs
}
```

Once your changes are complete, upload the __config.lua__ back into same directory.

Now you'll need to log into the server via SSH using the __coronium__ user:

```
ssh coronium@<your-instance-ip>
```

Once you are logged in, restart the __chatterbox__ service to make the new config active:

```
sudo monit restart chatterbox
```

Your instance will now be using the new configuration options.

!!! tip
    You can also use the __nano__ text editor on the server (nano config.lua), skipping the config.lua download steps.

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

## Viewing Logs

When developing your application, there may be times when you'd like to see what's happening on the server side. You can view the log file to acheive this.

!!! tip
    Log file timestamps are noted in UTC.

First, log into your instance using the __coronium__ user:

```
ssh coronium@<your-instance-ip>
```

To view the log file run the following on the command line:

```
cat logs/chatterbox.log
```

To follow the log in real-time, use:

```
tail -f logs/chatterbox.log
```

!!! note
    The log file is managed automatically, and will be "rotated" once it exceeds a certain size limit.
