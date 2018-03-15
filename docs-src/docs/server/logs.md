To view the log file, connect to the instance with the __coronium__ user.

```
ssh coronium@<your-instance-ip>
```

To watch the log in real-time:

```
tail -f ~/chatterbox/logs/chatterbox.log
```

Press __control-c__ to stop watching the log file.

!!! note
    The log file is managed automatically, and will be "rotated" once it exceeds a certain size limit.