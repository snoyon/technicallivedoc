---
type: Document
title: Chapitre 6
---

<div style="background-color:#ffffff;color:#1f2937;padding:20px;font-family:sans-serif;">

<span style="color:#1e40af;font-weight:bold;font-size:1.4rem;">Chapitre 6 — Implémenter l'authentification</span>

<div style="margin-top:12px;">
Ce chapitre couvre la partie restante du flux d'authentification : l'implémentation d'une logique custom via <span style="color:#1d4ed8;font-family:monospace;">AuthenticationProvider</span>, les méthodes HTTP Basic et form-based login, et la gestion du <span style="color:#1d4ed8;font-family:monospace;">SecurityContext</span>. Les chapitres 7 et 8 traiteront ensuite l'autorisation, qui suit l'authentification dans le traitement d'une requête HTTP.
</div>

<div style="margin-top:20px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.1rem;">6.1 Comprendre l'AuthenticationProvider</span>
</div>

<div style="margin-top:8px;">
Un framework ne peut pas couvrir tous les scénarios d'authentification possibles (mot de passe, code SMS, clé stockée dans un fichier, empreinte digitale...). Spring Security expose donc le contrat <span style="color:#1d4ed8;font-family:monospace;">AuthenticationProvider</span> pour implémenter n'importe quelle logique custom.
</div>

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">6.1.1 L'interface Authentication</span>
</div>
<div style="margin-top:6px;">
<span style="color:#1d4ed8;font-family:monospace;">Authentication</span> représente l'événement d'authentification et porte les détails de l'entité qui demande l'accès (le principal). Elle étend <span style="color:#1d4ed8;font-family:monospace;">Principal</span> (Java Security), ce qui facilite les migrations depuis d'autres frameworks.
</div>

<div style="margin-top:8px;margin-left:1rem;">
<div>• <span style="color:#1d4ed8;font-family:monospace;">isAuthenticated()</span> — indique si le processus d'authentification est terminé</div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">getCredentials()</span> — retourne le mot de passe ou tout autre secret</div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">getAuthorities()</span> — retourne la collection des autorités accordées</div>
</div>

```java
public interface Authentication extends Principal, Serializable {
  Collection<? extends GrantedAuthority> getAuthorities();
  Object getCredentials();
  Object getDetails();
  Object getPrincipal();
  boolean isAuthenticated();
  void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException;
}
```

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">6.1.2 Implémenter une logique d'authentification custom</span>
</div>
<div style="margin-top:6px;">
L'implémentation par défaut délègue la recherche de l'utilisateur à un <span style="color:#1d4ed8;font-family:monospace;">UserDetailsService</span> et la vérification du mot de passe à un <span style="color:#1d4ed8;font-family:monospace;">PasswordEncoder</span>.
</div>

```java
public interface AuthenticationProvider {
  Authentication authenticate(Authentication authentication) throws AuthenticationException;
  boolean supports(Class<?> authentication);
}
```

<div style="border-left:3px solid #1e40af;background-color:#dbeafe;color:#1e3a5f;padding:10px 14px;margin-top:10px;">
Règles d'implémentation d'<span style="color:#1d4ed8;font-family:monospace;">authenticate()</span> : lever une <span style="color:#1d4ed8;font-family:monospace;">AuthenticationException</span> en cas d'échec ; retourner <span style="color:#1d4ed8;font-family:monospace;">null</span> si le type d'objet n'est pas supporté ; retourner un objet <span style="color:#1d4ed8;font-family:monospace;">Authentication</span> pleinement authentifié (mot de passe généralement retiré) en cas de succès.
</div>

<div style="margin-top:10px;">
La méthode <span style="color:#1d4ed8;font-family:monospace;">supports(Class&lt;?&gt;)</span> indique si le provider gère un type d'authentification donné — même si elle retourne <span style="color:#1d4ed8;font-family:monospace;">true</span>, <span style="color:#1d4ed8;font-family:monospace;">authenticate()</span> peut quand même rejeter la requête en retournant <span style="color:#1d4ed8;font-family:monospace;">null</span>. Analogie du livre : une serrure (l'<span style="color:#1d4ed8;font-family:monospace;">AuthenticationManager</span>) qui délègue à plusieurs providers (carte, clé physique).
</div>

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">6.1.3 Exemple d'application</span>
</div>

