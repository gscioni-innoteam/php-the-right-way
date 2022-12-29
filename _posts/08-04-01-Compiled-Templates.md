---
isChild: true
title:   Template compilati
anchor:  compiled_templates
---

## Template compilati {#compiled_templates_title}

Nonostante PHP si sia evoluto fino a diventare un linguaggio maturo e orientato
agli oggetti, non [è migliorato molto][article_templating_engines] come
linguaggio di templating. I template compilati, come [Twig], [Brainy] o [Smarty]*,
sopperiscono a questa mancanza offrendo una nuova sintassi che è stato studiata
appositamente per il templating. Dall'escaping automatico all'ereditarietà,
passando per le strutture di controllo semplificate, i template compilati sono
disegnati per essere più semplici da scrivere, più puliti da leggere e più
sicuri da usare. I template compilati possono anche essere condivisi tra diversi
linguaggi, e [Mustache] ne è un buon esempio. Poiché questi template devono
essere compilati c'è un leggero impatto sulla performance, ma è minimo quando si
usa un sistema di caching appropriato.

**Nonostante Smarty offra l'escaping automatico, questa funzionalità NON è
abilitata di default.*

### Esempio semplice di template compilato

Usando la libreria [Twig].

{% highlight html+jinja %}
{% raw %}
{% include 'header.html' with {'title': 'Profilo utente'} %}

<h1>Profilo utente</h1>
<p>Ciao, {{ name }}</p>

{% include 'footer.html' %}
{% endraw %}
{% endhighlight %}

### Esempio di template compilato che usa l'ereditarietà

Usando la libreria [Twig].

{% highlight html+jinja %}
{% raw %}
// template.html

<html>
<head>
    <title>{% block title %}{% endblock %}</title>
</head>
<body>

<main>
    {% block content %}{% endblock %}
</main>

</body>
</html>
{% endraw %}
{% endhighlight %}

{% highlight html+jinja %}
{% raw %}
// user_profile.html

{% extends "template.html" %}

{% block title %}Profilo utente{% endblock %}
{% block content %}
<h1>Profilo utente</h1>
<p>Ciao, {{ name }}</p>
{% endblock %}
{% endraw %}
{% endhighlight %}

[article_templating_engines]: http://fabien.potencier.org/article/34/templating-engines-in-php
[Twig]: http://twig.sensiolabs.org/
[Brainy]: https://github.com/box/brainy
[Smarty]: http://www.smarty.net/
[Mustache]: http://mustache.github.io/
