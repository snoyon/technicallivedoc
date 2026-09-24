---
type: Document
title: Chapitre 9
---

<div style="font-family: Arial, sans-serif; margin: 0 auto; color: #1f2937; line-height: 1.6;">

<div style="font-size: 1.4rem; font-weight: bold; color: #1e40af; margin-bottom: 1rem;">Chapitre 9 — Configuration de la protection CSRF</div>

<div style="margin-top: 0.5rem;">
Ce chapitre explique le fonctionnement de la protection CSRF (cross-site request forgery) dans Spring Security, activée par défaut, et pourquoi les endpoints appelés jusqu'ici en POST nécessitaient de désactiver cette protection. Il couvre le mécanisme du filtre <span style="color: #1d4ed8; font-family: monospace;">CsrfFilter</span>, son usage dans des scénarios concrets avec formulaires, puis sa personnalisation (exclusion de chemins, gestion custom des tokens).
</div>

<div style="font-size: 1.1rem; font-weight: bold; color: #1e40af; margin-top: 1.5rem;">9.1 Comment fonctionne la protection CSRF dans Spring Security</div>

<div style="margin-top: 0.5rem;">
Une attaque CSRF suppose que l'utilisateur est déjà authentifié sur une application web. L'attaquant le piège pour qu'il ouvre une page contenant un script malveillant, qui exécute alors des actions sur l'application ciblée en se faisant passer pour l'utilisateur (le serveur fait confiance à la session active).
</div>

<div style="margin-top: 0.5rem;">
La protection vise à garantir que seul le frontend légitime de l'application peut réaliser des opérations mutantes (toute méthode HTTP autre que <span style="color: #1d4ed8; font-family: monospace;">GET</span>, <span style="color: #1d4ed8; font-family: monospace;">HEAD</span>, <span style="color: #1d4ed8; font-family: monospace;">TRACE</span>, <span style="color: #1d4ed8; font-family: monospace;">OPTIONS</span>). Le principe :
</div>

<div style="margin-left: 1rem; margin-top: 0.5rem;">
• Un premier appel GET génère un token CSRF unique côté serveur<br>
• Toute requête mutante (POST, PUT, DELETE…) doit inclure ce token dans un en-tête<br>
• Sans token valide, la requête est rejetée avec un statut <span style="color: #1d4ed8; font-family: monospace;">403 Forbidden</span>
</div>

<div style="margin-top: 0.5rem;">
Le composant central est <span style="color: #1d4ed8; font-family: monospace;">CsrfFilter</span>, un filtre de la chaîne qui laisse passer les méthodes sûres et exige le header token pour les autres. Il s'appuie sur un <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRepository</span> chargé de générer, stocker et invalider les tokens — par défaut stockés en session HTTP, sous forme de chaînes aléatoires.
</div>

<div style="margin-top: 0.5rem;">
Le token généré est déposé dans l'attribut de requête <span style="color: #1d4ed8; font-family: monospace;">_csrf</span> (instance de <span style="color: #1d4ed8; font-family: monospace;">CsrfToken</span>, méthode <span style="color: #1d4ed8; font-family: monospace;">getToken()</span>). Un exemple d'illustration utilise un filtre custom (<span style="color: #1d4ed8; font-family: monospace;">CsrfTokenLogger</span>) ajouté après <span style="color: #1d4ed8; font-family: monospace;">CsrfFilter</span> pour logger le token en console, puis un appel <span style="color: #1d4ed8; font-family: monospace;">curl</span> en POST avec le header <span style="color: #1d4ed8; font-family: monospace;">X-CSRF-TOKEN</span> et le cookie <span style="color: #1d4ed8; font-family: monospace;">JSESSIONID</span> pour réussir la requête.
</div>

<div style="border-left: 3px solid #b45309; background: #fef3c7; color: #7c2d12; padding: 0.6rem 1rem; margin-top: 0.8rem; border-radius: 4px;">
En pratique, un client ne peut ni deviner ni lire le token dans les logs serveur : c'est à l'application de renvoyer le token dans la réponse HTTP pour que le client puisse l'utiliser (voir section 9.2).
</div>

<div style="font-size: 1.1rem; font-weight: bold; color: #1e40af; margin-top: 1.5rem;">9.2 Utiliser la protection CSRF dans des scénarios pratiques</div>

<div style="margin-top: 0.5rem;">
La protection CSRF est pertinente pour les applications web dans un navigateur, où le même serveur sert le frontend et le backend — typiquement une application Spring MVC classique. Elle ne convient pas quand le client est indépendant du backend (application mobile, frontend Angular/React/Vue séparé) ; ces cas seront traités via OAuth 2 dans la partie 4 du livre.
</div>

