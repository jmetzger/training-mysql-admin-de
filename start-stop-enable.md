# Starten,stopppen und aktivieren von MySQL

## start/stop/status 

```
# als root - Benutzer
systemctl status mysql
systemctl stop mysql 
systemctl start mysql 
```

## Aktivieren (enable) 

```
# Automatischen Starten nach dem Booten (enable) 
systemctl enable mysql 
```
