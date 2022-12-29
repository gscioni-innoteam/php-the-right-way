---
isChild: true
title:   Estensione MySQL
anchor:  mysql_extension
---

## Estensione MySQL {#mysql_extension_title}

The [mysql] extension for PHP is incredibly old and has been superseded by two other extensions:

- [mysqli]
- [pdo]

L'estensione [mysql] per PHP non è più attivamente sviluppata ed è
[ufficialmente deprecata a partire da PHP 5.5.0][mysql_deprecated], il che
significa che sarà rimossa nei prossimi rilasci. Se nelle tue applicazioni stai
usando delle funzioni che iniziano con `mysql_*` come `mysql_connect()` o
`mysql_query()`, sappi che nelle versioni successive semplicemente non saranno
più disponibili. Questo significa che sarai obbligato a riscrivere il tuo codice
a un certo punto, quindi l'opzione migliore è rimpiazzare mysql con [mysqli] o
[PDO] secondo la tua tabella di marcia; in questo modo dopo non dovrai farlo in
fretta e furia.

**Se stai iniziando ora, non usare assolutamente l'estensione [mysql]: usa
l'[estensione MySQLi][mysqli] o [PDO].**

* [PHP: Scegliere una API per MySQL][mysql_api]
* [Tutorial PDO per sviluppatori MySQL][pdo4mysql_devs]

[mysql]: http://php.net/mysql
[mysql_deprecated]: http://php.net/migration55.deprecated
[mysqli]: http://php.net/mysqli
[pdo]: http://php.net/pdo
[mysql_api]: http://php.net/mysqlinfo.api.choosing
[pdo4mysql_devs]: http://wiki.hashphp.org/PDO_Tutorial_for_MySQL_Developers
