---
title:   Internazionalizzazione e Localizzazione
isChild: true
title:   Internazionalizzazione
anchor:  i18n_l10n
---

## Internazionalizzazione (i18n) e Localizzazione (l10n) {#i18n_l10n_title}

_Disclaimer per i nuovi arrivati: i18n e l10n sono numeronimi, una sorta di abbreviazione in cui i numeri vengono utilizzati per abbreviare
parole - nel nostro caso l'internazionalizzazione diventa i18n e la localizzazione l10n._

Prima di tutto, dobbiamo definire questi due concetti simili e altre cose correlate:

- L'**internazionalizzazione** è quando organizzi il tuo codice in modo che possa essere adattato a diverse lingue o regioni
  senza refactoring. Questa azione viene solitamente eseguita una volta, preferibilmente all'inizio del progetto, altrimenti lo farai
  probabilmente sono necessari alcuni enormi cambiamenti nella fonte!
- La **localizzazione** si verifica quando si adatta l'interfaccia (principalmente) traducendo i contenuti, in base al lavoro i18n svolto
  Prima. Di solito viene eseguito ogni volta che una nuova lingua o regione ha bisogno di supporto e viene aggiornato quando nuovi pezzi di interfaccia
  vengono aggiunti, in quanto devono essere disponibili in tutte le lingue supportate.
- La **pluralizzazione** definisce le regole richieste tra linguaggi distinti per interoperare stringhe contenenti numeri e
  contatori. Ad esempio, in inglese quando hai un solo oggetto, è singolare e qualsiasi cosa diversa da quello lo è
  chiamato plurale; il plurale in questa lingua si indica aggiungendo una S dopo alcune parole, e talvolta ne cambia parti.
  In altre lingue, come il russo o il serbo, ci sono due forme plurali oltre al singolare - potresti persino
  trova lingue con un totale di quattro, cinque o sei forme, come lo sloveno, l'irlandese o l'arabo.

## Modi comuni di implementazione
Il modo più semplice per internazionalizzare il software PHP è utilizzare file di array e utilizzare quelle stringhe in modelli, ad esempio
`<h1><?=$TRANS['title_about_page']?></h1>`. In questo modo, tuttavia, è poco raccomandabile per progetti seri, in quanto pone
alcuni problemi di manutenzione lungo la strada - alcuni potrebbero apparire proprio all'inizio, come la pluralizzazione. Quindi per favore,
non provare questo se il tuo progetto conterrà più di un paio di pagine.

Il modo più classico e spesso preso come riferimento per i18n e l10n è uno [strumento Unix chiamato `gettext`][gettext]. Data
risale al 1995 ed è ancora un'implementazione completa per la traduzione di software. È abbastanza facile iniziare a correre, mentre
ancora sfoggiando potenti strumenti di supporto. Si tratta di Gettext di cui parleremo qui. Inoltre, per aiutarti a non diventare disordinato
sulla riga di comando, presenteremo un'ottima applicazione GUI che può essere utilizzata per aggiornare facilmente il tuo sorgente l10n.

### Altri strumenti

Esistono librerie comuni utilizzate che supportano Gettext e altre implementazioni di i18n. Alcuni di loro possono sembrare più facili
installare o mettere in mostra funzionalità aggiuntive o formati di file i18n. In questo documento, ci concentriamo sugli strumenti forniti con il
PHP core, ma qui ne elenchiamo altri per il completamento:

- [aura/intl][aura-intl]: Fornisce strumenti di internazionalizzazione (I18N), in particolare messaggi orientati ai pacchetti per locale
  traduzione. Utilizza formati di array per i messaggi. Non fornisce un estrattore di messaggi, ma fornisce funzionalità avanzate
  formattazione dei messaggi tramite l'estensione `intl` (compresi i messaggi pluralizzati).
- [oscarotero/Gettext][oscarotero]: supporto Gettext con un'interfaccia OO; include funzioni di supporto migliorate, potenti
  estrattori per diversi formati di file (alcuni dei quali non supportati nativamente dal comando `gettext`), e possono anche esportare
  ad altri formati oltre ai file `.mo/.po`. Può essere utile se hai bisogno di integrare i tuoi file di traduzione in altri
  parti del sistema, come un'interfaccia JavaScript.
