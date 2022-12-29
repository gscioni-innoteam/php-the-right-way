---
isChild: true
title:   Caching degli oggetti
anchor:  object_caching
---

## Caching degli oggetti {#object_caching_title}

Alcune volte può essere utile mettere in cache oggetti singoli nel tuo codice,
come, per esempio, dati che sono lenti da ottenere o chiamate al database il cui
risultato cambia difficilmente. Puoi usare software di caching degli oggetti per
mantenere questi pezzi di dati in memoria per un successivo accesso estremamente
veloce. Se salvi questi elementi in un data store dopo averli recuperati, e poi
li prendi direttamente dalla cache per le richieste successive, puoi ottenere un
notevole miglioramento delle performance e riducendo il carico del tuo database.

Molte soluzioni famose di caching del bytecode ti permettono anche di mettere in
cache dati personalizzati, dunque ci sono ancora più ragioni per trarne
vantaggio. APCu, XCache e WinCache forniscono tutti API per salvare dati dal tuo
codice PHP nella loro memoria cache.

I sistemi di caching degli oggetti più comunemente usati sono APCu e memcached.
APCu è una scelta eccellente per il caching degli oggetti. Include una semplice
API per aggiungere i tuoi dati alla sua memoria cache ed è molto semplice da
configurare e usare. L'unica vera limitazione di APCu è che è legato al Web
server su cui è installato. Memcached, invece, è installato come un servizio
separato e può essere letto dalla rete, il che significa che puoi memorizzare
oggetti in un data store super-veloce in una posizione centrale e molti sistemi
diversi possono accedervi.

Nota che quando esegui PHP come un'applicazione (Fast-)CGI nel tuo Web server,
ogni processo PHP avrà la sua cache (i dati di APCu non sono condivisi tra i
processi). In questi casi, potresti voler usare memcached, che non è legato ai
processi PHP.

In una configurazione di rete APCu sarà generalmente più performante di
memcached in termini di velocità di accesso, ma memcached potrà scalare meglio e
più velocemente. Se non pensi di eseguire la tua applicazioni su server
multipli, o non ti servono le funzionalità aggiuntive che memcached offre,
allora APCu è probabilmente la scelta migliore per il caching degli oggetti.

Esempio di utilizzo con APCu:

{% highlight php %}
<?php
// controlla se ci sono dati salvati come 'expensive_data' in cache
$data = apc_fetch('expensive_data');
if ($data === false) {
    // dati non in cache; salva il risultato per uso successivo
    apc_add('expensive_data', $data = get_expensive_data());
}

print_r($data);
{% endhighlight %}

Prima di PHP 5.5, APC fornisce sia una cache degli oggetti che una cache del
bytecode. APCu è un progetto per portare la cache degli oggetti di APC a PHP 5.5
e successivi, dato che PHP Ora ha una cache del bytecode integrata (OPCache).

### Impara a usare i sistemi di caching degli oggetti più famosi:

* [APCu](https://github.com/krakjoe/apcu)
* [Funzioni APC](http://php.net/ref.apc)
* [Memcached](http://memcached.org/)
* [Redis](http://redis.io/)
* [API XCache](http://xcache.lighttpd.net/wiki/XcacheApi)
* [Funzioni WinCache](http://php.net/ref.wincache)
