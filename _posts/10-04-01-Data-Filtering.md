---
isChild: true
title:   Filtraggio dei dati
anchor:  data_filtering
---

## Filtraggio dei dati {#data_filtering_title}

Non fidarti mai (mai!) dell'input esterno introdotto nel tuo codice PHP.
Sanitizza e valida sempre l'input prima di usarlo nel tuo codice. Le funzioni
`filter_var` e `filter_input` possono sanitizzare il testo e validare certi
formati (es. indirizzi email).

L'input esterno può essere qualunque cosa: dati di form da `$_GET` e `$_POST`,
alcuni valori nella variabile superglobale `$_SERVER`, e il corpo della
richiesta HTTP recuperato tramite `fopen('php://input', 'r')`. Ricorda, l'input
esterno non è limitato ai form inviati dall'utente. Anche i file caricati e
scaricati, i valori di sessione, i dati nei cookie e i dati da servizi Web di
terze parti sono input esterno.

Anche se l'input esterno può essere salvato, combinato e letto successivamente,
è ancora input esterno. Ogni volta che processi, visualizzi, concateni o includi
dati nel tuo codice, chiediti se sono stati filtrati appropriatamente e se ci si
può fidare di essi.

I dati possono essere _filtrati_ diversamente a seconda del loro scopo. Per
esempio, quando input esterno non filtrato è passato nell'output HTML della
pagina, può esguire codice HTML e JavaScript sul tuo sito! Questo è conosciuto
come Cross-Site Scripting (XSS) e può essere un attacco molto pericoloso. Un
modo per evitare l'XSS è sanitizzare tutti i dati generati dall'utente prima di
visualizzarlo nella tua pagina, rimuovendo i tag HTML con la funzione
`strip_tags` o eseguendo l'escape di caratteri dal significato speciale nelle
loro rispettive entità HTML con le funzioni `htmlentities` o `htmlspecialchars`.

Un altro esempio è il passaggio di opzioni alla linea di comando. Questo può
essere molto pericoloso (ed è solitamente una cattiva idea), ma puoi usare la
funzione nativa `escapeshellarg` per sanitizzare gli argomenti del comando
eseguito.

Un ultimo esempio è l'accettazione di input esterno per determinare un file da
caricare dal filesystem. Questa vulnerabilità può essere sfruttata cambiando il
nome di file in un path di file. Devi rimuovere "/", "../", i [byte nulli][6] o
altri caratteri dal path in modo che non possa caricare file nascosti, non
pubblici o con informazioni sensibili.

* [Impara a filtare i dati][1]
* [Impara a usare `filter_var`][4]
* [Impara a usare `filter_input`][5]
* [Impara a gestire i byte nulli][6]

### Sanitizzazione

La sanitizzazione rimuove (o fa l'escape) i caratteri non permessi o pericolosi
dall'input esterno.

Per esempio, dovresti sanitizzare l'input esterno prima di includerlo in codice
HTML o inserirlo in una query SQL grezza. Quando usi i parametri di
[PDO](#database), esso sanitizzerà l'input per te.

A volte è richiesto di consnetire alcuni tag HTML sicuri nell'input quando
viene incluso nella pagina HTML. Questo è molto difficile da fare e molti lo
evitano usando una formattazione più ristretta come Markdown o BBCode, tuttavia
esistono alcune librerie come [HTML Purifier][html-purifier] per svolgere questo
compito.

[Vedi i filtri di sanitizzazione][2]

### Unserialization

È pericoloso deserializzare (`unserialize()`) dati da utenti o altre fonti non attendibili. Ciò può consentire agli utenti malintenzionati di creare un'istanza di oggetti (con proprietà definite dall'utente) i cui distruttori verranno eseguiti, **anche se gli oggetti stessi non vengono utilizzati**. Dovresti quindi evitare di deserializzare i dati non attendibili.

Se devi assolutamente deserializzare i dati da fonti non attendibili, usa l'opzione [`allowed_classes`][unserialize] di PHP 7 per limitare i tipi di oggetto che possono essere deserializzati.

### Validazione

La validazione serve per assicurarsi che l'input esterno contenga ciò che ti
aspetti. Per esempio, potresti voler validare un indirizzo email, un numero di
telefono o un'età quando processi una richiesta di registrazione.

[Vedi i filtri di validazione][3]

[1]: http://www.php.net/manual/it/book.filter.php
[2]: http://www.php.net/manual/it/filter.filters.sanitize.php
[3]: http://www.php.net/manual/it/filter.filters.validate.php
[4]: http://php.net/manual/it/function.filter-var.php
[5]: http://www.php.net/manual/it/function.filter-input.php
[6]: http://php.net/manual/it/security.filesystem.nullbytes.php
[html-purifier]: http://htmlpurifier.org/
[unserialize]: https://secure.php.net/manual/function.unserialize.php