- [symfony/translation][symfony]: supporta molti formati diversi, ma raccomanda l'uso di XLIFF dettagliati. No
  include funzioni di supporto né un estrattore integrato, ma supporta i segnaposto che utilizzano `strtr()` internamente.
- [laminas/laminas-i18n][laminas]: supporta file array e INI o formati Gettext. Implementa un livello di memorizzazione nella cache da cui salvarti
  leggendo il filesystem ogni volta. Include anche helper di visualizzazione e filtri e validatori di input compatibili con le impostazioni locali.
  Tuttavia, non ha un estrattore di messaggi.

Altri framework includono anche i moduli i18n, ma quelli non sono disponibili al di fuori delle loro basi di codice:

- [Laravel] supporta i file di array di base, non ha un estrattore automatico ma include un helper `@lang` per i file modello.
- [Yii] supporta la traduzione basata su array, Gettext e database e include un estrattore di messaggi. È supportato dal
  Estensione [`Intl`][intl], disponibile da PHP 5.3 e basata sul [progetto ICU]; questo consente a Yii di funzionare in modo potente
  sostituzioni, come l'ortografia dei numeri, la formattazione di date, orari, intervalli, valuta e ordinali.

Se decidi di utilizzare una delle librerie che non forniscono estrattori, potresti voler utilizzare i formati gettext, quindi
puoi usare la toolchain originale gettext (incluso Poedit) come descritto nel resto del capitolo.

## Gettext

### Installazione
Potrebbe essere necessario installare Gettext e la relativa libreria PHP utilizzando il gestore di pacchetti, come `apt-get` o `yum`.
Dopo l'installazione, abilitalo aggiungendo `extension=gettext.so` (Linux/Unix) o `extension=php_gettext.dll` (Windows) a
il tuo `php.ini`.

Qui useremo anche [Poedit] per creare file di traduzione. Probabilmente lo troverai nel pacchetto del tuo sistema
gestore; è disponibile per Unix, Mac e Windows, e può essere [scaricato gratuitamente dal loro sito][poedit_download]
anche.

### Struttura

#### Tipi di file
Ci sono tre file che di solito gestisci mentre lavori con gettext. I principali sono PO (Portable Object) e
File MO (Machine Object), il primo è un elenco di "oggetti tradotti" leggibili e il secondo, il corrispondente
binario da interpretare da gettext durante la localizzazione. C'è anche un file POT (Template), che contiene semplicemente
tutte le chiavi esistenti dai file di origine e può essere utilizzato come guida per generare e aggiornare tutti i file PO. Quel modello
i file non sono obbligatori: a seconda dello strumento che stai usando per fare l10n, puoi andare bene solo con i file PO/MO.
Avrai sempre una coppia di file PO/MO per lingua e regione, ma solo un POT per dominio.

### Domini
Ci sono alcuni casi, in grandi progetti, in cui potrebbe essere necessario separare le traduzioni quando le stesse parole trasmettono
significato diverso dato un contesto. In questi casi, li dividi in diversi _domini_. Sono, fondamentalmente, nominati
gruppi di file POT/PO/MO, dove il nome del file è detto _translation domain_. Solitamente progetti di piccole e medie dimensioni,
per semplicità, usa un solo dominio; il suo nome è arbitrario, ma useremo "main" per i nostri esempi di codice.
Nei progetti [Symfony], ad esempio, i domini vengono utilizzati per separare la traduzione per i messaggi di convalida.

#### Codice locale
Un locale è semplicemente un codice che identifica una versione di una lingua. È definito seguendo la [ISO 639-1][639-1] e
Specifiche [ISO 3166-1 alpha-2][3166-1]: due lettere minuscole per la lingua, facoltativamente seguite da una sottolineatura e due
lettere maiuscole che identificano il paese o il codice regionale. Per [rare lingue][rare], vengono usate tre lettere.

Per alcuni oratori, la parte country può sembrare ridondante. In effetti, alcune lingue hanno dialetti diversi
paesi, come il tedesco austriaco (`de_AT`) o il portoghese brasiliano (`pt_BR`). La seconda parte è usata per distinguere
tra quei dialetti - quando non è presente, è considerata una versione "generica" ​​o "ibrida" della lingua.

