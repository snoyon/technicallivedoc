---
type: Document
title: Modern Java
---

<div style="background-color: #f3f4f6; color: #1f2937; padding: 20px; font-family: 'Segoe UI', Arial, sans-serif;">
<div style="background-color: #1e3a8a; color: #ffffff; padding: 20px; border-radius: 8px; margin-bottom: 20px;">
<div style="font-size: 1.6rem; font-weight: bold;">Java Moderne — Panorama des concepts (Java 8 → 25)</div>
<div style="opacity: 0.8; margin-top: 5px;">Fiche technique de référence</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">λ Lambdas &amp; interfaces fonctionnelles (Java 8)</div>

<div style="margin-bottom: 10px;">Une <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">interface fonctionnelle</span> est une interface avec exactement une méthode abstraite (annotation <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">@FunctionalInterface</span> facultative mais recommandée). Une lambda en est l'implémentation littérale.</div>

<table style="width: 100%; border-collapse: collapse; margin-bottom: 10px;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Interface</th>
<th style="padding: 8px; text-align: left;">Signature</th>
<th style="padding: 8px; text-align: left;">Usage</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Function&lt;T,R&gt;</span></td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">R apply(T t)</span></td>
<td style="padding: 8px;">Transformation</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Predicate&lt;T&gt;</span></td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">boolean test(T t)</span></td>
<td style="padding: 8px;">Filtrage / condition</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Consumer&lt;T&gt;</span></td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">void accept(T t)</span></td>
<td style="padding: 8px;">Effet de bord</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Supplier&lt;T&gt;</span></td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">T get()</span></td>
<td style="padding: 8px;">Production paresseuse</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">BiFunction&lt;T,U,R&gt;</span></td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">R apply(T t, U u)</span></td>
<td style="padding: 8px;">Transformation à 2 entrées</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">UnaryOperator&lt;T&gt;</span></td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">T apply(T t)</span></td>
<td style="padding: 8px;">Function&lt;T,T&gt; spécialisée</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Runnable</span></td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">void run()</span></td>
<td style="padding: 8px;">Action sans entrée/sortie</td>
</tr>
</table>

```java
Function<Integer, Integer> carre = x -> x * x;
Predicate<String> estVide = String::isEmpty;
Consumer<String> afficher = System.out::println;
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px;">
⚠️ Une lambda peut capturer une variable locale uniquement si elle est <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">effectively final</span> (jamais réassignée après initialisation).
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🔗 Method references (Java 8)</div>

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Forme</th>
<th style="padding: 8px; text-align: left;">Exemple</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Méthode statique</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Integer::parseInt</span></td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Méthode d'instance liée</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">str::toUpperCase</span></td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Méthode d'instance non liée</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">String::toUpperCase</span></td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Constructeur</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">ArrayList::new</span></td>
</tr>
</table>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🌊 Stream API (Java 8)</div>

<div style="margin-bottom: 10px;">Pipeline : <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">source → opérations intermédiaires (lazy) → opération terminale (déclenche l'exécution)</span>.</div>

```java
List<String> resultat = personnes.stream()
        .filter(p -> p.age() > 18)
        .map(Personne::nom)
        .sorted()
        .distinct()
        .limit(10)
        .collect(Collectors.toList());
```

<table style="width: 100%; border-collapse: collapse; margin-top: 10px;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Catégorie</th>
<th style="padding: 8px; text-align: left;">Méthodes</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Intermédiaires (lazy)</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">filter, map, flatMap, sorted, distinct, limit, skip, peek</span></td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Terminales</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">collect, forEach, reduce, count, anyMatch, findFirst, toList</span></td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Spécialisés primitifs</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">IntStream, LongStream, DoubleStream</span> (évitent le boxing)</td>
</tr>
</table>

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px; margin-top: 10px;">
⚠️ Un stream ne peut être consommé qu'une seule fois — réutiliser un stream déjà terminé lève une <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">IllegalStateException</span>.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">📦 Optional (Java 8)</div>

<div style="margin-bottom: 10px;">Conteneur explicite pour représenter "valeur potentiellement absente", afin d'éviter le <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">NullPointerException</span> implicite.</div>

```java
Optional<String> resultat = repository.findById(id)
        .map(Entity::getName)
        .filter(n -> !n.isBlank());

String nom = resultat.orElse("Inconnu");
resultat.ifPresentOrElse(
        System.out::println,
        () -> System.out.println("Absent")
);
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px;">
⚠️ <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">Optional</span> n'est pas destiné aux champs de classe ni aux paramètres de méthode — uniquement aux types de retour.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🧱 var — inférence de type locale (Java 10)</div>

```java
var liste = new ArrayList<String>();  // type inféré : ArrayList<String>
var carte = Map.of("a", 1, "b", 2);
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px;">
⚠️ Utilisable uniquement pour les variables locales avec initialiseur — jamais pour un champ, un paramètre, ni un type de retour. Le type reste statique (déterminé à la compilation), <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">var</span> n'est pas du typage dynamique.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🔀 Switch expressions &amp; arrow syntax (Java 14)</div>

```java
int jours = switch (mois) {
    case FEVRIER -> 28;
    case AVRIL, JUIN, SEPTEMBRE, NOVEMBRE -> 30;
    default -> 31;
};

