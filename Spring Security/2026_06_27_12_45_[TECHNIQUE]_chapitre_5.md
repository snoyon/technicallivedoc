---
type: Document
title: Chapitre 5
---

<div style="font-size: 1.4rem; font-weight: bold; color: #1e40af;">Chapitre 5 — La sécurité d'une web app commence avec les filtres</div>

<div style="margin-top: 1rem;">Ce chapitre explore la chaîne de filtres (<span style="color: #1d4ed8; font-family: monospace;">filter chain</span>) de Spring Security : comment elle fonctionne, comment y insérer ses propres filtres, et quelles classes Spring Security fournit pour implémenter ces filtres.</div>

<span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">1. Le principe de la chaîne de filtres</span>

<div style="margin-top: 1rem;">Une requête HTTP traverse une succession de filtres, chacun exécutant une logique précise avant de transmettre la requête au filtre suivant via la <span style="color: #1d4ed8; font-family: monospace;">FilterChain</span>. L'auteur utilise l'analogie de l'aéroport : ticket, passeport, contrôle de sécurité, puis re-vérification à la porte d'embarquement — chaque étape est un filtre indépendant.</div>

<div style="margin-top: 1rem;">Techniquement, un filtre implémente l'interface <span style="color: #1d4ed8; font-family: monospace;">Filter</span> (package <span style="color: #1d4ed8; font-family: monospace;">jakarta.servlet</span>, depuis Spring Boot 3 qui remplace <span style="color: #1d4ed8; font-family: monospace;">javax.servlet</span>) et redéfinit la méthode <span style="color: #1d4ed8; font-family: monospace;">doFilter()</span>, qui reçoit :</div>

<div style="margin-left: 1rem; margin-top: 0.5rem;">
• <span style="color: #1d4ed8; font-family: monospace;">ServletRequest</span> — les détails de la requête<br>
• <span style="color: #1d4ed8; font-family: monospace;">ServletResponse</span> — pour modifier la réponse<br>
• <span style="color: #1d4ed8; font-family: monospace;">FilterChain</span> — pour transmettre au filtre suivant
</div>

<div style="margin-top: 1rem;">Spring Security fournit déjà plusieurs filtres (<span style="color: #1d4ed8; font-family: monospace;">BasicAuthenticationFilter</span>, <span style="color: #1d4ed8; font-family: monospace;">CsrfFilter</span>, <span style="color: #1d4ed8; font-family: monospace;">CorsFilter</span>...), chacun avec un ordre (un index) dans la chaîne. Cet ordre est consultable dans l'enum <span style="color: #1d4ed8; font-family: monospace;">SecurityWebFiltersOrder</span>.</div>

<div style="border-left: 3px solid #1e40af; background-color: #dbeafe; color: #1e3a5f; padding: 0.75rem; margin-top: 1rem;">Plusieurs filtres peuvent partager la même position dans la chaîne. Dans ce cas, Spring Security ne garantit <strong>aucun ordre</strong> entre eux.</div>

<span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">2. Ajouter un filtre avant un filtre existant</span>

<div style="margin-top: 1rem;">Cas d'usage : valider une requête <em>avant</em> l'authentification, pour éviter des traitements coûteux (accès BDD, etc.) sur des requêtes mal formées. Exemple traité : vérifier la présence d'un header obligatoire <span style="color: #1d4ed8; font-family: monospace;">Request-Id</span>.</div>

<div style="margin-top: 1rem;">Étapes :</div>
<div style="margin-left: 1rem; margin-top: 0.5rem;">
1. Implémenter un filtre (ex. <span style="color: #1d4ed8; font-family: monospace;">RequestValidationFilter</span>) qui implémente <span style="color: #1d4ed8; font-family: monospace;">Filter</span><br>
2. L'enregistrer via <span style="color: #1d4ed8; font-family: monospace;">addFilterBefore()</span> sur <span style="color: #1d4ed8; font-family: monospace;">HttpSecurity</span>, en le positionnant avant <span style="color: #1d4ed8; font-family: monospace;">BasicAuthenticationFilter.class</span>
</div>

```java
@Override
public void doFilter(
  ServletRequest request,
  ServletResponse response,
  FilterChain filterChain)
    throws IOException, ServletException {
  var httpRequest = (HttpServletRequest) request;
  var httpResponse = (HttpServletResponse) response;

  String requestId = httpRequest.getHeader("Request-Id");

  if (requestId == null || requestId.isBlank()) {
      httpResponse.setStatus(HttpServletResponse.SC_BAD_REQUEST);
      return;
  }

  filterChain.doFilter(request, response);
}
```

