---
isChild: true
title:   Configurazione Windows
anchor:  windows_setup
---

## Configurazione Windows {#windows_setup_title}

Puoi installare PHP su Windows in diversi modi. Puoi scaricare i
[binari][php-downloads] e, fino a poco tempo fa, potevi usare un installer
'.msi'. L'installer non è più disponibile e si ferma a PHP 5.3.0.

Per l'apprendimento e lo sviluppo locale puoi usare il webserver integrato in
PHP 5.4 e superiori, in modo da non doverlo configurare. Se preferisci un
pacchetto "all-in-one" che include un webserver e MySQL, allora strumenti come
[Web Platform Installer][wpi], [Zend Server CE][zsce], [XAMPP][xampp] e
[WAMP][wamp] ti aiuteranno a configurare un ambiente di sviluppo Windows in men
che non si dica. Detto questo, questi strumenti saranno diversi dall'ambiente di
produzione, quindi fai attenzione alle differenze se stai sviluppando su Windows
ma pubblicando su Linux.

Se devi far girare un ambiente di produzione su Windows, allora IIS7 ti fornirà
quello più stabile e performante. Puoi usare [phpmanager][phpmanager] (un plugin
GUI per IIS7) per rendere più semplice la configurazione e la gestione di PHP.
IIS7 integra già FastCGI, devi solo configurare PHP come handler. Per supporto e
ulteriori informazioni, c'è un'[area dedicata a PHP su iis.net][php-iis].

Generalmente eseguire la tua applicazione su differenti ambienti in sviluppo e produzione può portarti a riscontrare strani bugs quando porterai il tuo applicativo in produzione.
Se stai sviluppando su Windows e deployando verso un ambiente unix like (generalmente Linux, o qualsiasi sistema non-Windows) dovresti considerare l'utilizzo di una [Virtual Machine](/#virtualization_title)

Chris Tankersley ha realizzato un post davvero interessante su quali tools usare per [sviluppare PHP su Windows][windows-tools].

[easyphp]: https://www.easyphp.org/
[phpmanager]: http://phpmanager.codeplex.com/
[openserver]: https://ospanel.io/
[wamp]: https://www.wampserver.com/en/
[php-downloads]: https://windows.php.net/download/
[php-iis]: https://php.iis.net/
[windows-path]: https://www.windows-commandline.com/set-path-command-line/
[windows-tools]: https://ctankersley.com/2016/11/13/developing-on-windows-2016/
[xampp]: https://www.apachefriends.org/
