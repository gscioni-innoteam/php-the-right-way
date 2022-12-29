---
isChild: true
title:   File di configurazione
anchor:  configuration_files
---

## File di configurazione {#configuration_files_title}

Nella creazione di file di configurazione per la tua applicazione, le migliori
pratiche raccomandano l'utilizzo di uno dei seguenti metodi:

- È consigliato salvare le informazioni di configurazioni dove non possono
  essere lette direttamente tramite il file system

- Se devi salvare i tuoi file di configurazione nella root pubblica, chiama i
  file con estensione `.php`. Questo assicura che, anche se uno script è
  eseguito direttamente, non sarà visualizzato come testo semplice.

- Le informazioni nei file di configurazione dovrebbero essere protette
  appropriatamente, tramite crittografia o permessi del filesystem per
  gruppo/utente