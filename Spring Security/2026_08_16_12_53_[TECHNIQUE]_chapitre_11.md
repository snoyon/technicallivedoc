---
type: Document
title: Chapitre 11
---

<div style="font-family: Arial, sans-serif; color: #1f2937; line-height: 1.6;">
<div style="font-size: 1.4rem; font-weight: bold; color: #1e40af;">Chapitre 11 – Autorisation au niveau des méthodes (Method Security)</div>

<div style="margin-top: 0.5rem;">Jusqu'ici, l'autorisation n'était appliquée qu'au niveau des endpoints HTTP. Ce chapitre introduit la <span style="color: #1d4ed8; font-family: monospace;">method security</span>, qui permet d'appliquer des règles d'autorisation directement sur les méthodes (services, repositories, etc.), y compris dans des applications non-web. Elle repose sur un aspect Spring (AOP) qui intercepte les appels aux méthodes protégées.</div>

<div style="margin-top: 1.5rem; font-size: 1.1rem; font-weight: bold; color: #1e40af;">1. Deux familles de règles : call authorization</div>
<div style="margin-top: 0.5rem;">La <span style="color: #1d4ed8; font-family: monospace;">call authorization</span> se décline en deux approches :</div>
<div style="margin-left: 1rem; margin-top: 0.5rem;">
- Préautorisation : la règle est vérifiée <b>avant</b> l'exécution de la méthode. Si elle échoue, la méthode n'est jamais appelée et une <span style="color: #1d4ed8; font-family: monospace;">AccessDeniedException</span> est levée. C'est l'approche la plus utilisée.<br>
- Postautorisation : la règle est vérifiée <b>après</b> l'exécution de la méthode, en se basant sur la valeur retournée. La méthode s'exécute donc dans tous les cas.
</div>

<div style="margin-top: 1rem; border-left: 3px solid #b45309; background: #fef3c7; color: #7c2d12; padding: 0.7rem 1rem;">Attention avec la postautorisation : si la méthode modifie un état (écriture en base, etc.), la modification est effectuée même si l'autorisation échoue ensuite. Même avec <span style="font-family: monospace;">@Transactional</span>, l'exception de postautorisation survient après le commit de la transaction — pas de rollback automatique.</div>

<div style="margin-top: 1.5rem; font-size: 1.1rem; font-weight: bold; color: #1e40af;">2. Activer la method security</div>
<div style="margin-top: 0.5rem;">Désactivée par défaut, elle s'active via <span style="color: #1d4ed8; font-family: monospace;">@EnableMethodSecurity</span> sur la classe de configuration. Cette annotation active par défaut le support des annotations <span style="color: #1d4ed8; font-family: monospace;">@PreAuthorize</span> / <span style="color: #1d4ed8; font-family: monospace;">@PostAuthorize</span>.</div>

<div style="margin-top: 0.5rem;">

```java
@Configuration
@EnableMethodSecurity
public class ProjectConfig {
}
```

</div>

<div style="margin-top: 0.5rem;">Deux autres approches existent, désactivées par défaut, activables via les attributs de l'annotation : <span style="color: #1d4ed8; font-family: monospace;">jsr250Enabled</span> (pour <span style="color: #1d4ed8; font-family: monospace;">@RolesAllowed</span>) et <span style="color: #1d4ed8; font-family: monospace;">securedEnabled</span> (pour <span style="color: #1d4ed8; font-family: monospace;">@Secured</span>).</div>

<div style="margin-top: 1.5rem; font-size: 1.1rem; font-weight: bold; color: #1e40af;">3. Préautorisation avec @PreAuthorize</div>
<div style="margin-top: 0.5rem;">L'annotation prend une expression SpEL. On retrouve les mêmes expressions qu'au niveau endpoint : <span style="color: #1d4ed8; font-family: monospace;">hasAuthority()</span>, <span style="color: #1d4ed8; font-family: monospace;">hasAnyAuthority()</span>, <span style="color: #1d4ed8; font-family: monospace;">hasRole()</span>, <span style="color: #1d4ed8; font-family: monospace;">hasAnyRole()</span>.</div>

<div style="margin-top: 0.5rem;">

```java
@Service
public class NameService {

  @PreAuthorize("hasAuthority('write')")
  public String getName() {
    return "Fantastico";
  }
}
```

</div>

<div style="margin-top: 0.5rem;">On peut aussi référencer les paramètres de la méthode dans l'expression via <span style="color: #1d4ed8; font-family: monospace;">#nomDuParametre</span>, et accéder à l'utilisateur authentifié via l'objet <span style="color: #1d4ed8; font-family: monospace;">authentication</span> — par exemple pour vérifier qu'un utilisateur ne peut consulter que ses propres données :</div>

<div style="margin-top: 0.5rem;">

```java
@PreAuthorize("#name == authentication.principal.username")
public List<String> getSecretNames(String name) {
  return secretNames.get(name);
}
```

</div>

<div style="margin-top: 1.5rem; font-size: 1.1rem; font-weight: bold; color: #1e40af;">4. Postautorisation avec @PostAuthorize</div>
<div style="margin-top: 0.5rem;">Utile quand la règle dépend de la donnée retournée (ex. récupérée d'une base ou d'un service externe). L'expression peut référencer la valeur retournée via <span style="color: #1d4ed8; font-family: monospace;">returnObject</span>.</div>

<div style="margin-top: 0.5rem;">

```java
@PostAuthorize("returnObject.roles.contains('reader')")
public Employee getBookDetails(String name) {
  return records.get(name);
}
```

</div>

