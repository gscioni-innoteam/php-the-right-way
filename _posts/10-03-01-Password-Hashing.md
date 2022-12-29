---
isChild: true
title:   Hashing delle Password
anchor:  password_hashing
---

## Hashing delle Password {#password_hashing_title}

Prima o poi chiunque crea un'applicazione PHP che richiede il login degli
utenti. Username e password vengono salvati nel database e usati successivamente
per autenticare gli utenti al login.

E' davvero importante che si utilizzi una strategia di [_hash_][3] per le passwords prima che vengano salvate. Hash ed Encrypt [sono due cose completamente differenti][7]
che talvolta vengono confuse.

Il sistema di Hashing è irreversible, che procede in senso unico. Questo produce una stringa di lunghezza fissa che non può essere annullata in modo fattibile.
Ciò significa che puoi confrontare un hash con un altro per determinare se entrambi provengono dalla stessa stringa di origine, ma tu
Impossibile determinare la stringa originale. Se le password non sono sottoposte ad hashing e il tuo database è accessibile da un utente non autorizzato
di terze parti, tutti gli account utente sono ora compromessi.

A differenza dell'hashing, la crittografia è reversibile (a condizione che tu abbia la chiave). La crittografia è utile in altre aree, ma è scarsa
strategia per l'archiviazione sicura delle password.

Le password dovrebbero anche essere individualmente [_salted_][5] aggiungendo una stringa casuale a ciascuna password prima dell'hashing. Ciò impedisce gli attacchi del dizionario e l'uso di "tabelle arcobaleno" (un elenco inverso di hash crittografici per password comuni).

L'hashing e il salting sono fondamentali poiché spesso gli utenti utilizzano la stessa password per più servizi e la qualità della password può essere scarsa.

Inoltre, dovresti usare [un algoritmo specializzato di _password hashing_][6] piuttosto che veloce e generico
funzione hash crittografica (ad es. SHA256). Il breve elenco di algoritmi di hashing delle password accettabili (a giugno 2018)
da usare sono:

* Argon2 (available in PHP 7.2 and newer)
* Scrypt
* **Bcrypt** (PHP provides this one for you; see below)
* PBKDF2 with HMAC-SHA256 or HMAC-SHA512

Per fortuna, al giorno d'oggi PHP lo rende davvero semplice.

**Hashing passwords with `password_hash`**

In PHP 5.5 è stata introdotta la funzione `password_hash`. In questo momento
utilizza l'algoritmo BCrypt, che è il più potente attualmente supportato da PHP.
Sarà aggiornata in futuro per supportare altri algoritmi. La libreria
`password_compat` è stata creata per fornire compatibilità per PHP >= 5.3.7.

Nell'esempio sotto calcoliamo l'hash di una stringa, quindi confrontiamo l'hash
con una nuova stringa. Poiché le nostre stringhe di origine sono differenti
('secret-password' e 'bad-password') questo login fallirà.

{% highlight php %}
<?php
require 'password.php';

$passwordHash = password_hash('secret-password', PASSWORD_DEFAULT);

if (password_verify('bad-password', $passwordHash)) {
    // password corretta
} else {
    // password sbagliata
}
{% endhighlight %}

`password_hash()` si occupa del salting della password per te. Il sale viene memorizzato, insieme all'algoritmo e al "costo", come parte dell'hash. `password_verify()` estrae questo per determinare come controllare la password, quindi non hai bisogno di un campo di database separato per memorizzare i tuoi sali.

* [Impara a usare `password_hash()`] [1]
* [`password_compat` for PHP >= 5.3.7 && < 5.5] [2]
* [Scopri il ruolo della crittografia nell'hashing] [3]
* [RFC della funzione PHP `password_hash()`] [4]
* [Learn about salts] [5]

[1]: https://secure.php.net/function.password-hash
[2]: https://github.com/ircmaxell/password_compat
[3]: https://wikipedia.org/wiki/Cryptographic_hash_function
[4]: https://wiki.php.net/rfc/password_hash
[5]: https://wikipedia.org/wiki/Salt_(cryptography)
[6]: https://paragonie.com/blog/2016/02/how-safely-store-password-in-2016
[7]: https://paragonie.com/blog/2015/08/you-wouldnt-base64-a-password-cryptography-decoded

