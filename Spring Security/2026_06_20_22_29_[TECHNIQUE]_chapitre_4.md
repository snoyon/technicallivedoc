---
type: Document
title: Chapitre 4
---

<div style="font-family: Arial, sans-serif; color: #1f2937; line-height: 1.6;"><span style="color: #1e40af; font-weight: bold; font-size: 1.4rem;">Chapitre 4 — Managing Passwords (Spring Security in Action)</span><div style="margin-top: 0.5rem; color: #4b5563; font-style: italic;">Le contrat PasswordEncoder et les outils du Spring Security Crypto Module (SSCM)</div><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">1. Vue d'ensemble du chapitre</span></div><div style="margin-top: 0.5rem;">Après la gestion des utilisateurs (chapitre 3), ce chapitre traite de la gestion des mots de passe et secrets. Deux grands axes :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">• Le contrat <span style="color: #1d4ed8; font-family: monospace;">PasswordEncoder</span> et ses implémentations</div><div style="margin-left: 1rem;">• Le Spring Security Crypto Module (SSCM) : générateurs de clés et chiffreurs (encryptors)</div><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">2. Le contrat PasswordEncoder</span></div><div style="margin-top: 0.5rem;">Dans le flux d'authentification, l'<span style="color: #1d4ed8; font-family: monospace;">AuthenticationProvider</span> utilise le <span style="color: #1d4ed8; font-family: monospace;">PasswordEncoder</span> pour valider le mot de passe. L'interface :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
public interface PasswordEncoder {

  String encode(CharSequence rawPassword);
  boolean matches(CharSequence rawPassword, String encodedPassword);

  default boolean upgradeEncoding(String encodedPassword) {
    return false;
  }
}
```

</div><div style="margin-left: 1rem; margin-top: 0.5rem;">• <span style="color: #1d4ed8; font-family: monospace;">encode()</span> — transforme (chiffre ou hache) le mot de passe brut</div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">matches()</span> — vérifie qu'un mot de passe brut correspond à un mot de passe encodé</div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">upgradeEncoding()</span> — méthode par défaut (<span style="color: #1d4ed8; font-family: monospace;">false</span>) ; si surchargée à <span style="color: #1d4ed8; font-family: monospace;">true</span>, le mot de passe encodé est ré-encodé pour plus de sécurité</div><div style="margin-top: 0.5rem; border-left: 3px solid #1e40af; background-color: #dbeafe; color: #1e3a5f; padding: 0.75rem 1rem; border-radius: 0 4px 4px 0;">Note — Les deux méthodes abstraites sont fortement liées : une chaîne produite par <span style="color: #1d4ed8; font-family: monospace;">encode()</span> doit toujours être vérifiable par <span style="color: #1d4ed8; font-family: monospace;">matches()</span> du même encodeur.</div><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">3. Implémenter son propre PasswordEncoder</span></div><div style="margin-top: 0.5rem;">Deux exemples pédagogiques :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">• <span style="color: #1d4ed8; font-family: monospace;">PlainTextPasswordEncoder</span> — ne transforme rien, <span style="color: #1d4ed8; font-family: monospace;">matches()</span> compare juste les chaînes par <span style="color: #1d4ed8; font-family: monospace;">equals()</span> (équivalent du <span style="color: #1d4ed8; font-family: monospace;">NoOpPasswordEncoder</span> vu au chapitre 2)</div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">Sha512PasswordEncoder</span> — hache avec SHA-512 via <span style="color: #1d4ed8; font-family: monospace;">MessageDigest</span> ; <span style="color: #1d4ed8; font-family: monospace;">matches()</span> ré-encode le mot de passe brut et compare les deux hashs</div><div style="margin-top: 0.5rem;">Extrait de hachage SHA-512 :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
private String hashWithSHA512(String input) {
  StringBuilder result = new StringBuilder();
  try {
    MessageDigest md = MessageDigest.getInstance("SHA-512");
    byte [] digested = md.digest(input.getBytes());
    for (int i = 0; i < digested.length; i++) {
       result.append(Integer.toHexString(0xFF & digested[i]));
    }
  } catch (NoSuchAlgorithmException e) {
    throw new RuntimeException("Bad algorithm");
  }
  return result.toString();
}
```

