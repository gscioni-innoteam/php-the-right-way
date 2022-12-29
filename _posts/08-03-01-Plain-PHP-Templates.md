---
title:   Template PHP semplici
isChild: true
anchor:  plain_php_templates
---

## Template PHP semplici {#plain_php_templates_title}

I template scritti in PHP sono semplicemente dei template che usano codice PHP
nativo. Sono una scelta naturale dato che PHP stesso è un linguaggio per i
template. Questo significa semplicemente che puoi includere codice PHP in altro
codice, come l'HTML. Si tratta di un beneficio per gli sviluppatori PHP perché
non c'è nessuna nuova sintassi da imparare, conoscono le funzioni disponibili, e
i loro editor PHP hanno già l'evidenziazione della sintassi e
l'autocompletamento integrati. Inoltre, i template PHP sono generalmente molto
veloci perché non richiedono compilazione.

Ogni framework PHP moderno utilizza qualche tipo di sistema di template, la
maggior parte dei quali usano PHP di default. Al di là dei framework, librerie
come [Plates][plates] o [Aura.View][aura] semplificano il lavoro con i template
PHP offrendo funzionalità di templating moderne come l'ereditarietà, i layout e
le estensioni.

### Esempio semplice di template PHP

Usando la libreria [Plates][plates].

{% highlight php %}
<?php // user_profile.php ?>

<?php $this->insert('header', ['title' => 'Profilo utente']) ?>

<h1>Profilo utente</h1>
<p>Ciao, <?=$this->escape($name)?></p>

<?php $this->insert('footer') ?>
{% endhighlight %}

### Esempio di template PHP con ereditarietà

Usando la libreria [Plates][plates].

{% highlight php %}
<?php // template.php ?>

<html>
<head>
    <title><?=$title?></title>
</head>
<body>

<main>
    <?=$this->section('content')?>
</main>

</body>
</html>
{% endhighlight %}

{% highlight php %}
<?php // user_profile.php ?>

<?php $this->layout('template', ['title' => 'Profilo utente']) ?>

<h1>Profilo utente</h1>
<p>Ciao, <?=$this->escape($name)?></p>
{% endhighlight %}

[plates]: http://platesphp.com/
[aura]: https://github.com/auraphp/Aura.View
