# SSL - MySQL 

## Vorbereitung

```
Bei MySQL 8 werden Zertfikate in der Regel bereits erstellt. 
Ob ssl funktioniert können wir mit 

mysql>show variables like '%HAVE_SSL%';

Wollen wir diese Übung durchspielen sollten wir die Zertifikate löschen und
den Server neu starten 


systemctl restart mysql 
```

##


## Ref 

  * https://www.digitalocean.com/community/tutorials/how-to-configure-ssl-tls-for-mysql-on-ubuntu-18-04
  

