---
type: Document
title: Record
---

<div style="background-color: #f3f4f6; color: #1f2937; padding: 20px; font-family: 'Segoe UI', Arial, sans-serif;">
<div style="background-color: #1e3a8a; color: #ffffff; padding: 20px; border-radius: 8px; margin-bottom: 20px;">
<div style="font-size: 1.6rem; font-weight: bold;">Java Records — Héritage, Construction &amp; Builder</div>
<div style="opacity: 0.8; margin-top: 5px;">Fiche technique — OCP 1Z0-831</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🧬 Héritage : ce qui est permis ou non</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Action</th>
<th style="padding: 8px; text-align: left;">Possible ?</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Un record étend une classe</td>
<td style="padding: 8px;">❌ Non (étend implicitement <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">java.lang.Record</span>)</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Un record implémente une interface</td>
<td style="padding: 8px;">✅ Oui</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Une classe hérite d'un record</td>
<td style="padding: 8px;">❌ Non (record implicitement <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">final</span>)</td>
</tr>
</table>

<div style="margin-top: 10px;">

```java
public record Point(int x, int y) implements Comparable<Point> {
    @Override
    public int compareTo(Point other) {
        return Integer.compare(x + y, other.x + other.y);
    }
}
```

</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🏗️ Les 4 formes de constructeur</div>

<div style="margin-bottom: 10px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">1. Constructeur canonique</span> — généré automatiquement (un paramètre par composant).</div>

<div style="margin-bottom: 10px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">2. Constructeur canonique explicite</span> — pour ajouter de la validation.</div>

```java
public record Point(int x, int y) {
    public Point(int x, int y) {
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException("Coordonnées négatives interdites");
        }
        this.x = x;
        this.y = y;
    }
}
```

<div style="margin: 10px 0;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">3. Forme compacte</span> — sans parenthèses ni assignations explicites.</div>

```java
public record Point(int x, int y) {
    public Point(int x, int y) {
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException("Coordonnées négatives interdites");
        }
        // x et y sont assignés automatiquement en fin de bloc
    }
}
```

<div style="margin: 10px 0;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">4. Constructeur secondaire</span> — doit déléguer au canonique via <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">this(...)</span>.</div>

```java
public record Point(int x, int y) {
    public Point(int x) {
        this(x, 0);
    }
}
```

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 10px; border-radius: 6px; margin-top: 10px;">
⚠️ Piège OCP : dans la forme compacte, toute affectation explicite à <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">this.x</span> est interdite — on assigne seulement le paramètre <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">x</span> (le compilateur fait l'assignation finale lui-même).
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🔨 Pattern Builder pour un Record</div>

<div style="margin-bottom: 10px;">Aucun builder n'est généré nativement. Utile quand un record a <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">beaucoup de champs</span>, dont certains <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">optionnels</span>, pour éviter un constructeur à 8 paramètres.</div>

<div style="font-weight: bold; margin-bottom: 5px;">Le record cible :</div>

```java
public record PersonneOCP(String nom, String prenom, int age,
                           String email, String ville) {
}
```

<div style="font-weight: bold; margin: 10px 0 5px;">Builder classique (classe statique imbriquée) :</div>

```java
public record PersonneOCP(String nom, String prenom, int age,
                           String email, String ville) {

    public static Builder builder() {
        return new Builder();
    }

    public static class Builder {
        private String nom;
        private String prenom;
        private int age;
        private String email;
        private String ville;

        public Builder nom(String nom) {
            this.nom = nom;
            return this;
        }

        public Builder prenom(String prenom) {
            this.prenom = prenom;
            return this;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public Builder ville(String ville) {
            this.ville = ville;
            return this;
        }

        public PersonneOCP build() {
            return new PersonneOCP(nom, prenom, age, email, ville);
        }
    }
}
```

<div style="font-weight: bold; margin: 10px 0 5px;">Utilisation :</div>

```java
PersonneOCP p = PersonneOCP.builder()
        .nom("Durand")
        .prenom("Sylvain")
        .age(35)
        .email("sylvain@example.com")
        .ville("Vannes")
        .build();
```

<div style="background-color: #f0fdf4; color: #166534; padding: 8px 10px; border-radius: 4px; margin-top: 10px; display: inline-block;">
✅ Résultat : on garde l'immuabilité du record final, tout en ayant une construction fluide et lisible.
</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">📌 Points à retenir pour l'OCP</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Élément généré</th>
<th style="padding: 8px; text-align: left;">Détail</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Accesseurs</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">x()</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">y()</span> — pas de préfixe <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">get</span></td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Setters</td>
<td style="padding: 8px;">❌ Jamais générés (immuabilité)</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">equals()</span> / <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">hashCode()</span></td>
<td style="padding: 8px;">Basés sur tous les composants</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">toString()</span></td>
<td style="padding: 8px;">Format : <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Point[x=1, y=2]</span></td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Builder</td>
<td style="padding: 8px;">❌ Jamais généré — à écrire soi-même si besoin</td>
</tr>
</table>
</div>

</div>
