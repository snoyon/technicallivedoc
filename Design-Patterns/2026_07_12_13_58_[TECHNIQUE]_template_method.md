---
type: Document
title: Template Method
---

<div style="background-color:#f3f4f6;color:#1f2937;font-family:Arial,sans-serif;padding:20px;">

<div style="background-color:#1e3a8a;color:#ffffff;padding:20px 24px;border-radius:8px;margin-bottom:20px;">
<span style="font-size:1.6rem;font-weight:bold;">Design Pattern : Template Method</span><br>
<span style="opacity:0.8;">Deux implémentations en Java — classe abstraite vs interface avec default method</span>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">🎯 Principe du pattern</span>
<div style="margin-top:10px;">
Le <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">Template Method</span> définit le squelette d'un algorithme dans une méthode, en déléguant certaines étapes à des sous-classes (ou implémentations). La structure globale de l'algorithme reste fixe, seules les étapes variables sont personnalisables.
</div>
<div style="margin-top:10px;">
Deux façons de l'implémenter en Java : la classe abstraite (approche historique) et l'interface avec méthodes par défaut (possible depuis Java 8).
</div>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">🧱 Implémentation 1 : classe abstraite</span>

<div style="margin-top:10px;">
La classe abstraite déclare la <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">template method</span> en <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">final</span>, et les étapes variables comme méthodes <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">abstract</span> ou <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">protected</span>.

```java
public abstract class TrieurAbstrait {

    // Template method : verrouillée, non redéfinissable
    public final void trier(int[] donnees) {
        preparer(donnees);
        effectuerTri(donnees);
        afficherResultat(donnees);
    }

    protected void preparer(int[] donnees) {
        System.out.println("Préparation des données...");
    }

    // Étape variable : chaque sous-classe doit l'implémenter
    protected abstract void effectuerTri(int[] donnees);

    protected void afficherResultat(int[] donnees) {
        System.out.println("Résultat : " + java.util.Arrays.toString(donnees));
    }
}

public class TriRapide extends TrieurAbstrait {
    @Override
    protected void effectuerTri(int[] donnees) {
        // implémentation du quicksort
    }
}
```
</div>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">🔌 Implémentation 2 : interface avec default method</span>

<div style="margin-top:10px;">
Depuis Java 8, une interface peut porter la template method sous forme de <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">default method</span>, qui appelle une méthode abstraite (déclaration classique d'interface) restant à implémenter.

```java
public interface Trieur {

    // "Template method" : méthode par défaut qui orchestre l'algorithme
    default void trier(int[] donnees) {
        preparer(donnees);
        effectuerTri(donnees);
        afficherResultat(donnees);
    }

    default void preparer(int[] donnees) {
        System.out.println("Préparation des données...");
    }

    // Étape variable : chaque implémentation doit la définir
    void effectuerTri(int[] donnees);

    default void afficherResultat(int[] donnees) {
        System.out.println("Résultat : " + java.util.Arrays.toString(donnees));
    }
}

public class TriRapide implements Trieur {
    @Override
    public void effectuerTri(int[] donnees) {
        // implémentation du quicksort
    }
}
```
</div>
</div>

<div style="background-color:#fef9c3;border:1px solid #fde047;border-radius:8px;padding:14px 18px;color:#713f12;margin-bottom:16px;">
<span style="font-weight:bold;">⚠️ Limite structurelle des interfaces</span>
<div style="margin-top:6px;">
Une méthode <span style="background-color:#fef08a;padding:1px 5px;border-radius:3px;">default</span> ne peut pas être déclarée <span style="background-color:#fef08a;padding:1px 5px;border-radius:3px;">final</span>. N'importe quelle classe implémentant l'interface peut donc redéfinir <span style="background-color:#fef08a;padding:1px 5px;border-radius:3px;">trier()</span> et casser le squelette de l'algorithme — contrairement à la classe abstraite où ce verrou est possible.
</div>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">⚖️ Classe abstraite : avantages / inconvénients</span>

