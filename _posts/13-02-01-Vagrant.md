---
isChild: true
title:   Vagrant
anchor:  vagrant
---

## Vagrant {#vagrant_title}

[Vagrant] ti aiuta a costruire le tue macchine virtuali sfruttando
ambienti virtuali noti, e configura questi ambienti basandosi su un singolo file
di configurazione. Queste macchine possono essere configurate manualmente,
oppure puoi usare un software di "provisioning" come [Puppet] o
[Chef] per farlo al posto tuo. Eseguire il provisioning della macchina di
base è un ottimo modo per assicurarti che più macchine vengano configurate nella
stessa maniera e rimuove la necessità di mantenere complicate liste di comandi
d'installazione. Puoi anche "distruggere" la macchina e ricrearla senza troppi
passi manuali, rendendo così semplice creare un'installazione "fresca".

Vagrant crea delle cartelle per condividere il codice tra l'host e la tua
macchina virtuale, il che significa che puoi creare e modificare file sul tuo
host e poi eseguire il codice nella macchina virtuale.

### Un po' di aiuto

Se ti serve un picolo aiuto per iniziare a usare Vagrant ci sono dei servizi che
potrebbero essere utili:

- [Rove]: un servizio che ti permette di pre-generare delle build Vagrant
  tipiche (PHP tra le varie opzioni). Il provisioning è effettuato con Chef.

- [Puphpet]: una semplice GUI per configurare macchine virtuali per lo sviluppo
  PHP. **Fortemente concentrato su PHP**. Oltre alle macchine virtuali locali,
  può anche essere usato per configurare servizi cloud. Il provisioning è
  effettuato con Puppet.

- [Protobox]: è un livello costruito su Vagrant con una GUI web per configurare
  macchine virtuali per lo sviluppo web. Un singolo documento YAML controlla
  tutto ciò che viene installato sulla macchina virtuale.

- [Phansible]: fornisce un'interfaccia di semplice utilizzo che aiuta a generare
  Playbook Ansible per progetti basati su PHP.

[Vagrant]: http://vagrantup.com/
[Puppet]: http://www.puppetlabs.com/
[Chef]: http://www.opscode.com/
[Rove]: http://rove.io/
[Puphpet]: https://puphpet.com/
[Protobox]: http://getprotobox.com/
[Phansible]: http://phansible.com/