<div style="margin-top: 0.5rem;">Si l'objet retourné ne respecte pas la règle, l'appelant reçoit une erreur 403 même si la méthode s'est bien exécutée. <span style="color: #1d4ed8; font-family: monospace;">@PreAuthorize</span> et <span style="color: #1d4ed8; font-family: monospace;">@PostAuthorize</span> peuvent être combinées sur une même méthode.</div>

<div style="margin-top: 1.5rem; font-size: 1.1rem; font-weight: bold; color: #1e40af;">5. Permissions complexes : PermissionEvaluator</div>
<div style="margin-top: 0.5rem;">Pour éviter des expressions SpEL longues et illisibles, on externalise la logique dans une classe implémentant <span style="color: #1d4ed8; font-family: monospace;">PermissionEvaluator</span>, appelée via <span style="color: #1d4ed8; font-family: monospace;">hasPermission()</span> dans le SpEL.</div>

<div style="margin-top: 0.5rem;">Deux signatures possibles :</div>
<div style="margin-left: 1rem; margin-top: 0.5rem;">
- Par objet + permission : reçoit directement l'objet cible (ex. <span style="color: #1d4ed8; font-family: monospace;">returnObject</span> en postautorisation) et une permission (ex. un nom de rôle).<br>
- Par identifiant + type + permission : utile en préautorisation, quand on n'a que l'ID de l'objet et pas encore l'objet lui-même — l'evaluator va chercher l'objet via un repository.
</div>

<div style="margin-top: 0.5rem;">

```java
@Component
public class DocumentsPermissionEvaluator implements PermissionEvaluator {

  @Override
  public boolean hasPermission(Authentication authentication,
                                Object target, Object permission) {
    Document document = (Document) target;
    String p = (String) permission;

    boolean admin = authentication.getAuthorities().stream()
        .anyMatch(a -> a.getAuthority().equals(p));

    return admin || document.getOwner().equals(authentication.getName());
  }

  @Override
  public boolean hasPermission(Authentication authentication,
                                Serializable targetId, String targetType,
                                Object permission) {
    return false;
  }
}
```

</div>

<div style="margin-top: 0.5rem;">Utilisation dans le service :</div>

<div style="margin-top: 0.5rem;">

```java
@PostAuthorize("hasPermission(returnObject, 'ROLE_admin')")
public Document getDocument(String code) {
  return documentRepository.findDocument(code);
}
```

</div>

<div style="margin-top: 0.5rem;">Pour que Spring Security connaisse cet evaluator, il faut déclarer un bean <span style="color: #1d4ed8; font-family: monospace;">MethodSecurityExpressionHandler</span> en configuration :</div>

<div style="margin-top: 0.5rem;">

```java
@Bean
protected MethodSecurityExpressionHandler createExpressionHandler() {
  var expressionHandler = new DefaultMethodSecurityExpressionHandler();
  expressionHandler.setPermissionEvaluator(evaluator);
  return expressionHandler;
}
```

</div>

<div style="margin-top: 1rem; border-left: 3px solid #1e40af; background: #dbeafe; color: #1e3a5f; padding: 0.7rem 1rem;">Remarque : implémenter <span style="font-family: monospace;">PermissionEvaluator</span> n'est pas obligatoire. On peut aussi utiliser un simple bean Spring standard et l'appeler directement depuis l'expression SpEL avec la syntaxe <span style="font-family: monospace;">@nomDuBean</span>, en lui passant explicitement l'objet <span style="font-family: monospace;">authentication</span>. Par exemple, remplacer <span style="font-family: monospace;">@PostAuthorize("hasPermission(returnObject, 'ROLE_admin')")</span> par <span style="font-family: monospace;">@PostAuthorize("@documentsPermissionEvaluator.hasPermission(authentication, returnObject, 'ROLE_admin')")</span>, où <span style="font-family: monospace;">documentsPermissionEvaluator</span> est simplement un <span style="font-family: monospace;">@Component</span> exposant une méthode <span style="font-family: monospace;">hasPermission(Authentication, Object, String)</span>. Cette approche évite d'implémenter le contrat <span style="font-family: monospace;">PermissionEvaluator</span> et de déclarer un <span style="font-family: monospace;">MethodSecurityExpressionHandler</span>, au prix d'une syntaxe SpEL un peu moins idiomatique.</div>

<div style="margin-top: 1.5rem; font-size: 1.1rem; font-weight: bold; color: #1e40af;">6. Annotations alternatives : @Secured et @RolesAllowed</div>
<div style="margin-top: 0.5rem;">Moins puissantes que <span style="color: #1d4ed8; font-family: monospace;">@PreAuthorize</span>/<span style="color: #1d4ed8; font-family: monospace;">@PostAuthorize</span>, rarement utilisées en pratique. À activer explicitement via <span style="color: #1d4ed8; font-family: monospace;">@EnableMethodSecurity(jsr250Enabled = true, securedEnabled = true)</span>.</div>

<div style="margin-top: 0.5rem;">

```java
@RolesAllowed("ADMIN")
public String getName() { return "Fantastico"; }

@Secured("ROLE_ADMIN")
public String getName() { return "Fantastico"; }
```

</div>

<div style="margin-top: 1.5rem; border-left: 3px solid #1e40af; background: #dbeafe; color: #1e3a5f; padding: 0.7rem 1rem;">
À retenir : la method security s'active avec <span style="font-family: monospace;">@EnableMethodSecurity</span> et fonctionne sur n'importe quelle couche (contrôleur, service, repository...). La préautorisation bloque l'appel avant exécution ; la postautorisation laisse la méthode s'exécuter et filtre le résultat. Pour une logique d'autorisation complexe, mieux vaut externaliser dans un <span style="font-family: monospace;">PermissionEvaluator</span> plutôt que d'empiler des expressions SpEL longues.
</div>

</div>
