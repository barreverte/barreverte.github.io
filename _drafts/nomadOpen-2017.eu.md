---
layout: post
title: Nomad Open EU 2017
author:
  display_name: Philippe Blayo
tags:
- nomade
- conférences
- train
comments: true
---

La première non-conférence européenne sur le nomadisme professionel démarrait ce jeudi 17 Août.

# Le principe : faire l'expérience du nomadisme

L'idée est de vivre le sujet en travaillant dans les trains entre les différentes étapes. Le but à très long terme serait un jour de parvenir à pratiquer son métier sur des trajets longue distance comme le transibérien. En attendant cette première année est placée sous le signe de la modestie pour donner un aperçu du concept.

# La préparation : sans internet point de salut

J'anticipais que ma capacité à me connecter depuis mon ordinateur portable, même de manière très ponctuelle, serait critique. La raison : docker refuse de démarrer sans connection internet. Sans environnment docker je suis aveugle : je ne peux faire aucun test que ce soit manuel ou automatique. Même si programmer en aveugle sous vim est possible ce serait très limitant.

---
Il s'avèrera qu'il s'agit d'un problème spécifique à certaines versions de docker sous Ubuntu.
La première ligne éxecutée par le fichier `/etc/init/docker.conf` est

---
start on (filesystem and net-device-up IFACE!=lo)
---

Docker ne démarre pas si l'interface montée pour le réseau est la loopback !
Il suffit d'[inverser la condition](https://superuser.com/questions/918439/cannot-run-docker-without-connection) pour démarrer sans réseau :

---
start on (filesystem and net-device-up IFACE=lo)
---


---


Pour me préparer, je tente donc de me connecter à mon smartphone en mode hostspot. Je m'attends à une simple formalité mais surprise : mon téléphone n'apparaît dans aucun des réseaux wifi listés. Je tente sans trop y croire "Connect to Hidden Wi-Fi network..." (se connecter à un réseaux Wi-Fi caché). Bien entendu la première chose qui m'es demandée est le nom du point d'accès. Je l'obtiens sur mon téléphone par "Régl pt d'accès mobile". Je suis content de constater qu'un mot de passe a déjà été positionné par le constructeur. En le rendant visible je peux le recopier et je me retrouve connecté !

Mes batteries gonflées à bloc, je suis maintenant prêt à partir.


# Les premières difficultés

Mon activité se concentre rapidement sur comprendre pourquoi le câblage de mon nouveau component Vue.js ne se fait pas correctement. J'appelle mon binôme et les difficultés commencent. Nous avons vite besoin de copier-coller du code mais ma connection via hotspot sur mon Sony xperia s'avère extrèmement lente sur mon ordinateur. En fait une requête instantannée sur mon téléphone s'achève en timeout sur mon ordinateur.


# Ce que j'ai appris

* hotspot pourrit => cable pour partager
    * mon téléphone pompe plus vite l'électricité à mon ordinateur qu'une prise de courant ne le recharge
* la voix prend le pas sur la data : 4G se dégrade en 3G (H+)
* [appear.in](https://appear.in/) 
    * Une session de pair programming de 3h sur appear.in représente 2 Go


# Les discussions informelles

* la sinusoïde
* openfisca comme preuve de réplication des communs numériques autre que wikipédia et openstreetmap
* sublimetext : le multicurseur
* gemba = écran + cerveau du développeur
* matti = je vais plus vite à chercher qu'à poser la question
* quand bloqué : prendre 2h pour élaguer un problème afin d'en extraire une question sur stackoverflow
    * valeur de se répondre soi-même pour le futur
* les docs en local pour éviter les 800 ms de latence d'une recherche internet
    * repo github en local


# La rencontre des communautés locales

Un voyageur milanais me disait l'année dernière que la différence entre un touriste et un voyageur résidait dans la préparation de son séjour en amont pour vivre chaque étape comme un habitant le ferait.

Okiwi la veille => wine blockchain
courbe de spéculation des bitcoin et telurium

## Une conférence dans un bateau

<img class="right" alt="Une conférence en bateau" src="/images/nomadcruise_small.jpg"/>Un membre Espagnol de la communauté locale nous dit qu'il a entendu parlé d'une autre conférence qui se déplace : nomad cruise. Elle se déroule sur un bateau où présentations et ateliers s'égrennent le long de sa traversée de l'Atlantique.


# Du coworking dans une salle de bain

<img class="right" alt="coworking en pair programming à distance depuis la salle de bain" src="/images/coworking_salle_de_bain.jpg">L'une de mes découvertes : il est très important pour moi de ne pas déranger des gens alentour lorsque je suis au téléphone. Quand les deux pièces principales se sont révélées utilisées j'ai donc déplacé mon espace de travail dans la salle de bain pour faire une session de pair programming à distance.