String description = switch (note) {
    case 5 -> "Excellent";
    default -> {
        String s = "Note: " + note;
        yield s;
    }
};
```

<div style="margin-top: 10px;">Différences clés avec le <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">switch</span> classique : pas de <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">break</span>, pas de fall-through, exhaustivité vérifiée par le compilateur sur les enums et types scellés, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">yield</span> pour retourner une valeur depuis un bloc.</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">📝 Text blocks (Java 15)</div>

```java
String json = """
        {
          "nom": "Sylvain",
          "role": "Ingénieur Logiciel"
        }
        """;
```

<div>L'indentation minimale commune est automatiquement retirée. Pratique pour SQL, JSON, HTML embarqués.</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">📋 Records (Java 16)</div>

<div style="margin-bottom: 10px;">Type immuable porteur de données, avec génération automatique du constructeur canonique, des accesseurs, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">equals()</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">hashCode()</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">toString()</span>.</div>

```java
public record Point(int x, int y) implements Comparable<Point> {
    public Point {  // forme compacte
        if (x < 0 || y < 0) throw new IllegalArgumentException();
    }
    @Override
    public int compareTo(Point o) { return Integer.compare(x + y, o.x + o.y); }
}
```

<div style="margin-top: 10px;">Règles clés : ne peut pas étendre une classe (hérite implicitement de <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">java.lang.Record</span>), peut implémenter des interfaces, est implicitement <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">final</span> (pas d'héritage possible), aucun setter généré.</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🔒 Sealed classes &amp; interfaces (Java 17)</div>

<div style="margin-bottom: 10px;">Restreignent explicitement quelles classes peuvent étendre/implémenter un type — utile pour une exhaustivité garantie en pattern matching.</div>

```java
public sealed interface Forme permits Cercle, Carre, Triangle {}

public record Cercle(double rayon) implements Forme {}
public record Carre(double cote) implements Forme {}
public final class Triangle implements Forme {}
```

<div style="margin-top: 10px;">Chaque sous-type direct doit être <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">final</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">sealed</span>, ou <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">non-sealed</span>.</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🎯 Pattern matching — instanceof &amp; switch (Java 16 / 21)</div>

```java
// instanceof avec pattern (Java 16)
if (obj instanceof String s && !s.isBlank()) {
    System.out.println(s.toUpperCase());
}

// switch sur types + record patterns + guards (Java 21)
sealed interface Forme permits Cercle, Rectangle {}
record Cercle(double rayon) implements Forme {}
record Rectangle(double largeur, double hauteur) implements Forme {}

static double aire(Forme f) {
    return switch (f) {
        case Cercle(double r) when r <= 0 -> 0;
        case Cercle(double r) -> Math.PI * r * r;
        case Rectangle(double l, double h) -> l * h;
    };
}
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px; margin-top: 10px;">
⚠️ Avec une interface <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">sealed</span>, le compilateur vérifie l'exhaustivité du <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">switch</span> : pas besoin de <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">default</span> si tous les cas sont couverts.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🧵 Virtual threads (Java 21)</div>

<div style="margin-bottom: 10px;">Threads légers gérés par la JVM (et non par l'OS), permettant de créer des millions de threads sans épuiser les ressources système — pensés pour le code bloquant à haute concurrence (I/O).</div>

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    executor.submit(() -> traiterRequete());
}
```

<div style="margin-top: 10px;">Différence clé : un virtual thread bloqué (ex. attente I/O) <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">libère</span> le thread OS sous-jacent au lieu de le monopoliser, contrairement à un thread plateforme classique.</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🆕 Unnamed variables &amp; patterns (Java 22)</div>

<div style="margin-bottom: 10px;">Le caractère <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">_</span> marque explicitement une variable, un paramètre, ou une composante de pattern comme intentionnellement inutilisé.</div>

```java
// Variable de boucle ou catch non utilisée
try {
    traiter();
} catch (Exception _) {
    System.out.println("Erreur ignorée volontairement");
}

