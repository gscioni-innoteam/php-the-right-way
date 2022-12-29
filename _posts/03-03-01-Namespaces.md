---
isChild: true
title:   Namespace
anchor:  namespaces
---

## Namespace {#namespaces_title}

Come menzionato sopra, la comunità PHP ha molti sviluppatori che creano molto
codice. Questo significa che il codice PHP di una libreria potrebbe usare lo
stesso nome di un'altra per una classe. Quando entrambe le librerie vengono
usate nello stesso namespace, potrebbero collidere e causare problemi.

I _namespace_ risolvono questo problema. Come descritto nel manuale di PHP, si
può pensare ai namespace come directory del sistema operativo che dividono i
file; due file con lo stesso nome possono esistere in directory separate. Allo
stesso modo, due classi PHP con lo stesso nome possono esistere in namespace PHP
separati. È così semplice.

È importante inserire il codice in un namespace in modo che possa essere usato
da altri sviluppatori senza paura che esso collida con altre librerie.

Nel dicembre 2013 il PHP-FIG ha creato un nuovo standard per l'autoloading:
[PSR-4][psr4], che un giorno probabilmente rimpiazzerà PSR-0. Attualmente
entrambi sono ancora utilizzabili, perché PSR-4 richiede PHP 5.3 e molti
progetti che supportano solo PHP 5.2 attualmente implementano PSR-0. Se userai
un autoloader standard per una nuova applicazione o pacchetto allora vorrai
quasi sicuramente dare un'occhiata a PSR-4.

Un modo raccomandato di usare i namespace è delineato nel [PSR-4], che mira a
fornire una convenzione standard per la i nomi di file, classi e namespace, in
modo da consentire la scrittura di codice plug-and-play.

Nell'ottobre 2014 il PHP-FIG ha deprecato il precedente standard di autoloading,
[PSR-0][psr0], che è stato sostituito con [PSR-4][psr4]. Attualmente entrambi
sono ancora utilizzabili, giacché PSR-4 richiede PHP 5.3 e molti progetti solo
per PHP 5.2 implementano attualmente PSR-0. Se hai intenzione di usare uno
standard di autoloading per una nuova applicazione o pacchetto, probabilmente
PSR-4 è la scelta giusta.

* [Leggi sui namespace][namespaces]
* [Leggi il PSR-0][psr0]
* [Leggi il PSR-4][psr4]

[namespaces]: http://php.net/language.namespaces
[psr0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[psr4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md