<div style="margin-top: 1rem;">Côté configuration :</div>

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http)
  throws Exception {

  http.addFilterBefore(
     new RequestValidationFilter(), BasicAuthenticationFilter.class)
     .authorizeRequests(c -> c.anyRequest().permitAll());

  return http.build();
}
```

<div style="margin-top: 1rem;">Résultat : sans le header → <span style="color: #1d4ed8; font-family: monospace;">400 Bad Request</span> ; avec le header → <span style="color: #1d4ed8; font-family: monospace;">200 OK</span>.</div>

<span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">3. Ajouter un filtre après un filtre existant</span>

<div style="margin-top: 1rem;">Cas d'usage : exécuter une logique <em>après</em> l'authentification (notification d'un système externe, logging, audit). Exemple : logger chaque authentification réussie avec son <span style="color: #1d4ed8; font-family: monospace;">Request-Id</span>, via <span style="color: #1d4ed8; font-family: monospace;">AuthenticationLoggingFilter</span>, ajouté avec <span style="color: #1d4ed8; font-family: monospace;">addFilterAfter()</span>.</div>

```java
http.addFilterBefore(
        new RequestValidationFilter(),
        BasicAuthenticationFilter.class)
    .addFilterAfter(
        new AuthenticationLoggingFilter(),
        BasicAuthenticationFilter.class)
    .authorizeRequests(c -> c.anyRequest().permitAll());
```

<span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">4. Ajouter un filtre à la place d'un filtre existant</span>

<div style="margin-top: 1rem;">Utile pour remplacer entièrement le mécanisme d'authentification HTTP Basic par autre chose. Trois scénarios types évoqués :</div>

<div style="margin-left: 1rem; margin-top: 0.5rem;">
• Clé statique envoyée dans un header (simple, rapide, mais sécurité faible — souvent utilisée entre services backend)<br>
• Signature symétrique ou asymétrique de la requête<br>
• One-Time Password (OTP), typique du MFA
</div>

<div style="margin-top: 1rem;">Exemple détaillé : authentification par clé statique via <span style="color: #1d4ed8; font-family: monospace;">StaticKeyAuthenticationFilter</span>, qui compare le header <span style="color: #1d4ed8; font-family: monospace;">Authorization</span> à une valeur injectée depuis <span style="color: #1d4ed8; font-family: monospace;">application.properties</span> via <span style="color: #1d4ed8; font-family: monospace;">@Value</span>.</div>

```java
@Component
public class StaticKeyAuthenticationFilter implements Filter {

  @Value("${authorization.key}")
  private String authorizationKey;

  @Override
  public void doFilter(ServletRequest request,
                       ServletResponse response,
                       FilterChain filterChain)
    throws IOException, ServletException {

    var httpRequest = (HttpServletRequest) request;
    var httpResponse = (HttpServletResponse) response;

    String authentication =
           httpRequest.getHeader("Authorization");

    if (authorizationKey.equals(authentication)) {
        filterChain.doFilter(request, response);
    } else {
        httpResponse.setStatus(
            HttpServletResponse.SC_UNAUTHORIZED);
    }
  }
}
```

<div style="margin-top: 1rem;">On l'enregistre avec <span style="color: #1d4ed8; font-family: monospace;">addFilterAt()</span>, sans appeler <span style="color: #1d4ed8; font-family: monospace;">httpBasic()</span> (sinon <span style="color: #1d4ed8; font-family: monospace;">BasicAuthenticationFilter</span> serait quand même ajouté, en plus, à la même position) :</div>

```java
http.addFilterAt(filter, BasicAuthenticationFilter.class)
    .authorizeRequests(c -> c.anyRequest().permitAll());
