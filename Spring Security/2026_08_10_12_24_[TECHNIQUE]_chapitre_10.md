---
type: Document
title: Chapitre 10
---

<div style="font-family: system-ui, -apple-system, sans-serif; margin: 0 auto; padding: 20px; color: #1f2937;">

<div style="font-size: 1.4rem; font-weight: bold; color: #1e40af;">Chapitre 10 — Configuration de CORS</div>

<div style="margin-top: 8px; color: #4b5563;">Ce chapitre explique ce qu'est le CORS (Cross-Origin Resource Sharing), pourquoi il est nécessaire, et comment le configurer avec Spring Security de deux manières : via l'annotation <span style="color: #1d4ed8; font-family: monospace;">@CrossOrigin</span>, ou de façon centralisée via <span style="color: #1d4ed8; font-family: monospace;">CorsConfigurer</span>.</div>

<div style="margin-top: 24px; font-size: 1.1rem; font-weight: bold; color: #1e40af;">10.1 Comment fonctionne CORS ?</div>

<div style="margin-top: 8px;">Par défaut, un navigateur interdit à une page chargée depuis un domaine (ex. <span style="color: #1d4ed8; font-family: monospace;">example.com</span>) de faire des appels vers un autre domaine (ex. <span style="color: #1d4ed8; font-family: monospace;">api.example.com</span>), ou vers un domaine chargé dans une iframe. Le CORS est le mécanisme qui permet d'assouplir cette restriction sous certaines conditions, plutôt qu'une couche de sécurité supplémentaire comme l'authentification ou le CSRF.</div>

<div style="margin-top: 12px;">Le mécanisme repose sur des en-têtes HTTP :</div>

<div style="margin-left: 1rem; margin-top: 4px;">
<div>• <span style="color: #1d4ed8; font-family: monospace;">Access-Control-Allow-Origin</span> — domaines autorisés à accéder aux ressources</div>
<div>• <span style="color: #1d4ed8; font-family: monospace;">Access-Control-Allow-Methods</span> — méthodes HTTP autorisées pour un domaine donné</div>
<div>• <span style="color: #1d4ed8; font-family: monospace;">Access-Control-Allow-Headers</span> — en-têtes autorisés dans la requête</div>
</div>

<div style="margin-top: 12px;">Par défaut, Spring Security n'ajoute aucun de ces en-têtes. Si une application front-end appelle une API sur un autre domaine sans configuration CORS, le navigateur bloque la réponse avec une erreur du type <span style="color: #1d4ed8; font-family: monospace;">No 'Access-Control-Allow-Origin' header is present</span>, même si le endpoint a bien été exécuté côté serveur (les logs le confirment).</div>

<div style="margin-top: 12px; padding: 10px 14px; border-left: 3px solid #1e40af; background: #dbeafe; color: #1e3a5f;">Point important : le CORS est une décision prise par le navigateur, pas une protection d'endpoint. Le serveur répond normalement ; c'est le navigateur qui choisit de rejeter ou non la réponse selon les en-têtes reçus.</div>

<div style="margin-top: 12px;">Pour certaines requêtes (méthodes autres que GET/POST/HEAD simples, ou en-têtes personnalisés), le navigateur envoie d'abord une <span style="color: #1d4ed8; font-family: monospace;">preflight request</span> en HTTP <span style="color: #1d4ed8; font-family: monospace;">OPTIONS</span> pour vérifier si la requête réelle sera acceptée. Si le preflight échoue, la requête originale n'est jamais envoyée. Cette logique est entièrement gérée par le navigateur.</div>

<div style="margin-top: 24px; font-size: 1.1rem; font-weight: bold; color: #1e40af;">10.2 Appliquer CORS avec l'annotation @CrossOrigin</div>

<div style="margin-top: 8px;">L'annotation <span style="color: #1d4ed8; font-family: monospace;">@CrossOrigin</span> se place directement sur une méthode d'endpoint pour définir les origines, méthodes et en-têtes autorisés à cet endroit précis.</div>

```java
@PostMapping("/test")
@ResponseBody
@CrossOrigin("http://localhost:8080")
public String test() {
    logger.info("Test method called");
    return "HELLO";
}
```

