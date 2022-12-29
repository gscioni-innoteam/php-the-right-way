---
isChild: true
title:   Contenitore
anchor:  containers
---

## Contenitore {#containers_title}

La prima cosa che dovresti capire riguardo i contenitori di iniezione delle
dipendenze è che non sono uguali all'iniezione delle dipendenze. Un contenitore
è un'utilità che ti aiuta a implementare l'iniezione delle dipendenze. Tuttavia,
possono essere (e spesso sono) usati male per implementare un anti-pattern, la
localizzazione dei servizi. Iniettare nelle tue classi un contenitore DI come un
localizzatore di servizi crea una dipendenza dal contenitore ancora più forte di
quella che stai sostituendo. Inoltre rende il tuo codice molto meno trasparente
e più difficile da testare.

La maggior parte dei framework moderni ha il proprio contenitore di iniezione
delle dipendenze che ti permette di unire insieme le dipendenze tramite
configurazione. Questo significa che puoi scrivere del codice applicativo pulito
e indipendente quanto il framework su cui è basato.
