---
type: Document
title: Chapitre 7
---

<div style="background-color:#ffffff;color:#1f2937;padding:20px;font-family:sans-serif;">

<span style="color:#1e40af;font-weight:bold;font-size:1.4rem;">Chapitre 7 — Configurer l'autorisation au niveau des endpoints : restreindre l'accès</span>

<div style="font-size: 1.1rem; font-weight: bold; color: #1e40af; margin-top: 24px;">7.0 — Authentification vs. autorisation, et le filtre d'autorisation</div>
<div style="margin-top: 10px;">Jusqu'ici, seule l'authentification avait été traitée : l'application vérifiait qui appelait, sans juger si cette personne avait le droit d'accéder à la ressource. L'autorisation est le processus qui décide, une fois le client identifié, s'il a la permission d'accéder à la ressource demandée. L'autorisation intervient toujours après l'authentification.</div>
<div style="margin-top: 10px;">Dans Spring Security, une fois le flux d'authentification terminé, la requête est déléguée à un <span style="color: #1d4ed8; font-family: monospace;">Authorization filter</span> (filtre d'autorisation). Concrètement : le filtre d'authentification vérifie l'identité de l'utilisateur, place ses informations dans le <span style="color: #1d4ed8; font-family: monospace;">SecurityContext</span>, puis transmet la requête au filtre d'autorisation. Ce dernier détermine si la requête doit être acceptée, en se basant sur les informations utilisateur présentes dans le security context.</div>
<div style="margin-top: 10px;">Il n'y a pas de "marqueur" qu'on pose sur un filtre pour dire "toi, tu es un filtre d'authentification" ou "toi, tu es un filtre d'autorisation". Ce sont simplement deux classes de filtres différentes, positionnées à des endroits différents dans la chaîne. Ce ne sont pas des filtres génériques que le développeur configure lui-même — dans les exemples des chapitres 2 à 7, on n'ajoute jamais explicitement de filtre : on appelle des méthodes de configuration sur <span style="color: #1d4ed8; font-family: monospace;">HttpSecurity</span>, et c'est Spring Security qui, en coulisses, insère la bonne classe de filtre au bon endroit :</div>
<div style="margin-left: 1rem; margin-top: 6px;">
<div>• <span style="color: #1d4ed8; font-family: monospace;">http.httpBasic(...)</span> → insère un <span style="color: #1d4ed8; font-family: monospace;">BasicAuthenticationFilter</span></div>
<div>• <span style="color: #1d4ed8; font-family: monospace;">http.formLogin(...)</span> → insère un <span style="color: #1d4ed8; font-family: monospace;">UsernamePasswordAuthenticationFilter</span></div>
<div>• <span style="color: #1d4ed8; font-family: monospace;">http.authorizeHttpRequests(...)</span> → insère un <span style="color: #1d4ed8; font-family: monospace;">AuthorizationFilter</span></div>
</div>
<div style="margin-top: 10px;">Chacune de ces classes est un <span style="color: #1d4ed8; font-family: monospace;">Filter</span> (au sens Servlet) qui fait un travail précis : <span style="color: #1d4ed8; font-family: monospace;">BasicAuthenticationFilter</span> vérifie les identifiants et, si c'est bon, place l'utilisateur authentifié dans le <span style="color: #1d4ed8; font-family: monospace;">SecurityContext</span> ; <span style="color: #1d4ed8; font-family: monospace;">AuthorizationFilter</span> lit ce <span style="color: #1d4ed8; font-family: monospace;">SecurityContext</span> et applique les règles écrites avec <span style="color: #1d4ed8; font-family: monospace;">hasAuthority()</span>, <span style="color: #1d4ed8; font-family: monospace;">hasRole()</span>, etc.</div>
<div style="margin-top: 10px;">Quand on écrit un filtre personnalisé, on implémente simplement l'interface <span style="color: #1d4ed8; font-family: monospace;">Filter</span> sans jamais préciser à Spring Security s'il s'agit d'authentification ou d'autorisation. L'interface est générique : rien n'empêche de traiter les deux responsabilités dans la même méthode <span style="color: #1d4ed8; font-family: monospace;">doFilter()</span> (vérifier l'identité puis décider des permissions), comme le fait l'exemple <span style="color: #1d4ed8; font-family: monospace;">StaticKeyAuthenticationFilter</span> du chapitre 5. La distinction authentification/autorisation est donc une convention de conception que le développeur choisit de respecter (ou non), pas une contrainte imposée par le type Java.</div>

<!-- image-align: left -->
![image](/images/spring_filters_authentication_authorization.png)

