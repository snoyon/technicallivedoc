---
type: Document
title: Chapitre 8
---

<div style="font-family: Arial, sans-serif; margin: 0 auto; padding: 20px; color: #1f2937;">
<div style="font-size: 1.4rem; font-weight: bold; color: #1e40af; margin-bottom: 20px;">Chapitre 8 — Configuration de l'autorisation au niveau des endpoints : appliquer des restrictions</div>

<div style="font-size: 1.1rem; font-weight: bold; color: #1e40af; margin-top: 24px;">8.1 — La méthode requestMatchers()</div>
<div style="margin-top: 10px;">Jusqu'ici, les règles d'autorisation (chapitre 7) s'appliquaient à toutes les requêtes via <span style="color: #1d4ed8; font-family: monospace;">anyRequest()</span>. Ce chapitre introduit <span style="color: #1d4ed8; font-family: monospace;">requestMatchers()</span>, qui permet de cibler des groupes spécifiques de requêtes (par chemin, éventuellement combiné à une méthode HTTP) pour leur appliquer des règles différentes.</div>
<div style="margin-top: 10px;">Exemple de base : deux endpoints <span style="color: #1d4ed8; font-family: monospace;">/hello</span> (rôle ADMIN requis) et <span style="color: #1d4ed8; font-family: monospace;">/ciao</span> (rôle MANAGER requis).</div>

```java
http.authorizeHttpRequests(
  c -> c.requestMatchers("/hello").hasRole("ADMIN")
        .requestMatchers("/ciao").hasRole("MANAGER")
);
```

<div style="margin-top: 10px;">Tout endpoint non couvert par un matcher explicite reste accessible par défaut, y compris sans authentification. Bonne pratique : rendre ce comportement explicite avec <span style="color: #1d4ed8; font-family: monospace;">anyRequest().permitAll()</span> (ou <span style="color: #1d4ed8; font-family: monospace;">.authenticated()</span> / <span style="color: #1d4ed8; font-family: monospace;">.denyAll()</span> selon le besoin), placé en dernier dans la chaîne.</div>

<div style="border-left: 3px solid #1e40af; background: #dbeafe; color: #1e3a5f; padding: 10px 14px; margin-top: 14px; border-radius: 4px;">Les règles doivent être écrites du plus spécifique au plus général : <span style="color: #1d4ed8; font-family: monospace;">anyRequest()</span> ne peut jamais être placé avant un <span style="color: #1d4ed8; font-family: monospace;">requestMatchers()</span> plus précis.</div>

<div style="border-left: 3px solid #b45309; background: #fef3c7; color: #7c2d12; padding: 10px 14px; margin-top: 14px; border-radius: 4px;">Distinction authentification échouée vs. non authentifié : sur un endpoint en <span style="font-family: monospace;">permitAll()</span>, un appel sans identifiants passe (200), mais un appel avec des identifiants invalides échoue en amont à l'authentification et renvoie 401 — la requête n'atteint jamais le filtre d'autorisation.</div>

<div style="font-size: 1.1rem; font-weight: bold; color: #1e40af; margin-top: 24px;">8.2 — Sélectionner les requêtes : syntaxe ANT et méthodes HTTP</div>
<div style="margin-top: 10px;">Deux signatures de <span style="color: #1d4ed8; font-family: monospace;">requestMatchers()</span> :</div>
<div style="margin-left: 1rem; margin-top: 6px;">
<div>• <span style="color: #1d4ed8; font-family: monospace;">requestMatchers(HttpMethod method, String... patterns)</span> — cible une méthode HTTP précise pour un chemin donné</div>
<div>• <span style="color: #1d4ed8; font-family: monospace;">requestMatchers(String... patterns)</span> — s'applique à toutes les méthodes HTTP pour le chemin</div>
</div>
<div style="margin-top: 10px;">Exemple : GET sur <span style="color: #1d4ed8; font-family: monospace;">/a</span> nécessite une authentification, POST sur <span style="color: #1d4ed8; font-family: monospace;">/a</span> est libre, tout le reste est refusé.</div>

