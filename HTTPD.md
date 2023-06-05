> Per installare il servizio di HTTPD eseguire il comando: 
> - sudo yum -y install httpd

> Avviare/riavviare il servizio con:
> - systemctl start httpd
> - systemctl restart httpd
> - systemctl eneble httpd (per avviarlo al boot della macchina)

> Controllare lo stato con:
> - systemctl status httpd

---
I file .html o .php con all’interno contenuti statici/dinamici vanno inseriti all’interno di /var/www/html
Se si vuole creare una propria cartella come ad esempio /exam/esercizio4 bisogna farlo in questo modo: /var/www/html/exam/esercizio4

La document root può essere modificata in /etc/httpd/conf/httpd.conf
Oppure creare un file all'interno di /etc/httpd/conf.d/esercizio.conf con il seguente codice:

```
VirtualHost *:8080>
    DocumentRoot /var/www/html/exam/exercise6/
</VirtualHost>

<Directory /var/www/html/exam/exercise6/>
    Require all granted
</Directory>
```

All’interno del file httpd.conf modificare il path della document root nel nuovo path appena descritto prima.

<mark style="background: #FFB86CA6;">NB: è possibile che venga letto il file di configurazione in /etc/httpd/conf.d/00-default-vhost.conf<mark>

Riavviare il servizio httpd e controllare eventuali errori con lo stato.
Per verificare che funzioni: 
- sul browser del sistema digitare: localhost:8080/index.html o localhost:8080/index.php
- all’interno del terminale digitare: curl -k localhost:80/index.html | localhost:80/index.php (oppure l’indirizzo della macchina).
---
> Per permettere connessioni dall'esterno in maniera permanente:
> - firewall-cmd --add-port=8080/tcp --permanent
> - firewall-cmd --reload
---
Per l'installazione del modulo php eseguire il seguente comando:
> - yum install -y httpd mod_php