### Struttura della directory
Per utilizzare Gettext, dovremo aderire a una specifica struttura di cartelle. Innanzitutto, dovrai selezionare un arbitrario
root per i tuoi file l10n nel tuo repository di origine. Al suo interno, avrai una cartella per ogni locale necessario e un file
corretta la cartella `LC_MESSAGES` che conterrà tutte le tue coppie PO/MO. Esempio:

{% highlight console %}
<project root>
 ├─ src/
 ├─ templates/
 └─ locales/
    ├─ forum.pot
    ├─ site.pot
    ├─ de/
    │  └─ LC_MESSAGES/
    │     ├─ forum.mo
    │     ├─ forum.po
    │     ├─ site.mo
    │     └─ site.po
    ├─ es_ES/
    │  └─ LC_MESSAGES/
    │     └─ ...
    ├─ fr/
    │  └─ ...
    ├─ pt_BR/
    │  └─ ...
    └─ pt_PT/
       └─ ...
{% endhighlight %}

### Forme plurali
Come abbiamo detto nell'introduzione, lingue diverse potrebbero avere regole plurali diverse. Tuttavia, gettext ci salva da
questo problema ancora una volta. Quando crei un nuovo file `.po`, dovrai dichiarare le [regole plurali][plurale] per questo
lingua e i pezzi tradotti che sono sensibili al plurale avranno una forma diversa per ciascuna di queste regole. quando
chiamando Gettext in codice, dovrai specificare il numero relativo alla frase, e funzionerà il corretto
form da utilizzare, anche utilizzando la sostituzione di stringhe se necessario.

Le regole plurali includono il numero di plurali disponibili e un test booleano con "n" che definirebbe in quale regola il
dato numero diminuisce (iniziando il conteggio con 0). Per esempio:

- Giapponese: `nplurals=1; plural=0` - solo una regola
- Inglese: `nplurals=2; plural=(n != 1);` - due regole, la prima se N è uno, la seconda altrimenti
- Portoghese brasiliano: `nplurals=2; plural=(n > 1);` - due regole, seconda se N è maggiore di uno, prima altrimenti

Ora che hai compreso le basi di come funzionano le regole plurali - e se non l'hai fatto, dai un'occhiata a una spiegazione più approfondita
nel [tutorial di LingoHub][lingohub_plurals] -, potresti invece voler copiare quelli che ti servono da una [lista][plurale]
di scriverle a mano.

Quando chiami Gettext per eseguire la localizzazione su frasi con contatori, dovrai fornirgli il file
anche il relativo numero. Gettext risolverà quale regola dovrebbe essere in vigore e utilizzerà la versione localizzata corretta.
Dovrai includere nel file `.po` una frase diversa per ogni regola plurale definita.

### Esempio di implementazione
Dopo tutta quella teoria, diventiamo un po' pratici. Ecco un estratto di un file `.po` - non preoccuparti del suo formato,
ma con il contenuto complessivo invece; imparerai come modificarlo facilmente in seguito:

{% highlight po %}
msgid ""
msgstr ""
"Language: pt_BR\n"
"Content-Type: text/plain; charset=UTF-8\n"
"Plural-Forms: nplurals=2; plural=(n > 1);\n"

msgid "We are now translating some strings"
msgstr "Nós estamos traduzindo algumas strings agora"

msgid "Hello %1$s! Your last visit was on %2$s"
msgstr "Olá %1$s! Sua última visita foi em %2$s"

msgid "Only one unread message"
msgid_plural "%d unread messages"
msgstr[0] "Só uma mensagem não lida"
msgstr[1] "%d mensagens não lidas"
{% endhighlight %}

La prima sezione funziona come un'intestazione, avendo `msgid` e `msgstr` particolarmente vuoti. Descrive la codifica del file,
forme plurali e altre cose che sono meno rilevanti.
La seconda sezione traduce una semplice stringa dall'inglese a
portoghese brasiliano, e il terzo fa lo stesso, ma sfruttando la sostituzione delle stringhe da [`sprintf`][sprintf] così il
la traduzione può contenere il nome utente e la data della visita.
L'ultima sezione è un esempio di forme di pluralizzazione, visualizzazione
la versione singolare e plurale come `msgid` in inglese e le loro traduzioni corrispondenti come `msgstr` 0 e 1
(dopo il numero dato dalla regola del plurale). Lì viene utilizzata anche la sostituzione delle stringhe in modo che il numero possa essere visto
direttamente nella frase, utilizzando `%d`. Le forme plurali hanno sempre due `msgid` (singolare e plurale), così è
consigliato di non utilizzare un linguaggio complesso come fonte di traduzione.

