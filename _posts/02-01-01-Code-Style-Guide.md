---
anchor: code_style_guide
title:  Stile di codifica
---

# Stile di codifica {#code_style_guide_title}

La comunità PHP è grande e diversificata, fatta di innumerevoli librerie,
framework e componenti. È comune per gli sviluppatori PHP scegliere diversi di
questi e combinarli in un singolo progetto. È importante che il codice PHP
aderisca (il più fedelmente possibile) a uno stile di codifica comune per
rendere facile agli sviluppatori mischiare e usare diverse librerie nei loro
progetti.

Il [Framework Interop Group][fig] ha proposto e approvato una serie di
raccomandazioni di stile. Non tutte riguardano lo stile del codice, ma quelle
che lo riguardano sono note come [PSR-1][psr1], [PSR-12][psr12] and [PSR-4][psr4]. Queste raccomandazioni non sono che una lista di regole che
alcuni progetti come Drupal, Zend, Symfony, CakePHP, phpBB, AWS SDK, FuelPHP,
Lithium etc. iniziano ad adottare. Puoi usarle in uno dei tuoi progetti,
o continuare a usare il tuo stile personale.

Idealmente dovresti scrivere codice PHP che aderisce ad uno standard noto. Può
essere una qualunque combinazione dei PSR o uno degli standard di codifica di
PHP o Zend. Questo significa che altri sviluppatori potranno facilmente leggere
e lavorare col tuo codice, e le applicazioni che implementano i componenti
potranno essere consistenti anche quando lavorano con molto codice di terze
parti.

* [Read about PSR-1][psr1]
* [Read about PSR-12][psr12]
* [Read about PSR-4][psr4]
* [Read about PEAR Coding Standards][pear-cs]
* [Read about Symfony Coding Standards][symfony-cs]

Puoi usare [PHP_CodeSniffer][phpcs] per controllare che il codice rispetti
queste raccomandazioni, e plugin per editor di testo come
[Sublime Text 2][st-cs] per avere feedback in tempo reale.

Puoi anche aggiustare automaticamente il layout del codice utilizzando uno di questi tools:

- [PHP Coding Standards Fixer][phpcsfixer] di
  Fabien Potencier, testato scrupolosamente. È più grande e più lento, ma molto
  stabile e usato da alcuni grandi progetti come Magento e Symfony.
- Also, the [PHP Code Beautifier and Fixer][phpcbf] tool incluso con PHP_CodeSniffer in grado di aggiustare il tuo codice.

Puoi eseguire phpcs manualmente da shell:

    phpcs -sw --standard=PSR1 file.php

Ti mostrerà gli errori insieme ad una descrizione su come aggiustarli.
Può anche essere un ottimo tool per eseguire operazioni automatizzate all'interno degli hooks di Git.
In questo modo qualsiasi commit che contiene delle violazioni riguardo gli standard non potrà essere committato fino a quando questi
 errori non verranno aggiustati.

Se hai PHP_CodeSniffer, puoi aggiustare il codice automaticamente con [PHP Code Beautifier and Fixer][phpcbf].

    phpcbf -w --standard=PSR1 file.php

Un'altra opzione è quella di usare [PHP Coding Standards Fixer][phpcsfixer].
Ti mostrerà che tipo di errori sono presenti prima di aggiustarli automaticamente.

    php-cs-fixer fix -v --rules=@PSR1 file.php

La lingua inglese è preferita per tutti i nomi di simboli e infrastrutture del
codice. I commenti possono essere scritti in qualunque lingua facilmente
comprensibile da tutte le parti presenti e future che dovranno lavorare sul
codice.

Infine, una buona risorsa supplementare per scrivere del buon codice è [Clean Code PHP][cleancode].

[fig]: https://www.php-fig.org/
[psr1]: https://www.php-fig.org/psr/psr-1/
[psr12]: https://www.php-fig.org/psr/psr-12/
[psr4]: https://www.php-fig.org/psr/psr-4/
[pear-cs]: https://pear.php.net/manual/en/standards.php
[symfony-cs]: https://symfony.com/doc/current/contributing/code/standards.html
[phpcs]: https://pear.php.net/package/PHP_CodeSniffer/
[phpcbf]: https://github.com/squizlabs/PHP_CodeSniffer/wiki/Fixing-Errors-Automatically
[st-cs]: https://github.com/benmatselby/sublime-phpcs
[phpcsfixer]: https://cs.symfony.com/
[cleancode]: https://github.com/jupeter/clean-code-php
