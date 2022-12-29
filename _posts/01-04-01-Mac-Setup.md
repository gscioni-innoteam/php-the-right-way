---
isChild: true
title:   Configurazione Mac
anchor:  mac_setup
---

## Configurazione Mac {#mac_setup_title}

macOS viene fornito con una versione pre-pacchettizzata di PHP, ma è normalmente un po' più vecchia dell'ultima versione stabile. Ci sono diversi modi per installare PHP su macOS.

### Installare PHP tramite Homebrew

[Homebrew] è un potente gestore di pacchetti per macOS che può aiutarti a installare facilmente PHP e varie estensioni. Homebrew PHP è un repository che contiene "formule" per Homebrew relative a PHP e ti permetterà di installare PHP. A questo punto, puoi installare PHP 5.6, 7.0, 7.1, 7.2, 7.3, 7.4, 8.0, 8.1 e PHP 8.2 usando il comando brew install e cambiando la versione attiva modificando la variabile PATH.

```
brew install php@8.1
```

Puoi anche switchare tra le varie versioni modificando la variabile `PATH`. In alternativa, puoi utilizzare [brew-php-switcher][brew-php-switcher] per cambiare versione automaticamente.

Puoi anche utilizzare diverse versioni di PHP manualmente utilizzando le azioni link e unlink di brew:

```
brew unlink php
brew link --overwrite php@8.0
```

```
brew unlink php
brew link --overwrite php@8.1
```

### Installare PHP tramite Macports

Il progetto MacPorts è un'iniziativa della comunità open source per il design di un sistema di semplice utilizzo per la compilazione, l'installazione e l'aggiornamento di software open source per linea di comando, X11 o Acqua sul sistema operativo OS X.

MacPorts supporta i binari precompilati, quindi non devi ricompilare le dipendenze dai sorgenti ogni volta. È un salvavita se non hai alcun pacchetto installato sul sistema.

In questo momento puoi installare `php54`, `php55`, `php56`, `php70`, `php71`, `php72`, `php73`, `php74`, `php80`, `php81` o `php82` usando il comando `port install`, per esempio:

    sudo port install php74
    sudo port install php81

E puoi eseguire il comando `select` per cambiare la versione attiva di PHP:

    sudo port select --set php php81

### Installare PHP tramite phpbrew

[phpbrew] è uno strumento per l'installazione e la gestione di versioni multiple di PHP. Può essere molto utile se due applicazioni/progetti differenti richiedono versioni differenti di PHP e non stai usando le macchine virtuali.

### Installare PHP dall'installer binario di Liip

Un'altra opzione popolare è [php-osx.liip.ch] che fornisce un metodo di installazione alternativo per le versioni 5.3 fino al 7.3.
Questo non sovrascriverà i binari PHP installati da Apple ma provvederà ad una installazione su percorso separato (/usr/local/php5).

### Compilare il sorgente

Un'altra opzione che ti fornisce controllo sulla versione di PHP che installi è 
[compilarlo tu stesso]. In questo caso assicurati di avere installato [Xcode] o il sostituto di Apple 
["Strumenti da riga di comando per XCode"], scaricabile dal Centro Sviluppatori Mac di Apple.

### Installatori all-in-one

Le soluzioni elencate sopra gestiscono principalmente solo PHP, e non forniscono cose come Apache, Nginx o un server SQL. 
Le soluzioni "all-in-one" come [MAMP] e [XAMPP] installeranno questi altri software per te e li integreranno l'uno con l'altro, 
ma la facilità d'installazione compromette la flessibilità.

[Homebrew]: https://brew.sh/
[Homebrew PHP]: https://github.com/Homebrew/homebrew-php#installation
[MacPorts]: https://www.macports.org/install.php
[phpbrew]: https://github.com/phpbrew/phpbrew
[php-osx.liip.ch]: https://web.archive.org/web/20220505163210/https://php-osx.liip.ch/
[mac-compile]: https://secure.php.net/install.macosx.compile
[xcode-gcc-substitution]: https://github.com/kennethreitz/osx-gcc-installer
["Command Line Tools for XCode"]: https://developer.apple.com/downloads
[apache]: https://httpd.apache.org/
[nginx]: https://www.nginx.com/
[mamp-downloads]: https://www.mamp.info/en/downloads/
[xampp]: https://www.apachefriends.org/
[brew-php-switcher]: https://github.com/philcook/brew-php-switcher