### Discussione sulle chiavi l10n
Come avrai notato, stiamo usando come ID sorgente la frase vera e propria in inglese. Quel `msgid` è lo stesso usato
in tutti i tuoi file `.po`, il che significa che altre lingue avranno lo stesso formato e gli stessi campi `msgid` ma
righe `msgstr` tradotte.

Parlando di chiavi di traduzione, qui ci sono due "scuole" principali:

1. _`msgid` come frase reale_.
   I principali vantaggi sono:
    - se ci sono parti del software non tradotte in una data lingua, la chiave visualizzata ne manterrà comunque alcune
      senso. Esempio: se ti capita di tradurre a memoria dall'inglese allo spagnolo ma hai bisogno di aiuto per tradurre in francese,
      potresti pubblicare la nuova pagina con frasi francesi mancanti e parti del sito Web verrebbero visualizzate in inglese
      invece;
    - è molto più facile per il traduttore capire cosa sta succedendo e fare una traduzione corretta basata sul
      `msgid`;
    - ti dà l10n "gratuito" per una lingua - quella sorgente;
    - L'unico svantaggio: se hai bisogno di cambiare il testo effettivo, dovresti sostituire lo stesso `msgid`
      in diversi file di lingua.

2. _`msgid` come chiave univoca e strutturata_.
   Descriverebbe il ruolo della frase nell'applicazione in modo strutturato, incluso il modello o la parte in cui il file
   stringa si trova al posto del suo contenuto.
    - è un ottimo modo per organizzare il codice, separando il contenuto del testo dalla logica del template.
    - tuttavia, ciò potrebbe portare problemi al traduttore che perderebbe il contesto. Un file della lingua di origine sarebbe
      necessario come base per altre traduzioni. Esempio: lo sviluppatore dovrebbe idealmente avere un file `en.po`, that
      i traduttori leggerebbero per capire cosa scrivere in `fr.po` per esempio.
    - le traduzioni mancanti mostrerebbero tasti senza significato sullo schermo (`top_menu.welcome` invece di `Hello there, User!`
      sulla suddetta pagina francese non tradotta). Va bene perché costringerebbe la traduzione a essere completa prima della pubblicazione -
      tuttavia, i gravi problemi di traduzione sarebbero notevolmente terribili nell'interfaccia. Alcune librerie, tuttavia, includono un file
      opzione per specificare un dato linguaggio come "fallback", con un comportamento simile all'altro approccio.

Il [manuale Gettext][manuale] favorisce il primo approccio in quanto, in generale, è più facile per traduttori e utenti in
caso di guai. È così che lavoreremo anche qui. Tuttavia, la [documentazione di Symfony][symfony-keys] favorisce
traduzione basata su parole chiave, per consentire modifiche indipendenti di tutte le traduzioni senza influire anche sui modelli.

### Uso quotidiano
In un'applicazione tipica, useresti alcune funzioni Gettext mentre scrivi testo statico nelle tue pagine. Quelle frasi
apparirebbe quindi nei file `.po`, verrebbe tradotto, compilato in file `.mo` e quindi utilizzato da Gettext durante il rendering
l'interfaccia vera e propria. Detto questo, colleghiamo insieme ciò che abbiamo discusso finora in un esempio passo-passo:

#### 1. Un file modello di esempio, che include alcune diverse chiamate gettext
{% highlight php %}
<?php include 'i18n_setup.php' ?>
<div id="header">
    <h1><?=sprintf(gettext('Welcome, %s!'), $name)?></h1>
    <!-- code indented this way only for legibility -->
    <?php if ($unread): ?>
        <h2><?=sprintf(
            ngettext('Only one unread message',
                     '%d unread messages',
                     $unread),
            $unread)?>
        </h2>
    <?php endif ?>
</div>