```java
@Component
public class CustomAuthenticationProvider implements AuthenticationProvider {

  private final UserDetailsService userDetailsService;
  private final PasswordEncoder passwordEncoder;

  @Override
  public boolean supports(Class<?> authenticationType) {
    return authenticationType.equals(UsernamePasswordAuthenticationToken.class);
  }

  @Override
  public Authentication authenticate(Authentication authentication) {
    String username = authentication.getName();
    String password = authentication.getCredentials().toString();
    UserDetails u = userDetailsService.loadUserByUsername(username);

    if (passwordEncoder.matches(password, u.getPassword())) {
      return new UsernamePasswordAuthenticationToken(username, password, u.getAuthorities());
    } else {
      throw new BadCredentialsException("Something went wrong!");
    }
  }
}
```

<div style="margin-top:8px;">
Enregistrement dans la configuration :
</div>

```java
@Configuration
public class ProjectConfig {

  private final AuthenticationProvider authenticationProvider;

  @Bean
  public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http.httpBasic(Customizer.withDefaults());
    http.authenticationProvider(authenticationProvider);
    http.authorizeHttpRequests(c -> c.anyRequest().authenticated());
    return http.build();
  }
}
```

<div style="border-left:3px solid #b45309;background-color:#fef3c7;color:#7c2d12;padding:10px 14px;margin-top:10px;">
Anecdote de l'auteur : une équipe accusait Spring Security d'être "difficile à personnaliser" alors qu'elle n'utilisait que 10 % des capacités du framework, en réimplémentant en code custom des fonctionnalités déjà fournies par la filter chain. Leçon : bien comprendre les contrats avant de conclure que le framework est en cause.
</div>

<div style="margin-top:20px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.1rem;">6.2 Le SecurityContext</span>
</div>

<div style="margin-top:8px;">
Après authentification réussie, l'<span style="color:#1d4ed8;font-family:monospace;">AuthenticationManager</span> stocke l'objet <span style="color:#1d4ed8;font-family:monospace;">Authentication</span> dans le <span style="color:#1d4ed8;font-family:monospace;">SecurityContext</span>, accessible via <span style="color:#1d4ed8;font-family:monospace;">SecurityContextHolder</span>.
</div>

```java
public interface SecurityContext extends Serializable {
  Authentication getAuthentication();
  void setAuthentication(Authentication authentication);
}
```

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">Trois stratégies de gestion</span>
</div>
<div style="margin-top:6px;margin-left:1rem;">
<div>• <span style="color:#1d4ed8;font-family:monospace;">MODE_THREADLOCAL</span> (défaut) — chaque thread a son propre contexte ; adapté à une application web thread-per-request</div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">MODE_INHERITABLETHREADLOCAL</span> — copie le contexte vers le thread enfant, utile pour les méthodes <span style="color:#1d4ed8;font-family:monospace;">@Async</span></div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">MODE_GLOBAL</span> — tous les threads partagent le même contexte, utile pour une application standalone (attention aux problèmes de concurrence, <span style="color:#1d4ed8;font-family:monospace;">SecurityContext</span> n'est pas thread-safe)</div>
</div>

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">6.2.1 Accès simple au contexte</span>
</div>

