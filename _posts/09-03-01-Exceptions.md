---
isChild: true
title:   Eccezioni
anchor:  exceptions
---

## Eccezioni {#exceptions_title}

Le eccezioni sono una parte standard della maggior parti dei linguaggi di
programmazione famosi, ma sono spesso ignorate dai programmatori PHP. Linguaggi
come Ruby fanno un uso massiccio delle eccezioni, quindi ogni volta che qualcosa
va storto come una richiesta HTTP che fallisce, o una query al database che non
funziona, o anche se un'immagine non può essere trovata, Ruby (o le gem usate)
lancerà un'eccezione a schermo in modo che tu sappia subito che c'è un errore.

PHP stesso è piuttosto negligente in questo senso: una chiamata a
`file_get_contents()` su un file inesistente non farà che restituire `FALSE` e
un warning. Molti vecchi framework PHP come CodeIgniter si limiteranno a
restituire false, loggare un messaggio nei loro log proprietari e a volte usare
un metodo come `$this->upload->get_error()` per vedere cos'è andato storto. Il
problema qui è che devi cercare l'errore e controllare la documentazione per
vedere come recuperare l'errore di quella classe, invece di averlo subito ovvio.

Un altro problema è quando le classi lanciano un errore a schermo
automaticamente e terminano il processo. Facendo questo, impedisci a un altro
sviluppatore di gestire dinamicamente l'errore. Le eccezioni dovrebbero essere
lanciate per mettere a conoscenza lo sviluppatore di un errore; starà a loro
scegliere come gestirlo. Esempio:

{% highlight php %}
<?php
$email = new Fuel\Email;
$email->subject('Mio oggetto');
$email->body('Come stai?');
$email->to('guy@example.com', 'Un tizio');

try
{
    $email->send();
}
catch(Fuel\Email\ValidationFailedException $e)
{
    // La validazione è fallita
}
catch(Fuel\Email\SendingFailedException $e)
{
    // Il driver non è riuscito a mandare l'email
}
finally
{
    // Eseguito anche se l'eccezione non è stata lanciata, e prima che la normale esecuzione riprenda
}
{% endhighlight %}

### Eccezioni SPL

La classe generica `Exception` fornisce molte poche informazioni di debug per lo
sviluppatore; tuttavia, per rimediare a questo, è possibile creare una versione
specializzata di `Exception` estendola:

{% highlight php %}
<?php
class ValidationException extends Exception {}
{% endhighlight %}

Questo significa che puoi aggiungere più blocchi catch e gestire differenti
eccezioni in modo differente. Questo può portare alla creazione di
<em>molte</em> eccezioni personalizzate, alcune delle quali avrebbero potuto
essere evitate usando le eccezioni SPL fornite dall'[estensione SPL][splext].

Se per esempio usi il metodo magico `__call()` e un metodo non valido è
richiesto, invece di lanciare un'eccezione standard, troppo vaga, o creare
un'eccezione personalizzata solo per quello, potresti solo eseguire `throw new
BadFunctionCallException()`.

* [Leggi sulle eccezioni][exceptions]
* [Leggi sulle eccezioni SPL][splexe]
* [Nidificare le eccezioni in PHP][nesting-exceptions-in-php]
* [Migliori pratiche per le eccezioni in PHP 5.3][exception-best-practices53]

[splext]: /#standard_php_library
[exceptions]: http://php.net/language.exceptions
[splexe]: http://php.net/spl.exceptions
[nesting-exceptions-in-php]: http://www.brandonsavage.net/exceptional-php-nesting-exceptions-in-php/
[exception-best-practices53]: http://ralphschindler.com/2010/09/15/exception-best-practices-in-php-5-3
