---
isChild: true
title:   Segnalazione degli errori
anchor:  error_reporting
---

## Segnalazione degli errori {#error_reporting_title}

Il logging degli errori può essere utile per trovare i punti problematici della
tua applicazione, ma può anche esporre informazioni riguardo la sua struttura al
mondo esterno. Per proteggere efficacemente la tua applicazione da problemi che
potrebbero essere causati dalla visualizzazione di questi messaggi, devi
configurare il tuo server diversamente negli ambienti di sviluppo e produzione.

### Sviluppo

Per mostrare ogni possibile errore durate lo <strong>sviluppo</strong>,
configura le seguenti opzioni nel tuo `php.ini`:

{% highlight ini %}
display_errors = On
display_startup_errors = On
error_reporting = -1
log_errors = On
{% endhighlight %}

> Il valore `-1` visualizzerà ogni possibile errore, anche quando nuovi livelli
> verranno aggiunti in versioni future di PHP. Anche la costante `E_ALL` si
> comporta in questo modo a partire da PHP 5.4. -
> [php.net](http://php.net/function.error-reporting)

Il livello di errore `E_STRICT` è stato introdotto nella versione 5.3.0 e non è
parte di `E_ALL`, tuttavia lo è diventato nella 5.4.0. Questo significa che se
vuoi segnalare ogni possibile errore nella 5.3 devi usare `-1` o
`E_ALL | E_STRICT`.

**Segnalare ogni possibile errore nelle diverse versioni di PHP**

* &lt; 5.3 `-1` o `E_ALL`
* &nbsp; 5.3 `-1` o `E_ALL | E_STRICT`
* &gt; 5.3 `-1` o `E_ALL`

### Produzione

Per nascondere gli errori in <strong>produzione</strong> configura così il tuo
`php.ini`:

{% highlight ini %}
display_errors = Off
display_startup_errors = Off
error_reporting = E_ALL
log_errors = On
{% endhighlight %}

Con queste impostazioni in produzione, gli errori saranno comunque loggati nei
log del web server, ma non saranno visualizzati all'utente. Per maggiori
informazioni su queste impostazioni, vedi il manuale di PHP:

* [error_reporting](http://php.net/errorfunc.configuration#ini.error-reporting)
* [display_errors](http://php.net/errorfunc.configuration#ini.display-errors)
* [display_startup_errors](http://php.net/errorfunc.configuration#ini.display-startup-errors)
* [log_errors](http://php.net/errorfunc.configuration#ini.log-errors)
