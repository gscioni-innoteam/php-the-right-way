---
isChild: true
title:   Concetto di base
anchor:  basic_concept
---

## Concetto di base {#basic_concept_title}

Possiamo dimostrare il concetto con un esempio semplice ma primitivo.

Abbiamo una classe `Database` che richiede un adattatore per comunicare col
database. Istanziamo un adattatore nel costruttore e creiamo una dipendenza
cablata a codice. Questo rende il testing difficoltoso e la classe `Database`
fortemente legata all'adattatore.

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct()
    {
        $this->adapter = new MySqlAdapter;
    }
}

class MysqlAdapter {}
{% endhighlight %}

Questo codice può essere rifattorizzato in modo che usi l'iniezione delle
dipendenze, e dunque renda la dipendenza più elastica.

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(MySqlAdapter $adapter)
    {
        $this->adapter = $adapter;
    }
}

class MysqlAdapter {}
{% endhighlight %}

Ora forniamo alla classe `Database` la sua dipendenza invece di crearla noi.
Potremmo anche creare un metodo che accetti un argomento della dipendenza e
impostarla così, o se la proprietà `$adapter` fosse `public` potremmo impostarla
direttamente.