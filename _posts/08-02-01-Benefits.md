---
isChild: true
title:   Benefici
anchor:  templating_benefits
---

## Benefici {#templating_benefits_title}

Il beneficio principale derivante dall'uso dei template è la separazione che
creano tra la logica di presentazione e il resto della tua applicazione. I
template hanno la sola responsabilità di visualizzare contenuto formattato. Non
sono responsabili per il recupero o la persistenza dei dati o altri compiti più
complessi. Questo porta alla scrittura di codice più pulito e più facile da
leggere, particolarmente utile in un team dove gli sviluppatori lavorano sul
codice server-side (controller e modelli) e i designer lavorano sul client-side
(markup).

I template, inoltre, migliorano l'organizzazione del codice di presentazione. I
template sono generalmente posti in una cartella "views", ciascuno definito in
un singolo file. Questo approccio incoraggia il riutilizzo del codice: grandi
blocchi di codice vengono spezzati in pezzi più piccoli e riutilizzabili, spesso
chiamati template o viste parziali ("partials"). Per esempio, l'header e il
footer del tuo sito possono essere definiti ciascuno in un template e venire poi
inclusi prima e dopo ciascun template di pagina.

Infine, a seconda della libreria che usi, i template possono offrire maggiore
sicurezza eseguendo l'escape automatico del contenuto generato dall'utente.
Alcune librerie offrono anche una funzionalità di sand-boxing: in questo caso i
designer dei template hanno accesso solo a variabili e funzioni autorizzate.
