---
title:   Lavorare con UTF-8
isChild: true
anchor:  php_and_utf8
---

## Lavorare con UTF-8 {#php_and_utf8_title}

_Questa sezione è stata originariamente scritta da
[Alex Cabal](https://alexcabal.com)
su [PHP Best Practices](https://phpbestpractices.org/#utf-8) ed è stata usata
come base per i nostri consigli su UTF-8._

### Non c'è una soluzione unica. Sii attento, dettagliato e consistente.

Attualmente PHP non supporta Unicode al basso livello. Ci sono dei modi per
assicurarsi che le stringhe UTF-8 vengano processate correttamente, ma non è
semplice, ed è necessario scavare in quasi tutti i livelli dell'applicazione
web, dall'HTML all'SQL al PHP. Cercheremo di scrivere un sommario breve e
pratico.

### UTF-8 al livello PHP

Le operazioni di base con le stringhe, come il concatenamento di due stringhe e
l'assegnamento di stringhe a variabili, non richiedono nulla di speciale per il
supporto UTF-8. Tuttavia la maggior parte delle funzioni per le stringhe, come
`strpos()` e `strlen()`, richiedono particolare considerazione. Queste funzioni
hanno solitamente una controparte `mb_*`: per esempio, `mb_strpos()` e
`mb_strlen()`. Queste stringhe `mb_*` vengono fornite tramite l'[Estensione
Multibyte String], e sono specificatamente disegnate per operare su stringhe
Unicode.

Devi usare le funzioni `mb_*` ogni qualvolta operi su una stringa Unicode. Per
esempio, se usi `substr()` su una stringa UTF-8, c'è una buona possibilità che
il risultato includa dei caratteri incomprensibili. La funzione corretta da
usare sarebbe la controparte multibyte, `mb_substr()`.

La parte difficile è ricordarsi di usare le funzioni `mb_*` ogni volta. Se ti
dimentichi anche una sola volta, la tua stringa Unicode rischia di divenire
incomprensibile durante le operazioni successive.

Non tutte le funzioni per le stringhe hanno una controparte `mb_*`. Se non ce
n'è una per quello che vuoi fare, allora peggio per te.

Inoltre, dovresti usare la funzione `mb_internal_encoding()` all'inizio di ogni
script PHP che scrivi (o in cima al tuo script incluso globalmente), e la
funzione `mb_http_output()` subito dopo se il tuo script invia dati a un
browser. Definire esplicitamente la codifica delle tue stringhe in ogni script
ti salverà molti futuri mal di testa.

Infine, molte funzioni PHP che operano sulle stringhe hanno un parametro
opzionale che ti permette di specificare la codifica dei caratteri. Dovresti
sempre esplicitamente indicare UTF-8 quando ne hai la possibilità. Per esempio,
`htmlentities()` ha un'opzione per la codifica dei caratteri, e dovresti sempre
specificare UTF-8 se hai a che fare con certe stringhe. Tieni a mente che, a
partire da PHP 5.4.0, UTF-8 è la codifica predefinita per `htmlentities()` e
`htmlspecialchars()`.

Infine, se stai costruendo un'applicazione distribuita e non sei certo che
l'estensione `mbstring` sarà disponibile, considera l'idea di usare il pacchetto
Composer [patchwork/utf8]. Questo userà `mbstring` se è disponibile, e le
funzioni non UTF-8 se non lo è.

[Estensione Multibyte String]: http://php.net/book.mbstring
[patchwork/utf8]: https://packagist.org/packages/patchwork/utf8

### UTF-8 al livello database

Se il tuo script PHP accede a MySQL, è possibile che le tue stringhe vengano
salvate nel database come stringhe non UTF-8 anche se segui tutte le precauzioni
di cui sopra.

Per assicurarti che le tue stringhe vadano da PHP a MySQL come UTF-8, assicurati
che il set e la collation di caratteri del tuo database e delle tue tabelle
siano impostati a `utf8mb4`, e usa il set di caratteri `utf8mb4` nella stringa
di connessione PDO. Vedi l'esempio di codice sotto. Questo è _estremamente
importante_.

Tieni a mente che devi usare il set di caratteri `utf8mb4` per il supporto
completo a UTF-8, non il set di caratteri `utf8`! Vedi Ulteriori letture per
scoprire perché.

### UTF-8 al livello browser

Usa la funzione `mb_http_output()` per assicurarti che il tuo script PHP invii
stringhe UTF-8 al tuo browser.

Il browser avrà poi bisogno di sapere dalla risposta HTTP che questa pagina
dev'essere considerata come UTF-8. L'approccio storico è usare il [`<meta>` tag
charset](http://htmlpurifier.org/docs/enduser-utf8.html) nel tag `<head>` della
tua pagina. Questo approccio è perfettamente valido, ma impostare il charset
nell'header `Content-Type` è in realtà
[molto più veloce](https://developers.google.com/speed/docs/best-practices/rendering#SpecifyCharsetEarly).

{% highlight php %}
<?php
// Comunica a PHP che useremo stringhe UTF-8 fino alla fine dello script
mb_internal_encoding('UTF-8');

// Comunica a PHP che invieremo stringhe UTF-8 al browser
mb_http_output('UTF-8');

// La nostra stringa UTF-8 di test
$string = 'Êl síla erin lû e-govaned vîn.';

// Trasforma la stringa in qualche modo con una funzione multibyte
// Nota come, a scopo dimostrativo, tagliamo la stringa a un carattere non ASCII
$string = mb_substr($string, 0, 15);
<<<<<<< HEAD

// Connettiti al database per salvare la stringa trasformata
// Vedi l'esempio PDO in questo documento per maggiori informazioni
// Nota il comando `set names utf8mb4`!
$link = new \PDO(
    'mysql:host=tuo-hostname;dbname=tuo-db;charset=utf8mb4',
    'tuo-username',
    'tua-password',
    array(
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_PERSISTENT => false
    )
);

// Salva la stringa trasformata come UTF-8 nel nostro database
// Il tuo DB e le tabelle usano il set e la collation di caratteri utf8mb4, giusto?
$handle = $link->prepare('insert into FrasiElviche (Id, Body) values (?, ?)');
$handle->bindValue(1, 1, PDO::PARAM_INT);
$handle->bindValue(2, $string);
$handle->execute();

// Recupera la stringa che abbiamo appena memorizzato per provare che è stata salvata correttamente
$handle = $link->prepare('select * from FrasiElviche where Id = ?');
$handle->bindValue(1, 1, PDO::PARAM_INT);
$handle->execute();

// Salva il risultato in un oggetto che dopo invieremo nel nostro HTML
$result = $handle->fetchAll(\PDO::FETCH_OBJ);

header('Content-Type: text/html; charset=UTF-8');
?><!doctype html>
<html>
    <head>
      <meta charset="UTF-8">
      <title>Pagina di test UTF-8</title>
    </head>
    <body>
        <?php
        foreach($result as $row){
            print($row->Body);  // Questo dovrebbe inviare correttamente la nostra stringa UTF-8 trasformata al browser
        }
        ?>
    </body>
</html>
{% endhighlight %}

### Ulteriori letture

* [PHP Manual: String Operations](http://php.net/language.operators.string)
* [PHP Manual: String Functions](http://php.net/ref.strings)
  * [`strpos()`](http://php.net/function.strpos)
  * [`strlen()`](http://php.net/function.strlen)
  * [`substr()`](http://php.net/function.substr)
* [PHP Manual: Multibyte String Functions](http://php.net/ref.mbstring)
  * [`mb_strpos()`](http://php.net/function.mb-strpos)
  * [`mb_strlen()`](http://php.net/function.mb-strlen)
  * [`mb_substr()`](http://php.net/function.mb-substr)
  * [`mb_internal_encoding()`](http://php.net/function.mb-internal-encoding)
  * [`mb_http_output()`](http://php.net/function.mb-http-output)
  * [`htmlentities()`](http://php.net/function.htmlentities)
  * [`htmlspecialchars()`](http://php.net/function.htmlspecialchars)
* [PHP UTF-8 Cheatsheet](http://blog.loftdigital.com/blog/php-utf-8-cheatsheet)
* [Handling UTF-8 with PHP](http://www.phpwact.org/php/i18n/utf-8)
* [Stack Overflow: What factors make PHP Unicode-incompatible?](http://stackoverflow.com/questions/571694/what-factors-make-php-unicode-incompatible)
* [Stack Overflow: Best practices in PHP and MySQL with international strings](http://stackoverflow.com/questions/140728/best-practices-in-php-and-mysql-with-international-strings)
* [How to support full Unicode in MySQL databases](http://mathiasbynens.be/notes/mysql-utf8mb4)
* [Bringing Unicode to PHP with Portable UTF-8](http://www.sitepoint.com/bringing-unicode-to-php-with-portable-utf8/)
* [Stack Overflow: DOMDocument loadHTML does not encode UTF-8 correctly](http://stackoverflow.com/questions/8218230/php-domdocument-loadhtml-not-encoding-utf-8-correctly)