<h1><?=gettext('Introduction')?></h1>
<p><?=gettext('We\'re now translating some strings')?></p>
{% endhighlight %}

- [`gettext()`][func] traduce semplicemente un `msgid` nel corrispondente `msgstr` per una data lingua. C'è anche
  la funzione abbreviata `_()` che funziona allo stesso modo;
- [`ngettext()`][n_func] fa lo stesso ma con regole plurali;
- Ci sono anche [`dgettext()`][d_func] e [`dngettext()`][dn_func], che ti permettono di sovrascrivere il dominio per un singolo
  chiamata. Maggiori informazioni sulla configurazione del dominio nel prossimo esempio.

#### 2. Un file di installazione di esempio (`i18n_setup.php` come usato sopra), selezionando la locale corretta e configurando Gettext
{% highlight php %}
<?php
/**
 * Verifies if the given $locale is supported in the project
 * @param string $locale
 * @return bool
 */
function valid($locale) {
   return in_array($locale, ['en_US', 'en', 'pt_BR', 'pt', 'es_ES', 'es']);
}

//setting the source/default locale, for informational purposes
$lang = 'en_US';

if (isset($_GET['lang']) && valid($_GET['lang'])) {
    // the locale can be changed through the query-string
    $lang = $_GET['lang'];    //you should sanitize this!
    setcookie('lang', $lang); //it's stored in a cookie so it can be reused
} elseif (isset($_COOKIE['lang']) && valid($_COOKIE['lang'])) {
    // if the cookie is present instead, let's just keep it
    $lang = $_COOKIE['lang']; //you should sanitize this!
} elseif (isset($_SERVER['HTTP_ACCEPT_LANGUAGE'])) {
    // default: look for the languages the browser says the user accepts
    $langs = explode(',', $_SERVER['HTTP_ACCEPT_LANGUAGE']);
    array_walk($langs, function (&$lang) { $lang = strtr(strtok($lang, ';'), ['-' => '_']); });
    foreach ($langs as $browser_lang) {
        if (valid($browser_lang)) {
            $lang = $browser_lang;
            break;
        }
    }
}

// here we define the global system locale given the found language
putenv("LANG=$lang");

// this might be useful for date functions (LC_TIME) or money formatting (LC_MONETARY), for instance
setlocale(LC_ALL, $lang);

// this will make Gettext look for ../locales/<lang>/LC_MESSAGES/main.mo
bindtextdomain('main', '../locales');

// indicates in what encoding the file should be read
bind_textdomain_codeset('main', 'UTF-8');

// if your application has additional domains, as cited before, you should bind them here as well
bindtextdomain('forum', '../locales');
bind_textdomain_codeset('forum', 'UTF-8');

// here we indicate the default domain the gettext() calls will respond to
textdomain('main');

// this would look for the string in forum.mo instead of main.mo
// echo dgettext('forum', 'Welcome back!');
?>
{% endhighlight %}

#### 3. Preparazione della traduzione per la prima esecuzione
Uno dei grandi vantaggi di Gettext rispetto ai pacchetti i18n del framework personalizzato è il suo formato di file ampio e potente.
"Oh amico, è abbastanza difficile da capire e modificare a mano, un semplice array sarebbe più facile!" Non fare errori,
applicazioni come [Poedit] sono qui per aiutarti - _molto_. Puoi ottenere il programma dal [loro sito web][poedit_download],
è gratuito e disponibile per tutte le piattaforme. È uno strumento abbastanza facile a cui abituarsi e molto potente allo stesso tempo
tempo - utilizzando tutte le funzionalità che Gettext ha a disposizione. Questa guida è basata su PoEdit 1.8.

Nella prima esecuzione, dovresti selezionare "File > Nuovo..." dal menu. Ti verrà chiesto subito la lingua:
qui puoi selezionare/filtrare la lingua in cui vuoi tradurre o utilizzare quel formato che abbiamo menzionato prima, come ad esempio
`en_US` o `pt_BR`.

Ora salva il file, usando anche quella struttura di directory che abbiamo menzionato. Quindi dovresti fare clic su "Estrai da fonti",
e qui configurerai varie impostazioni per le attività di estrazione e traduzione. Sarai in grado di trovare tutti quelli
successivamente tramite “Catalogo > Proprietà”:

