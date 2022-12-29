---
layout: page
title:  Programmazione funzionale in PHP
sitemap: true
---

# Programmazione funzionale in PHP

PHP supporta funzioni di prima classe, il che significa che una funzione può
essere assegnata a una variabile. Sia alle funzioni definite dall'utente che
alle funzioni native ci si può riferire tramite una variabile in modo da
invocarle dinamicamente. Le funzioni possono essere passate come argomenti di
altre funzioni e una funzione può restituire altre funzioni (una caratteristica
chiamata funzioni di ordine superiore).

La ricorsione, una caratteristicache permette a una funzione di chiamare se
stessa, è supportata dal linguaggio, ma la maggior parte del codice PHP si basa
sull'iterazione.

Le funzioni anonime (che supportano le chiusure) sono state presenti da PHP 5.3
(2009).

PHP 5.4 ha aggiunto la possibilità di legare le chiusure allo scope di un
oggetto e ha migliorato il supporto per i callback, in modo che possano essere
usati quasi sempre in modo intercambiabile con le funzioni anonime.

L'uso più comune delle funzioni di ordine superiore si ha nell'implementazione
di un pattern strategico. La funzione nativa `array_filter` richiede un array di
input (dati) e una funzione (una strategia o un callback) usata come filtro per
ogni elemento dell'array.

{% highlight php %}
<?php
$input = array(1, 2, 3, 4, 5, 6);

// Crea una nuova funzione anonima e la assegna a una variabile
$filter_even = function($item) {
    return ($item % 2) == 0;
};

// La funzione nativa array_filter accetta i dati e la funzione
$output = array_filter($input, $filter_even);

// La funzione non dev'essere per forza assegnata a una variabile. Anche questo è valido:
$output = array_filter($input, function($item) {
    return ($item % 2) == 0;
});

print_r($output);
{% endhighlight %}

Una chiusura è una funzione anonima che può accedere a variabili importate dallo
scope esterno senza usare variabili globali. In teoria, una chiusura è una
funzione con alcuni argomenti chiusi (fissi) dall'ambiente quando è definita. Le
chiusure possono aggirare le restrizioni imposte dallo scope delle variabili in
un modo pulito.

Nel prossimo esempio useremo le chiusure per definire una funzione che
restituisce un singolo filtro per `array_filter`, preso da una famiglia di
funzioni filtro.

{% highlight php %}
<?php
/**
 * Crea una funzione anonima che accetta gli elementi > $min
 *
 * Restituisce un singolo filtro da una famiglia di filtri "maggiori di n"
 */
function criteria_greater_than($min)
{
    return function($item) use ($min) {
        return $item > $min;
    };
}

$input = array(1, 2, 3, 4, 5, 6);

// Usa array_filter sull'input con il filtro selezionato
$output = array_filter($input, criteria_greater_than(3));

print_r($output); // items > 3
{% endhighlight %}

Ogni funzione filtro della famiglia accetta solo elementi maggiori di un valore
minimo. Il filtro restituito da `criteria_greater_than` è una chiusura con
l'argomento `$min` chiuso da un valore nello scope (passato come argomento
quando viene chiamata `criteria_greater_than`).

Il binding statico è usato di default per importare la variabile `$min` nella
funzione creata. Per le vere chiusure con binding dinamico bisognerebbe usare un
riferimento nell'importazione. Immagina una libreria per il templating o la
validazione dell'input, dove la chiusura è definita per catturare variabili
nello scope e accedere a essere dopo quando la funzione anonima viene eseguita.

* [Leggi sulle Funzioni anonime][anonymous-functions]
* [Più dettagli nell'RFC delle chiusure][closures-rfc]
* [Impara a chiamare le funzioni dinamicamente con `call_user_func_array`][call-user-func-array]

[anonymous-functions]: http://www.php.net/manual/it/functions.anonymous.php
[closures-rfc]: https://wiki.php.net/rfc/closures
[call-user-func-array]: http://php.net/function.call-user-func-array
