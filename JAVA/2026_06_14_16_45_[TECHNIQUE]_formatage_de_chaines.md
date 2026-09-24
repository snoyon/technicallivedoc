---
type: Document
title: Formatage De Chaines
---

<div style="font-family: 'Segoe UI', Arial, sans-serif; max-width: 860px; margin: 0 auto; padding: 24px; background: #f3f4f6; color: #1f2937; font-size: 14px; line-height: 1.6;">

<div style="background: #1e3a8a; color: white; padding: 18px 24px; border-radius: 8px; margin-bottom: 24px;">
  <div style="font-size: 20px; font-weight: bold;">String.format() — Fiche de référence Java</div>
  <div style="font-size: 12px; opacity: 0.8; margin-top: 4px;">Formatage de chaînes via spécificateurs de format</div>
</div>

<!-- Signature -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 10px;">📌 Signature</div>

```java
String.format(String format, Object... args)
String.format(Locale locale, String format, Object... args)
```

  <div style="margin-top: 10px; font-size: 13px; color: #374151;">
    Équivalent à <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">printf()</span> mais retourne un <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">String</span> au lieu d'écrire dans un flux.
    Depuis Java 15, on peut aussi utiliser <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">"hello %s".formatted(arg)</span>.
  </div>
</div>

<!-- Syntaxe générale d'un spécificateur -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 10px;">🔤 Syntaxe d'un spécificateur</div>
  <div style="font-family: monospace; background: #f0f4ff; border: 1px solid #c7d2fe; border-radius: 6px; padding: 10px 14px; font-size: 13px; color: #1e3a8a;">
    %[argument_index$][flags][width][.precision]conversion
  </div>
  <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 12px; font-size: 13px;">
    <div><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">argument_index$</span> — ex. <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">1$</span>, <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">2$</span> pour référencer un argument par position</div>
    <div><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">flags</span> — <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">-</span> <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">+</span> <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">0</span> <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">(espace)</span> <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">,</span> <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">(</span></div>
    <div><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">width</span> — largeur minimale du champ (entier positif)</div>
    <div><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">.precision</span> — nb de décimales (float) ou nb de chars max (String)</div>
  </div>
</div>

<!-- Conversions courantes -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 12px;">🔡 Conversions courantes</div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Spécificateur</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Type</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Exemple</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Résultat</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%s</span></td>
        <td style="padding: 7px 10px;">String / Object</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("Bonjour %s", "Alice")</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">Bonjour Alice</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%S</span></td>
        <td style="padding: 7px 10px;">String → majuscules</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%S", "alice")</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">ALICE</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%d</span></td>
        <td style="padding: 7px 10px;">Entier décimal</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%d", 42)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">42</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%f</span></td>
        <td style="padding: 7px 10px;">Flottant décimal</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%f", 3.14)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">3.140000</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%e</span></td>
        <td style="padding: 7px 10px;">Notation scientifique</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%e", 123456.789)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">1.234568e+05</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%x</span> / <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%X</span></td>
        <td style="padding: 7px 10px;">Hexadécimal</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%x", 255)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">ff</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%o</span></td>
        <td style="padding: 7px 10px;">Octal</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%o", 8)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">10</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%c</span></td>
        <td style="padding: 7px 10px;">Caractère</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%c", 65)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">A</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%b</span></td>
        <td style="padding: 7px 10px;">Booléen</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%b", null)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">false</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%n</span></td>
        <td style="padding: 7px 10px;">Saut de ligne (OS-dépendant)</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("ligne1%nligne2")</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">ligne1↵ligne2</span></td>
      </tr>
      <tr>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">%%</span></td>
        <td style="padding: 7px 10px;">Littéral %</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("100%%")</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">100%</span></td>
      </tr>
    </tbody>
  </table>
</div>

