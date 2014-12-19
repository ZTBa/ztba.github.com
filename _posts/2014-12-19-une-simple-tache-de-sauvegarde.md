---
layout: post
title: "une simple tache de sauvegarde"
description: ""
category: skills
tags: [windows,skill]
---
{% include JB/setup %}

On me demande de sauvegarder un repertoire partagé dans notre réseau, sur une disque dur externe.

Je le cosidere comme une tache très simple, mais je me suis trompe.

J'utilise d'abord `KillCopy` pour transferer les données, c'est un outil légé et rapide.  Il peut afficher le vitesse
en temps réel, je l'aime bien.
10 miniutes plus tard, le transfert n'avance plus, une erreur de "nom de fichier trops long" s'apparait.
Eventuellement c'est la limite de 255 charactaire pour les noms de fichier (chemin inclue) depuis windows 98.
Apres quelque recherche sur Google, je décide de changer l'outil de copier pour dépasse cette limite,
Le sauvaur: `FastCopy`.

Mais ce n'est pas fini. La tache s'arrete 4 heures plus tard, avec une erreur de ''espace disque insufisant".
je sais que ce n'est pas vrais: la disque dur est tout neuf et sa capacité est bouble que la taille de repertoire.
je trouve qu'elle en trains de copier un gros fichier de base d'outlook (.pst) à taille de 5Go. Ca me pense
tout suite la limite de Fat32.

La vérification confirme mon hypothese.

J'ai obligé de convertir le format de Fat32 à NTFS

  > lancer 'Invite de Commandes' en tant que d'administrateur  
  > taper `convert f: /fs:ntfs`

La durée de conversation est 5 minutes environ, je peut finalement démarrer ma tache de sauvegarde.
