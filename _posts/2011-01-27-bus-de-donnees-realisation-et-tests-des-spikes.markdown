---
layout: post
status: publish
published: true
title: Bus de données, réalisation et tests des spikes
author:
  display_name: Bruno Thomas
  login: bruno
  email: bruno@barreverte.fr
  url: http://www.barreverte.fr
author_login: bruno
author_email: bruno@barreverte.fr
author_url: http://www.barreverte.fr
excerpt: ! "Nous avons choisi d'évaluer les solutions suivantes :\r\n<ol>\r\n\t<li><a
  href=\"http://activemq.apache.org/\">ActiveMQ</a>, broker JMS apache</li>\r\n\t<li><a
  href=\"http://www.rabbitmq.com/\">RabbitMQ</a>, broker AMQP en erlang</li>\r\n\t<li><a
  href=\"http://qpid.apache.org/\">Qpid</a>, autre boker AMQP apache</li>\r\n</ol>\r\nNos
  spikes doivent être fonctionnels, et pouvoir être testés en charge, avec une architecture
  assez proche de ce que nous avons en production. Comme le service est un service
  web, et que les statistiques sont émises dans des servlets, nous choisissons de
  réaliser une petite servlet qui envoie un objet représentant une statistique sur
  le bus de données, et un consommateur qui reçoit cet objet.\r\n"
wordpress_id: 938
wordpress_url: /?p=938
date: !binary |-
  MjAxMS0wMS0yNyAyMToxODowNyArMDEwMA==
date_gmt: !binary |-
  MjAxMS0wMS0yNyAyMDoxODowNyArMDEwMA==
categories:
- agilité
- java
- tests
tags:
- bus de données
comments:
- id: 58
  author: Antoine Contal
  author_email: antoine.contal@gmail.com
  author_url: ''
  date: !binary |-
    MjAxMS0wMi0yMCAxODoyMzo0MyArMDEwMA==
  date_gmt: !binary |-
    MjAxMS0wMi0yMCAxNzoyMzo0MyArMDEwMA==
  content: Bel exemple de <a href="http://xp123.com/articles/set-based-concurrent-engineering/"
    rel="nofollow">set-based concurrent design</a>.
- id: 82
  author: Conférence Agile France 2011 | Barre Verte !
  author_email: ''
  author_url: /conference-agile-france-2011
  date: !binary |-
    MjAxMS0wNS0xNSAxNjoxNToxMCArMDIwMA==
  date_gmt: !binary |-
    MjAxMS0wNS0xNSAxNToxNToxMCArMDIwMA==
  content: ! '[...] Bus de données, réalisation et tests des spikes [...]'