```java
@GetMapping("/hello")
public String hello(Authentication a) {
  return "Hello, " + a.getName() + "!";
}
```

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">6.2.2 Cas des méthodes asynchrones</span>
</div>
<div style="margin-top:6px;">
Une méthode <span style="color:#1d4ed8;font-family:monospace;">@Async</span> s'exécute sur un autre thread qui n'hérite pas du contexte par défaut (<span style="color:#1d4ed8;font-family:monospace;">NullPointerException</span> à la lecture de l'authentification). Solution : passer en <span style="color:#1d4ed8;font-family:monospace;">MODE_INHERITABLETHREADLOCAL</span>.
</div>

```java
@Configuration
@EnableAsync
public class ProjectConfig {

  @Bean
  public InitializingBean initializingBean() {
    return () -> SecurityContextHolder.setStrategyName(
      SecurityContextHolder.MODE_INHERITABLETHREADLOCAL);
  }
}
```

<div style="border-left:3px solid #b45309;background-color:#fef3c7;color:#7c2d12;padding:10px 14px;margin-top:10px;">
Cette stratégie ne fonctionne que pour les threads créés par le framework lui-même (ex. <span style="color:#1d4ed8;font-family:monospace;">@Async</span>). Pour des threads créés directement par le code applicatif, le framework ne peut pas propager le contexte automatiquement.
</div>

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">6.2.3 Application standalone (MODE_GLOBAL)</span>
</div>
<div style="margin-top:6px;">
Change la stratégie de la même façon que pour <span style="color:#1d4ed8;font-family:monospace;">MODE_INHERITABLETHREADLOCAL</span>, en passant <span style="color:#1d4ed8;font-family:monospace;">MODE_GLOBAL</span>. Déconseillé pour une application web (les requêtes doivent rester isolées).
</div>

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">6.2.4 et 6.2.5 Propager le contexte vers des threads auto-gérés</span>
</div>
<div style="margin-top:6px;">
Pour des threads que l'application crée elle-même (hors connaissance du framework), Spring Security fournit des classes utilitaires qui décorent la tâche ou le pool de threads pour copier le contexte :
</div>

<div style="margin-top:8px;margin-left:1rem;">
<div>• <span style="color:#1d4ed8;font-family:monospace;">DelegatingSecurityContextRunnable</span> — décore un <span style="color:#1d4ed8;font-family:monospace;">Runnable</span></div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">DelegatingSecurityContextCallable</span> — décore un <span style="color:#1d4ed8;font-family:monospace;">Callable&lt;T&gt;</span></div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">DelegatingSecurityContextExecutor</span> / <span style="color:#1d4ed8;font-family:monospace;">ExecutorService</span> / <span style="color:#1d4ed8;font-family:monospace;">ScheduledExecutorService</span> — décorent directement le pool de threads plutôt que la tâche</div>
</div>

```java
@GetMapping("/ciao")
public String ciao() throws Exception {
  Callable<String> task = () -> {
    SecurityContext context = SecurityContextHolder.getContext();
    return context.getAuthentication().getName();
  };

  ExecutorService e = Executors.newCachedThreadPool();
  try {
    var contextTask = new DelegatingSecurityContextCallable<>(task);
    return "Ciao, " + e.submit(contextTask).get() + "!";
  } finally {
    e.shutdown();
  }
}
```

<div style="margin-top:20px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.1rem;">6.3 HTTP Basic et form-based login</span>
</div>

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">6.3.1 Configurer HTTP Basic</span>
</div>
<div style="margin-top:6px;">
Le realm et la réponse en cas d'échec sont personnalisables via un <span style="color:#1d4ed8;font-family:monospace;">Customizer</span> et un <span style="color:#1d4ed8;font-family:monospace;">AuthenticationEntryPoint</span> custom (méthode <span style="color:#1d4ed8;font-family:monospace;">commence()</span>).
</div>

```java
@Bean
public SecurityFilterChain configure(HttpSecurity http) throws Exception {
  http.httpBasic(c -> {
    c.realmName("OTHER");
    c.authenticationEntryPoint(new CustomEntryPoint());
  });
  http.authorizeHttpRequests(c -> c.anyRequest().authenticated());
  return http.build();
}
```

<div style="border-left:3px solid #1e40af;background-color:#dbeafe;color:#1e3a5f;padding:10px 14px;margin-top:10px;">
L'<span style="color:#1d4ed8;font-family:monospace;">AuthenticationEntryPoint</span> est en réalité utilisé par l'<span style="color:#1d4ed8;font-family:monospace;">ExceptionTranslationManager</span>, qui traduit les <span style="color:#1d4ed8;font-family:monospace;">AuthenticationException</span>/<span style="color:#1d4ed8;font-family:monospace;">AccessDeniedException</span> en réponses HTTP. Son rôle n'est donc pas limité aux échecs d'authentification au sens strict : il gère aussi le cas où un utilisateur authentifié n'a pas les autorisations nécessaires (autorisation, pas authentification). Le nom de l'interface ne reflète pas très bien cet usage plus large — le livre le signale explicitement comme une petite ambiguïté de nommage.
</div>

<div style="margin-top:12px;">
<span style="color:#1e40af;font-weight:bold;">6.3.2 Form-based login</span>
</div>
<div style="margin-top:6px;">
<span style="color:#1d4ed8;font-family:monospace;">formLogin()</span> auto-configure une page de login et de logout. Adapté aux petites applications utilisant une session côté serveur pour gérer le <span style="color:#1d4ed8;font-family:monospace;">SecurityContext</span> (moins adapté à la scalabilité horizontale — sujet traité avec OAuth2 aux chapitres 12–15).
</div>

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
  http.formLogin(Customizer.withDefaults());
  http.authorizeHttpRequests(c -> c.anyRequest().authenticated());
  return http.build();
}
```

<div style="margin-top:8px;">
Personnalisations possibles : <span style="color:#1d4ed8;font-family:monospace;">defaultSuccessUrl()</span>, ou des implémentations de <span style="color:#1d4ed8;font-family:monospace;">AuthenticationSuccessHandler</span> et <span style="color:#1d4ed8;font-family:monospace;">AuthenticationFailureHandler</span> pour des redirections ou réponses conditionnelles.
</div>

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
  http.formLogin(c ->
    c.successHandler(authenticationSuccessHandler)
     .failureHandler(authenticationFailureHandler)
  );
  http.httpBasic(Customizer.withDefaults());
  http.authorizeHttpRequests(c -> c.anyRequest().authenticated());
  return http.build();
}
```

