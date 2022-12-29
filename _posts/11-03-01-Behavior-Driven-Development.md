---
isChild: true
title:   Behavior Driven Development
anchor:  behavior_driven_development
---

## Behavior Driven Development {#behavior_driven_development_title}

Ci sono due differenti tipi di Behavior-Driven Development (BDD): SpecBDD e
StoryBDD. Lo SpecBDD si concentra sul comportamento tecnico del codice, mentre
lo StoryBDD si concentra sulle interazioni. PHP ha dei framework per entrambi i
tipi di BDD.

Con lo StoryBDD, scrivi delle storie leggibili da esseri umani che descrivono il
comportamento della tua applicazione. Queste storia possono poi essere eseguite
come veri e propri test per la tua applicazione. Il framework usato nelle
applicazioni PHP per lo StoryBDD è Behat, che si ispira al progetto Ruby
[Cucumber] e implementa il DSL Gherkin per la descrizione delle funzionalità.

Con lo SpecBDD, scrivi delle specifiche che descrivono come il tuo codice
dovrebbe funzionare. Invece di testare una funzione o un metodo, descrivi come
quella funzione o metodo dovrebbero comportarsi. PHP offre il framework PHPSpec
per questo scopo. Questo framework si ispira al progetto Ruby [RSpec].

### Link sul BDD

* [Behat], il framework StoryBDD per PHP, ispirato al progetto Ruby [Cucumber];
* [PHPSpec], il framework SpecBDD per PHP, ispirato al progetto Ruby [RSpec];
* [Codeception] è un framework di testing completo che adotta i principi BDD;

[Behat]: http://behat.org/
[Cucumber]: http://cukes.info/
[PHPSpec]: http://www.phpspec.net/
[RSpec]: http://rspec.info/
[Codeception]: http://codeception.com/
