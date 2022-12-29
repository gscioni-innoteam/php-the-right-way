---
isChild: true
title:   Register globals
anchor:  register_globals
---

## Register globals {#register_globals_title}

**NOTA:** A partire da PHP 5.4.0 l'opzione `register_globals` è stata rimossa e
non può più essere usata. Questo è incluso solo come avvertimento per chiunque
stia aggiornando una vecchia applicazione.

Quando abilitata, l'opzione `register_globals` farà sì che molti tipi di
variabili (incluse quelle da `$_POST`, `$_GET` e `$_REQUEST`) siano disponibili
nello scope globale dell'applicazione. Questo può facilmente portare a problemi
di sicurezza, perché la tua applicazione non può stabilire con certezza da dove
arrivano i dati.

Per esempio: `$_GET['foo']` sarebbe disponibile tramite `$foo`, che può
sovrascrivere le variabili non dichiarate. Se stai usando PHP < 5.4.0,
__assicurati__ che `register_globals` sia __off__.

* [register_globals nel manuale PHP](http://php.net/security.globals)

