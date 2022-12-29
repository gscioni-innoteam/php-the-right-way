---
isChild: true
title:   Problema complesso
anchor:  complex_problem
---

## Problema complesso {#complex_problem_title}

Se hai mai letto qualcosa sull'iniezione delle dipendenze allora hai
probabilmente visto i termini *"Inversione del controllo"* o *"Principio di
inversione della dipendenza"*. Questi sono problemi complessi risolti
dall'iniezione delle dipendenze.

### Inversione del controllo

L'inversione del controllo consiste, come suggerisce il nome, nell'"invertire il
controllo" di un sistema tenendo il controllo organizzativo completamente
separato dai nostri oggetti. Con l'iniezione delle dipendenze, questo significa
rendere le dipendenze più flessibili controllandole e istanziandole in un altro
punto del sistema.

Per anni, i framework PHP hanno usato l'inversione del controllo. Tuttavia, la
domanda è diventata: quale parte del controllo stai invertendo, e dove? Per
esempio, i framework MVC generalmente fornivano un oggetto padre o un controller
di base che gli altri controller dovevano estendere per accedere alle sue
dipendenze. Questa **è** inversione del controllo, ma invece di rendere le
dipendenze più flessibili, questo metodo semplicemente le spostava.

L'iniezione delle dipendenze ci permette di risolvere questo problema in maniera
più elegante, iniettando solo le dipendenze che ci servono, quando ci servono,
senza il bisogno di alcuna dipendenza rigida.

### S.O.L.I.D.

#### Single Responsibility Principle

Una domanda comune tra coloro che iniziano a scrivere programmi per il web è "dove metto le mie cose?" Nel corso degli anni, questa risposta è stata costantemente "dove si trova `DocumentRoot`". Sebbene questa risposta non sia completa, è un ottimo punto di partenza.

Per motivi di sicurezza, i file di configurazione non dovrebbero essere accessibili ai visitatori di un sito; pertanto, gli script pubblici vengono conservati in una directory pubblica e le configurazioni ei dati privati ​​vengono conservati all'esterno di tale directory.

Per ogni team, CMS o framework in cui si lavora, ciascuna di queste entità utilizza una struttura di directory standard. Tuttavia, se si sta iniziando un progetto da soli, sapere quale struttura di filesystem utilizzare può essere scoraggiante.

Il principio di responsabilità unica riguarda gli attori e l'architettura di alto livello. Afferma che “Una classe dovrebbe avere
un solo motivo per cambiare”. Ciò significa che ogni classe dovrebbe _solo_ avere la responsabilità su una singola parte del
funzionalità fornite dal software. Il più grande vantaggio di questo approccio è che consente di migliorare il codice
_riutilizzabilità_. Progettando la nostra classe per fare solo una cosa, possiamo usarla (o riutilizzarla) in qualsiasi altro programma senza
cambiandolo.

#### Open/Closed Principle

Il principio Open/Closed riguarda la progettazione delle classi e le estensioni delle funzionalità. Afferma che “le entità software (classi,
moduli, funzioni, ecc.) dovrebbero essere aperti per l'estensione, ma chiusi per la modifica. Ciò significa che dovremmo progettare
i nostri moduli, classi e funzioni in modo tale che quando è necessaria una nuova funzionalità, non dovremmo modificare il nostro esistente
codice ma piuttosto scrivere nuovo codice che verrà utilizzato dal codice esistente. In pratica, questo significa che dovremmo scrivere
classi che implementano e aderiscono a _interfaces_, quindi digitare suggerimenti contro quelle interfacce invece di classi specifiche.

Il più grande vantaggio di questo approccio è che possiamo estendere molto facilmente il nostro codice con il supporto per qualcosa di nuovo senza
dover modificare il codice esistente, il che significa che possiamo ridurre i tempi di QA e il rischio di impatto negativo sull'applicazione
è sostanzialmente ridotto. Possiamo implementare nuovo codice, più velocemente e con maggiore sicurezza.

