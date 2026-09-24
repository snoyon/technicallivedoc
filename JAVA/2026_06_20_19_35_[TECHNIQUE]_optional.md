---
type: Document
title: Optional
---

<div style="font-family: 'Segoe UI', Arial, sans-serif; max-width: 860px; margin: 0 auto; padding: 24px; background: #f3f4f6; color: #1f2937; font-size: 14px; line-height: 1.6;">

<div style="background: #1e3a8a; color: white; padding: 18px 24px; border-radius: 8px; margin-bottom: 24px;">
  <div style="font-size: 20px; font-weight: bold;">Optional&lt;T&gt; — Fiche de référence Java</div>
  <div style="font-size: 12px; opacity: 0.8; margin-top: 4px;">java.util.Optional — encapsuler une valeur potentiellement absente</div>
</div>

<!-- Création -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 12px;">🏗️ Création</div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Méthode</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Comportement</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Exemple</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Optional.of(v)</span></td>
        <td style="padding: 7px 10px;">Lève <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">NullPointerException</span> si v est null</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">Optional.of("Alice")</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Optional.ofNullable(v)</span></td>
        <td style="padding: 7px 10px;">Accepte null → retourne un Optional vide dans ce cas</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">Optional.ofNullable(nom)</span></td>
      </tr>
      <tr>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Optional.empty()</span></td>
        <td style="padding: 7px 10px;">Optional vide explicite</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">Optional&lt;String&gt; o = Optional.empty()</span></td>
      </tr>
    </tbody>
  </table>
</div>

<!-- Tester la présence -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 12px;">🔎 Tester la présence</div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Méthode</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Retour</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Exemple</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">isPresent()</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">true</span> si une valeur est présente</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">if (opt.isPresent()) { ... }</span></td>
      </tr>
      <tr>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">isEmpty()</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">true</span> si vide (Java 11+)</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">if (opt.isEmpty()) { ... }</span></td>
      </tr>
    </tbody>
  </table>
</div>

<!-- Récupérer la valeur -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 12px;">📤 Récupérer la valeur</div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Méthode</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Comportement si vide</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Exemple</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">get()</span></td>
        <td style="padding: 7px 10px;">Lève <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">NoSuchElementException</span> — déconseillé seul</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">String s = opt.get()</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">orElse(x)</span></td>
        <td style="padding: 7px 10px;">Retourne x — <strong>toujours évalué</strong>, même si valeur présente</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">opt.orElse("inconnu")</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">orElseGet(supplier)</span></td>
        <td style="padding: 7px 10px;">Retourne supplier.get() — évalué <strong>seulement si vide</strong> (paresseux)</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">opt.orElseGet(() -> chargerDepuisBDD())</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">orElseThrow()</span></td>
        <td style="padding: 7px 10px;">Lève <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">NoSuchElementException</span> (Java 10+)</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">String s = opt.orElseThrow()</span></td>
      </tr>
      <tr>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">orElseThrow(supplier)</span></td>
        <td style="padding: 7px 10px;">Lève l'exception personnalisée fournie par le supplier</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">opt.orElseThrow(() -> new IllegalStateException())</span></td>
      </tr>
    </tbody>
  </table>
</div>

<!-- Traiter conditionnellement -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 12px;">⚡ Traiter la valeur si présente</div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Méthode</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Comportement</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Exemple</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">ifPresent(consumer)</span></td>
        <td style="padding: 7px 10px;">Exécute consumer si valeur présente, sinon ne fait rien</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">opt.ifPresent(System.out::println)</span></td>
      </tr>
      <tr>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">ifPresentOrElse(c, r)</span></td>
        <td style="padding: 7px 10px;">Exécute c si présent, sinon exécute le Runnable r (Java 9+)</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">opt.ifPresentOrElse(v -&gt; log(v), () -&gt; log("vide"))</span></td>
      </tr>
    </tbody>
  </table>
</div>

<!-- Transformation -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 12px;">🔄 Transformer le contenu (style stream)</div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Méthode</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Comportement</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Exemple</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">map(fn)</span></td>
        <td style="padding: 7px 10px;">fn: T → R. Transforme la valeur si présente, sinon reste vide</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">opt.map(String::toUpperCase)</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">flatMap(fn)</span></td>
        <td style="padding: 7px 10px;">fn: T → Optional&lt;R&gt;. Évite l'imbrication Optional&lt;Optional&lt;T&gt;&gt;</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">opt.flatMap(repository::findById)</span></td>
      </tr>
      <tr>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">filter(predicate)</span></td>
        <td style="padding: 7px 10px;">Garde la valeur si le prédicat est vrai, sinon devient vide</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">opt.filter(s -&gt; s.length() &gt; 3)</span></td>
      </tr>
    </tbody>
  </table>
</div>

<!-- Stream -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 10px;">🌊 Conversion en Stream</div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Méthode</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Comportement</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Exemple</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">stream()</span></td>
        <td style="padding: 7px 10px;">Stream avec 0 ou 1 élément (Java 9+)</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">opt.stream().count()</span></td>
      </tr>
    </tbody>
  </table>
  <div style="font-size: 13px; color: #374151; margin-top: 10px;">
    Pratique avec <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">flatMap</span> dans un pipeline de streams :
  </div>

```java
List<String> noms = ids.stream()
    .map(repository::findById)
    .flatMap(Optional::stream)
    .map(Utilisateur::getNom)
    .toList();
```

</div>

<!-- Exemple combiné -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 12px;">💡 Exemple combiné</div>

```java
public String trouverNomUtilisateur(Long id) {
    return repository.findById(id)
        .map(Utilisateur::getNom)
        .filter(nom -> !nom.isBlank())
        .orElseThrow(() -> new UtilisateurNonTrouveException(id));
}
```

</div>

<!-- Attention -->
<div style="background: #fef9c3; border: 1px solid #fde047; border-radius: 8px; padding: 14px 18px; margin-bottom: 18px; font-size: 13px;">
  <span style="font-weight: bold; color: #713f12;">⚠️ Attention</span>
  <div style="margin-top: 6px; color: #713f12;">
    <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">orElse(x)</span> évalue x systématiquement, même si l'Optional contient déjà une valeur. Si x est coûteux à calculer (appel réseau, requête BDD), utiliser <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">orElseGet</span> pour bénéficier de l'évaluation paresseuse.
  </div>
</div>

<!-- Bonnes pratiques -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 10px;">✅ Bonnes pratiques</div>
  <div style="font-size: 13px; color: #374151; line-height: 1.7;">
    • Ne jamais utiliser <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Optional</span> comme type de champ de classe ou de paramètre de méthode — réservé aux types de retour.<br>
    • Ne jamais appeler <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">get()</span> sans vérifier <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">isPresent()</span> au préalable ; préférer <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">orElseThrow</span>/<span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">orElse</span>/<span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">map</span>.<br>
    • Éviter <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Optional&lt;List&lt;T&gt;&gt;</span> : retourner une liste vide plutôt qu'un Optional vide.<br>
    • Privilégier les chaînes fonctionnelles (<span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">map</span>/<span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">filter</span>/<span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">orElseThrow</span>) plutôt que <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">isPresent()</span> + <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">get()</span> imbriqués.
  </div>
</div>

</div>
