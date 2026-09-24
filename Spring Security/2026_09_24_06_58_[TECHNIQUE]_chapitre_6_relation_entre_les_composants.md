---
type: Document
title: Chapitre 6 - Relation entre les composants
---

<div style="background:#f3f4f6;color:#1f2937;padding:1.5rem;font-family:sans-serif;line-height:1.6;">
<div style="background:#1e3a8a;color:#ffffff;padding:1.2rem 1.5rem;border-radius:8px;margin-bottom:1rem;">
<span style="font-size:1.4rem;font-weight:bold;">Spring Security : UserDetailsService, UserDetails, GrantedAuthority et Authentication</span>
<div style="opacity:0.8;margin-top:0.3rem;">Relations entre les contrats clés de l'authentification par identifiant et mot de passe</div>
</div>
<div style="background:#ffffff;border-radius:8px;border-left:4px solid #1d4ed8;padding:1rem 1.2rem;margin-bottom:1rem;">
<div style="color:#1e40af;font-weight:bold;font-size:1.1rem;">🎯 Vue d'ensemble</div>
<div style="margin-top:0.5rem;">Le <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">DaoAuthenticationProvider</span> orchestre l'authentification : il appelle le <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UserDetailsService</span> pour charger un <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UserDetails</span> (qui porte des <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">GrantedAuthority</span>), puis produit un <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">Authentication</span> authentifié. Cette classe est standard (package <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">org.springframework.security.authentication.dao</span>) : avec Spring Boot, exposer un bean <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UserDetailsService</span> et un <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">PasswordEncoder</span> suffit à la mettre en place.</div>
</div>
<div style="background:#ffffff;border-radius:8px;border-left:4px solid #1d4ed8;padding:1rem 1.2rem;margin-bottom:1rem;">
<div style="color:#1e40af;font-weight:bold;font-size:1.1rem;">📋 Les quatre contrats</div>
<table style="width:100%;border-collapse:collapse;margin-top:0.6rem;">
<tr>
<th style="background:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;padding:8px 10px;text-align:left;">Contrat</th>
<th style="background:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;padding:8px 10px;text-align:left;">Rôle</th>
<th style="background:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;padding:8px 10px;text-align:left;">Méthodes clés</th>
<th style="background:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;padding:8px 10px;text-align:left;">Implémentation standard</th>
</tr>
<tr>
<td style="background:#ffffff;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UserDetailsService</span></td>
<td style="background:#ffffff;padding:8px 10px;vertical-align:top;">Charge un utilisateur à partir de son nom (base, LDAP, etc.)</td>
<td style="background:#ffffff;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UserDetails loadUserByUsername(String)</span></td>
<td style="background:#ffffff;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">InMemoryUserDetailsManager</span>, <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">JdbcUserDetailsManager</span></td>
</tr>
<tr>
<td style="background:#fafafa;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UserDetails</span></td>
<td style="background:#fafafa;padding:8px 10px;vertical-align:top;">Représente l'utilisateur tel que Spring Security le voit</td>
<td style="background:#fafafa;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">getUsername()</span>, <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">getPassword()</span>, <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">getAuthorities()</span>, <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">isEnabled()</span>, <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">isAccountNonLocked()</span>, <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">isAccountNonExpired()</span>, <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">isCredentialsNonExpired()</span></td>
<td style="background:#fafafa;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">User</span> (builder <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">User.withUsername()</span>)</td>
</tr>
<tr>
<td style="background:#ffffff;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">GrantedAuthority</span></td>
<td style="background:#ffffff;padding:8px 10px;vertical-align:top;">Droit accordé : un rôle (<span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">ROLE_ADMIN</span>) ou une permission (<span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">READ_ORDERS</span>)</td>
<td style="background:#ffffff;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">String getAuthority()</span></td>
<td style="background:#ffffff;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">SimpleGrantedAuthority</span></td>
</tr>
<tr>
<td style="background:#fafafa;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">Authentication</span></td>
<td style="background:#fafafa;padding:8px 10px;vertical-align:top;">Jeton d'authentification : demande de connexion, puis identité courante</td>
<td style="background:#fafafa;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">getPrincipal()</span>, <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">getCredentials()</span>, <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">getAuthorities()</span>, <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">isAuthenticated()</span></td>
<td style="background:#fafafa;padding:8px 10px;vertical-align:top;"><span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UsernamePasswordAuthenticationToken</span></td>
</tr>
</table>
</div>
<div style="background:#ffffff;border-radius:8px;border-left:4px solid #1d4ed8;padding:1rem 1.2rem;margin-bottom:1rem;">
<div style="color:#1e40af;font-weight:bold;font-size:1.1rem;">🔗 Relations entre les contrats</div>
<table style="width:100%;border-collapse:collapse;margin-top:0.6rem;">
<tr>
<th style="background:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;padding:8px 10px;text-align:left;">Source</th>
<th style="background:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;padding:8px 10px;text-align:left;">Relation</th>
<th style="background:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;padding:8px 10px;text-align:left;">Cible</th>
</tr>
<tr>
<td style="background:#ffffff;padding:8px 10px;">DaoAuthenticationProvider</td>
<td style="background:#ffffff;padding:8px 10px;">appelle</td>
<td style="background:#ffffff;padding:8px 10px;">UserDetailsService</td>
</tr>
<tr>
<td style="background:#fafafa;padding:8px 10px;">UserDetailsService</td>
<td style="background:#fafafa;padding:8px 10px;">retourne</td>
<td style="background:#fafafa;padding:8px 10px;">UserDetails</td>
</tr>
<tr>
<td style="background:#ffffff;padding:8px 10px;">UserDetails</td>
<td style="background:#ffffff;padding:8px 10px;">contient 0..n</td>
<td style="background:#ffffff;padding:8px 10px;">GrantedAuthority</td>
</tr>
<tr>
<td style="background:#fafafa;padding:8px 10px;">UserDetails</td>
<td style="background:#fafafa;padding:8px 10px;">sert de principal à</td>
<td style="background:#fafafa;padding:8px 10px;">Authentication</td>
</tr>
<tr>
<td style="background:#ffffff;padding:8px 10px;">GrantedAuthority</td>
<td style="background:#ffffff;padding:8px 10px;">reprises comme authorities dans</td>
<td style="background:#ffffff;padding:8px 10px;">Authentication</td>
</tr>
<tr>
<td style="background:#fafafa;padding:8px 10px;">DaoAuthenticationProvider</td>
<td style="background:#fafafa;padding:8px 10px;">crée si succès</td>
<td style="background:#fafafa;padding:8px 10px;">Authentication</td>
</tr>
</table>
</div>
<div style="background:#ffffff;border-radius:8px;border-left:4px solid #1d4ed8;padding:1rem 1.2rem;margin-bottom:1rem;">
<div style="color:#1e40af;font-weight:bold;font-size:1.1rem;">🔄 Enchaînement lors d'une authentification</div>
<div style="margin-top:0.5rem;">1. Le filtre (<span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UsernamePasswordAuthenticationFilter</span>) crée un <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">Authentication</span> non authentifié avec le login et le mot de passe saisis.</div>
<div style="margin-top:0.3rem;">2. Le <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">ProviderManager</span> délègue au <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">DaoAuthenticationProvider</span>.</div>
<div style="margin-top:0.3rem;">3. Le provider appelle <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">loadUserByUsername(username)</span> et récupère un <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UserDetails</span>.</div>
<div style="margin-top:0.3rem;">4. Il contrôle l'état du compte (verrouillé, désactivé, expiré) puis compare le mot de passe via <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">PasswordEncoder.matches()</span>.</div>
<div style="margin-top:0.3rem;">5. Si tout est valide, il crée un nouvel <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">Authentication</span> authentifié : principal = <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UserDetails</span>, authorities = <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">getAuthorities()</span>.</div>
<div style="margin-top:0.3rem;">6. Les credentials sont effacés, puis l'<span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">Authentication</span> est stocké dans le <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">SecurityContext</span> (accessible via <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">SecurityContextHolder</span>).</div>
</div>
<div style="background:#ffffff;border-radius:8px;border-left:4px solid #1d4ed8;padding:1rem 1.2rem;margin-bottom:1rem;">
<div style="color:#1e40af;font-weight:bold;font-size:1.1rem;">💻 Exemple : UserDetailsService personnalisé</div>
<div style="margin-top:0.5rem;">Chargement d'un utilisateur applicatif et conversion en <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">UserDetails</span> :</div>

```java
@Service
public class AppUserDetailsService implements UserDetailsService {

    private final UserRepository userRepository;

    public AppUserDetailsService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        return userRepository.findByUsername(username)
            .map(u -> User.withUsername(u.getUsername())
                .password(u.getPasswordHash())
                .authorities(u.getPermissions().toArray(String[]::new))
                .build())
            .orElseThrow(() -> new UsernameNotFoundException("Utilisateur introuvable : " + username));
    }
}
```

<div style="margin-top:0.5rem;">Lecture de l'utilisateur courant depuis le <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">SecurityContext</span> :</div>

```java
Authentication auth = SecurityContextHolder.getContext().getAuthentication();
UserDetails user = (UserDetails) auth.getPrincipal();
Collection<? extends GrantedAuthority> authorities = auth.getAuthorities();
```

<div style="margin-top:0.5rem;">Résultat possible de <span style="background:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">auth.getAuthorities()</span> : <span style="background:#f0fdf4;color:#166534;padding:1px 5px;border-radius:3px;">[ROLE_ADMIN, READ_ORDERS]</span></div>
</div>
<div style="background:#fef9c3;border:1px solid #fde047;color:#713f12;border-radius:8px;padding:1rem 1.2rem;margin-bottom:1rem;">
<div style="font-weight:bold;font-size:1.1rem;">⚠️ Points d'attention</div>
<div style="margin-top:0.5rem;">• <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">loadUserByUsername()</span> doit lever <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">UsernameNotFoundException</span> si l'utilisateur n'existe pas, jamais retourner <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">null</span>.</div>
<div style="margin-top:0.3rem;">• Le builder <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">roles("ADMIN")</span> ajoute le préfixe <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">ROLE_</span> : c'est équivalent à <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">authorities("ROLE_ADMIN")</span>. Côté autorisation, <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">hasRole("ADMIN")</span> correspond à <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">hasAuthority("ROLE_ADMIN")</span>.</div>
<div style="margin-top:0.3rem;">• Avant authentification, le principal de l'<span style="background:#fef08a;padding:1px 5px;border-radius:3px;">Authentication</span> est le simple login saisi. Après, il est en général un <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">UserDetails</span>, mais ce n'est pas garanti (OAuth2, JWT : <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">Jwt</span>, <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">OidcUser</span>, etc.).</div>
<div style="margin-top:0.3rem;">• Par défaut, le <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">ProviderManager</span> efface les credentials après une authentification réussie : ne pas compter sur <span style="background:#fef08a;padding:1px 5px;border-radius:3px;">getCredentials()</span> une fois connecté.</div>
</div>
</div>

![image](/images/spring_security_1790226051192.png)

