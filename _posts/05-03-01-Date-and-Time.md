---
title:   Data e ora
isChild: true
anchor:  date_and_time
---

## Data e ora {#date_and_time_title}

PHP ha una classe chiamata DateTime che aiuta nella lettura, scrittura,
confronto e calcolo di date e ore. Non ci sono molte funzioni relative alle date
e all'ora in PHP a parte DateTime, ma essa fornisce una interfaccia orientata
agli oggetti ai casi di uso più comuni. Può gestire i fusi orari, ma ciò esula
da questa breve introduzione.

Per iniziare a lavorare con DateTime, converti le rappresentazioni grezze di
date e ora in un oggetto con il metodo `createFromFormat()` o esegui
`new \DateTime` per ottenere la data e l'ora attuali. Usa il metodo `format()`
per convertire DateTime in una stringa da visualizzare.

{% highlight php %}
<?php
$raw = '22. 11. 1968';
$start = DateTime::createFromFormat('d. m. Y', $raw);

echo 'Data inizio: ' . $start->format('Y-m-d') . "\n";
{% endhighlight %}

Il calcolo con DateTime è possibile grazie alla classe DateInterval. DateTime ha
dei metodi come `add()` e `sub()` che prendono un DateInterval per argomento.
Non scrivere codice che presume ci sia lo stesso numero di secondi in ogni
giorno, sia l'ora legale che i cambiamenti nel fuso orario altereranno i
risultati. Invece usa gli intervalli di data. Per calcolare la differenza tra
date usa il metodo `diff()`. Restituirà un nuovo DateInterval, che è molto
facile da visualizzare.

{% highlight php %}
<?php
// crea una copia di $start e aggiungi un mese e 6 giorni
$end = clone $start;
$end->add(new DateInterval('P1M6D'));

$diff = $end->diff($start);
echo 'Differenza: ' . $diff->format('%m mese, %d giorni (total: %a giorni)') . "\n";
// Differenza: 1 mese, 6 giorni (total: 37 giorni)
{% endhighlight %}

Sugli oggetti DateTime è possibile eseguire un confronto standard:

{% highlight php %}
<?php
if ($start < $end) {
    echo "L'inizio è prima della fine!\n";
}
{% endhighlight %}

Un ultimo esempio per dimostrare l'utilizzo della classe DatePeriod. Viene usata
per iterare su eventi ricorrenti. Può prendere due oggetti DateTime, inizio e
fine, e l'intervallo per il quale restituirà tutti gli eventi compresi.

{% highlight php %}
<?php
// mostra tutti i giovedì tra $start e $end
$periodInterval = \DateInterval::createFromDateString('first thursday');
$periodIterator = new \DatePeriod($start, $periodInterval, $end, \DatePeriod::EXCLUDE_START_DATE);

foreach ($periodIterator as $date) {
    // visualizza ogni data nel periodo
    echo $date->format('Y-m-d') . ' ';
}
{% endhighlight %}

* [Leggi su DateTime][datetime]
* [Leggi sulla formattazione delle date][dateformat] (opzioni di formato delle date accettate)

[datetime]: http://php.net/book.datetime
[dateformat]: http://php.net/function.date