<table style="width:100%;border-collapse:collapse;margin-top:12px;">
<tr style="background-color:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;">
<td style="padding:8px 10px;font-weight:bold;">Avantages</td>
<td style="padding:8px 10px;font-weight:bold;">Inconvénients</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Template method verrouillable en <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">final</span>, structure de l'algorithme protégée</td>
<td style="padding:8px 10px;">Occupe le seul emplacement d'héritage disponible en Java (pas d'héritage multiple de classes)</td>
</tr>
<tr style="background-color:#fafafa;">
<td style="padding:8px 10px;">Peut porter un état interne (champs d'instance)</td>
<td style="padding:8px 10px;">Hiérarchie plus rigide : toute sous-classe est liée à cette classe mère unique</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Étapes personnalisables en <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">protected</span>, invisibles de l'extérieur du package/hiérarchie</td>
<td style="padding:8px 10px;">Nécessite une classe dédiée même pour un algorithme très simple</td>
</tr>
<tr style="background-color:#fafafa;">
<td style="padding:8px 10px;">Peut avoir un constructeur pour initialiser l'état</td>
<td style="padding:8px 10px;">-</td>
</tr>
</table>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">⚖️ Interface + default method : avantages / inconvénients</span>

<table style="width:100%;border-collapse:collapse;margin-top:12px;">
<tr style="background-color:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;">
<td style="padding:8px 10px;font-weight:bold;">Avantages</td>
<td style="padding:8px 10px;font-weight:bold;">Inconvénients</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Une classe peut implémenter plusieurs interfaces (héritage multiple de comportement)</td>
<td style="padding:8px 10px;">Impossible d'empêcher la redéfinition de la template method (pas de <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">final</span> sur une default method)</td>
</tr>
<tr style="background-color:#fafafa;">
<td style="padding:8px 10px;">Plus léger à mettre en place pour un algorithme simple sans état</td>
<td style="padding:8px 10px;">Aucun champ d'instance possible (pas d'état interne porté par l'interface)</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">La classe implémentante reste libre d'étendre une autre classe si besoin</td>
<td style="padding:8px 10px;">Toutes les méthodes sont publiques (pas d'équivalent à <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">protected</span> pour cacher les hooks)</td>
</tr>
<tr style="background-color:#fafafa;">
<td style="padding:8px 10px;">-</td>
<td style="padding:8px 10px;">Pas de constructeur, donc pas d'initialisation complexe possible</td>
</tr>
</table>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">📊 Tableau comparatif de synthèse</span>

<table style="width:100%;border-collapse:collapse;margin-top:12px;">
<tr style="background-color:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;">
<td style="padding:8px 10px;font-weight:bold;">Critère</td>
<td style="padding:8px 10px;font-weight:bold;">Classe abstraite</td>
<td style="padding:8px 10px;font-weight:bold;">Interface (default method)</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Empêcher la redéfinition de l'algorithme</td>
<td style="padding:8px 10px;"><span style="background-color:#f0fdf4;color:#166534;padding:1px 5px;border-radius:3px;">Oui (final)</span></td>
<td style="padding:8px 10px;">Non</td>
</tr>
<tr style="background-color:#fafafa;">
<td style="padding:8px 10px;">État interne (champs)</td>
<td style="padding:8px 10px;"><span style="background-color:#f0fdf4;color:#166534;padding:1px 5px;border-radius:3px;">Oui</span></td>
<td style="padding:8px 10px;">Non</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Visibilité protected pour les hooks</td>
<td style="padding:8px 10px;"><span style="background-color:#f0fdf4;color:#166534;padding:1px 5px;border-radius:3px;">Oui</span></td>
<td style="padding:8px 10px;">Non (public uniquement)</td>
</tr>
<tr style="background-color:#fafafa;">
<td style="padding:8px 10px;">Constructeur possible</td>
<td style="padding:8px 10px;"><span style="background-color:#f0fdf4;color:#166534;padding:1px 5px;border-radius:3px;">Oui</span></td>
<td style="padding:8px 10px;">Non</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Héritage multiple</td>
<td style="padding:8px 10px;">Non (une seule classe mère)</td>
<td style="padding:8px 10px;"><span style="background-color:#f0fdf4;color:#166534;padding:1px 5px;border-radius:3px;">Oui (plusieurs interfaces)</span></td>
</tr>
</table>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">✅ Recommandation</span>
<div style="margin-top:10px;">
Classe abstraite : à privilégier si la structure de l'algorithme doit être protégée contre toute redéfinition, ou si un état interne / une initialisation via constructeur est nécessaire.
</div>
<div style="margin-top:6px;">
Interface + default method : pertinente pour un algorithme simple, sans état, quand la flexibilité de l'implémentation multiple (implémenter plusieurs interfaces) est un critère important — en acceptant le risque que le squelette soit redéfini.
</div>
</div>

</div>