<div style="border-left:3px solid #b45309;background-color:#fef3c7;color:#7c2d12;padding:10px 14px;margin-top:10px;">
HTTP Basic et form-based login peuvent être combinés sur la même <span style="color:#1d4ed8;font-family:monospace;">SecurityFilterChain</span> — sans <span style="color:#1d4ed8;font-family:monospace;">httpBasic()</span> en plus, une requête Basic valide serait quand même redirigée vers le formulaire de login (HTTP 302).
</div>

<div style="margin-top:20px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.1rem;">Résumé</span>
</div>
<div style="margin-top:8px;margin-left:1rem;">
<div>• <span style="color:#1d4ed8;font-family:monospace;">AuthenticationProvider</span> permet d'implémenter une logique d'authentification custom, en délégant à <span style="color:#1d4ed8;font-family:monospace;">UserDetailsService</span> et <span style="color:#1d4ed8;font-family:monospace;">PasswordEncoder</span></div>
<div>• Le <span style="color:#1d4ed8;font-family:monospace;">SecurityContext</span> conserve les détails de l'entité authentifiée après succès</div>
<div>• Trois stratégies de gestion : <span style="color:#1d4ed8;font-family:monospace;">THREADLOCAL</span>, <span style="color:#1d4ed8;font-family:monospace;">INHERITABLETHREADLOCAL</span>, <span style="color:#1d4ed8;font-family:monospace;">GLOBAL</span></div>
<div>• Pour les threads gérés par l'application elle-même, utiliser les classes <span style="color:#1d4ed8;font-family:monospace;">DelegatingSecurityContext*</span></div>
<div>• <span style="color:#1d4ed8;font-family:monospace;">formLogin()</span> auto-configure login/logout et se combine avec HTTP Basic</div>
</div>

</div>
