---
isChild: true
title:   Opcode Cache
anchor:  opcode_cache
---

## Opcode Cache {#opcode_cache_title}

W## Opcode cache {#opcode_cache_title}

Quando un nuovo PHP file viene eseguito, è prima compilato in opcode; solo dopo
l'opcode viene eseguito. Se un file PHP non viene modificato, l'opcode rimane lo
stesso. Questo significa che il processo di compilazione è uno spreco di risorse
computazionali.

È qui che le cache dell'opcode entrano in gioco. Evitano compilazioni inutili
salvando l'opcode in memoria e riusandolo nelle chiamate successive. Impostare
una cache dell'opcode è una questione di minuti, e la tua applicazione sarà
molto più veloce. Non c'è alcuna ragione per non usarla.

A partire da PHP 5.5, c'è una cache dell'opcode integrata chiamata
[OPcache][opcache-book]. È anche disponibile per versioni precedenti.

Altre risorse sulle cache dell'opcode:

* [OPcache][opcache-book] (integrata a partire da PHP 5.5)
* [APC] (PHP 5.4 e precedenti)
* [XCache]
* [Zend Optimizer+] (parte del pacchetto Zend Server)
* [WinCache] (estensione per MS Windows Server)
* [lista di acceleratori PHP su Wikipedia][PHP_accelerators]
* [PHP Preloading] - PHP >= 7.4


[opcache-book]: https://secure.php.net/book.opcache
[APC]: https://www.php.net/book.apcu
[XCache]: https://github.com/lighttpd/xcache
[Zend Optimizer+]: https://github.com/zendtech/ZendOptimizerPlus
[WinCache]: https://www.iis.net/downloads/microsoft/wincache-extension
[PHP_accelerators]: https://wikipedia.org/wiki/List_of_PHP_accelerators
[PHP Preloading]: https://www.php.net/opcache.preloading
