# Journalctl

## List all the units
```
systemctl list-unit-files --all
```

## See and follow logs of a unit
```
journalctl -f -u <systemd_unit>
```

## Logs cleanup
````
journalctl --vacuum-size=BYTES     Reduce disk usage below specified size
journalctl --vacuum-files=INT      Leave only the specified number of journal files
journalctl --vacuum-time=TIME      Remove journal files older than specified time
```