<div style="margin-top: 8px;">Le paramètre <span style="color: #1d4ed8; font-family: monospace;">value</span> accepte un tableau pour plusieurs origines (ex. <span style="color: #1d4ed8; font-family: monospace;">@CrossOrigin({"example.com", "example.org"})</span>), et des attributs <span style="color: #1d4ed8; font-family: monospace;">allowedHeaders</span> / <span style="color: #1d4ed8; font-family: monospace;">methods</span> pour affiner. L'astérisque (<span style="color: #1d4ed8; font-family: monospace;">*</span>) autorise tout.</div>

<div style="margin-top: 12px; padding: 10px 14px; border-left: 3px solid #b45309; background: #fef3c7; color: #7c2d12;">L'auteur déconseille d'autoriser toutes les origines, même en environnement de test : cela expose à des requêtes XSS pouvant mener à des attaques DDoS. Les environnements de test partagent parfois la même infrastructure que la production, donc mieux vaut traiter chaque couche de sécurité indépendamment.</div>

<div style="margin-top: 12px;">Avantage de <span style="color: #1d4ed8; font-family: monospace;">@CrossOrigin</span> : la règle est visible directement sur l'endpoint. Inconvénients : verbeux si répété sur beaucoup d'endpoints, et risque d'oubli sur les nouveaux endpoints.</div>

<div style="margin-top: 24px; font-size: 1.1rem; font-weight: bold; color: #1e40af;">10.3 Appliquer CORS via un CorsConfigurer</div>

<div style="margin-top: 8px;">Pour centraliser la configuration CORS plutôt que de la répéter par endpoint, on la définit dans la classe de configuration via <span style="color: #1d4ed8; font-family: monospace;">http.cors()</span>.</div>

```java
@Configuration
public class ProjectConfig {

  @Bean
  public SecurityFilterChain securityFilterChain(HttpSecurity http)
    throws Exception {

    http.cors(c -> {
      CorsConfigurationSource source = request -> {
        CorsConfiguration config = new CorsConfiguration();
        config.setAllowedOrigins(
            List.of("example.com", "example.org"));
        config.setAllowedMethods(
            List.of("GET", "POST", "PUT", "DELETE"));
        config.setAllowedHeaders(List.of("*"));
        return config;
      };
      c.configurationSource(source);
    });

    http.csrf(c -> c.disable());

    http.authorizeHttpRequests(
      c -> c.anyRequest().permitAll()
    );

    return http.build();
  }
}
```

<div style="margin-top: 8px;">La méthode <span style="color: #1d4ed8; font-family: monospace;">cors()</span> reçoit un <span style="color: #1d4ed8; font-family: monospace;">Customizer&lt;CorsConfigurer&gt;</span>, auquel on fournit un <span style="color: #1d4ed8; font-family: monospace;">CorsConfigurationSource</span> retournant un <span style="color: #1d4ed8; font-family: monospace;">CorsConfiguration</span> (origines, méthodes, en-têtes autorisés).</div>

<div style="margin-top: 12px; padding: 10px 14px; border-left: 3px solid #b45309; background: #fef3c7; color: #7c2d12;">Si seules les origines sont définies sans les méthodes, aucune requête ne sera autorisée : <span style="color: #1d4ed8; font-family: monospace;">CorsConfiguration</span> ne définit aucune méthode par défaut.</div>

<div style="margin-top: 12px;">L'auteur recommande d'extraire l'implémentation de <span style="color: #1d4ed8; font-family: monospace;">CorsConfigurationSource</span> dans une classe séparée plutôt que de la définir en lambda dans la classe de configuration, surtout pour des configurations plus longues en production.</div>

<div style="margin-top: 24px; font-size: 1.1rem; font-weight: bold; color: #1e40af;">Résumé</div>

<div style="margin-left: 1rem; margin-top: 8px;">
<div>• Le CORS concerne les appels d'une application web hébergée sur un domaine vers un autre domaine</div>
<div>• Par défaut, le navigateur bloque ces appels ; CORS permet d'autoriser explicitement certaines origines</div>
<div>• Deux façons de configurer : par endpoint avec <span style="color: #1d4ed8; font-family: monospace;">@CrossOrigin</span>, ou centralisée dans la classe de configuration avec <span style="color: #1d4ed8; font-family: monospace;">http.cors()</span></div>
</div>

</div>

