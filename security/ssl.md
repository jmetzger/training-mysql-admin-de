# SSL - MySQL 

## Teil 1: 1-Weg-Sicherheit (nur auf Server validiert) 

```
Bei MySQL 8 werden Zertfikate in der Regel bereits erstellt. 
Ob ssl funktioniert können wir mit 

mysql>show variables like '%HAVE_SSL%';

# Es funktioniert bereits, allerdings mit den automatisch
# erstellten Zertifikaten

```

### Herausfinden, ob SSL verwendet wird 

```
# auf client auf dem mysql-server
mysql
mysql>status
SSL:			Not in use
Connection:		Localhost via UNIX socket
```

### Bitte das nicht verwenden, weil man damit nicht den Common Name setzen kann

```
sudo mysql_ssl_rsa_setup --uid=mysql
```

### CA (Certificate Authority) und Server-Key erstellen 

```
# On Server - create ca and certificates 
mkdir -p /etc/mysql/ssl
cd /etc/mysql/ssl

# create ca.  
openssl genrsa 4096 > ca-key.pem

# create ca-certificate 
# Common Name: MariaDB CA 
openssl req -new -x509 -nodes -days 365 -key ca-key.pem -out ca-cert.pem

# create server-cert 
# Common Name: MariaDB Server
# Password: --- leave empty ----
openssl req -newkey rsa:2048 -days 365 -nodes -keyout server-key.pem -out server-req.pem

# Next process the rsa - key 
openssl rsa -in server-key.pem -out server-key.pem

# Now sign the key 
openssl x509 -req -in server-req.pem -days 365 -CA ca-cert.pem -CAkey ca-key.pem -set_serial 01 -out server-cert.pem

```

### Zertifikate validieren  

```
openssl verify -CAfile ca-cert.pem server-cert.pem
```

### Configure Server 
```
# create file 
# /etc/mysql/mysql.cnf.d/mysqld.cnf 
[mysqld]
ssl-ca=/etc/mysql/ssl/ca-cert.pem
ssl-cert=/etc/mysql/ssl/server-cert.pem
ssl-key=/etc/mysql/ssl/server-key.pem
## Set up TLS version here. For example TLS version 1.2 and 1.3 ##
tls_version = TLSv1.2,TLSv1.3

# Set ownership 
chown -vR mysql:mysql /etc/mysql/ssl/

```

### Restart and check for errors 
```
systemctl restart mysql
journalctl -u mysql

```

### Externen user auf server einrichten

```
# Einloggen in mysql-client als root

mysql> create user ext@'%' identified by 'P@ssw0rd';
Query OK, 0 rows affected (0.01 sec)

mysql> grant all on sakila.* to ext@'%';
Query OK, 0 rows affected (0.00 sec)

mysql> alter user ext@'%' REQUIRE SSL;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user where user = 'ext' \G

ssl_type: ANY
ssl_cipher: 0x
x509_issuer: 0x
x509_subject: 0x
```

### Test on Client 

```
mysql -uext -p -h<ip-des-servers>
mysql>status
```


## 





## Ref 

  * https://www.digitalocean.com/community/tutorials/how-to-configure-ssl-tls-for-mysql-on-ubuntu-18-04
  

