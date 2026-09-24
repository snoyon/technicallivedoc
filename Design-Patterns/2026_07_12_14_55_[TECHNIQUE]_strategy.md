---
type: Document
title: Strategy
---

<div style="background-color:#f3f4f6;color:#1f2937;font-family:Arial,sans-serif;padding:20px;">

<div style="background-color:#1e3a8a;color:#ffffff;padding:20px 24px;border-radius:8px;margin-bottom:20px;">
<span style="font-size:1.6rem;font-weight:bold;">Design Pattern : Strategy</span><br>
<span style="opacity:0.8;">Encapsuler des comportements interchangeables par composition</span>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">🎯 Principe du pattern</span>
<div style="margin-top:10px;">
Le <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">Strategy</span> définit une famille d'algorithmes interchangeables, encapsule chacun d'eux dans une classe séparée implémentant une interface commune, et les rend interchangeables au sein d'une classe cliente via composition (injection de la stratégie), plutôt que par héritage.
</div>
<div style="margin-top:10px;">
Contrairement au Template Method (qui fixe le squelette d'un algorithme et laisse varier certaines étapes par redéfinition dans une sous-classe), le Strategy délègue la variation entière du comportement à un objet séparé, injecté au runtime.
</div>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">🧩 Structure</span>
<div style="margin-top:10px;">
Trois rôles principaux :
</div>
<div style="margin-left:1rem;margin-top:8px;">
<div>• <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">Strategy</span> — interface définissant le contrat commun à toutes les stratégies</div>
<div>• <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">ConcreteStrategy</span> — une ou plusieurs implémentations concrètes de l'algorithme</div>
<div>• <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">Context</span> — classe cliente qui détient une référence vers une Strategy et lui délègue l'exécution</div>
</div>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">💻 Implémentation Java</span>

<div style="margin-top:10px;">

```java
// Strategy
public interface StrategieRemise {
    double appliquer(double montant);
}

// ConcreteStrategy 1
public class RemiseFidelite implements StrategieRemise {
    @Override
    public double appliquer(double montant) {
        return montant * 0.9;
    }
}

// ConcreteStrategy 2
public class RemiseNoel implements StrategieRemise {
    @Override
    public double appliquer(double montant) {
        return montant - 10;
    }
}

// Context
public class Panier {

    private final StrategieRemise strategieRemise;

    public Panier(StrategieRemise strategieRemise) {
        this.strategieRemise = strategieRemise;
    }

    public double calculerTotal(double montantBrut) {
        return strategieRemise.appliquer(montantBrut);
    }
}
```

</div>
<div style="margin-top:10px;">
Utilisation : la stratégie est choisie et injectée au moment de la construction (ou via un setter), ce qui permet de changer le comportement dynamiquement, sans toucher à la classe <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">Panier</span>.

```java
Panier panier = new Panier(new RemiseFidelite());
double total = panier.calculerTotal(100.0);
```
</div>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">⚙️ Variante avec lambda (Java 8+)</span>
<div style="margin-top:10px;">
Si l'interface Strategy est fonctionnelle (une seule méthode abstraite), elle peut être annotée <span style="background-color:#e0e7ff;color:#1d4ed8;padding:1px 5px;border-radius:3px;">@FunctionalInterface</span> et instanciée par lambda, sans créer de classe concrète dédiée pour chaque stratégie simple.

```java
@FunctionalInterface
public interface StrategieRemise {
    double appliquer(double montant);
}

Panier panier = new Panier(montant -> montant * 0.9);
```
</div>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">⚖️ Avantages / inconvénients</span>

<table style="width:100%;border-collapse:collapse;margin-top:12px;">
<tr style="background-color:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;">
<td style="padding:8px 10px;font-weight:bold;">Avantages</td>
<td style="padding:8px 10px;font-weight:bold;">Inconvénients</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Comportement modifiable au runtime (changement de stratégie dynamique)</td>
<td style="padding:8px 10px;">Le client doit connaître les différentes stratégies pour choisir la bonne</td>
</tr>
<tr style="background-color:#fafafa;">
<td style="padding:8px 10px;">Chaque stratégie est testable isolément, sans dépendre du Context</td>
<td style="padding:8px 10px;">Multiplication du nombre de classes/types si beaucoup de variantes</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Pas de hiérarchie d'héritage, pas de fragile base class problem</td>
<td style="padding:8px 10px;">Pas de partage direct d'état entre étapes comme pourrait le permettre une classe abstraite avec des champs</td>
</tr>
<tr style="background-color:#fafafa;">
<td style="padding:8px 10px;">Respecte l'Open/Closed Principle : ajouter une stratégie n'impacte pas le Context</td>
<td style="padding:8px 10px;">Verbeux si une seule méthode par stratégie et peu de variantes (une interface + une classe pour chaque cas)</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Composable avec d'autres stratégies (plusieurs axes de variation indépendants)</td>
<td style="padding:8px 10px;">-</td>
</tr>
</table>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;margin-bottom:16px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">📊 Strategy vs Template Method</span>

<table style="width:100%;border-collapse:collapse;margin-top:12px;">
<tr style="background-color:#eff6ff;color:#1e3a8a;border-bottom:2px solid #bfdbfe;">
<td style="padding:8px 10px;font-weight:bold;">Critère</td>
<td style="padding:8px 10px;font-weight:bold;">Strategy</td>
<td style="padding:8px 10px;font-weight:bold;">Template Method</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Mécanisme</td>
<td style="padding:8px 10px;">Composition (délégation à un objet injecté)</td>
<td style="padding:8px 10px;">Héritage (redéfinition dans une sous-classe)</td>
</tr>
<tr style="background-color:#fafafa;">
<td style="padding:8px 10px;">Changement de comportement au runtime</td>
<td style="padding:8px 10px;"><span style="background-color:#f0fdf4;color:#166534;padding:1px 5px;border-radius:3px;">Oui</span></td>
<td style="padding:8px 10px;">Non (fixé à la compilation, par le type de la sous-classe)</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Comportement par défaut réutilisable sans code côté client</td>
<td style="padding:8px 10px;">Non (il faut toujours fournir une implémentation)</td>
<td style="padding:8px 10px;"><span style="background-color:#f0fdf4;color:#166534;padding:1px 5px;border-radius:3px;">Oui (hook methods)</span></td>
</tr>
<tr style="background-color:#fafafa;">
<td style="padding:8px 10px;">Axes de variation indépendants recombinables</td>
<td style="padding:8px 10px;"><span style="background-color:#f0fdf4;color:#166534;padding:1px 5px;border-radius:3px;">Oui</span></td>
<td style="padding:8px 10px;">Non (une seule ligne d'héritage)</td>
</tr>
<tr style="background-color:#ffffff;">
<td style="padding:8px 10px;">Protection du squelette de l'algorithme</td>
<td style="padding:8px 10px;">Non applicable (pas de squelette imposé)</td>
<td style="padding:8px 10px;"><span style="background-color:#f0fdf4;color:#166534;padding:1px 5px;border-radius:3px;">Oui (final sur classe abstraite)</span></td>
</tr>
</table>
</div>

<div style="background-color:#ffffff;border-left:4px solid #1d4ed8;border-radius:8px;padding:16px 20px;">
<span style="color:#1e40af;font-weight:bold;font-size:1.15rem;">✅ Quand choisir Strategy</span>
<div style="margin-top:10px;">
Quand plusieurs algorithmes/comportements doivent être interchangeables sans modifier la classe cliente, notamment si le choix doit pouvoir se faire au runtime ou si les variantes doivent rester recombinables entre plusieurs axes indépendants (ex : un builder de requête et un exécuteur combinables librement).
</div>
<div style="margin-top:6px;">
À éviter si la majorité du comportement est stable avec seulement quelques points d'extension ponctuels ayant un défaut sensé — dans ce cas, le Template Method évite la verbosité d'une composition systématique.
</div>
</div>

</div>