<div style="margin-top: 0.5rem;">
Le formulaire de login par défaut de Spring Security envoie déjà le token CSRF via un champ <span style="color: #1d4ed8; font-family: monospace;">input</span> caché — c'est pourquoi le login en POST fonctionne sans configuration explicite. Pour un formulaire personnalisé, il faut faire la même chose manuellement, par exemple avec Thymeleaf :
</div>

```java
<input type="hidden"
       th:name="${_csrf.parameterName}"
       th:value="${_csrf.token}" />
```

<div style="margin-top: 0.5rem;">
Sans ce champ, la soumission du formulaire déclenche un <span style="color: #1d4ed8; font-family: monospace;">403 Forbidden</span>. Avec le champ ajouté, la requête POST est acceptée et le contrôleur s'exécute normalement.
</div>

<div style="border-left: 3px solid #b45309; background: #fef3c7; color: #7c2d12; padding: 0.6rem 1rem; margin-top: 0.8rem; border-radius: 4px;">
Règle à ne jamais enfreindre : ne jamais implémenter d'opération mutante derrière un endpoint HTTP GET, car GET est exempté de vérification CSRF par conception.
</div>

<div style="font-size: 1.1rem; font-weight: bold; color: #1e40af; margin-top: 1.5rem;">9.3 Personnaliser la protection CSRF</div>

<div style="margin-top: 0.5rem;">
Deux axes de personnalisation courants : exclure certains chemins de la protection, et changer la façon dont les tokens sont gérés (stockage).
</div>

<div style="margin-top: 0.8rem; font-weight: bold; color: #1e3a5f;">Exclure des chemins</div>

<div style="margin-top: 0.3rem;">
Via l'objet <span style="color: #1d4ed8; font-family: monospace;">CsrfConfigurer</span> et sa méthode <span style="color: #1d4ed8; font-family: monospace;">ignoringRequestMatchers()</span>, on peut exclure un chemin par simple String, par <span style="color: #1d4ed8; font-family: monospace;">MvcRequestMatcher</span>, ou par <span style="color: #1d4ed8; font-family: monospace;">RegexRequestMatcher</span> pour des règles plus complexes (chemin + méthode HTTP).
</div>

```java
http.csrf(c -> {
    c.ignoringRequestMatchers("/ciao");
});
```

<div style="margin-top: 0.8rem; font-weight: bold; color: #1e3a5f;">Personnaliser la gestion des tokens</div>

<div style="margin-top: 0.3rem;">
Le stockage en session HTTP (par défaut) est stateful et limite le passage à l'échelle horizontal. Spring Security expose trois contrats à implémenter pour une gestion custom (ex. stockage en base de données) :
</div>

<div style="margin-left: 1rem; margin-top: 0.5rem;">
• <span style="color: #1d4ed8; font-family: monospace;">CsrfToken</span> — décrit le token lui-même (nom du header, nom de l'attribut, valeur)<br>
• <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRepository</span> — crée, stocke et charge les tokens (méthodes <span style="color: #1d4ed8; font-family: monospace;">generateToken()</span>, <span style="color: #1d4ed8; font-family: monospace;">saveToken()</span>, <span style="color: #1d4ed8; font-family: monospace;">loadToken()</span>)<br>
• <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRequestHandler</span> — gère le dépôt du token sur la requête HTTP
</div>

<div style="margin-top: 0.5rem;">
L'exemple du livre implémente un <span style="color: #1d4ed8; font-family: monospace;">CustomCsrfTokenRepository</span> basé sur JPA, où un identifiant client transmis via un header <span style="color: #1d4ed8; font-family: monospace;">X-IDENTIFIER</span> remplace l'ID de session pour associer chaque token à un client en base. Le token est ensuite envoyé via l'en-tête <span style="color: #1d4ed8; font-family: monospace;">X-CSRF-TOKEN</span>. Le handler par défaut fourni par le framework, <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRequestAttributeHandler</span>, peut être remplacé (l'implémentation par défaut réelle, <span style="color: #1d4ed8; font-family: monospace;">XorCsrfTokenRequestAttributeHandler</span>, applique en plus un XOR avec une valeur aléatoire pour renforcer la sécurité du token transmis).
</div>

<div style="border-left: 3px solid #1e40af; background: #dbeafe; color: #1e3a5f; padding: 0.6rem 1rem; margin-top: 0.8rem; border-radius: 4px;">
Une alternative à l'association par identifiant client est un token à durée de vie limitée (expiration), stocké en base sans lien direct à un utilisateur — il suffit de vérifier existence + non-expiration.
</div>

<div style="margin-top: 0.8rem; font-weight: bold; color: #1e3a5f;">Flow d'appel des méthodes pour un CsrfTokenRepository custom</div>

<div style="margin-top: 0.3rem;">
Voici le déroulé exact des appels quand tu branches un <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRepository</span> custom (comme le <span style="color: #1d4ed8; font-family: monospace;">CustomCsrfTokenRepository</span> du chapitre 9) :
</div>

