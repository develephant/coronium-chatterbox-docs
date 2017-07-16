When your __Coronium ChatterBox__ server starts, its monitored by a utility called __Monit__, which makes sure that the __chatterbox__ process is active. In the event that the process runs into an issue or crashes, it will be restarted shortly.

In the rare case where you need to manually stop or start the process, log in using the __coronium__ user.

```
ssh coronium@<your-instance-ip>
```

To stop the process, on the command line, enter:

```
sudo monit stop chatterbox
```

To start the process, use:

```
sudo monit start chatterbox
```

!!! caution
    You should rarely need to manually control the __chatterbox__ process.