```

<div style="border-left: 3px solid #b45309; background-color: #fef3c7; color: #7c2d12; padding: 0.75rem; margin-top: 1rem;"><span style="color: #1d4ed8; font-family: monospace;">addFilterAt()</span> n'écrase pas un filtre existant à cette position — il s'ajoute. Si on ne veut pas du filtre HTTP Basic par défaut, il ne faut tout simplement pas appeler <span style="color: #1d4ed8; font-family: monospace;">httpBasic()</span>.</div>

<div style="margin-top: 1rem;">Note pratique : comme aucun <span style="color: #1d4ed8; font-family: monospace;">UserDetailsService</span> n'est nécessaire ici (pas de notion d'utilisateur, juste une clé partagée), on peut désactiver son auto-configuration :</div>

```java
@SpringBootApplication(exclude =
  {UserDetailsServiceAutoConfiguration.class })
```

<span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">5. Les classes fournies par Spring Security pour implémenter un filtre</span>

<div style="margin-top: 1rem;">Plutôt que d'implémenter directement <span style="color: #1d4ed8; font-family: monospace;">Filter</span>, on peut étendre des classes abstraites fournies par Spring :</div>

<div style="margin-left: 1rem; margin-top: 0.5rem;">
• <span style="color: #1d4ed8; font-family: monospace;">GenericFilterBean</span> — apporte la gestion de paramètres d'initialisation (héritage de <span style="color: #1d4ed8; font-family: monospace;">web.xml</span>), rarement utile en pratique<br>
• <span style="color: #1d4ed8; font-family: monospace;">OncePerRequestFilter</span> — garantit que le filtre ne s'exécute <strong>qu'une fois par requête</strong>
</div>

<div style="margin-top: 1rem;">L'auteur insiste : Spring Security ne garantit pas qu'un filtre ajouté à la chaîne ne sera appelé qu'une fois. <span style="color: #1d4ed8; font-family: monospace;">OncePerRequestFilter</span> résout ce problème, et présente d'autres avantages :</div>

<div style="margin-left: 1rem; margin-top: 0.5rem;">
• Cast automatique en <span style="color: #1d4ed8; font-family: monospace;">HttpServletRequest</span>/<span style="color: #1d4ed8; font-family: monospace;">HttpServletResponse</span> (plus besoin de caster soi-même)<br>
• Possibilité d'exclure certaines requêtes via <span style="color: #1d4ed8; font-family: monospace;">shouldNotFilter()</span><br>
• Par défaut, ne s'applique pas aux requêtes asynchrones ni aux dispatchs d'erreur (modifiable via <span style="color: #1d4ed8; font-family: monospace;">shouldNotFilterAsyncDispatch()</span> et <span style="color: #1d4ed8; font-family: monospace;">shouldNotFilterErrorDispatch()</span>)
</div>

<div style="margin-top: 1rem;">Reprise de l'exemple de logging avec cette classe :</div>

```java
public class AuthenticationLoggingFilter
  extends OncePerRequestFilter {

  private final Logger logger =
          Logger.getLogger(
            AuthenticationLoggingFilter.class.getName());

  @Override
  protected void doFilterInternal(
    HttpServletRequest request,
    HttpServletResponse response,
    FilterChain filterChain) throws
      ServletException, IOException {

      String requestId = request.getHeader("Request-Id");

      logger.info("Successfully authenticated request with id " +
                   requestId);

      filterChain.doFilter(request, response);
  }
}
```

<div style="border-left: 3px solid #1e40af; background-color: #dbeafe; color: #1e3a5f; padding: 0.75rem; margin-top: 1rem;">Conseil de l'auteur : ne pas étendre <span style="color: #1d4ed8; font-family: monospace;">GenericFilterBean</span> par réflexe (copié depuis des exemples en ligne) si la fonctionnalité supplémentaire n'est pas nécessaire — préférer l'implémentation la plus simple possible, et n'utiliser <span style="color: #1d4ed8; font-family: monospace;">OncePerRequestFilter</span> que si son comportement spécifique est réellement utile.</div>

<span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">Points clés à retenir</span>

<div style="margin-left: 1rem; margin-top: 0.5rem;">
• La chaîne de filtres est la première couche qui intercepte les requêtes HTTP<br>
• Trois méthodes de personnalisation : <span style="color: #1d4ed8; font-family: monospace;">addFilterBefore()</span>, <span style="color: #1d4ed8; font-family: monospace;">addFilterAfter()</span>, <span style="color: #1d4ed8; font-family: monospace;">addFilterAt()</span><br>
• Plusieurs filtres à la même position = ordre d'exécution non garanti entre eux<br>
• La personnalisation de la chaîne permet d'adapter authentification <em>et</em> autorisation aux besoins réels de l'application
</div>