- Percorsi di origine: qui devi includere tutte le cartelle del progetto in cui vengono chiamati `gettext()` (e fratelli) - questo
  di solito è la cartella o le cartelle dei modelli/visualizzazioni. Questa è l'unica impostazione obbligatoria;
- Proprietà di traduzione:
    - Nome e versione del progetto, Team e indirizzo email del Team: informazioni utili che vanno nell'header del file .po;
    - Forme plurali: ecco quelle regole che abbiamo menzionato prima - c'è anche un collegamento con i campioni. Puoi
      lascialo con l'opzione predefinita per la maggior parte del tempo, poiché PoEdit include già un pratico database di regole plurali per
      molte lingue.
    - Set di caratteri: UTF-8, preferibilmente;
    - Set di caratteri del codice sorgente: imposta qui il set di caratteri utilizzato dalla tua base di codice, probabilmente anche UTF-8, giusto?
- Parole chiave di origine: il software sottostante sa come appaiono `gettext()` e chiamate di funzioni simili in diversi
  linguaggi di programmazione, ma potresti anche creare le tue funzioni di traduzione. Sarà qui che aggiungerai quelli
  altri metodi. Questo sarà discusso più avanti nella sezione "Suggerimenti".

Dopo aver impostato questi punti, eseguirà una scansione dei file di origine per trovare tutte le chiamate di localizzazione. Dopo ogni
scan PoEdit visualizzerà un riepilogo di ciò che è stato trovato e ciò che è stato rimosso dai file di origine. Le nuove voci verranno alimentate
vuoto nella tabella di traduzione e inizierai a digitare le versioni localizzate di quelle stringhe. Salvalo e un .mo
file verrà (ri)compilato nella stessa cartella e ta-dah: il tuo progetto è internazionalizzato.

#### 4. Traduzione di stringhe
Come avrai notato in precedenza, esistono due tipi principali di stringhe localizzate: quelle semplici e quelle con il plurale
le forme. I primi hanno semplicemente due riquadri: sorgente e stringa localizzata. La stringa di origine non può essere modificata come
Gettext/Poedit non include i poteri per modificare i tuoi file di origine: dovresti cambiare la fonte stessa e ripetere la scansione
i file. Suggerimento: puoi fare clic con il pulsante destro del mouse su una riga di traduzione e ti suggerirà i file di origine e le righe in cui si trova
viene utilizzata la stringa.
D'altra parte, le stringhe in forma plurale includono due caselle per mostrare le due stringhe di origine e schede in modo da poterle configurare
le diverse forme finali.

Ogni volta che cambi le tue fonti e hai bisogno di aggiornare le traduzioni, basta premere Aggiorna e Poedit eseguirà nuovamente la scansione del codice,
rimuovendo voci inesistenti, unendo quelle modificate e aggiungendone di nuove. Potrebbe anche provare a indovinarne alcuni
traduzioni, basate su altre che hai fatto. Quelle ipotesi e le voci modificate riceveranno un indicatore "Fuzzy",
indicando che necessita di revisione, apparendo dorato nell'elenco. È anche utile se hai un team di traduzione e qualcuno
prova a scrivere qualcosa di cui non è sicuro: basta segnare Fuzzy e qualcun altro esaminerà in seguito.

Infine, si consiglia di lasciare contrassegnato "Visualizza > Prima le voci non tradotte", in quanto ti aiuterà _molto_ a non dimenticare
qualsiasi ingresso. Da quel menu, puoi anche aprire parti dell'interfaccia utente che ti consentono di lasciare informazioni contestuali per
traduttori se necessario.

### Suggerimenti e trucchi

#### Possibili problemi di memorizzazione nella cache
Se stai eseguendo PHP come modulo su Apache (`mod_php`), potresti riscontrare problemi con il file `.mo` che viene memorizzato nella cache. Esso
accade la prima volta che viene letto e quindi, per aggiornarlo, potrebbe essere necessario riavviare il server. Su Nginx e PHP5 it
di solito sono necessari solo un paio di aggiornamenti di pagina per aggiornare la cache di traduzione e su PHP7 è raramente necessario.