<div style="margin-top: 0.6rem; font-weight: bold; color: #1e3a5f;">1. Requête GET (chargement de la page)</div>

<div style="margin-left: 1rem; margin-top: 0.3rem;">
• <span style="color: #1d4ed8; font-family: monospace;">CsrfFilter</span> intercepte la requête.<br>
• Il délègue au <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRequestHandler</span> (ex. <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRequestAttributeHandler</span>), qui appelle <span style="color: #1d4ed8; font-family: monospace;">loadToken()</span> sur ton repository pour voir si un token existe déjà pour ce client.<br>
• Si <span style="color: #1d4ed8; font-family: monospace;">loadToken()</span> renvoie <span style="color: #1d4ed8; font-family: monospace;">null</span> (aucun token trouvé, comme dans l'exemple où on cherche par <span style="color: #1d4ed8; font-family: monospace;">X-IDENTIFIER</span>), le filtre appelle <span style="color: #1d4ed8; font-family: monospace;">generateToken()</span> pour créer un nouveau <span style="color: #1d4ed8; font-family: monospace;">CsrfToken</span> (typiquement un <span style="color: #1d4ed8; font-family: monospace;">DefaultCsrfToken</span> avec un UUID).<br>
• Le <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRequestHandler</span> dépose ensuite ce token sur l'attribut <span style="color: #1d4ed8; font-family: monospace;">_csrf</span> de la requête (accessible par la vue, ex. via Thymeleaf).<br>
• <span style="color: #1d4ed8; font-family: monospace;">saveToken()</span> est appelé pour persister ce nouveau token (dans l'exemple : recherche si l'identifiant existe déjà en base, puis update ou insert).<br>
• La réponse part avec le token exposé (champ caché du formulaire, ou à récupérer côté client pour une future requête).
</div>

<div style="margin-top: 0.6rem; font-weight: bold; color: #1e3a5f;">2. Requête mutante (POST/PUT/DELETE)</div>

<div style="margin-left: 1rem; margin-top: 0.3rem;">
• <span style="color: #1d4ed8; font-family: monospace;">CsrfFilter</span> intercepte à nouveau la requête.<br>
• Il appelle <span style="color: #1d4ed8; font-family: monospace;">loadToken()</span> pour récupérer le token attendu côté serveur (dans l'exemple, via l'identifiant transmis dans <span style="color: #1d4ed8; font-family: monospace;">X-IDENTIFIER</span>).<br>
• Il compare cette valeur à celle envoyée par le client dans le header <span style="color: #1d4ed8; font-family: monospace;">X-CSRF-TOKEN</span> (ou le paramètre du formulaire).<br>
• Si les deux correspondent → la requête continue dans la chaîne de filtres. Sinon → <span style="color: #1d4ed8; font-family: monospace;">403 Forbidden</span>.
</div>

<div style="margin-top: 0.5rem;">
Donc l'ordre logique des méthodes du contrat <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRepository</span> est :
</div>

```java
loadToken() → (si absent) generateToken() → saveToken()   // au GET
loadToken() → comparaison avec le token reçu              // au POST/PUT/DELETE
```

<div style="border-left: 3px solid #1e40af; background: #dbeafe; color: #1e3a5f; padding: 0.6rem 1rem; margin-top: 0.8rem; border-radius: 4px;">
Le <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRequestHandler</span> est la pièce qui orchestre l'appel entre <span style="color: #1d4ed8; font-family: monospace;">CsrfFilter</span> et ton repository : c'est lui qui décide quand appeler <span style="color: #1d4ed8; font-family: monospace;">generateToken()</span>/<span style="color: #1d4ed8; font-family: monospace;">loadToken()</span> et comment exposer le résultat sur la requête. C'est aussi pour ça qu'il faut fournir les deux — le repository seul ne suffit pas, il faut aussi enregistrer un <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRequestHandler</span> (au minimum <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRequestAttributeHandler</span>) pour que le flow fonctionne de bout en bout.
</div>

<div style="font-size: 1.1rem; font-weight: bold; color: #1e40af; margin-top: 1.5rem;">Résumé</div>

<div style="margin-left: 1rem; margin-top: 0.5rem;">
• Une attaque CSRF piège un utilisateur authentifié pour exécuter des actions à son insu via un script tiers<br>
• La protection CSRF est activée par défaut dans Spring Security<br>
• Le point d'entrée est un filtre HTTP, <span style="color: #1d4ed8; font-family: monospace;">CsrfFilter</span><br>
• Trois contrats permettent une personnalisation complète : <span style="color: #1d4ed8; font-family: monospace;">CsrfToken</span>, <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRepository</span>, <span style="color: #1d4ed8; font-family: monospace;">CsrfTokenRequestHandler</span>
</div>

</div>

