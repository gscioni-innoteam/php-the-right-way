---
layout: page
title:  Design Patterns
sitemap: true
---

# Design Pattern

Ci sono diversi modi di strutturare il codice e il progetto per la tua
applicazione Web, e puoi sforzarti quanto preferisci nell'architettarli.
Generalmente, però, è una buona idea seguire dei modelli comuni, in modo da
rendere il tuo codice più semplice da gestire per te e più facile da capire per
gli altri.

* [Modelli architetturali su Wikipedia](https://en.wikipedia.org/wiki/Architectural_pattern)
* [Design pattern del software su Wikipedia](https://en.wikipedia.org/wiki/Software_design_pattern)
* [Collezione di esempi d'implementazione](https://github.com/domnikl/DesignPatternsPHP)

## Factory

Uno dei design pattern più usati è il pattern factory. In questo pattern, una
classe crea l'oggetto che vuoi utilizzare. Considera il seguente esempio di
pattern factory:

{% highlight php %}
<?php
class Automobile
{
    private $vehicleMake;
    private $vehicleModel;

    public function __construct($make, $model)
    {
        $this->vehicleMake = $make;
        $this->vehicleModel = $model;
    }

    public function getMakeAndModel()
    {
        return $this->vehicleMake . ' ' . $this->vehicleModel;
    }
}

class AutomobileFactory
{
    public static function create($make, $model)
    {
        return new Automobile($make, $model);
    }
}

// facciamo creare l'oggetto Automobile alla factory
$veyron = AutomobileFactory::create('Bugatti', 'Veyron');

print_r($veyron->getMakeAndModel()); // visualizza "Bugatti Veyron"
{% endhighlight %}

Questo codice usa una _factory_ per creare l'oggetto Automobile. Ci sono due
potenziali benefici nello strutturare il codice in questo modo. Il primo è che,
se in futuro avrai bisogno di rinominare, modificare o sostituire la classe
Automobile, dovrai solamente modificare la _factory_ invece di ogni riga del tuo
codice che usa la classe Automobile. Il secondo possibile beneficio è che, se
creare un oggetto è un lavoro complicato, puoi fare tutto nella _factory_ invece
di ripeterlo ogni volta che vuoi creare una nuova istanza.

Usare il _Factory Method_ non è sempre necessario (né saggio). Il codice di
esempio mostrato qui è così semplice che aggiungere una _factory_ significa solo
complicare inutilmente l'applicazione. Tuttavia, se stai creando un progetto
grande o complesso, potresti volerti risparmiare molta fatica usando le
_factory_.

* [Factory Method su Wikipedia](https://it.wikipedia.org/wiki/Factory_method)

## Singleton

Durante la progettazione di un'applicazione Web, è spesso sensato
concettualmente e architetturalmente permettere l'accesso a una e una sola
istanza di una specifica classe. Il pattern _singleton_ ci permette di fare ciò.

{% highlight php %}
<?php
class Singleton
{
    /**
     * Returns the *Singleton* instance of this class.
     *
     * @staticvar Singleton $instance The *Singleton* instances of this class.
     *
     * @return Singleton The *Singleton* instance.
     */
    public static function getInstance()
    {
        static $instance = null;
        if (null === $instance) {
            $instance = new static();
        }

        return $instance;
    }

    /**
     * Protected constructor to prevent creating a new instance of the
     * *Singleton* via the `new` operator from outside of this class.
     */
    protected function __construct()
    {
    }

    /**
     * Private clone method to prevent cloning of the instance of the
     * *Singleton* instance.
     *
     * @return void
     */
    private function __clone()
    {
    }

    /**
     * Private unserialize method to prevent unserializing of the *Singleton*
     * instance.
     *
     * @return void
     */
    private function __wakeup()
    {
    }
}

class SingletonChild extends Singleton
{
}

$obj = Singleton::getInstance();
var_dump($obj === Singleton::getInstance());             // bool(true)

$anotherObj = SingletonChild::getInstance();
var_dump($anotherObj === Singleton::getInstance());      // bool(false)

var_dump($anotherObj === SingletonChild::getInstance()); // bool(true)
{% endhighlight %}

Il codice qui sopra implementa il pattern singleton usando una
[variabile *statica*](http://php.net/language.variables.scope#language.variables.scope.static)
e il metodo statico di creazione `getInstance()`.

* Il costruttore
[`__construct`](http://php.net/language.oop5.decon#object.construct) è
dichiarato protetto per impedire la creazione di una nuova istanza fuori dalla
classe tramite l'operatore `new`.

* Il metodo magico
[`__clone`](http://php.net/language.oop5.cloning#object.clone) è dichiarato
privato per impedire la clonazione di un'istanza della classe tramite
l'operatore [`clone`](http://php.net/language.oop5.cloning).

* Il metodo magico
[`__wakeup`](http://php.net/language.oop5.magic#object.wakeup) è dichiarato
privato per impedire la deserializzazione di un'istanza della classe tramite la
funzione globale [`unserialize()`](http://php.net/function.unserialize).

* Una nuova istanza è creata tramite [binding statico dinamico](http://php.net/language.oop5.late-static-bindings)
nel metodo statico di creazione `getInstance()` con la parole chiave `static`.
Questo permette di estendere la classe di esempio `Singleton`.

Dovresti fare molta attenzione quando usi il pattern singleton, poiché per sua
natura introduce uno stato globale nella tua applicazione, riducendo la
testabilità. Nella maggior parte dei casi, l'iniezione delle dipendenze può (e
dovrebbe) essere usata al posto di una classe singleton. Usando l'iniezione
delle dipendenze eviti di introdurre un legame superfluo nel design della tua
applicazione, perché l'oggetto che usa la risorsa condivisa o globale non deve
conoscere alcuna classe concretamente definita.

* [Singleton su Wikipedia](https://it.wikipedia.org/wiki/Singleton)

## Strategy

Con lo strategy pattern puoi incapsulare famiglie di algoritmi specifici,
facendo in modo che la classe responsabile per la creazione di un particolare
algoritmo non conosca la vera implementazione. Ci sono diverse variazioni dello
strategy pattern, la più semplice delle quali è presentata qui sotto.

Questo primo snippet di codice delinea una famiglia di algoritmi; potresti
volere un array serializzato, una stringa JSON o un semplice array di dati:

{% highlight php %}
<?php

interface OutputInterface
{
    public function load();
}

class SerializedArrayOutput implements OutputInterface
{
    public function load()
    {
        return serialize($arrayOfData);
    }
}

class JsonStringOutput implements OutputInterface
{
    public function load()
    {
        return json_encode($arrayOfData);
    }
}

class ArrayOutput implements OutputInterface
{
    public function load()
    {
        return $arrayOfData;
    }
}
{% endhighlight %}

Incapsulando gli algoritmi permetti agli sviluppatori di aggiungere altri tipi
di output senza che questo influisca sul codice che li utilizza.

Avrai notato che ogni classe concreta di 'output' implementa una
'OutputInterface'. In questo modo, in primo luogo, si fornisce una serie di
regole a cui tutte le implementazioni si dovranno attenere. Inoltre,
implementando un'interfaccia comune potrai utilizzare il [Type Hinting](http://php.net/language.oop5.typehinting)
per assicurarti che il client stia effettivamente utilizzando un oggetto del
tipo corretto (in questo caso 'OutputInterface').

Il prossimo snippet mostra come una classe può usare uno di questi algoritmi e,
ancora meglio, impostare quello preferito durante il runtime:

{% highlight php %}
<?php
class SomeClient
{
    private $output;

    public function setOutput(OutputInterface $outputType)
    {
        $this->output = $outputType;
    }

    public function loadOutput()
    {
        return $this->output->load();
    }
}
{% endhighlight %}

La classe qui sopra ha una proprietà privata che dev'essere impostata durante il
runtime ed essere del tipo 'OutputInterface'. Una volta che questa proprietà
sarà stata impostata, una chiamata a loadOutput() chiamerà il metodo load()
nella classe concreta del tipo di output impostato.

{% highlight php %}
<?php
$client = new SomeClient();

// Vuoi un array?
$client->setOutput(new ArrayOutput());
$data = $client->loadOutput();

// Vuoi del JSON?
$client->setOutput(new JsonStringOutput());
$data = $client->loadOutput();

// Vuoi un array serializzato?
$client->setOutput(new SerializedArrayOutput());
$data = $client->loadOutput();

{% endhighlight %}

* [Strategy pattern su Wikipedia](http://it.wikipedia.org/wiki/Strategy_pattern)

## Front Controller

Il front controller pattern consiste nell'utilizzo di un singolo punto di
entrata per la tua applicazione Web (es. index.php) che gestisce tutte le
richieste. Questo codice è responsabile di caricare tutte le dipendenze, di
processare la richiesta e di inviare la risposta al browser. Il front controller
pattern può portare benefici perché incoraggia l'uso di codice modulare e
fornisce un punto centrale in cui inserire codice che dev'essere eseguito per
ogni richiesta (come la sanificazione dell'input).

* [Front Controller Pattern su Wikipedia](https://it.wikipedia.org/wiki/Front_Controller_pattern)

## Model-View-Controller

Il pattern _Model-View-Controller_ (MVC), e i "parenti" HMVC e MVVM, permettono
di dividere il codice in oggetti logici che hanno funzioni altamente specifiche.
I modelli servono come livello di accesso ai dati, dove i dati vengono
recuperati e restituiti in formati utilizzabili all'interno della tua
applicazione. I controller gestiscono la richiesta, processano le informazioni
restituite dai modelli e caricano le viste che inviano in risposta. Le viste
sono dei template (markup, XML ecc.) che vengono inviati in risposta al browser
web.

MVC è il pattern architetturale più comune usato nei
[framework PHP](https://github.com/codeguy/php-the-right-way/wiki/Frameworks)
famosi.

Impara di più riguardo l'MVC e i pattern ad esso collegati:

* [MVC](https://it.wikipedia.org/wiki/Model-View-Controller)
* [HMVC](https://en.wikipedia.org/wiki/Hierarchical_model%E2%80%93view%E2%80%93controller)
* [MVVM](https://en.wikipedia.org/wiki/Model_View_ViewModel)