```java
http.authorizeHttpRequests(
  c -> c.requestMatchers(HttpMethod.GET, "/a").authenticated()
        .requestMatchers(HttpMethod.POST, "/a").permitAll()
        .anyRequest().denyAll()
);
http.csrf(c -> c.disable());
```

<div style="margin-top: 10px;">La protection CSRF, activée par défaut, doit être désactivée temporairement pour tester les méthodes POST/PUT/DELETE dans ces exemples (approfondi au chapitre 9).</div>
<div style="margin-top: 10px;">Pour appliquer les mêmes règles à un groupe de chemins partageant un préfixe, l'opérateur <span style="color: #1d4ed8; font-family: monospace;">**</span> remplace un nombre quelconque de segments : <span style="color: #1d4ed8; font-family: monospace;">/a/b/**</span> couvre <span style="color: #1d4ed8; font-family: monospace;">/a/b</span> et <span style="color: #1d4ed8; font-family: monospace;">/a/b/c</span>. L'opérateur <span style="color: #1d4ed8; font-family: monospace;">*</span> ne remplace qu'un seul segment.</div>
<div style="margin-top: 10px;">Les variables de chemin peuvent être contraintes par une regex directement dans l'expression : <span style="color: #1d4ed8; font-family: monospace;">/product/{code:^[0-9]*$}</span> n'autorise que des valeurs numériques.</div>

<div style="margin-left: 1rem; margin-top: 10px;">Tableau récapitulatif des expressions de chemin (table 8.1) :</div>
<div style="margin-left: 1rem; margin-top: 6px;">
<div>• <span style="color: #1d4ed8; font-family: monospace;">/a</span> — uniquement le chemin /a</div>
<div>• <span style="color: #1d4ed8; font-family: monospace;">/a/*</span> — un seul segment après /a (/a/b, pas /a/b/c)</div>
<div>• <span style="color: #1d4ed8; font-family: monospace;">/a/**</span> — /a et tous ses sous-chemins</div>
<div>• <span style="color: #1d4ed8; font-family: monospace;">/a/{param}</span> — chemin avec un paramètre</div>
<div>• <span style="color: #1d4ed8; font-family: monospace;">/a/{param:regex}</span> — paramètre contraint par une regex</div>
</div>

<div style="font-size: 1.1rem; font-weight: bold; color: #1e40af; margin-top: 24px;">8.3 — Les expressions régulières avec les request matchers</div>
<div style="margin-top: 10px;">Pour des exigences trop complexes pour les expressions de chemin (plusieurs variables de chemin combinées, formats particuliers), on utilise une regex complète via un matcher dédié. Exemple : n'autoriser <span style="color: #1d4ed8; font-family: monospace;">/video/{country}/{language}</span> que pour certaines combinaisons pays/langue.</div>

```java
http.authorizeHttpRequests(
  c -> c.regexMatchers(".*/(us|uk|ca)+/(en|fr).*").authenticated()
        .anyRequest().hasAuthority("premium")
);
```

<div style="margin-top: 10px;">Les regex offrent une flexibilité totale mais nuisent fortement à la lisibilité, d'autant plus qu'elles s'allongent avec la complexité des règles (l'auteur illustre avec une regex de validation d'email longue et illisible). Elles restent donc un dernier recours, à utiliser seulement quand les expressions de chemin classiques ne suffisent pas.</div>

<div style="font-size: 1.1rem; font-weight: bold; color: #1e40af; margin-top: 24px;">Points clés à retenir</div>
<div style="margin-left: 1rem; margin-top: 6px;">
<div>• <span style="color: #1d4ed8; font-family: monospace;">requestMatchers()</span> cible des sous-ensembles de requêtes par chemin et/ou méthode HTTP</div>
<div>• Les règles s'évaluent dans l'ordre déclaré, du plus spécifique au plus général</div>
<div>• <span style="color: #1d4ed8; font-family: monospace;">**</span> couvre plusieurs segments, <span style="color: #1d4ed8; font-family: monospace;">*</span> un seul</div>
<div>• Les variables de chemin peuvent porter une contrainte regex inline</div>
<div>• Les regex matchers complets sont réservés aux cas que les expressions de chemin ne peuvent pas exprimer</div>
</div>

</div>

