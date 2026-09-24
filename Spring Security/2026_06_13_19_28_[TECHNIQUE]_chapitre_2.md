---
type: Document
title: Chapitre 2
---

<span style="color:#1e40af;font-size:1.4rem;font-weight:bold;">Chapitre 2 – Hello, Spring Security</span>
<div style="margin-top:1.2rem;">
<span style="color:#1e40af;font-size:1.1rem;font-weight:bold;">Vue d'ensemble</span>
<div style="margin-top:.5rem;">Ce chapitre introduit Spring Security sur un projet Spring Boot minimal. Il couvre le comportement par défaut, l'architecture d'authentification, et plusieurs façons de remplacer les composants autoconfigurés.</div>
<div style="margin-top:.5rem;">Objectifs :</div>
<div style="margin-left:1rem;margin-top:.3rem;">• Observer le comportement par défaut (HTTP Basic, user/password générés)</div>
<div style="margin-left:1rem;">• Comprendre les acteurs principaux du flux d'authentification</div>
<div style="margin-left:1rem;">• Surcharger <span style="color:#1d4ed8;font-family:monospace;">UserDetailsService</span> et <span style="color:#1d4ed8;font-family:monospace;">PasswordEncoder</span></div>
<div style="margin-left:1rem;">• Configurer les règles d'autorisation sur les endpoints</div>
<div style="margin-left:1rem;">• Implémenter un <span style="color:#1d4ed8;font-family:monospace;">AuthenticationProvider</span> personnalisé</div>
</div>
<div style="margin-top:1.4rem;">
<span style="color:#1e40af;font-size:1.1rem;font-weight:bold;">2.1 – Premier projet</span>
<div style="margin-top:.5rem;">Dépendances minimales dans <span style="color:#1d4ed8;font-family:monospace;">pom.xml</span> : <span style="color:#1d4ed8;font-family:monospace;">spring-boot-starter-web</span> + <span style="color:#1d4ed8;font-family:monospace;">spring-boot-starter-security</span>.</div>
<div style="margin-top:.5rem;">Dès l'ajout de la dépendance, Spring Boot autoconfigure :</div>
<div style="margin-left:1rem;margin-top:.3rem;">• Un utilisateur par défaut : <span style="color:#1d4ed8;font-family:monospace;">user</span></div>
<div style="margin-left:1rem;">• Un mot de passe UUID aléatoire imprimé dans la console au démarrage</div>
<div style="margin-left:1rem;">• HTTP Basic et Form Login activés</div>
<div style="margin-left:1rem;">• Tous les endpoints sécurisés par défaut</div>
<div style="margin-top:.6rem;padding:.6rem 1rem;border-left:3px solid #1e40af;color:#1e3a5f;background:#dbeafe;">Sans header <span style="color:#1d4ed8;font-family:monospace;">Authorization</span> → HTTP 401. Avec les bons credentials → HTTP 200. Le header Basic encode <span style="color:#1d4ed8;font-family:monospace;">user:password</span> en Base64 — encodage seulement, pas chiffrement. Toujours coupler avec HTTPS en production.</div>
</div>
<div style="margin-top:1.4rem;">
<span style="color:#1e40af;font-size:1.1rem;font-weight:bold;">2.2 – Architecture : vue d'ensemble</span>
<div style="margin-top:.6rem;"><span style="color:#1e40af;font-weight:bold;">Flux d'authentification</span></div>
<div style="margin-top:.4rem;font-family:monospace;color:#1e40af;">AuthFilter → AuthManager → AuthProvider → UserDetailsService</div>
<div style="margin-top:.2rem;font-family:monospace;color:#374151;font-size:.9rem;">AuthProvider utilise aussi : PasswordEncoder — résultat stocké dans : SecurityContext</div>
<div style="margin-top:.8rem;"><span style="color:#1e40af;font-weight:bold;">Composants clés</span></div>
<div style="margin-left:1rem;margin-top:.3rem;">• <span style="font-weight:bold;">UserDetailsService</span> – gère les détails utilisateur. Par défaut : <span style="color:#1d4ed8;font-family:monospace;">InMemoryUserDetailsManager</span>, credentials en mémoire volatile.</div>
<div style="margin-left:1rem;margin-top:.2rem;">• <span style="font-weight:bold;">PasswordEncoder</span> – encode et vérifie les mots de passe. Obligatoire dès qu'on surcharge <span style="color:#1d4ed8;font-family:monospace;">UserDetailsService</span>.</div>
<div style="margin-left:1rem;margin-top:.2rem;">• <span style="font-weight:bold;">AuthenticationProvider</span> – implémente la logique d'authentification en déléguant aux deux précédents.</div>
<div style="margin-left:1rem;margin-top:.2rem;">• <span style="font-weight:bold;">SecurityContext</span> – stocke l'état d'authentification pour la durée de la requête (modèle thread-per-request).</div>
<div style="margin-top:.6rem;padding:.6rem 1rem;border-left:3px solid #b45309;color:#7c2d12;background:#fef3c7;">La configuration par défaut ne convient pas à la production : password UUID éphémère et credentials en mémoire non persistés.</div>
</div>
<div style="margin-top:1.4rem;">
<span style="color:#1e40af;font-size:1.1rem;font-weight:bold;">2.3.1 – UserDetailsService personnalisé</span>
<div style="margin-top:.5rem;">Déclarer un bean <span style="color:#1d4ed8;font-family:monospace;">UserDetailsService</span> dans une classe <span style="color:#1d4ed8;font-family:monospace;">@Configuration</span> suffit à remplacer l'autoconfiguré :</div>

