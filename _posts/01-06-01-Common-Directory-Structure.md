---
title:   Struttura comune della directory
isChild: true
anchor:  common_directory_structure
---

## Struttura comune della directory {#common_directory_structure_title}

Una domanda comune tra coloro che iniziano a scrivere programmi per il web è "dove metto le mie cose?".
Nel corso degli anni, questa risposta è stata costantemente "dove si trova `DocumentRoot`". 
Sebbene questa risposta non sia completa, è un ottimo punto di partenza.

Per motivi di sicurezza, i file di configurazione non dovrebbero essere accessibili ai visitatori di un sito; 
pertanto, gli script pubblici vengono conservati in una directory pubblica e le configurazioni ei dati privati ​​vengono conservati all'esterno di tale directory.

Per ogni team, CMS o framework in cui si lavora, ciascuna di queste entità utilizza una struttura di directory standard. Tuttavia, se si sta iniziando un progetto da soli, sapere quale struttura di filesystem utilizzare può essere scoraggiante.

[Paul M. Jones] ha condotto delle fantastiche ricerche sulle pratiche comuni di decine di migliaia di progetti github nel regno di PHP. Ha compilato una struttura standard di file e directory, lo [scheletro del pacchetto PHP standard], basato su questa ricerca. In questa struttura di directory, `DocumentRoot` dovrebbe puntare a `public/`, gli unit test dovrebbero essere nella directory `tests/` e le librerie di terze parti, come installate da [composer], dovrebbero appartenere alla directory `vendor/`. Per altri file e directory, il rispetto dello [scheletro del pacchetto PHP standard] avrà più senso per i contributori di un progetto.

[Paul M. Jones]: http://paul-m-jones.com/
[Standard PHP Package Skeleton]: https://github.com/php-pds/skeleton
[Composer]: /#composer_and_packagist