---
<p>Nous avons choisi d'évaluer les solutions suivantes :</p>
<ol>
<li><a href="http://activemq.apache.org/">ActiveMQ</a>, broker JMS apache</li>
<li><a href="http://www.rabbitmq.com/">RabbitMQ</a>, broker AMQP en erlang</li>
<li><a href="http://qpid.apache.org/">Qpid</a>, autre boker AMQP apache</li>
</ol>
<p>Nos spikes doivent être fonctionnels, et pouvoir être testés en charge, avec une architecture assez proche de ce que nous avons en production. Comme le service est un service web, et que les statistiques sont émises dans des servlets, nous choisissons de réaliser une petite servlet qui envoie un objet représentant une statistique sur le bus de données, et un consommateur qui reçoit cet objet.<br />
<a id="more"></a><a id="more-938"></a><br />
Voici l'architecture de la plateforme de bench nous servant à tester nos spikes :</p>
<p style="text-align: center;">
<a href="/images/bench_stats.png"><img class="size-medium wp-image-1321 aligncenter" title="Architecture des tests de charge" src="/images/bench_stats-279x300.png" alt="Architecture des tests de charge" width="279" height="300" /></a></p>
<p>Nous avons défini des catégories de critères :</p>
<ul>
<li>fonctionnels : par exemple, pas d'impact sur les performances des AS, pas de perte de messages, ne pas avoir des messages traités 2 fois même avec plusieurs consommateurs, un temps d'insertion en base inférieur à 10s</li>
<li>tenue en charge : 10 Millions de messages par jour, environ 120 messages par seconde</li>
<li>techniques : monitoring, logging, installation, facilité d'utilisation des API, adhérence et intrusivité dans le code, qualité de la documentation</li>
</ul>
<p>Pour chaque solution, nous cheminons de manière incrémentale :</p>
<ul>
<li>installer les solutions sur nos poste pour pouvoir les utiliser en local</li>
<li>d'abord implémenter un petit programme producteur/consommateur (deux classes main java) et voir si l'objet représentant un message de statistique qui contient des attributs de différents types est bien reçu et décodé</li>
<li>ensuite coder une servlet qui va utiliser le producteur et envoyer un message en dur dans sa méthode doGet. Cette étape nous permet de voir comment utiliser l'API d'envoi de message dans un contexte multithreadé. Nous avons déjà du faire un peu de design pour pouvoir nous adapter au contexte d'un container de servlet</li>
<li>déployer la solution en plateforme d'intégration, avec une seule machine de bus, en travaillant avec l'équipe système. Cette étape nous permet d'évaluer l'exploitabilité de chacune des solutions</li>
<li>déployer la solution en plateforme de bench, avec un cluster de deux machines de bus, toujours avec l'équipe système. Nous pouvons évaluer la difficulté de mise en œuvre du cluster</li>
<li>faire une petite campagne de bench, afin d'avoir des tirs reproductibles que nous savons expliquer.</li>
<li>finaliser la synthèse de toutes les observations recueillies au fil de l'eau pour chaque solution, et capitaliser les connaissances acquises : configurations, contournements éventuels, lecture des logs applicatives, etc.</li>
</ul>
<p>Nous avons sous-traité la solution Qpid à une autre équipe, car nous ne pouvions pas réaliser les trois spikes dans le temps imparti. Assez rapidement, le développeur qui utilisait Qpid nous a remonté des difficultés importantes de stabilité du broker : régressions entre différentes versions, difficultés de mise en oeuvre. Nous avons décidé en équipe d'abandonner Qpid.</p>
<p>Restaient RabbitMQ et ActiveMQ. Deux parties de l'équipe ont réalisé de manière simultanée ces spikes et cela à amené une compétition saine entre les différentes parties prenantes de chacune des solutions. Régulièrement, lors de nos "dailies", nous échangions autour des difficultés rencontrées par chacune des équipes. Nous avons souhaité arriver au terme des tests de charge dans des conditions similaires, pour pouvoir comparer le plus objectivement possible. Il y avait du challenge, mais aussi de l'entraide.</p>
<p>Après deux semaines de tests, nous sommes arrivé au terme de notre campagne de spike avec le tableau de synthèse suivant :</p>
<table class="comparatif">
<tbody>
<tr>
<td>+/-</td>
<td>ActiveMQ</td>
<td>+/-</td>
<td>RabbitMQ</td>
</tr>
<tr>
<td>+</td>
<td>installation plus simple</td>
<td>-</td>
<td>RabbitMQ installé à partir tar.gz car .deb dans testing</td>
</tr>
<tr>
<td>+</td>
<td>paramétrage plus complet</td>
<td>+</td>
<td>paramétrage plus simple</td>
</tr>
<tr>
<td>+</td>
<td>monitoring JMX</td>
<td></td>
<td>monitoring peut être scripté sur rabbitmqctl ou par le plugin SNMP</td>
</tr>
<tr>
<td>+</td>
<td>logs java par log4j</td>
<td>-</td>
<td>logs local7 à paramétrer/développer</td>
</tr>
<tr>
<td>+</td>
<td>docs + complète</td>
<td>-</td>
<td>docs moins complète</td>
</tr>
<tr>
<td>-</td>
<td>comportement en bench difficile à expliquer, plus de temps passé sur les tirs pour arriver au résultat. Suivant la configuration, influence des consommateurs sur les producteurs</td>
<td>++</td>
<td>bench prédictibles et expliquables</td>
</tr>
<tr>
<td>-</td>
<td>nombre de lignes de code pour implémenter le spike + important (559 lignes java, 478 lignes XML)</td>
<td>+</td>
<td>nombre de lignes plus faible (347 lignes java)</td>
</tr>
<tr>
<td>-</td>
<td>couplage à une API/Framework : JMS activeMQ</td>
<td>+</td>
<td>AMQP : Advanced Message Queuing Protocol, protocole interopérable niveau réseau permet d'ouvrir l'API du bus</td>
</tr>
<tr>
<td>+</td>
<td>comportement fonctionnel en charge</td>
<td>++</td>
<td>asynchronisme obtenu sans efforts</td>
</tr>
<tr>
<td>+</td>
<td>utilisation répandue</td>
<td>+</td>
<td>utilisation répandue. Bâti sur erlang qui a été fait pour faire tourner des programmes sans interruption</td>
</tr>
<tr>
<td>-</td>
<td>nombre de lignes de code du produit activemq-core=158383 (SLOC activemq-parent-5.3.2)</td>
<td>+</td>
<td>cf <a href="http://www.rabbitmq.com/resources/RabbitMQ_FITEclub_2009.pdf">http://www.rabbitmq.com/resources/RabbitMQ_FITEclub_2009.pdf</a> : ~5000 lignes de code. Cela entraîne une réduction du risque de défauts</td>
</tr>
<tr>
<td></td>
<td></td>
<td>+</td>
<td>possibilité de modifier la configuration, et le code à chaud. Possibilité de se connecter à chaud sur la VM erlang et d'afficher les flux de données "en live". Pas nécessaire de modifier des fichiers de configuration, de redémarrer le service pour changer par exemple le niveau de log.</td>
</tr>
</tbody>
</table>
<p>Le choix est éclairé par les atteintes des objectifs fixés avant la campagne de prototypage, et la confiance de l'équipe dans la solution.</p>
<p>Nous avons organisé une réunion commune avec l'équipe système, durant laquelle nous avons parcouru les points positifs et négatifs de chacune des solutions du tableau ci-dessus. Puis nous avons décidé du choix ensemble. Comme nous avions 10 points positifs pour rabbitMQ contre 7 points positifs pour activeMQ, nous avons opté pour rabbitMQ. Il y a eu un <a href="http://www.darkcoding.net/behaviour/decider-protocol/">decider</a> et nous avons eu la plupart de l'équipe d'accord, quelques supports, et un non (dans l'équipe système).</p>
<p>Maintenant que le choix était fait, il fallait implémenter la solution et passer l'épreuve de la production : nous en parlerons dans d'autres posts à venir...</p>