// Composante de record pattern ignorée
record Point(int x, int y) {}
if (p instanceof Point(int x, _)) {
    System.out.println("x = " + x);
}
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px;">
⚠️ <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">_</span> ne peut apparaître qu'une fois par pattern — <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">Point(_, _)</span> est interdit (utiliser <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">Point _</span> à la place pour ignorer tout le record).
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🏗️ Flexible constructor bodies (Java 25, JEP 513)</div>

<div style="margin-bottom: 10px;">Avant Java 25, un constructeur devait <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">obligatoirement</span> commencer par <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">super(...)</span> ou <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">this(...)</span>. Désormais, un constructeur est divisé en deux phases : un <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">prologue</span> (avant l'appel explicite) et un <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">épilogue</span> (après).</div>

```java
class Personne {
    Personne(int age) {
        System.out.println("Personne créée, âge=" + age);
    }
}

class Etudiant extends Personne {
    Etudiant(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("Âge invalide");
        }
        int ageValide = Math.abs(age);  // calcul autorisé dans le prologue
        super(ageValide);               // appel explicite : fin du prologue
        System.out.println("Etudiant initialisé");  // épilogue
    }
}
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px; margin-top: 10px;">
⚠️ Dans le prologue : autorisé d'utiliser variables locales, paramètres, méthodes <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">statiques</span>. Interdit de référencer <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">this</span> comme valeur, d'appeler une méthode d'instance, ou d'accéder à un champ d'instance — l'objet n'est pas encore initialisé.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🧺 Stream Gatherers (Java 24, JEP 485)</div>

<div style="margin-bottom: 10px;">Permettent de définir des opérations intermédiaires <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">personnalisées</span> via <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Stream.gather(Gatherer)</span>, là où avant seules les opérations prédéfinies (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">filter</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">map</span>...) étaient possibles.</div>

```java
List<String> noms = List.of("Alex", "John", "James", "Michael");

// Gatherer prédéfini : regrouper par paires glissantes
List<List<String>> paires = noms.stream()
        .gather(Gatherers.windowSliding(2))
        .toList();
// [[Alex, John], [John, James], [James, Michael]]
```

<div style="margin-top: 10px;">Utile pour des traitements à état (fenêtre glissante, regroupement par lots, fusion) impossibles à exprimer proprement avec les opérations de stream classiques.</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">📜 Compact source files &amp; instance main methods (Java 25, JEP 512)</div>

<div style="margin-bottom: 10px;">Pensé pour simplifier les petits programmes : plus besoin de déclarer une classe ni un <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">main</span> statique avec <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">public static void main(String[] args)</span>.</div>

```java
// Fichier .java complet et valide en Java 25 :
void main() {
    IO.println("Bonjour, Sylvain !");
}
```

<div style="margin: 10px 0;">Le compilateur génère implicitement une classe (sans nom utilisable en code source). La nouvelle classe <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">java.lang.IO</span> simplifie les E/S console (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">IO.println()</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">IO.readln()</span>).</div>

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px;">
⚠️ La classe implicite ne peut pas être instanciée avec <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">new</span> — elle n'a pas de nom accessible en code source. Pensé pour l'apprentissage et les scripts courts, pas pour remplacer les classes classiques en production.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">📥 Module import declarations (Java 25, JEP 511)</div>

<div style="margin-bottom: 10px;">Permet d'importer en une seule déclaration <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">tous</span> les packages exportés par un module, sans avoir besoin que le fichier appartienne lui-même à un module explicite.</div>

```java
import module java.base;   // remplace de nombreux "import java.util.*", etc.

void main() {
    var liste = List.of("a", "b", "c");  // List vient de java.base, auto-disponible
}
```

<div style="margin-top: 10px;">Les fichiers <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">compact source files</span> (ci-dessus) importent <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">java.base</span> automatiquement, comme si <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">import module java.base;</span> était présent en tête de fichier.</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🧩 Generics &amp; wildcards (rappel, antérieur mais central)</div>

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Notation</th>
<th style="padding: 8px; text-align: left;">Signification</th>
<th style="padding: 8px; text-align: left;">Mnémotechnique</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">? extends T</span></td>
<td style="padding: 8px;">T ou sous-type — lecture seule</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">PECS : Producer → extends</span></td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">? super T</span></td>
<td style="padding: 8px;">T ou super-type — écriture seule</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Consumer → super</span></td>
</tr>
</table>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🗺️ Vue d'ensemble chronologique</div>

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Version</th>
<th style="padding: 8px; text-align: left;">Concepts majeurs</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Java 8 (LTS)</td>
<td style="padding: 8px;">Lambdas, interfaces fonctionnelles, Stream API, Optional, method references</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Java 9</td>
<td style="padding: 8px;">Modules (JPMS), factory methods sur collections (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">List.of()</span>)</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Java 10</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">var</span> (inférence locale)</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Java 14</td>
<td style="padding: 8px;">Switch expressions</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Java 15</td>
<td style="padding: 8px;">Text blocks</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Java 16</td>
<td style="padding: 8px;">Records, pattern matching pour instanceof</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Java 17 (LTS)</td>
<td style="padding: 8px;">Sealed classes/interfaces</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Java 21 (LTS)</td>
<td style="padding: 8px;">Pattern matching pour switch, record patterns, virtual threads, sequenced collections</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Java 22</td>
<td style="padding: 8px;">Unnamed variables &amp; patterns (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">_</span>)</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Java 24</td>
<td style="padding: 8px;">Stream Gatherers (final)</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Java 25 (LTS)</td>
<td style="padding: 8px;">Flexible constructor bodies, compact source files &amp; instance main, module import declarations</td>
</tr>
</table>
</div>

</div>