</div><div style="margin-top: 0.5rem; border-left: 3px solid #b45309; background-color: #fef3c7; color: #7c2d12; padding: 0.75rem 1rem; border-radius: 0 4px 4px 0;">Avertissement — Ces deux implémentations sont purement pédagogiques (notamment l'absence de salage pour SHA-512). En pratique, mieux vaut utiliser les implémentations fournies par Spring Security, vues juste après.</div><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">4. Les implémentations fournies par Spring Security</span></div><div style="margin-top: 0.5rem;">Cinq implémentations sont présentées :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">• <span style="color: #1d4ed8; font-family: monospace;">NoOpPasswordEncoder</span> — pas d'encodage, cleartext. Réservé aux exemples, jamais en production</div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">StandardPasswordEncoder</span> — SHA-256. <strong>Dépréciée</strong>, jugée trop faible aujourd'hui ; à éviter pour du nouveau code</div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">Pbkdf2PasswordEncoder</span> — PBKDF2 (HMAC répété un grand nombre de fois)</div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">BCryptPasswordEncoder</span> — fonction de hachage bcrypt</div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">SCryptPasswordEncoder</span> — fonction de hachage scrypt</div><div style="margin-top: 0.5rem;"><span style="color: #1e40af; font-weight: bold;">NoOpPasswordEncoder</span> — singleton, instance via méthode statique :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
PasswordEncoder p = NoOpPasswordEncoder.getInstance();
```

</div><div style="margin-top: 0.5rem;"><span style="color: #1e40af; font-weight: bold;">StandardPasswordEncoder</span> — accepte un secret optionnel (vide par défaut) :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
PasswordEncoder p = new StandardPasswordEncoder();
PasswordEncoder p = new StandardPasswordEncoder("secret");
```

</div><div style="margin-top: 0.5rem;"><span style="color: #1e40af; font-weight: bold;">Pbkdf2PasswordEncoder</span> — clé secrète, nombre d'itérations, taille du hash, algorithme HMAC :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
PasswordEncoder p =
   new Pbkdf2PasswordEncoder("secret", 16, 310000, Pbkdf2PasswordEncoder.SecretKeyFactoryAlgorithm.PBKDF2WithHmacSHA256);
```

</div><div style="margin-left: 1rem; margin-top: 0.5rem;">Variantes possibles pour le 4ème paramètre : <span style="color: #1d4ed8; font-family: monospace;">PBKDF2WithHmacSHA1</span>, <span style="color: #1d4ed8; font-family: monospace;">PBKDF2WithHmacSHA256</span>, <span style="color: #1d4ed8; font-family: monospace;">PBKDF2WithHmacSHA512</span>. Plus d'itérations / hash plus long = plus robuste, mais plus coûteux en ressources : un compromis est nécessaire.</div><div style="margin-top: 0.5rem;"><span style="color: #1e40af; font-weight: bold;">BCryptPasswordEncoder</span> — log rounds (4 à 31, nombre d'itérations = 2^log rounds) et <span style="color: #1d4ed8; font-family: monospace;">SecureRandom</span> optionnels :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
PasswordEncoder p = new BCryptPasswordEncoder();
PasswordEncoder p = new BCryptPasswordEncoder(4);

SecureRandom s = SecureRandom.getInstanceStrong();
PasswordEncoder p = new BCryptPasswordEncoder(4, s);
```

</div><div style="margin-top: 0.5rem;"><span style="color: #1e40af; font-weight: bold;">SCryptPasswordEncoder</span> — constructeur à 5 paramètres permettant de régler coût CPU, coût mémoire, longueur de clé et longueur de salt (pas de détail du code dans le texte, renvoie à une figure du livre).</div><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">5. DelegatingPasswordEncoder</span></div><div style="margin-top: 0.5rem;">Cas d'usage typique : faire coexister plusieurs algorithmes d'encodage (par exemple lors d'une migration suite à une vulnérabilité découverte sur l'algorithme en place) sans devoir réencoder tous les mots de passe existants.</div><div style="margin-top: 0.5rem;">Principe : le <span style="color: #1d4ed8; font-family: monospace;">DelegatingPasswordEncoder</span> ne fait pas lui-même l'encodage — il délègue à l'implémentation correspondant au préfixe du hash (ex. <span style="color: #1d4ed8; font-family: monospace;">{bcrypt}</span>, <span style="color: #1d4ed8; font-family: monospace;">{noop}</span>, <span style="color: #1d4ed8; font-family: monospace;">{scrypt}</span>), via une map clé → <span style="color: #1d4ed8; font-family: monospace;">PasswordEncoder</span>.</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
@Configuration
public class ProjectConfig {

  @Bean
  public PasswordEncoder passwordEncoder() {
    Map<String, PasswordEncoder> encoders = new HashMap<>();

    encoders.put("noop", NoOpPasswordEncoder.getInstance());
    encoders.put("bcrypt", new BCryptPasswordEncoder());
    encoders.put("scrypt", new SCryptPasswordEncoder());

    return new DelegatingPasswordEncoder("bcrypt", encoders);
  }
}
```

</div><div style="margin-top: 0.5rem; border-left: 3px solid #1e40af; background-color: #dbeafe; color: #1e3a5f; padding: 0.75rem 1rem; border-radius: 0 4px 4px 0;">Note — Les accolades font partie du préfixe (ex. <span style="color: #1d4ed8; font-family: monospace;">{noop}12345</span>). Le premier paramètre du constructeur (<span style="color: #1d4ed8; font-family: monospace;">"bcrypt"</span> ici) définit l'encodeur par défaut, utilisé si le hash n'a aucun préfixe.</div><div style="margin-top: 0.5rem; border-left: 3px solid #1e40af; background-color: #dbeafe; color: #1e3a5f; padding: 0.75rem 1rem; border-radius: 0 4px 4px 0;">Note — C'est le <span style="color: #1d4ed8; font-family: monospace;">DelegatingPasswordEncoder</span> lui-même qui ajoute le préfixe lors de l'appel à <span style="color: #1d4ed8; font-family: monospace;">encode()</span> : il délègue l'encodage à l'implémentation par défaut, puis enrobe le résultat avec la clé correspondante (ex. <span style="color: #1d4ed8; font-family: monospace;">{bcrypt}</span>). L'encodeur délégué (<span style="color: #1d4ed8; font-family: monospace;">BCryptPasswordEncoder</span> dans cet exemple) ne connaît rien du préfixe, il renvoie un hash brut. Symétriquement, lors de <span style="color: #1d4ed8; font-family: monospace;">matches()</span>, le <span style="color: #1d4ed8; font-family: monospace;">DelegatingPasswordEncoder</span> lit le préfixe pour choisir l'encodeur à utiliser, puis lui transmet le hash sans ce préfixe.</div><div style="margin-top: 0.5rem;">Raccourci pratique fourni par Spring Security, avec mapping complet et bcrypt comme défaut :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
PasswordEncoder passwordEncoder = PasswordEncoderFactories.createDelegatingPasswordEncoder();
```

</div><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">6. Encodage, chiffrement, hachage : clarification des termes</span></div><div style="margin-left: 1rem; margin-top: 0.5rem;">• <strong>Encodage</strong> — toute transformation d'une entrée (ex. inverser une chaîne)</div><div style="margin-left: 1rem;">• <strong>Chiffrement</strong> — encodage paramétré par une clé : <span style="color: #1d4ed8; font-family: monospace;">(x, k) → y</span>. Si la même clé sert au déchiffrement, on parle de clé symétrique ; sinon de paire clé publique/clé privée (chiffrement asymétrique)</div><div style="margin-left: 1rem;">• <strong>Hachage</strong> — encodage à sens unique : <span style="color: #1d4ed8; font-family: monospace;">x → y</span>, irréversible, mais vérifiable via une fonction de correspondance <span style="color: #1d4ed8; font-family: monospace;">(x, y) → boolean</span>. Un sel (<em>salt</em>) aléatoire peut être ajouté à l'entrée pour renforcer la fonction : <span style="color: #1d4ed8; font-family: monospace;">(x, k) → y</span></div><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">7. Tableau récapitulatif des contrats d'authentification</span></div><table style="width: 100%; border-collapse: collapse; margin-top: 0.5rem;"><tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;"><td style="padding: 6px 10px; font-weight: bold;">Contrat</td><td style="padding: 6px 10px; font-weight: bold;">Description</td></tr><tr style="background-color: #ffffff;"><td style="padding: 6px 10px;"><span style="color: #1d4ed8; font-family: monospace;">UserDetails</span></td><td style="padding: 6px 10px;">Représente l'utilisateur tel que vu par Spring Security.</td></tr><tr style="background-color: #fafafa;"><td style="padding: 6px 10px;"><span style="color: #1d4ed8; font-family: monospace;">GrantedAuthority</span></td><td style="padding: 6px 10px;">Définit une action permise à l'utilisateur (lecture, écriture, suppression, etc.).</td></tr><tr style="background-color: #ffffff;"><td style="padding: 6px 10px;"><span style="color: #1d4ed8; font-family: monospace;">UserDetailsService</span></td><td style="padding: 6px 10px;">Récupère les détails d'un utilisateur par son nom.</td></tr><tr style="background-color: #fafafa;"><td style="padding: 6px 10px;"><span style="color: #1d4ed8; font-family: monospace;">UserDetailsManager</span></td><td style="padding: 6px 10px;">Étend UserDetailsService ; permet aussi de créer/modifier des utilisateurs.</td></tr><tr style="background-color: #ffffff;"><td style="padding: 6px 10px;"><span style="color: #1d4ed8; font-family: monospace;">PasswordEncoder</span></td><td style="padding: 6px 10px;">Définit comment chiffrer/hacher un mot de passe et vérifier une correspondance.</td></tr></table><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">8. Le Spring Security Crypto Module (SSCM)</span></div><div style="margin-top: 0.5rem;">Le SSCM est la brique cryptographie de Spring Security ; il évite d'ajouter une dépendance externe pour générer des clés ou chiffrer des données. Deux familles d'outils :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">• Générateurs de clés (<span style="color: #1d4ed8; font-family: monospace;">key generators</span>)</div><div style="margin-left: 1rem;">• Chiffreurs (<span style="color: #1d4ed8; font-family: monospace;">encryptors</span>)</div><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">9. Les générateurs de clés</span></div><div style="margin-top: 0.5rem;">Deux contrats, construits via la factory <span style="color: #1d4ed8; font-family: monospace;">KeyGenerators</span> :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
public interface StringKeyGenerator {
    String generateKey();
}
```

</div><div style="margin-left: 1rem; margin-top: 0.5rem;">Génère une clé de 8 octets encodée en hexadécimal, typiquement utilisée comme salt :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
StringKeyGenerator keyGenerator = KeyGenerators.string();
String salt = keyGenerator.generateKey();
```

</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
public interface BytesKeyGenerator {
  int getKeyLength();
  byte[] generateKey();
}
```

</div><div style="margin-left: 1rem; margin-top: 0.5rem;">Clé aléatoire de 8 octets par défaut, longueur personnalisable :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
BytesKeyGenerator keyGenerator = KeyGenerators.secureRandom();
byte [] key = keyGenerator.generateKey();
int keyLength = keyGenerator.getKeyLength();

BytesKeyGenerator keyGenerator2 = KeyGenerators.secureRandom(16);
```

</div><div style="margin-top: 0.5rem; border-left: 3px solid #1e40af; background-color: #dbeafe; color: #1e3a5f; padding: 0.75rem 1rem; border-radius: 0 4px 4px 0;">Note — <span style="color: #1d4ed8; font-family: monospace;">KeyGenerators.secureRandom()</span> produit une clé différente à chaque appel. Si l'on veut au contraire une clé stable et réutilisable, on utilise <span style="color: #1d4ed8; font-family: monospace;">KeyGenerators.shared(int length)</span> : tous les appels à <span style="color: #1d4ed8; font-family: monospace;">generateKey()</span> renvoient alors la même valeur.</div><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">10. Les chiffreurs (encryptors)</span></div><div style="margin-top: 0.5rem; border-left: 3px solid #b45309; background-color: #fef3c7; color: #7c2d12; padding: 0.75rem 1rem; border-radius: 0 4px 4px 0;">Avertissement — Les encryptors ne servent <strong>pas</strong> à protéger des mots de passe. Le chiffrement est réversible par construction (on peut retrouver la valeur d'origine avec la bonne clé), alors qu'un mot de passe doit être haché, c'est-à-dire transformé de façon irréversible (voir section 6). Si on chiffrait des mots de passe, toute personne obtenant la clé de déchiffrement (ou compromettant l'application qui la détient) pourrait retrouver tous les mots de passe en clair d'un coup — exactement le risque que le hachage cherche à éliminer. Les encryptors visent un autre besoin : des données qu'on doit pouvoir relire en clair plus tard (secrets de configuration, clé d'API d'un service tiers, données échangées entre composants du système), pas des données qu'on doit seulement vérifier par correspondance.</div><div style="margin-top: 0.5rem;">Deux contrats selon le type de données manipulées :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
public interface TextEncryptor {
  String encrypt(String text);
  String decrypt(String encryptedText);
}

public interface BytesEncryptor {
  byte[] encrypt(byte[] byteArray);
  byte[] decrypt(byte[] encryptedByteArray);
}
```

</div><div style="margin-top: 0.5rem;">La factory <span style="color: #1d4ed8; font-family: monospace;">Encryptors</span> propose plusieurs niveaux :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">• <span style="color: #1d4ed8; font-family: monospace;">Encryptors.standard(password, salt)</span> — AES 256 bits en mode CBC (considéré plus faible)</div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">Encryptors.stronger(password, salt)</span> — AES 256 bits en mode GCM (plus robuste)</div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">Encryptors.text(password, salt)</span> — TextEncryptor basé sur <span style="color: #1d4ed8; font-family: monospace;">standard()</span></div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">Encryptors.delux(password, salt)</span> — TextEncryptor basé sur <span style="color: #1d4ed8; font-family: monospace;">stronger()</span></div><div style="margin-left: 1rem;">• <span style="color: #1d4ed8; font-family: monospace;">Encryptors.noOpText()</span> — TextEncryptor factice (ne chiffre rien), utile pour les démos ou les tests de performance</div><div style="margin-top: 0.5rem;">Exemple avec BytesEncryptor :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
String salt = KeyGenerators.string().generateKey();
String password = "secret";
String valueToEncrypt = "HELLO";

BytesEncryptor e = Encryptors.standard(password, salt);
byte [] encrypted = e.encrypt(valueToEncrypt.getBytes());
byte [] decrypted = e.decrypt(encrypted);
```

</div><div style="margin-top: 0.5rem;">Exemple avec TextEncryptor :</div><div style="margin-left: 1rem; margin-top: 0.5rem;">

```java
String salt = KeyGenerators.string().generateKey();
String password = "secret";
String valueToEncrypt = "HELLO";

TextEncryptor e = Encryptors.text(password, salt);
String encrypted = e.encrypt(valueToEncrypt);
String decrypted = e.decrypt(encrypted);
```

</div><div style="margin-top: 1.5rem;"><span style="color: #1e40af; font-weight: bold; font-size: 1.1rem;">11. Points clés à retenir</span></div><div style="margin-left: 1rem; margin-top: 0.5rem;">• <span style="color: #1d4ed8; font-family: monospace;">PasswordEncoder</span> porte une des responsabilités les plus critiques du flux d'authentification</div><div style="margin-left: 1rem;">• Spring Security fournit plusieurs algorithmes de hachage prêts à l'emploi : le choix devient une question de configuration, pas d'implémentation</div><div style="margin-left: 1rem;">• Le SSCM fournit générateurs de clés et chiffreurs, évitant une dépendance crypto externe</div><div style="margin-left: 1rem;">• Les générateurs de clés produisent des clés pour des algorithmes cryptographiques (souvent des salts)</div><div style="margin-left: 1rem;">• Les chiffreurs permettent de chiffrer/déchiffrer des données, en bytes ou en texte</div></div>
