---
title:   Server virtuali o dedicati
isChild: true
anchor:  virtual_or_dedicated_servers
---

## Server virtuali o dedicati {#virtual_or_dedicated_servers_title}

Se hai dimestichezza con l'amministrazione di un sistema o vuoi impararla, i
server virtuali o dedicati ti danno totale controllo sull'ambiente di produzione
della tua applicazione.

### nginx e PHP-FPM

PHP, tramite il FastCGI Process Manager (FPM) integrato, si integra molto bene
con [nginx], che è un server leggero dalle alte prestazioni. Usa meno memoria di
Apache e gestisce meglio le richieste simultanee. Questo è particolarmente
importante su server virtuali che non hanno molta memoria.

* [Scopri nginx][nginx]
* [Scopri PHP-FPM][phpfpm]
* [Scopri come configurare nginx e PHP-FPM in modo sicuro][secure-nginx-phpfpm]

### Apache e PHP

PHP e Apache hanno fatto molta storia insieme. Apache è totalmente configurabile
e ha molti [moduli][apache-modules] che estendono le sue funzionalità. È una
scelta comune per i server condivisi e una configurazione facile per i framework
PHP e le applicazioni open source come WordPress. Sfortunatamente, Apache usa
più risorse di nginx di default e non riesce a gestire così tanti visitatori
contemporeanei.

Ci sono molte configurazioni diverse per eseguire Apache con PHP. La più comune
e la più semplice da configurare è il [prefork MPM] con mod_php5. Nonostante
questa non sia la più efficiente in termini di consumo di memoria, è la più
semplice da configurare e usare. Questa è probabilmente la scelta migliore se
non vuoi addentrarti troppo in profondità nell'amministrazione dei server. Nota
che per usare mod_php5 DEVI usare il prefork MPM.

In alternativa, se vuoi ottenere migliori performance e stabilità con Apache
puoi usare lo stesso sistema FPM di nginx ed eseguire l'[MPM worker] o
l'[MPM event] con mod_fastcgi o mod_fcgid. Questa configurazione sarà molto più
efficiente nell'utilizzo di memoria e molto più veloce, ma è più complicata da
installare.

Se stai utilizzando Apache 2.4 o successivo, puoi utilizzare [mod_proxy_fcgi] per ottenere ottime prestazioni facili da configurare.

* [Leggi di più su Apache][apache]
* [Leggi di più su Multi-Processing Modules][apache-MPM]
* [Leggi di più su mod_fastcgi][mod_fastcgi]
* [Leggi di più su mod_fcgid][mod_fcgid]
* [Leggi di più su mod_proxy_fcgi][mod_proxy_fcgi]
* [Leggi di più su impostare Apache e PHP-FPM con mod_proxy_fcgi][tutorial-mod_proxy_fcgi]


[nginx]: https://nginx.org/
[phpfpm]: https://secure.php.net/install.fpm
[secure-nginx-phpfpm]: https://nealpoole.com/blog/2011/04/setting-up-php-fastcgi-and-nginx-dont-trust-the-tutorials-check-your-configuration/
[apache-modules]: https://httpd.apache.org/docs/2.4/mod/
[prefork MPM]: https://httpd.apache.org/docs/2.4/mod/prefork.html
[worker MPM]: https://httpd.apache.org/docs/2.4/mod/worker.html
[event MPM]: https://httpd.apache.org/docs/2.4/mod/event.html
[apache]: https://httpd.apache.org/
[apache-MPM]: https://httpd.apache.org/docs/2.4/mod/mpm_common.html
[mod_fastcgi]: https://blogs.oracle.com/opal/post/php-fpm-fastcgi-process-manager-with-apache-2
[mod_fcgid]: https://httpd.apache.org/mod_fcgid/
[mod_proxy_fcgi]: https://httpd.apache.org/docs/current/mod/mod_proxy_fcgi.html
[tutorial-mod_proxy_fcgi]: https://serversforhackers.com/video/apache-and-php-fpm