```java
@Configuration
public class ProjectConfig {

  @Bean
  UserDetailsService userDetailsService() {
    var user = User.withUsername("john")
                   .password("12345")
                   .authorities("read")
                   .build();
    return new InMemoryUserDetailsManager(user);
  }

  @Bean
  PasswordEncoder passwordEncoder() {
    return NoOpPasswordEncoder.getInstance();
  }
}
```

<div style="margin-top:.6rem;padding:.6rem 1rem;border-left:3px solid #b45309;color:#7c2d12;background:#fef3c7;"><span style="color:#1d4ed8;font-family:monospace;">NoOpPasswordEncoder</span> traite les mots de passe en clair. Uniquement pour exemples/POC — classe marquée <span style="color:#1d4ed8;font-family:monospace;">@Deprecated</span>.</div>
<div style="margin-top:.5rem;padding:.6rem 1rem;border-left:3px solid #1e40af;color:#1e3a5f;background:#dbeafe;">Surcharger <span style="color:#1d4ed8;font-family:monospace;">UserDetailsService</span> impose de déclarer aussi un <span style="color:#1d4ed8;font-family:monospace;">PasswordEncoder</span>, sans quoi Spring Security lève une <span style="color:#1d4ed8;font-family:monospace;">IllegalArgumentException</span> au runtime.</div>
</div>
<div style="margin-top:1.4rem;">
<span style="color:#1e40af;font-size:1.1rem;font-weight:bold;">2.3.2 – Règles d'autorisation sur les endpoints</span>
<div style="margin-top:.5rem;">Un bean <span style="color:#1d4ed8;font-family:monospace;">SecurityFilterChain</span> via <span style="color:#1d4ed8;font-family:monospace;">HttpSecurity</span> :</div>

```java
@Bean
SecurityFilterChain configure(HttpSecurity http) throws Exception {
  http.httpBasic(Customizer.withDefaults());
  http.authorizeHttpRequests(
    c -> c.anyRequest().authenticated() // ou .permitAll()
  );
  return http.build();
}
```

<div style="margin-left:1rem;margin-top:.4rem;">• <span style="color:#1d4ed8;font-family:monospace;">httpBasic()</span> – active HTTP Basic</div>
<div style="margin-left:1rem;">• <span style="color:#1d4ed8;font-family:monospace;">anyRequest().authenticated()</span> – tous les endpoints requièrent une authentification</div>
<div style="margin-left:1rem;">• <span style="color:#1d4ed8;font-family:monospace;">anyRequest().permitAll()</span> – tous les endpoints sont publics</div>
<div style="margin-top:.6rem;padding:.6rem 1rem;border-left:3px solid #1e40af;color:#1e3a5f;background:#dbeafe;"><span style="color:#1d4ed8;font-family:monospace;">Customizer</span> est une interface fonctionnelle. <span style="color:#1d4ed8;font-family:monospace;">withDefaults()</span> est un no-op. La syntaxe lambda remplace l'ancienne syntaxe fluent chaînée, supprimée en Spring Security 6.</div>
<div style="margin-top:.5rem;padding:.6rem 1rem;border-left:3px solid #b45309;color:#7c2d12;background:#fef3c7;">Ne plus étendre <span style="color:#1d4ed8;font-family:monospace;">WebSecurityConfigurerAdapter</span> — supprimé en Spring Security 6.</div>
</div>
<div style="margin-top:1.4rem;">
<span style="color:#1e40af;font-size:1.1rem;font-weight:bold;">2.3.3 – Styles de configuration</span>
<div style="margin-top:.5rem;">Deux approches équivalentes pour déclarer <span style="color:#1d4ed8;font-family:monospace;">UserDetailsService</span> :</div>
<div style="margin-left:1rem;margin-top:.3rem;">• <span style="font-weight:bold;">Bean dans le contexte</span> — injectable ailleurs dans l'application.</div>
<div style="margin-left:1rem;">• <span style="font-weight:bold;">Via <span style="color:#1d4ed8;font-family:monospace;">HttpSecurity</span></span> — passé directement à <span style="color:#1d4ed8;font-family:monospace;">http.userDetailsService(...)</span> en local dans le bean <span style="color:#1d4ed8;font-family:monospace;">SecurityFilterChain</span>.</div>
<div style="margin-top:.6rem;padding:.6rem 1rem;border-left:3px solid #b45309;color:#7c2d12;background:#fef3c7;">Ne jamais mélanger les styles dans un même projet — nuit à la lisibilité et à la maintenabilité.</div>
</div>
<div style="margin-top:1.4rem;">
<span style="color:#1e40af;font-size:1.1rem;font-weight:bold;">2.3.4 – AuthenticationProvider personnalisé</span>
<div style="margin-top:.5rem;">Implémenter <span style="color:#1d4ed8;font-family:monospace;">AuthenticationProvider</span> pour une logique d'authentification sur mesure :</div>

