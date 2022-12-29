---
isChild: true
title:   Test Driven Development
anchor:  test_driven_development
---

## Test Driven Development {#test_driven_development_title}

Da [Wikipedia](http://it.wikipedia.org/wiki/Test_Driven_Development):

> Il Test Driven Development, in sigla TDD (in italiano: Sviluppo guidato dalle
> verifiche) è un processo di sviluppo del software in cui lo sviluppo vero e
> proprio è preceduto (e guidato, driven) dalla stesura di test automatici. Il
> processo si articola sulla ripetizione di brevi cicli di sviluppo e collaudo
> (noti come "cicli TDD", TDD cycles) suddivisi in tre fasi successive,
> sintetizzate dal motto "Red-Green-Refactor".

Ci sono molti modi diversi di testare la tua applicazione.

### Unit testing

Lo unit testing è un approccio di programmazione usato per assicurarsi che
funzioni, classi e metodi funzionino come ci si aspetta durante l'intero ciclo
di sviluppo. Controllando i valori di input e output delle funzioni e dei
metodi, puoi accertarti che la logica interna funzioni correttamente. Usando
l'iniezione delle dipendenze e creando classi mock e stub puoi verificare che le
dipendenze vengano usate correttamente per una copertura dei test ancora
migliore.

Quando crei una classe o funzione dovresti creare uno unit test per ogni
comportamento che deve assumere. Come minimo dovresti assicurarti che generi un
errore se le invii argomenti non validi e che funzioni se le invii argomenti
validi. Questo aiuterà ad assicurarsi che, quando cambi qualcosa in questa
classe o funzione successivamente nel ciclo di sviluppo, la vecchia
caratteristica continuerà a funzionare come previsto. L'unica alternativa a
questo sarebbe var_dump() in un file test.php, il che non è un modo corretto per
creare un'applicazione, sia piccola che grande.

L'altro utilizzo per gli unit test è nel contributo a progetti open source. Se
riesci a scrivere un test che mostra una caratteristica non funzionante, lo
sistemi e mostri che ora il test passa, le patch saranno accettate molto più
facilmente. Se hai intenzione di lanciare un progetto che accetta contributi
esterni, questo dovrebbe essere un requisito.

[PHPUnit](http://phpunit.de) è lo standard de-facto per la scrittura di unit
test per applicazioni PHP, ma ci sono diverse alternative:

* [atoum](https://github.com/atoum/atoum)
* [Enhance PHP](https://github.com/Enhance-PHP/Enhance-PHP)
* [PUnit](http://punit.smf.me.uk/)
* [SimpleTest](http://simpletest.org)

### Integration testing

Da [Wikipedia](http://en.wikipedia.org/wiki/Integration_testing):

> L'integration testing (a volte chiamato Integration and Testing, abbreviato
> "I&T") è la fase del testing software in cui i moduli individuali vengono
> combinati e testati come un insieme. Si piazza dopo lo unit testing e prima
> del validation testing. L'integration testing prende come input moduli su cui
> è già stato effettuato unit testing, li raggruppa in aggregati più grandi,
> applica i test definiti nel piano di integration testing a questi aggregati e
> invia come output il sistema integrato, pronto per l'esecuzione del system
> testing.

Molti degli strumenti per lo unit testing possono essere usati anche per
l'integration testing, giacché molti dei principi di base sono gli stessi.

### Functional testing

A volte conosciuto anche come acceptance testing, il functional testing consiste
nell'utilizzo di strumenti per la creazione di test automatizzati che usino
realmente la tua applicazione invece di limitarsi a verificare che unità
individuali di codice si comportino correttamente e riescano a parlare l'una con
l'altra. Questi strumenti funzionano generalmente utilizzando dati reali e
simulati utenti dell'applicazione esistenti.

#### Strumenti per il functional testing

* [Selenium](http://seleniumhq.com)
* [Mink](http://mink.behat.org)
* [Codeception](http://codeception.com) è un framework completo che include
  strumenti di acceptance testing
* [Storyplayer](http://datasift.github.io/storyplayer) è un framework completo
  per il testing che include il supporto per la creazione e la distruzione di
  ambienti di test su richiesta
