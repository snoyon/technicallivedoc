---
type: Document
title: Interfaces Fonctionnelles Et Lambda
---

<div style="font-family: 'Segoe UI', Arial, sans-serif; max-width: 860px; margin: 0 auto; padding: 24px; background: #f3f4f6; color: #1f2937; font-size: 14px; line-height: 1.6;">

<div style="background: #1e3a8a; color: white; padding: 18px 24px; border-radius: 8px; margin-bottom: 24px;">
  <div style="font-size: 20px; font-weight: bold;">Interfaces fonctionnelles, classes anonymes et lambdas</div>
  <div style="font-size: 12px; opacity: 0.8; margin-top: 4px;">Fiche de référence Java — syntaxe et pièges courants (OCP 1Z0-831)</div>
</div>

<!-- 1. Interface fonctionnelle -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 10px;">1️⃣ Interface fonctionnelle</div>
  <div style="font-size: 13px; color: #374151; margin-bottom: 12px;">
    Interface avec <strong>exactement une méthode abstraite</strong> (SAM — Single Abstract Method). Peut contenir des méthodes <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">default</span> et <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">static</span>.
  </div>

```java
@FunctionalInterface
public interface Transformer<T, R> {
    R transform(T input);            // SAM — seule méthode abstraite

    default Transformer<T, R> andLog() {  // OK : méthode default
        return t -> { R r = transform(t); System.out.println(r); return r; };
    }
}
```

  <div style="font-weight: bold; color: #1e40af; margin-top: 16px; margin-bottom: 10px;">Interfaces built-in courantes</div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Interface</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Signature</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Exemple de lambda</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Function&lt;T, R&gt;</span></td>
        <td style="padding: 7px 10px;">prend un T, retourne un R</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">x -&gt; x * 2</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Predicate&lt;T&gt;</span></td>
        <td style="padding: 7px 10px;">prend un T, retourne un boolean</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">s -&gt; s.isEmpty()</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Consumer&lt;T&gt;</span></td>
        <td style="padding: 7px 10px;">prend un T, ne retourne rien</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">s -&gt; System.out.println(s)</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Supplier&lt;T&gt;</span></td>
        <td style="padding: 7px 10px;">ne prend rien, retourne un T</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">() -&gt; "valeur"</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">BiFunction&lt;T, U, R&gt;</span></td>
        <td style="padding: 7px 10px;">prend T et U, retourne R</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">(a, b) -&gt; a + b</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">UnaryOperator&lt;T&gt;</span></td>
        <td style="padding: 7px 10px;">spécialisation de Function&lt;T, T&gt;</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">x -&gt; x + 1</span></td>
      </tr>
      <tr>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Runnable</span></td>
        <td style="padding: 7px 10px;">ne prend rien, ne retourne rien <span style="font-size: 11px; color: #6b7280;">(java.lang, pas java.util.function)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">() -&gt; System.out.println("go")</span></td>
      </tr>
    </tbody>
  </table>
</div>

<!-- 2. Classe anonyme -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 10px;">2️⃣ Classe anonyme</div>
  <div style="font-size: 13px; color: #374151; margin-bottom: 12px;">
    Instancie et définit une classe en une seule expression. Peut implémenter une interface <em>ou</em> étendre une classe. Génère un fichier <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">.class</span> séparé (<span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Outer$1.class</span>).
  </div>

```java
Comparator<String> comp = new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.compareToIgnoreCase(b);
    }
};
```

  <div style="font-size: 13px; color: #374151; margin-top: 12px; line-height: 1.7;">
    ⚠ <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">this</span> désigne <strong>l'instance anonyme</strong>, pas la classe englobante.<br>
    ⚠ Capture les variables locales <em>effectively final</em>, comme les lambdas.<br>
    ℹ Seul cas nécessaire aujourd'hui : implémenter une <strong>interface non-fonctionnelle</strong> ou <strong>étendre une classe</strong> à la volée.
  </div>
</div>

