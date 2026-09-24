---
type: Document
title: Pattern Matching
---

<div style="background-color: #f3f4f6; color: #1f2937; padding: 20px; font-family: 'Segoe UI', Arial, sans-serif;">
<div style="background-color: #1e3a8a; color: #ffffff; padding: 20px; border-radius: 8px; margin-bottom: 20px;">
<div style="font-size: 1.6rem; font-weight: bold;">Pattern Matching en Java</div>
<div style="opacity: 0.8; margin-top: 5px;">Fiche technique de référence — instanceof, switch, records, guards</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">💡 Principe général</div>
<div>Le pattern matching remplace un test + cast manuel par une <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">vérification</span> et une <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">extraction</span> en une seule expression. Une valeur est confrontée à un <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">pattern</span> (type, record, littéral...) ; si elle correspond, ses composantes sont automatiquement extraites dans des variables.</div>

```java
// Avant (Java < 16)
if (obj instanceof String) {
    String s = (String) obj;   // cast manuel répétitif
    System.out.println(s.length());
}

// Avec pattern matching (Java 16+)
if (obj instanceof String s) {
    System.out.println(s.length());   // s déjà typé, pas de cast
}
```
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🗺️ Chronologie des JEPs</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">JEP</th>
<th style="padding: 8px; text-align: left;">Fonctionnalité</th>
<th style="padding: 8px; text-align: left;">Version / statut</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">JEP 394</td>
<td style="padding: 8px;">Pattern matching pour <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">instanceof</span></td>
<td style="padding: 8px;">Java 16 — final</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">JEP 440</td>
<td style="padding: 8px;">Record patterns</td>
<td style="padding: 8px;">Java 21 — final</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">JEP 441</td>
<td style="padding: 8px;">Pattern matching pour <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">switch</span> (inclut le <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">when</span>)</td>
<td style="padding: 8px;">Java 21 — final</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">JEP 456</td>
<td style="padding: 8px;">Unnamed variables &amp; patterns (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">_</span>)</td>
<td style="padding: 8px;">Java 22 — final</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">JEP 507 / 530 / 532</td>
<td style="padding: 8px;">Primitive type patterns (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">case int v</span>)</td>
<td style="padding: 8px;">⚠️ Toujours en preview (Java 25/26/27)</td>
</tr>
</table>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">1️⃣ Pattern matching pour instanceof (Java 16)</div>

<div style="margin-bottom: 10px;">Fusionne le test de type et le cast. La variable capturée n'est <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">in scope</span> que dans les branches où le test est garanti vrai.</div>

```java
Object obj = "bonjour";

if (obj instanceof String s && s.length() > 3) {
    System.out.println(s.toUpperCase());   // s visible ici
}
// s n'est PAS visible ici (hors du if)

if (!(obj instanceof String s)) {
    return;
}
System.out.println(s.length());   // s visible ici : flow scoping négatif
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px;">
⚠️ C'est ce qu'on appelle le <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">flow scoping</span> : la portée de la variable dépend de l'analyse de flux du compilateur, pas seulement des accolades — elle reste accessible après un <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">if (!(... instanceof ...)) return;</span>.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">2️⃣ Pattern matching pour switch (Java 21)</div>

<div style="margin-bottom: 10px;">Le <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">case</span> teste un pattern de type plutôt qu'une égalité de constante.</div>

```java
static String decrire(Object obj) {
    return switch (obj) {
        case null -> "C'est null";
        case Integer i -> "Entier : " + i;
        case String s -> "Texte : " + s;
        case int[] tab -> "Tableau de taille " + tab.length;
        default -> "Autre chose";
    };
}
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px;">
⚠️ Le <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">case null</span> est explicite et obligatoire si on veut gérer le cas <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">null</span> — sinon un <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">switch</span> pattern matching lève une <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">NullPointerException</span> sur un sélecteur null (sauf si un <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">default</span> couvre aussi null via <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">case null, default -></span>).
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">3️⃣ Guard clause — when (Java 21, partie de JEP 441)</div>

<div style="margin-bottom: 10px;">Le <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">when</span> ajoute une condition booléenne arbitraire après le filtrage par pattern. Il n'existe pas en dehors du pattern matching — un <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">case</span> classique (constante) ne peut pas avoir de <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">when</span>.</div>

```java
static String classifier(Object obj) {
    return switch (obj) {
        case Integer i when i < 0 -> "Négatif";
        case Integer i when i == 0 -> "Zéro";
        case Integer i -> "Positif";
        default -> "Pas un entier";
    };
}
```

<div style="margin-top: 10px;">L'ordre compte : les <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">case</span> sont évalués séquentiellement, donc place les conditions les plus spécifiques avant les plus générales (comme dans une cascade <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">if / else if</span>).</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">4️⃣ Record patterns (Java 21) — déconstruction</div>

<div style="margin-bottom: 10px;">Permet de <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">déconstruire</span> un record directement dans le pattern, y compris de façon imbriquée.</div>

```java
record Point(int x, int y) {}
record Ligne(Point depart, Point arrivee) {}

static String decrire(Object obj) {
    return switch (obj) {
        case Point(int x, int y) when x == y -> "Point diagonal";
        case Point(int x, int y) -> "Point (" + x + "," + y + ")";
        case Ligne(Point(var x1, var y1), Point(var x2, var y2)) ->
            "Ligne de (" + x1 + "," + y1 + ") à (" + x2 + "," + y2 + ")";
        default -> "Inconnu";
    };
}
```

<div style="margin-top: 10px;">Fonctionne aussi avec <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">instanceof</span> :</div>

```java
if (obj instanceof Point(int x, int y)) {
    System.out.println(x + y);
}
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px; margin-top: 10px;">
⚠️ Seuls les <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">records</span> peuvent être déconstruits ainsi — une classe Lombok <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">@Value</span> ou une classe normale ne le permettent pas, car le compilateur a besoin de la garantie structurelle propre aux records.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">5️⃣ Unnamed patterns — _ (Java 22)</div>

<div style="margin-bottom: 10px;">Ignore explicitement une composante sans lui donner de nom.</div>

```java
record Point(int x, int y) {}

if (p instanceof Point(int x, _)) {
    System.out.println("x = " + x);   // y ignoré
}

try {
    risque();
} catch (Exception _) {   // exception ignorée
    System.out.println("Échec, ignoré");
}
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px;">
⚠️ <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">_</span> ne peut apparaître qu'une fois par pattern : <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">Point(_, _)</span> est une erreur de compilation. Pour ignorer tout l'objet, utilise <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">case Point _ -></span> plutôt que de déconstruire.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">✅ Exhaustivité avec sealed (Java 17 + 21)</div>

<div style="margin-bottom: 10px;">Avec une hiérarchie <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">sealed</span>, le compilateur peut garantir l'exhaustivité du <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">switch</span> sans <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">default</span>.</div>

```java
sealed interface Forme permits Cercle, Rectangle, Triangle {}
record Cercle(double rayon) implements Forme {}
record Rectangle(double largeur, double hauteur) implements Forme {}
record Triangle(double base, double hauteur) implements Forme {}

static double aire(Forme f) {
    return switch (f) {
        case Cercle(double r) -> Math.PI * r * r;
        case Rectangle(double l, double h) -> l * h;
        case Triangle(double b, double h) -> 0.5 * b * h;
        // pas de default nécessaire : tous les sous-types sont couverts
    };
}
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px;">
⚠️ Si tu ajoutes un nouveau type au <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">permits</span> sans mettre à jour le <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">switch</span>, la compilation échoue — c'est une protection précieuse lors de l'évolution du code (contrairement à un <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">default</span> qui masquerait silencieusement l'oubli).
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🔬 Primitive type patterns — preview (JEP 507/530/532)</div>

<div style="margin-bottom: 10px;">Permettrait d'utiliser des types primitifs directement comme pattern (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">case int v</span>), avec vérification de <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">conversion exacte</span> (sans perte d'information) au lieu d'une simple correspondance de type.</div>

```java
// Nécessite --enable-preview, pas encore stable
Object valeur = 42;
String resultat = switch (valeur) {
    case int v when v > 0 -> "Entier positif : " + v;
    case int v -> "Entier négatif ou nul";
    default -> "Pas un int";
};
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px;">
⚠️ Cette fonctionnalité a déjà connu 3 previews (Java 23, 24, 25) et une 4ᵉ est prévue pour Java 26 puis une 5ᵉ pour Java 27 — elle n'est <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">toujours pas finalisée</span> à ce jour (juin 2026). Ne pas s'appuyer sur elle en code de production.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">📌 Récapitulatif</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Contexte</th>
<th style="padding: 8px; text-align: left;">Pattern supporté</th>
<th style="padding: 8px; text-align: left;">Depuis</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">instanceof</span></td>
<td style="padding: 8px;">Type pattern, record pattern</td>
<td style="padding: 8px;">Java 16 / 21</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">switch</span></td>
<td style="padding: 8px;">Type pattern, record pattern, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">null</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">when</span></td>
<td style="padding: 8px;">Java 21</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Composante ignorée</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">_</span></td>
<td style="padding: 8px;">Java 22</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Type primitif comme pattern</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">case int v</span></td>
<td style="padding: 8px;">⚠️ Toujours preview</td>
</tr>
</table>
</div>

</div>