<div style="margin-top:20px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.1rem;">7.1 Restreindre l'accès selon les autorités et les rôles</span>
</div>

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">Le contrat GrantedAuthority</span>
</div>
<div style="margin-top:6px;">
Une autorité (<span style="color:#1d4ed8;font-family:monospace;">GrantedAuthority</span>) représente une action qu'un utilisateur peut effectuer (ex. <span style="color:#1d4ed8;font-family:monospace;">read</span>, <span style="color:#1d4ed8;font-family:monospace;">write</span>, <span style="color:#1d4ed8;font-family:monospace;">delete</span>). <span style="color:#1d4ed8;font-family:monospace;">UserDetails</span> expose une collection de <span style="color:#1d4ed8;font-family:monospace;">GrantedAuthority</span> via <span style="color:#1d4ed8;font-family:monospace;">getAuthorities()</span>, utilisée après authentification pour accorder les permissions.
</div>

```java
public interface GrantedAuthority extends Serializable {
  String getAuthority();
}
```

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">7.1.1 Restreindre l'accès selon les autorités</span>
</div>
<div style="margin-top:6px;">
Trois façons de configurer les règles d'accès sur <span style="color:#1d4ed8;font-family:monospace;">authorizeHttpRequests()</span> :
</div>

<div style="margin-top:8px;margin-left:1rem;">
<div>• <span style="color:#1d4ed8;font-family:monospace;">hasAuthority(String)</span> — une seule autorité requise</div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">hasAnyAuthority(String...)</span> — au moins une des autorités listées ; recommandée avec <span style="color:#1d4ed8;font-family:monospace;">hasAuthority()</span> pour sa lisibilité</div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">access(AuthorizationManager)</span> — le plus flexible (souvent via <span style="color:#1d4ed8;font-family:monospace;">WebExpressionAuthorizationManager</span> et une expression SpEL), mais moins lisible ; à réserver aux cas que les deux méthodes précédentes ne couvrent pas</div>
</div>

```java
@Bean
public UserDetailsService userDetailsService() {
  var manager = new InMemoryUserDetailsManager();

  var user1 = User.withUsername("john")
                  .password("12345")
                  .authorities("READ")
                  .build();

  var user2 = User.withUsername("jane")
                  .password("12345")
                  .authorities("WRITE")
                  .build();

  manager.createUser(user1);
  manager.createUser(user2);
  return manager;
}
```

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
  http.httpBasic(Customizer.withDefaults());
  http.authorizeHttpRequests(
    c -> c.anyRequest().hasAuthority("WRITE")
  );
  return http.build();
}
```

<div style="margin-top:8px;">
Avec cette config, Jane (WRITE) obtient 200 OK, John (READ) obtient 403 Forbidden. En remplaçant par <span style="color:#1d4ed8;font-family:monospace;">hasAnyAuthority("WRITE", "READ")</span>, les deux utilisateurs sont acceptés.
</div>

<div style="margin-top:10px;">
Exemple avec <span style="color:#1d4ed8;font-family:monospace;">access()</span> pour une règle plus complexe (lecture autorisée, mais pas si l'utilisateur a aussi le droit de suppression) :
</div>

```java
String expression = "hasAuthority('read') and !hasAuthority('delete')";

http.authorizeHttpRequests(
  c -> c.anyRequest()
           .access(new WebExpressionAuthorizationManager(expression))
);
```

<div style="border-left:3px solid #1e40af;background-color:#dbeafe;color:#1e3a5f;padding:10px 14px;margin-top:10px;">
Avec cette expression, John (autorité <span style="color:#1d4ed8;font-family:monospace;">read</span> seule) accède à l'endpoint, mais Jane (<span style="color:#1d4ed8;font-family:monospace;">read</span>, <span style="color:#1d4ed8;font-family:monospace;">write</span>, <span style="color:#1d4ed8;font-family:monospace;">delete</span>) reçoit un 403 Forbidden, malgré son autorité <span style="color:#1d4ed8;font-family:monospace;">read</span>, à cause de la clause <span style="color:#1d4ed8;font-family:monospace;">!hasAuthority('delete')</span>.
</div>

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">7.1.2 Restreindre l'accès selon les rôles</span>
</div>
<div style="margin-top:6px;">
Les rôles sont des autorités "grossières" (coarse-grained) : un badge regroupant plusieurs privilèges (ex. <span style="color:#1d4ed8;font-family:monospace;">ADMIN</span> = read + write + delete). En interne, un rôle est représenté par le même contrat <span style="color:#1d4ed8;font-family:monospace;">GrantedAuthority</span>, avec le préfixe <span style="color:#1d4ed8;font-family:monospace;">ROLE_</span> obligatoire à la déclaration.
</div>

<div style="margin-top:8px;margin-left:1rem;">
<div>• <span style="color:#1d4ed8;font-family:monospace;">hasRole(String)</span> — un seul rôle requis (sans le préfixe <span style="color:#1d4ed8;font-family:monospace;">ROLE_</span> à l'usage)</div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">hasAnyRole(String...)</span> — au moins un des rôles listés</div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">access()</span> — via SpEL (<span style="color:#1d4ed8;font-family:monospace;">hasRole('ADMIN')</span>, <span style="color:#1d4ed8;font-family:monospace;">hasAnyRole(...)</span>) pour des règles plus complexes</div>
</div>

<div style="border-left:3px solid #1e40af;background-color:#dbeafe;color:#1e3a5f;padding:10px 14px;margin-top:10px;">
Rôle vs autorité : au niveau du framework, un rôle <span style="color:#1d4ed8;font-family:monospace;">est</span> une <span style="color:#1d4ed8;font-family:monospace;">GrantedAuthority</span> comme une autre — la seule différence mécanique est le préfixe <span style="color:#1d4ed8;font-family:monospace;">ROLE_</span>. La vraie différence est un niveau d'abstraction dans la conception : une autorité décrit une action précise (<span style="color:#1d4ed8;font-family:monospace;">read</span>, <span style="color:#1d4ed8;font-family:monospace;">write</span>, <span style="color:#1d4ed8;font-family:monospace;">delete</span>), tandis qu'un rôle regroupe un paquet cohérent d'autorités sous un seul nom (<span style="color:#1d4ed8;font-family:monospace;">ADMIN</span> = read + write + delete). Cela évite de dupliquer la logique "quelles autorités vont ensemble" dans chaque règle d'autorisation — on la factorise une fois dans la définition du rôle.
</div>

```java
var user1 = User.withUsername("john")
                .password("12345")
                .roles("ADMIN")
                .build();