```java
@Component
public class CustomAuthenticationProvider implements AuthenticationProvider {

  @Override
  public Authentication authenticate(Authentication authentication)
      throws AuthenticationException {
    String username = authentication.getName();
    String password = String.valueOf(authentication.getCredentials());
    if ("john".equals(username) && "12345".equals(password)) {
      return new UsernamePasswordAuthenticationToken(
          username, password, Arrays.asList());
    }
    throw new AuthenticationCredentialsNotFoundException("Error!");
  }

  @Override
  public boolean supports(Class<?> authenticationType) {
    return UsernamePasswordAuthenticationToken.class
               .isAssignableFrom(authenticationType);
  }
}
```

<div style="margin-top:.5rem;">Enregistrement dans la configuration : <span style="color:#1d4ed8;font-family:monospace;">http.authenticationProvider(authenticationProvider);</span></div>
<div style="margin-top:.6rem;padding:.6rem 1rem;border-left:3px solid #1e40af;color:#1e3a5f;background:#dbeafe;">Même avec un <span style="color:#1d4ed8;font-family:monospace;">AuthenticationProvider</span> custom, il est recommandé de déléguer la gestion des credentials à <span style="color:#1d4ed8;font-family:monospace;">UserDetailsService</span> et <span style="color:#1d4ed8;font-family:monospace;">PasswordEncoder</span> conformément à l'architecture Spring Security.</div>
</div>
<div style="margin-top:1.4rem;">
<span style="color:#1e40af;font-size:1.1rem;font-weight:bold;">2.3.5 – Classes de configuration multiples</span>
<div style="margin-top:.5rem;">Bonne pratique : séparer les responsabilités en plusieurs <span style="color:#1d4ed8;font-family:monospace;">@Configuration</span> :</div>
<div style="margin-left:1rem;margin-top:.3rem;">• <span style="color:#1d4ed8;font-family:monospace;">UserManagementConfig</span> — déclare <span style="color:#1d4ed8;font-family:monospace;">UserDetailsService</span> et <span style="color:#1d4ed8;font-family:monospace;">PasswordEncoder</span></div>
<div style="margin-left:1rem;">• <span style="color:#1d4ed8;font-family:monospace;">WebAuthorizationConfig</span> — déclare le bean <span style="color:#1d4ed8;font-family:monospace;">SecurityFilterChain</span></div>
</div>
<div style="margin-top:1.4rem;">
<span style="color:#1e40af;font-size:1.1rem;font-weight:bold;">Résumé</span>
<div style="margin-left:1rem;margin-top:.3rem;">• Spring Boot autoconfigure HTTP Basic, un utilisateur <span style="color:#1d4ed8;font-family:monospace;">user</span> et un password UUID au démarrage.</div>
<div style="margin-left:1rem;">• Les trois composants à personnaliser : <span style="color:#1d4ed8;font-family:monospace;">UserDetailsService</span>, <span style="color:#1d4ed8;font-family:monospace;">PasswordEncoder</span>, <span style="color:#1d4ed8;font-family:monospace;">AuthenticationProvider</span></div>
<div style="margin-left:1rem;">• Un bean dans une classe <span style="color:#1d4ed8;font-family:monospace;">@Configuration</span> suffit à surcharger chaque composant.</div>
<div style="margin-left:1rem;">• Les règles d'authentification et d'autorisation se déclarent via un bean <span style="color:#1d4ed8;font-family:monospace;">SecurityFilterChain</span>.</div>
<div style="margin-left:1rem;">• Plusieurs styles de configuration existent — en choisir un seul par projet.</div>
<div style="margin-left:1rem;">• Séparer les classes de configuration par responsabilité améliore la maintenabilité.</div>
</div>

<!-- image-align: left -->
![image](/images/schema_spring_security.png)