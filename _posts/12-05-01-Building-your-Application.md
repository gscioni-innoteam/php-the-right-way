---
isChild: true
title:   Costruire e pubblicare la tua applicazione
anchor:  building_and_deploying_your_application
---

## Costruire e pubblicare la tua applicazione {#building_and_deploying_your_application_title}

Se ti trovi a dover modificare lo schema del database o eseguire i test prima
dell'aggiornamento dei file manualmente, ripensaci! Con ogni compito manuale in
più che devi eseguire per pubblicare una nuova versione della tua applicazione
aumentano le possibilità di un errore fatale. Che tu stia gestendo un semplice
aggiornamento, un processo di build completo o una strategia di integrazione
continua, l'[automazione dello sviluppo][buildautomation] è tua amica.

Tra i compiti che potresti voler automatizzare ci sono:

* Gestione delle dipendenze
* Compilazione e minificazione dei media
* Esecuzione dei test
* Creazione di documentazione
* Pacchettizzazione
* Pubblicazione

Questi strumenti possono essere descritti come un insieme di script che
gestiscono compiti comuni nella pubblicazione del software. Lo strumento non è
parte del tuo software, ma agisce su di esso da 'fuori'.

Ci sono molti strumenti open source disponibili per aiutarti con l'automazione
dello sviluppo, alcuni scritti in PHP, altri no. Questo non dovrebbe impedirti
di usarli, se sono più adatti per uno specifico compito. Ecco alcuni esempi.

[Phing] è il modo più facile per iniziare con la pubblicazione automatica nel
mondo di PHP. Con Phing puoi controllare la pacchettizazione, la pubblicazione o
il processo di testing tramite un semplice file XML. Phing (che è basato su
[Apache Ant]) fornisce un ricco set di compiti solitamente richiesti per
installare o aggiornare un'applicazione Web e può essere esteso con compiti
personalizzati, scritti in PHP.

[Capistrano] è un sistema per *programmatori medio-avanzati* per eseguire
comandi in modo strutturato e ripetibile su una o più macchine remote. È pre-
configurato per la pubblicazione di applicazioni Ruby on Rails, ma molti lo
usano per **pubblicare applicazioni PHP**. Il suo corretto utilizzo dipende
dalla conoscenza di Ruby e Rake.

Il post [PHP Deployment with Capistrano][phpdeploy_capistrano] sul blog di Dave
Gardner è un buon punto d'inizio per programmatori PHP interessati a Capistrano.

[Chef] è più di un framework di pubblicazione. È un framework di integrazione di
sistema molto potente scritto in Ruby che non solo pubblica la tua applicazione
ma può costruire l'intero ambiente server o macchina virtuale.