var user2 = User.withUsername("jane")
                .password("12345")
                .roles("MANAGER")
                .build();
```

```java
http.authorizeHttpRequests(
  c -> c.anyRequest().hasRole("ADMIN")
);
```

<div style="border-left:3px solid #b45309;background-color:#fef3c7;color:#7c2d12;padding:10px 14px;margin-top:10px;">
Ne pas confondre <span style="color:#1d4ed8;font-family:monospace;">authorities()</span> et <span style="color:#1d4ed8;font-family:monospace;">roles()</span> sur le builder <span style="color:#1d4ed8;font-family:monospace;">User</span> : avec <span style="color:#1d4ed8;font-family:monospace;">authorities()</span>, il faut inclure soi-même le préfixe <span style="color:#1d4ed8;font-family:monospace;">ROLE_</span> (ex. <span style="color:#1d4ed8;font-family:monospace;">"ROLE_ADMIN"</span>). Avec <span style="color:#1d4ed8;font-family:monospace;">roles()</span>, le préfixe est ajouté automatiquement — le fournir soi-même provoque une exception.
</div>

<div style="margin-top:10px;">
<span style="color:#1e40af;font-weight:bold;">Aller plus loin avec access()</span>
</div>
<div style="margin-top:6px;">
<span style="color:#1d4ed8;font-family:monospace;">WebExpressionAuthorizationManager</span> accepte n'importe quelle expression SpEL, pas seulement liée aux autorités/rôles. Exemple : autoriser l'accès seulement après midi :
</div>

```java
T(java.time.LocalTime).now().isAfter(T(java.time.LocalTime).of(12, 0))
```

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">7.1.3 Restreindre l'accès à tous les endpoints</span>
</div>
<div style="margin-top:6px;">
À l'opposé de <span style="color:#1d4ed8;font-family:monospace;">permitAll()</span>, la méthode <span style="color:#1d4ed8;font-family:monospace;">denyAll()</span> refuse toutes les requêtes correspondantes.
</div>

```java
http.authorizeHttpRequests(
   c -> c.anyRequest().denyAll()
);
```

<div style="margin-top:8px;">
Cas d'usage typiques évoqués par l'auteur : rejeter les requêtes dont un paramètre (ex. une adresse e-mail en variable de chemin) ne respecte pas un format attendu (via une regex groupant les requêtes concernées) ; ou, dans une architecture à plusieurs gateways, faire en sorte que chaque gateway ne serve que ses propres chemins et refuse explicitement tout le reste.
</div>

<div style="margin-top:20px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.1rem;">Résumé</span>
</div>
<div style="margin-top:8px;margin-left:1rem;">
<div>• L'autorisation décide si une requête authentifiée est permise ; elle intervient toujours après l'authentification</div>
<div>• Les règles d'accès se configurent selon les autorités (<span style="color:#1d4ed8;font-family:monospace;">hasAuthority</span>, <span style="color:#1d4ed8;font-family:monospace;">hasAnyAuthority</span>) ou les rôles (<span style="color:#1d4ed8;font-family:monospace;">hasRole</span>, <span style="color:#1d4ed8;font-family:monospace;">hasAnyRole</span>), avec <span style="color:#1d4ed8;font-family:monospace;">access()</span> en dernier recours pour des règles SpEL plus complexes</div>
<div>• Certaines requêtes peuvent être autorisées même sans authentification (<span style="color:#1d4ed8;font-family:monospace;">permitAll()</span>)</div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">denyAll()</span> permet de rejeter systématiquement certaines requêtes</div>
</div>

</div>

