# Produce coloured log output in adb logcat style

This script is intended to be used for producing coloured logfile output, in 
similar fashion to `adb logcat`, which highlights each log line with a different
colour, based on its loglevel.

The script looks for log lines that start with `<timestamp> <name> <loglevel>`,
where the log level must be one of DEBUG, INFO, WARNING, ERROR or CRITICAL, 
written in capital letters, and colours the line accordingly. Any other lines
are left untouched (default terminal colour). The intended logfile output should
be produced by Python, e.g.:

```python
import logging
logformat = logging.Formatter("%(asctime)s %(name)s %(levelname)s %(message)s")
```

The script can be used as a `tail -f` replacement. If no input is provided on
stdin, it will by default follow the given file:
```console
user@server ~# colortail /var/log/someservice.log
```
The script will show the last 80 lines of the file and follow it, or keep 
trying if the file is not yet accessible. I.e. same as `tail -n 80 -F`.

Or it can accept piped input:
```console
user@server ~# colortail < /var/log/someservice.log
user@server ~# cat somefile.log | colortail
```

## Samples

A logfile with `<timestamp> <additional> <producer name> <log level> <message>`:
![out](https://github.com/mensonen/colortail/assets/2760970/98e4a1e5-76be-4ee2-91e3-96f22cd5eeb8)

A logfile with `<timestamp> <producer name> <log level> <thread> <message>`,
showing all possible log levels and their colours:
![out](https://github.com/mensonen/colortail/assets/2760970/5fc5056e-c4ee-4547-82a1-b6c63bcada41)