<!-- 3. Expression lambda -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 10px;">3️⃣ Expression lambda</div>
  <div style="font-size: 13px; color: #374151; margin-bottom: 12px;">
    Syntaxe compacte pour implémenter une interface fonctionnelle. Pas de fichier <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">.class</span> dédié — implémentée via <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">invokedynamic</span>.
  </div>

```java
// Syntaxes équivalentes
Comparator<String> c1 = (a, b) -> a.compareToIgnoreCase(b);

Predicate<String>  p  = s -> s.isEmpty();

Runnable           r  = () -> System.out.println("hello");

Function<Integer, Integer> square = x -> {
    int result = x * x;
    return result;   // bloc : return obligatoire
};
```

  <div style="font-size: 13px; color: #374151; margin-top: 12px; line-height: 1.7;">
    ⚠ <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">this</span> désigne la <strong>classe englobante</strong> (pas la lambda).<br>
    ⚠ Variable capturée = <em>effectively final</em> obligatoire.
  </div>
</div>

<!-- 4. Référence de méthode -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 10px;">4️⃣ Référence de méthode</div>
  <div style="font-size: 13px; color: #374151; margin-bottom: 12px;">
    Raccourci quand la lambda ne fait que déléguer à une méthode existante (<span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Classe::methode</span>).
  </div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Forme</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Type de référence</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Integer::parseInt</span></td>
        <td style="padding: 7px 10px;">méthode <strong>statique</strong></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">str::toUpperCase</span></td>
        <td style="padding: 7px 10px;">méthode d'<strong>instance</strong> sur objet connu</td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">String::isEmpty</span></td>
        <td style="padding: 7px 10px;">méthode d'<strong>instance</strong> par type (1er arg = receveur)</td>
      </tr>
      <tr>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">ArrayList::new</span></td>
        <td style="padding: 7px 10px;"><strong>constructeur</strong></td>
      </tr>
    </tbody>
  </table>

  <div style="font-size: 13px; color: #374151; margin-top: 12px;">Lambda vs. référence — strictement identiques :</div>

```java
list.stream().map(s -> s.toUpperCase())
list.stream().map(String::toUpperCase)   // ← à préférer
```

</div>

<!-- 5. Comparatif rapide -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 12px;">5️⃣ Comparatif rapide</div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Critère</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Classe anonyme</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Lambda</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;">Interfaces multiples</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">✓</span></td>
        <td style="padding: 7px 10px;">✗</td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;">Étend une classe</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">✓</span></td>
        <td style="padding: 7px 10px;">✗</td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;">Fichier .class dédié</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">✓</span></td>
        <td style="padding: 7px 10px;">✗ (invokedynamic)</td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">this</span> = classe englobante</td>
        <td style="padding: 7px 10px;">✗</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">✓</span></td>
      </tr>
      <tr>
        <td style="padding: 7px 10px;">Variables effectively final</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">✓</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">✓</span></td>
      </tr>
    </tbody>
  </table>
</div>

<!-- 6. Pièges OCP -->
<div style="background: #fef9c3; border: 1px solid #fde047; border-radius: 8px; padding: 14px 18px; font-size: 13px;">
  <span style="font-weight: bold; color: #713f12;">⚠️ Pièges fréquents à l'exam (OCP 1Z0-831)</span>
  <div style="margin-top: 8px; color: #713f12; line-height: 1.7;">
    • Une interface ne déclarant que des méthodes de <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">Object</span> (<span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">equals</span>, <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">hashCode</span>…) reste fonctionnelle si elle a par ailleurs exactement 1 SAM.<br>
    • Variable capturée par une lambda : toute réassignation, même après la capture, est une erreur de compilation.<br>
    • <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">@FunctionalInterface</span> est <strong>optionnelle</strong> mais recommandée — elle force la vérification du SAM à la compilation.<br>
    • Une lambda peut lever une <em>checked exception</em> uniquement si la SAM la déclare dans son <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">throws</span>.
  </div>
</div>

</div>