[Deployer] è uno strumento di pubblicazione scritto in PHP; è semplice e
[funzionale. Esegue i compiti in parallelo, supporta la pubblicazione atomica,
[mantiene la consistenza tra i server. Ha ricette per compiti comuni relativi a
[Symfony, Laravel, Zend Framework e Yii.

[Magallanes] è un altro strumento scritto in PHP con una semplice configurazione fatta in file YAML. Ha il supporto per più server e ambienti, distribuzione atomica e ha alcune attività integrate che puoi sfruttare per strumenti e framework comuni.

#### Risorse Chef per sviluppatori PHP

* [Serie in tre parti sulla pubblicazione di un'applicazione LAMP con Chef, Vagrant ed EC2][chef_vagrant_and_ec2]
* [Articolo sull'installazione e la configurazione di PHP 5.3 e PEAR con Chef][Chef_cookbook]
* [Video tutorial su Chef][Chef_tutorial] fatto da Opscode, i creatori di Chef
* 
#### Altre letture:

* [Automatizza il tuo progetto con Apache Ant][apache_ant_tutorial]
* [Distribuisci applicazioni PHP][deploying_php_applications] - libro a pagamento sulle migliori pratiche e strumenti per la distribuzione di PHP.

### Provisioning del server

La gestione e la configurazione dei server può essere un'attività ardua quando ci si trova di fronte a molti server. Esistono strumenti per gestire questo in modo da poter automatizzare la tua infrastruttura per assicurarti di avere i server giusti e che siano configurati correttamente. Spesso si integrano con i provider di hosting cloud più grandi (Amazon Web Services, Heroku, DigitalOcean, ecc.) per la gestione delle istanze, il che rende molto più semplice il ridimensionamento di un'applicazione.

[Ansible] è uno strumento che gestisce la tua infrastruttura tramite file YAML. È facile iniziare e può gestire applicazioni complesse e su larga scala. Esiste un'API per la gestione delle istanze cloud e può gestirle tramite un inventario dinamico utilizzando determinati strumenti.

[Puppet] è uno strumento che ha il proprio linguaggio e tipi di file per la gestione di server e configurazioni. Può essere utilizzato in una configurazione master/client o può essere utilizzato in modalità "master-less". Nella modalità master/client i client interrogheranno i master centrali per una nuova configurazione a intervalli prestabiliti e si aggiorneranno se necessario. Nella modalità master-less puoi eseguire il push delle modifiche ai tuoi nodi.

[Chef] è un potente framework di integrazione di sistema basato su Ruby con cui puoi creare l'intero ambiente server o scatole virtuali. Si integra bene con Amazon Web Services attraverso il loro servizio chiamato OpsWorks.

#### Altre letture:

* [An Ansible Tutorial][an_ansible_tutorial]
* [Ansible for DevOps][ansible_for_devops] - paid book on everything Ansible
* [Ansible for AWS][ansible_for_aws] - paid book on integrating Ansible and Amazon Web Services
* [Three part blog series about deploying a LAMP application with Chef, Vagrant, and EC2][chef_vagrant_and_ec2]
* [Chef Cookbook which installs and configures PHP and the PEAR package management system][Chef_cookbook]
* [Chef video tutorial series][Chef_tutorial]

### Integrazione continua

> L'integrazione continua è una pratica di sviluppo software in cui i membri di un team integrano frequentemente il proprio lavoro,
> di solito ogni persona si integra almeno quotidianamente, portando a più integrazioni al giorno. Molte squadre trovano che questo
> approccio porta a problemi di integrazione significativamente ridotti e consente a un team di sviluppare un software più coeso
> rapidamente.

*-- Martin Fowler*

Esistono diversi modi per implementare l'integrazione continua per PHP. [Travis CI] ha fatto un ottimo lavoro
rendere l'integrazione continua una realtà anche per piccoli progetti. Travis CI è un servizio di integrazione continua in hosting
per la comunità open source. È integrato con GitHub e offre un supporto di prima classe per molte lingue tra cui
PHP.

#### Altre letture:

* [Integrazione continua con Jenkins][Jenkins]
* [Integrazione continua con PHPCI][PHPCI]
* [Integrazione continua con Teamcity][Teamcity]
* [Continuous Integration with PHP Censor][PHP Censor]

[buildautomation]: https://wikipedia.org/wiki/Build_automation
[Phing]: https://www.phing.info/
[Apache Ant]: https://ant.apache.org/
[Capistrano]: https://capistranorb.com/
[Ansistrano]: https://ansistrano.com
[phpdeploy_deployer]: https://www.sitepoint.com/deploying-php-applications-with-deployer/
[Chef]: https://www.chef.io/
[chef_vagrant_and_ec2]: https://web.archive.org/web/20190307220000/http://www.jasongrimes.org/2012/06/managing-lamp-environments-with-chef-vagrant-and-ec2-1-of-3/
[Chef_cookbook]: https://github.com/chef-cookbooks/php
[Chef_tutorial]: https://www.youtube.com/playlist?list=PL11cZfNdwNyNYcpntVe6js-prb80LBZuc
[apache_ant_tutorial]: https://code.tutsplus.com/tutorials/automate-your-projects-with-apache-ant--net-18595
[Travis CI]: https://www.travis-ci.com/
[Jenkins]: https://jenkins.io/
[PHPCI]: https://github.com/dancryer/phpci
[PHP Censor]: https://github.com/php-censor/php-censor
[Teamcity]: https://www.jetbrains.com/teamcity/
[Deployer]: https://deployer.org/
[Rocketeer]: http://rocketeer.autopergamene.eu/
[Magallanes]: https://www.magephp.com/
[deploying_php_applications]: https://deployingphpapplications.com/
[Ansible]: https://www.ansible.com/
[Puppet]: https://puppet.com/
[ansible_for_devops]: https://leanpub.com/ansible-for-devops
[ansible_for_aws]: https://leanpub.com/ansible-for-aws
[an_ansible_tutorial]: https://serversforhackers.com/an-ansible-tutorial