#### Liskov Substitution Principle

Il principio di sostituzione di Liskov riguarda la sottotipizzazione e l'ereditarietà. Afferma che “Le classi per bambini non dovrebbero mai interrompersi
le definizioni del tipo della classe genitore. Oppure, nelle parole di Robert C. Martin, “i sottotipi devono essere sostituibili per la loro base
tipi.”

Ad esempio, se abbiamo un'interfaccia `FileInterface` che definisce un metodo `embed()` e abbiamo `Audio` e `Video`
classi che implementano entrambe l'interfaccia `FileInterface`, allora possiamo aspettarci che l'uso del metodo `embed()` sarà sempre
fare la cosa che intendiamo. Se successivamente creiamo una classe `PDF` o una classe `Gist` che implementa `FileInterface`
interfaccia, sapremo già e capiremo cosa farà il metodo `embed()`. Il più grande vantaggio di questo approccio
è che abbiamo la capacità di creare programmi flessibili e facilmente configurabili, perché quando cambiamo un oggetto di a
type (ad esempio, `FileInterface`) a un altro non abbiamo bisogno di cambiare nient'altro nel nostro programma.

#### Interface Segregation Principle

Il principio di segregazione dell'interfaccia (ISP) riguarda la comunicazione _business-logic-to-clients_. Afferma che “Nessun cliente
dovrebbe essere costretto a dipendere da metodi che non usa”. Ciò significa che invece di avere un'unica interfaccia monolitica
che tutte le classi conformi devono implementare, dovremmo invece fornire un insieme di interfacce più piccole, specifiche per concetto
che una classe conforme implementa uno o più di.

Ad esempio, una classe `Car` o `Bus` sarebbe interessata a un metodo `steeringWheel()`, ma una `Motorcycle` o `Tricycle`
la classe no. Al contrario, una classe `Motorcycle` o `Tricycle` sarebbe interessata a un metodo `handlebars()`, ma una
La classe `Car` o `Bus` non lo farebbe. Non è necessario che tutti questi tipi di veicoli implementino il supporto per entrambi
`steeringWheel()` così come `handlebars()`, quindi dovremmo separare l'interfaccia sorgente.

#### Dependency Inversion Principle

Il principio di inversione della dipendenza è la "D" nell'insieme di principi di
design orientato agli oggetti S.O.L.I.D., secondo cui si dovrebbe *"Dipendere
dalle astrazioni. Non dipendere dalle concrezioni."* Detto semplicemente, questo
significa che le nostre dipendenze dovrebbero essere interfacce/contratti o
classi astratte, non implementazioni concrete. Possiamo facilmente
rifattorizzare l'esempio sopra in modo che segua questo principio.

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(AdapterInterface $adapter)
    {
        $this->adapter = $adapter;
    }
}

interface AdapterInterface {}

class MysqlAdapter implements AdapterInterface {}
{% endhighlight %}

Ci sono diversi benefici all'approccio seguito, in cui la classe `Database`
dipende ora da un'interfaccia piuttosto che da un'implementazione concreta.

Pensa di lavorare in team, e che un collega stia lavorando all'adattatore. Nel
nostro primo esempio, avremmo dovuto aspettare che tale collega finisse
l'adattore prima di poterlo imitare appropriatamente per i nostri unit test. Ora
che la dipendenza è un'interfaccia/contratto, possiamo felicemente imitare
quell'interfaccia sapendo che il nostro collega costruirà l'adattatore basandosi
su quel contratto.

Un beneficio ancora più grande è che ora il nostro codice è molto più scalabile.
Se tra un anno decidiamo che vogliamo migrare verso un tipo diverso di database,
possiamo scrivere un adattatore che implementi l'interfaccia originale e
iniettare quello. Non servirebbe alcun'altra rifattorizzazione, perché saremmo
sicuri che l'adattore segue il contratto stabilito dall'interfaccia.