<!-- Flags -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 12px;">🚩 Flags</div>
  <table style="width: 100%; border-collapse: collapse; font-size: 13px;">
    <thead>
      <tr style="background: #eff6ff; color: #1e3a8a;">
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Flag</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Effet</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Exemple</th>
        <th style="padding: 8px 10px; text-align: left; border-bottom: 2px solid #bfdbfe;">Résultat</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">-</span></td>
        <td style="padding: 7px 10px;">Alignement à gauche</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%-10s|", "abc")</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">abc       |</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">+</span></td>
        <td style="padding: 7px 10px;">Toujours afficher le signe</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%+d", 42)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">+42</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">0</span></td>
        <td style="padding: 7px 10px;">Padding avec des zéros</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%05d", 42)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">00042</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb; background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">,</span></td>
        <td style="padding: 7px 10px;">Séparateur de milliers</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%,d", 1000000)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">1,000,000</span></td>
      </tr>
      <tr style="border-bottom: 1px solid #e5e7eb;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">(</span></td>
        <td style="padding: 7px 10px;">Parenthèses pour négatifs</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%(f", -12.5)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">(12.500000)</span></td>
      </tr>
      <tr style="background: #fafafa;">
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">#</span></td>
        <td style="padding: 7px 10px;">Forme alternative (0x pour hex, 0 pour octal)</td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; font-size: 12px;">format("%#x", 255)</span></td>
        <td style="padding: 7px 10px;"><span style="font-family: monospace; background: #f0fdf4; color: #166534; padding: 1px 5px; border-radius: 3px;">0xff</span></td>
      </tr>
    </tbody>
  </table>
</div>

<!-- Exemples pratiques -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; margin-bottom: 18px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 12px;">💡 Exemples pratiques</div>

  <div style="margin-bottom: 14px;">
    <div style="font-size: 12px; color: #6b7280; margin-bottom: 4px;">Précision sur un flottant</div>

```java
String.format("%.2f", 3.14159)   // → "3.14"
String.format("%8.2f", 3.14159)  // → "    3.14"  (width 8, right-aligned)
```

  </div>

  <div style="margin-bottom: 14px;">
    <div style="font-size: 12px; color: #6b7280; margin-bottom: 4px;">Troncature d'une String</div>

```java
String.format("%.5s", "Bonjour") // → "Bonjo"
```

  </div>

  <div style="margin-bottom: 14px;">
    <div style="font-size: 12px; color: #6b7280; margin-bottom: 4px;">Indexation explicite des arguments (réutilisation)</div>

```java
String.format("%1$s a dit : \"%1$s\"", "Alice")
// → "Alice a dit : \"Alice\""
```

  </div>

  <div style="margin-bottom: 14px;">
    <div style="font-size: 12px; color: #6b7280; margin-bottom: 4px;">Tableau formaté en colonnes</div>

```java
for (String[] row : data) {
    System.out.println(String.format("%-15s %5d %8.2f", row[0], row[1], row[2]));
}
// → "Alice            42  1234.56"
```

  </div>

  <div>
    <div style="font-size: 12px; color: #6b7280; margin-bottom: 4px;">Locale explicite (séparateur décimal)</div>

```java
String.format(Locale.FRENCH, "%.2f", 3.14)   // → "3,14"
String.format(Locale.US,     "%.2f", 3.14)   // → "3.14"
```

  </div>
</div>

<!-- Note -->
<div style="background: #fef9c3; border: 1px solid #fde047; border-radius: 8px; padding: 14px 18px; margin-bottom: 18px; font-size: 13px;">
  <span style="font-weight: bold; color: #713f12;">⚠️ Attention</span>
  <div style="margin-top: 6px; color: #713f12;">
    <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">%b</span> retourne <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">false</span> pour tout objet <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">null</span>, et <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">true</span> pour toute référence non-null (y compris <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">"false"</span> en String !). Préférer <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">Boolean.toString()</span> pour plus de clarté.
    <br><br>
    <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">%n</span> produit <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">\r\n</span> sur Windows et <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">\n</span> sur Unix. Utiliser <span style="font-family: monospace; background: #fef08a; padding: 1px 5px; border-radius: 3px;">\n</span> si on veut un comportement constant (ex. dans des logs).
  </div>
</div>

<!-- Alternatives modernes -->
<div style="background: white; border-radius: 8px; padding: 18px 20px; border-left: 4px solid #1d4ed8;">
  <div style="font-weight: bold; color: #1e40af; margin-bottom: 10px;">🆕 Alternatives modernes</div>
  <div style="font-size: 13px; color: #374151; margin-bottom: 10px;">À partir de Java 15, <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">String::formatted</span> est équivalent et plus idiomatique :</div>

```java
"Bonjour %s, tu as %d ans".formatted("Alice", 30)
// → "Bonjour Alice, tu as 30 ans"
```

  <div style="font-size: 13px; color: #374151; margin-top: 10px;">Pour les cas complexes ou les templates multi-lignes, les <strong>Text Blocks</strong> (Java 15+) combinés à <span style="font-family: monospace; background: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">formatted()</span> sont une option élégante :</div>

```java
String json = """
    {
      "name": "%s",
      "age": %d
    }
    """.formatted("Alice", 30);
```

</div>

</div>

