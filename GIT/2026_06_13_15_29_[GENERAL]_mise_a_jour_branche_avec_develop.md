---
type: Document
title: Mise A Jour Branche Avec Develop
---

# Mise a jour branche avec develop

2 possibilités :

1) Avec git pull
<pour le moment je suis dans ma branche> -> je switche sur develop
git checkout develop
git pull --rebase origin develop 
git checkout <mabranche>
git rebase develop

2) Avec Git fetch origin

-> pour le moment je suis dans ma branche et je reste dans ma branche
git fetch origin
git rebase origin/develop

Explication :
git pull --rebase origin develop   -> fetch + rebase en une commande
Equivalent à 
git fetch origin                   -> fetch explicite
git rebase origin/develop          -> rebase sur la ref distante

origin/develop : correspond à ma copie locale de develop en remote, git fetch origin met à jour la copie locale de develop "remote" depuis le remote


<div style="background:#eff6ff;border-left:4px solid #3b82f6;color:#1e3a5f;padding:1rem 1.25rem;border-radius:0.375rem;margin:1rem 0;">
Git fonctionne entièrement en local — toutes les opérations comme rebase, merge, diff etc. travaillent exclusivement sur les fichiers de ton dépôt local (dans .git/). Il n'y a aucune opération qui va lire directement sur le remote à la volée.
Le fetch est donc l'étape obligatoire pour "rapatrier" les infos du remote avant de pouvoir travailler dessus. C'est une contrainte de conception de Git — contrairement à des outils comme SVN qui pouvaient travailler directement avec le serveur, Git est fondamentalement un système décentralisé où chaque dépôt est autonome et complet.
</div>