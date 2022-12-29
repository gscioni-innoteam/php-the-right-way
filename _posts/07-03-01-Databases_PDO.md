---
isChild: true
title:   Estensione PDO
anchor:  pdo_extension
---

## Estensione PDO {#pdo_extension_title}

[PDO] è una libreria di astrazione della connessione al database (integrata in
PHP a partire dalla versione 5.1.0) che fornisce un'interfaccia comune per la
comunicazione con molti database differenti. Per esempio, puoi usare
praticamente lo stesso codice per interfacciarti con MySQL o SQLite:

{% highlight php %}
<?php
// PDO + MySQL
$pdo = new PDO('mysql:host=example.com;dbname=database', 'utente', 'password');
$statement = $pdo->query("SELECT un_campo FROM una_tabella");
$row = $statement->fetch(PDO::FETCH_ASSOC);
echo htmlentities($row['un_campo']);

// PDO + SQLite
$pdo = new PDO('sqlite:/path/db/foo.sqlite');
$statement = $pdo->query("SELECT un_campo FROM una_tabella");
$row = $statement->fetch(PDO::FETCH_ASSOC);
echo htmlentities($row['un_campo']);
{% endhighlight %}

PDO non tradurrà le tue query SQL e non emulerà le funzionalità mancanti; viene
usato solo per connettersi a diversi tipi di database con la stessa API.

Ancora più importante, PDO ti permette di inserire in maniera sicura dell'input
esterno (es. ID) nelle tue query SQL senza preoccuparti di attacchi di tipo SQL
injection. È possibile farlo grazie agli statement e ai bound parameters di PDO.

Supponiamo che uno script PHP riceva un ID numerico come parametro della query.
Questo ID dovrebbe essere usato per recuperare il record di un utente dal
database. Ecco il modo **sbagliato** di farlo:

{% highlight php %}
<?php
$pdo = new PDO('sqlite:/path/db/utenti.db');
$pdo->query("SELECT nome FROM utenti WHERE id = " . $_GET['id']); // <-- NO!
{% endhighlight %}

Questo codice è terribile. Stai inserendo direttamente un parametro della query
in una query SQL. Questa vulnerabilità sarà sfruttata in un batter d'occhio,
usando una pratica chiamata [SQL Injection]. Immagina solo cosa accadrebbe se un
hacker passasse un parametro `id` fittizio chiamadno un URL come
`http://domain.com/?id=1%3BDELETE+FROM+users`. In questo modo la variabile
`$_GET['id']` sarebbe impostata a `1;DELETE FROM users`, che cancellerebbe tutti
i tuoi utenti! Per evitarlo, dovresti sempre pulire l'input ID usando i bound
parameters di PDO.

{% highlight php %}
<?php
$pdo = new PDO('sqlite:/path/db/utenti.db');
$stmt = $pdo->prepare('SELECT nome FROM utenti WHERE id = :id');
$id = filter_input(INPUT_GET, 'id', FILTER_SANITIZE_NUMBER_INT); // <-- filtra prima i tuoi dati (vedi [Filtraggio dei dati](#data_filtering)), in particolare per operazioni INSERT, UPDATE etc.
$stmt->bindParam(':id', $id, PDO::PARAM_INT); // <-- Pulito automaticamente da PDO
$stmt->execute();
{% endhighlight %}

Questo è codice corretto. Usa un bound parameter in uno statement PDO. In questo
modo viene effettuato l'escape dell'input esterno ID prima che questo venga
introdotto nel database, evitando potenziali attacchi di tipo SQL injection.

Per le operazioni di scrittura, come INSERT e UPDATE, è particolarmente
importante [filtrare prima i dati](#data_filtering) e poi pulirli (rimozione di
tag HTML, JavaScript etc.). PDO li pulirà solo rispetto all'SQL, non alla tua
applicazione.

* [Impara a usare PDO]

Dovresti anche tenere in considerazione che le connessioni al database usano
risorse, e non sarebbe strano che tali risorse si esaurissero se le connessioni
non vengono chiuse implicitamente (ma è più comune in altri linguaggi). Usando
PDO, puoi chiudere implicitamente la connessioni distruggendo l'oggetto, ovvero
assicurandoti che tutti i rimanenti riferimenti a esso vengano cancellati
(impostati a NULL). Se non lo fai esplicitamente, PHP chiuderà automaticamente
la connessione quando il tuo script termina l'esecuzione, a meno che,
ovviamente, tu non stia usando delle connessioni persistenti.

* [Impara a usare le connessioni PDO]

[pdo]: http://php.net/pdo
[SQL Injection]: http://wiki.hashphp.org/Validation
[Impara a usare PDO]: http://php.net/book.pdo
[Impara a usare le connessioni PDO]: http://php.net/pdo.connections
