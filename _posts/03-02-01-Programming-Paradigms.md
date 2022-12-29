---
isChild: true
title:   Paradigmi di programmazione
anchor:  programming_paradigms
---

## Paradigmi di programmazione {#programming_paradigms_title}

PHP è un linguaggio flessibile e dinamico che supporta diverse tecniche di
programmazione. Si è evoluto moltissimo negli ultimi anni, in particolare
aggiungendo un solido modello a oggetti in PHP 5.0 (2004), le funzioni anonime e
i namespace in PHP 5.3 (2009) e i trait in PHP 5.4 (2012).

### Programmazione orientata agli oggetti

PHP ha un set molto completo di funzionalità riguardanti la programmazione
orientata agli oggetti inclusi il supporto per le classi, le classi astratte, le
interfacce, l'ereditarietà, i costruttori, la clonazione, le eccezioni e altro.

* [Leggi riguardo la programmazione orientata agli oggetti in PHP][oop]
* [Leggi riguardo i trait][traits]

### Programmazione funzionale

PHP supporta le funzioni di prima classe, il che significa che una funzione può
essere assegnata a una variabile. Ci si può riferire tramite variabili sia alle
funzioni definite dall'utente che a quelle native, ed entrambe possono essere
invocate dinamicamente. Le funzioni possono essere passate ad altre funzioni
come argomenti (caratteristica chiamata _Funzioni di ordine superiore_) e le
funzioni possono restituire altre funzioni.

La ricorsione, una caratteristica che permette a una funzione di chiamare se
stessa, è supportata dal linguaggio, ma la maggior parte del codice PHP è
incentrato sull'iterazione.

Le nuove funzioni anonime (con supporto per le chiusure) esistono da PHP 5.3
(2009).

PHP 5.4 ha aggiunto la possibilità di legare le chiusure allo scope di un
oggetto e ha migliorato il supporto per i callback in modo che possano essere
usati quasi sempre in modo intercambiabile con le funzioni anonime.

* Continua a leggere sulla [Programmazione funzionale in PHP](/pages/Functional-Programming.html)
* [Leggi sulle funzioni anonime][anonymous-functions]
* [Leggi sulla classe Closure][closure-class]
* [Leggi l'RFC sulle chiusure][closures-rfc]
* [Leggi sui callback][callables]
* [Leggi sulla chiamata dinamica alle funzioni con `call_user_func_array`][call-user-func-array]

### Metaprogrammazione

PHP supporta varie forme di metaprogrammazione tramite meccanismi come la
Reflection API e i metodi magici. Ci sono diversi metodi magici disponibili come
`__get()`, `__set()`, `__clone()`, `__toString()`, `__invoke()` etc. che
permettono agli sviluppatori di modificare il funzionamento di una classe. Gli
sviluppatori Ruby dicono spesso che a PHP manca il metodo `method_missing`, ma è
disponibile sotto il nome di `__call()` e `__callStatic()`.

* [Leggi sui metodi magici][magic-methods]
* [Leggi sulla Reflection API][reflection]
* [Leggi sull'overloading][overloading]

[oop]: http://php.net/language.oop5
[traits]: http://php.net/language.oop5.traits
[anonymous-functions]: http://php.net/functions.anonymous
[closure-class]: http://php.net/class.closure
[closures-rfc]: https://wiki.php.net/rfc/closures
[callables]: http://php.net/language.types.callable
[call-user-func-array]: http://php.net/function.call-user-func-array
[magic-methods]: http://php.net/language.oop5.magic
[reflection]: http://php.net/intro.reflection
[overloading]: http://php.net/language.oop5.overloading