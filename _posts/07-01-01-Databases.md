---
title:  Database
anchor: databases
---

# Database {#databases_title}

Molte volte il tuo codice PHP utilizzerà un database per memorizzare
informazioni. Hai diverse opzioni per connetterti e interagire con il tuo
database. L'opzione consigliata **fino a PHP 5.1.0** era di usare i driver
nativi come [mysqli], [pgsql], [mssql] etc.

I driver nativi vanno bene se usi _un_ database nella tua applicazione ma se,
per esempio, stai usando MySQL e un po' di MSSQL, o devi connetterti a un
database Oracle, allora non potrai usare gli stessi driver. Dovrai imparare una
nuova API per ogni database e può diventare faticoso.

[mysqli]: http://php.net/mysqli
[pgsql]: http://php.net/pgsql
[mssql]: http://php.net/mssql
