Test the server connection by using __telnet__ on your local command prompt:

```
telnet <your-instance-ip> 7175
```

The server should repond with a "handshake" prompt:

```
{"_handshake":1}
```

!!! tip
    The server will disconnect any client who does not properly respond to a handshake request after 5 seconds.