#### Ulteriori funzioni di supporto
Come preferito da molte persone, è più facile usare `_()` invece di `gettext()`. Molte librerie i18n personalizzate da
i framework usano anche qualcosa di simile a `t()`, per rendere il codice tradotto più breve. Tuttavia, questa è l'unica funzione
che sfoggia una scorciatoia. Potresti voler aggiungere al tuo progetto altri, come `__()` o `_n()` per `ngettext()`,
o forse un elegante `_r()` che unirebbe le chiamate `gettext()` e `sprintf()`. Altre librerie, ad es
[oscarotero's Gettext][oscarotero] fornisce anche funzioni di supporto come queste.

In questi casi, dovrai istruire l'utilità Gettext su come estrarre le stringhe da quelle nuove funzioni.
Non aver paura; è molto facile. È solo un campo nel file `.po` o una schermata Impostazioni su Poedit. Nell'editore,
tale opzione è all'interno di "Catalogo > Proprietà > Parole chiave sorgente". Ricorda: Gettext conosce già le funzioni predefinite
per molte lingue, quindi non temere se l'elenco sembra vuoto. È necessario includere lì le specifiche di quelli
nuove funzioni, seguendo [un formato specifico][func_format]:

- se crei qualcosa come `t()` che restituisce semplicemente la traduzione di una stringa, puoi specificarla come `t`.
  Gettext saprà che l'unico argomento della funzione è la stringa da tradurre;
- se la funzione ha più di un argomento, puoi specificare in quale si trova la prima stringa - e se necessario, il
  anche la forma plurale. Ad esempio, se chiamiamo la nostra funzione in questo modo: `__('one user', '%d users', $number)`, il
  la specifica sarebbe `__:1,2`, il che significa che la prima forma è il primo argomento e la seconda forma è il secondo
  discussione. Se invece il tuo numero arriva come primo argomento, la specifica sarebbe `__:2,3`, a indicare che la prima forma è
  il secondo argomento, e così via.

Dopo aver incluso queste nuove regole nel file `.po`, una nuova scansione riporterà le nuove stringhe con la stessa facilità di prima.

### Riferimenti

* [Wikipedia: i18n and l10n](https://en.wikipedia.org/wiki/Internationalization_and_localization)
* [Wikipedia: Gettext](https://en.wikipedia.org/wiki/Gettext)
* [LingoHub: PHP internationalization with gettext tutorial][lingohub]
* [PHP Manual: Gettext](https://secure.php.net/manual/book.gettext.php)
* [Gettext Manual][manual]

[Poedit]: https://poedit.net
[poedit_download]: https://poedit.net/download
[lingohub]: https://lingohub.com/blog/2013/07/php-internationalization-with-gettext-tutorial/
[lingohub_plurals]: https://lingohub.com/blog/2013/07/php-internationalization-with-gettext-tutorial/#Plurals
[plural]: http://docs.translatehouse.org/projects/localization-guide/en/latest/l10n/pluralforms.html
[gettext]: https://en.wikipedia.org/wiki/Gettext
[manual]: https://www.gnu.org/software/gettext/manual/gettext.html
[639-1]: https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
[3166-1]: https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2
[rare]: https://www.gnu.org/software/gettext/manual/gettext.html#Rare-Language-Codes
[func_format]: https://www.gnu.org/software/gettext/manual/gettext.html#Language-specific-options
[aura-intl]: https://github.com/auraphp/Aura.Intl
[oscarotero]: https://github.com/oscarotero/Gettext
[symfony]: https://symfony.com/components/Translation
[laminas]: https://docs.laminas.dev/laminas-i18n/
[laravel]: https://laravel.com/docs/master/localization
[yii]: https://www.yiiframework.com/doc/guide/2.0/en/tutorial-i18n
[intl]: https://secure.php.net/manual/intro.intl.php
[ICU project]: https://icu.unicode.org/
[symfony-keys]: https://symfony.com/doc/current/translation.html#using-real-or-keyword-messages

[sprintf]: https://secure.php.net/manual/function.sprintf.php
[func]: https://secure.php.net/manual/function.gettext.php
[n_func]: https://secure.php.net/manual/function.ngettext.php
[d_func]: https://secure.php.net/manual/function.dgettext.php
[dn_func]: https://secure.php.net/manual/function.dngettext.php
