---
type: Document
title: Chapitre 1
---

<span style="color:#3b82f6; font-size:1.3em; font-weight:bold;">Résumé — Chapitre 1 : Security Today</span>

<br>
<span style="font-weight:bold;">Spring Security en deux mots</span>
<br>
Spring Security est le framework de référence pour sécuriser les applications Spring. Il fournit des mécanismes d'<span style="font-weight:bold;">authentification</span>, d'<span style="font-weight:bold;">autorisation</span> et de protection contre les attaques courantes, de façon configurable et intégrable dans le style Spring habituel (annotations, beans, SpEL). Il ne sécurise pas magiquement une application — c'est au développeur de le configurer correctement.
<br>
<span style="font-weight:bold;">La sécurité applicative : de quoi parle-t-on ?</span>
La sécurité est une <span style="font-weight:bold;">préoccupation transversale</span> qui s'applique en couches (réseau, infrastructure, application…). Spring Security intervient à la couche applicative, qui couvre notamment :

<span style="color:#3b82f6;">■</span> <span style="font-weight:bold;">L'authentification</span> : identifier qui fait une requête
<span style="color:#3b82f6;">■</span> <span style="font-weight:bold;">L'autorisation</span> : décider ce que cette entité a le droit de faire
<span style="color:#3b82f6;">■</span> <span style="font-weight:bold;">La protection des données au repos</span> : chiffrement, hachage, gestion des secrets
<span style="color:#3b82f6;">■</span> <span style="font-weight:bold;">La sécurité des données en transit</span> : validation et intégrité des communications entre composants

<br><br>

<span style="font-weight:bold;">Pourquoi c'est important ?</span>

Le chapitre insiste sur un point pragmatique : <span style="font-weight:bold;">le coût d'une attaque dépasse presque toujours le coût de la prévention</span>. Les conséquences peuvent être financières, réputationnelles, voire juridiques (RGPD). Même une petite faille — un CSRF mal géré, une absence de contrôle d'accès sur une méthode — peut avoir des effets catastrophiques.

<br>

<span style="font-weight:bold;">Ce que le livre abordera</span>
<span style="color:#3b82f6;">■</span> Architecture et composants de Spring Security
<span style="color:#3b82f6;">■</span> Authentification/autorisation, OAuth 2, OpenID Connect
<span style="color:#3b82f6;">■</span> Sécurité multi-couches (y compris <em>method security</em>)
<span style="color:#3b82f6;">■</span> Applications réactives
<span style="color:#3b82f6;">■</span> Tests des implémentations de sécurité

<br><br>

<div style="border-left: 3px solid #3b82f6; padding-left: 10px; color: #555;">
Ce chapitre introductif pose surtout le <em>pourquoi</em> avant le <em>comment</em> : sécuriser n'est pas optionnel, c'est une responsabilité du développeur dès le début du projet, et Spring Security est l'outil Spring pour y répondre.
</div>