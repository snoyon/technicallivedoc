---
type: Document
title: Certification Java 25
---

<div style="background-color: #f3f4f6; color: #1f2937; padding: 20px; font-family: 'Segoe UI', Arial, sans-serif;">
<div style="background-color: #1e3a8a; color: #ffffff; padding: 20px; border-radius: 8px; margin-bottom: 20px;">
<div style="font-size: 1.6rem; font-weight: bold;">Périmètre OCP 1Z0-831 — Java SE 25 Developer</div>
<div style="opacity: 0.8; margin-top: 5px;">Fiche technique de référence — vue d'ensemble du programme officiel</div>
</div>

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 12px; border-radius: 6px; margin-bottom: 20px;">
⚠️ Le code <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">1Z0-831</span> correspond à <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">Java SE 25</span> (LTS, sept. 2025), examen officiellement publié par Oracle le 1ᵉʳ mai 2026 — pas Java 21 (<span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">1Z0-830</span>). Le périmètre inclut donc tout Java 21 <span style="background-color: #fef08a; padding: 1px 5px; border-radius: 3px;">+</span> les nouveautés 22 à 25 (constructeurs flexibles, unnamed variables, gather operations, modules import...).
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🔢 1. Valeurs : nombres, texte, dates, booléens</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Sous-thème</th>
<th style="padding: 8px; text-align: left;">Points clés</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Primitifs &amp; wrappers</td>
<td style="padding: 8px;">8 types primitifs, autoboxing/unboxing, cache des <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Integer</span> (-128 à 127)</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Expressions</td>
<td style="padding: 8px;">Précédence d'opérateurs, conversions implicites, cast explicite, classe <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Math</span></td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Texte</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">String</span> (immuable), <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">StringBuilder</span> (mutable), text blocks</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Date-Time API</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">LocalDate/Time/DateTime</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Duration</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Period</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Instant</span>, fuseaux et heure d'été</td>
</tr>
</table>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🔁 2. Contrôle de flux</div>
<div>Conditions (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">if/else</span>), <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">switch</span> statement <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">et</span> expression, boucles (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">for</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">while</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">do-while</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">for-each</span>), <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">break</span>/<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">continue</span> avec labels.</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🧱 3. Objets, classes, records — cœur OO</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Sous-thème</th>
<th style="padding: 8px; text-align: left;">Points clés</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Cycle de vie objet</td>
<td style="padding: 8px;">Création, réaffectation de références, garbage collection (non déterministe)</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Classes &amp; records</td>
<td style="padding: 8px;">Champs/méthodes d'instance et statiques, constructeurs, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">initializer blocks</span> (instance/static)</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">⭐ Flexible constructor bodies</td>
<td style="padding: 8px;">Nouveauté Java 22+ : code <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">avant</span> l'appel à <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">super()</span>/<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">this()</span> (validation des arguments)</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Surcharge</td>
<td style="padding: 8px;">Méthodes surchargées, varargs (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">String... args</span>)</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Portée &amp; encapsulation</td>
<td style="padding: 8px;">Modificateurs d'accès, immuabilité, classes imbriquées (statiques, internes, locales, anonymes)</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Inférence locale</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">var</span> + ⭐ <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">unnamed variables</span> (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">_</span>, Java 22+, ex. <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">catch (Exception _)</span>)</td>
</tr>
</table>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🧬 4. Héritage, polymorphisme, interfaces</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Sous-thème</th>
<th style="padding: 8px; text-align: left;">Points clés</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Héritage</td>
<td style="padding: 8px;">Classes abstraites, classes <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">sealed</span>, records (pas d'héritage, implémentation d'interfaces seulement)</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Override</td>
<td style="padding: 8px;">Y compris <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">equals()</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">hashCode()</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">toString()</span> de <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Object</span></td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Polymorphisme</td>
<td style="padding: 8px;">Type de référence vs type d'objet, liaison dynamique (méthodes) vs statique (champs)</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Casting &amp; pattern matching</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">instanceof</span> avec pattern, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">switch</span> avec pattern matching + record patterns</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Interfaces</td>
<td style="padding: 8px;">Interfaces fonctionnelles, méthodes <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">private</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">static</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">default</span></td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Enums</td>
<td style="padding: 8px;">Champs, méthodes, constructeurs d'enum (souvent un sujet sous-estimé mais dense à l'examen)</td>
</tr>
</table>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🚨 5. Exceptions</div>
<div><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">try/catch/finally</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">try-with-resources</span> (interface <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">AutoCloseable</span>), <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">multi-catch</span>, exceptions personnalisées, hiérarchie <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">checked</span> vs <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">unchecked</span>.</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">📚 6. Tableaux &amp; collections</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Structure</th>
<th style="padding: 8px; text-align: left;">Points clés</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Arrays</td>
<td style="padding: 8px;">Tableaux multi-dimensionnels, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Arrays.sort()</span></td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">List / Set / Map / Deque</td>
<td style="padding: 8px;">Implémentations courantes (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">ArrayList</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">HashMap</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">TreeSet</span>...), CRUD, tri</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">⭐ Sequenced collections</td>
<td style="padding: 8px;">Java 21 : interface <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">SequencedCollection</span>, méthodes <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">getFirst()/getLast()/reversed()</span></td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Generics</td>
<td style="padding: 8px;">Wildcards <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">? extends</span> / <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">? super</span> (PECS), bornes de type</td>
</tr>
</table>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🌊 7. Streams &amp; lambdas</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Sous-thème</th>
<th style="padding: 8px; text-align: left;">Points clés</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Lambdas &amp; interfaces fonctionnelles</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Function</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Predicate</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Consumer</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Supplier</span> et variantes</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Object &amp; primitive streams</td>
<td style="padding: 8px;"><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Stream&lt;T&gt;</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">IntStream</span>/<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">LongStream</span>/<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">DoubleStream</span></td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Opérations</td>
<td style="padding: 8px;">Filtrage, transformation, tri, décomposition (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">flatMap</span>), concaténation, réduction, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">groupingBy</span>/<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">partitioningBy</span></td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">⭐ Gather operations</td>
<td style="padding: 8px;">Nouveauté Java 22+ : <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Stream.gather()</span> avec <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Gatherers</span> (transformations stateful intermédiaires custom)</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Streams parallèles</td>
<td style="padding: 8px;">Liés au thème concurrence (point 8)</td>
</tr>
</table>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🧵 8. Concurrence</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Sous-thème</th>
<th style="padding: 8px; text-align: left;">Points clés</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Threads</td>
<td style="padding: 8px;">Threads <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">platform</span> ET <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">virtual</span> (Java 21+)</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Thread-safety</td>
<td style="padding: 8px;">Mécanismes de verrouillage (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">synchronized</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">ReentrantLock</span>), API concurrente (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">ExecutorService</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">ConcurrentHashMap</span>)</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Traitement parallèle</td>
<td style="padding: 8px;">Collections concurrentes, streams parallèles (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">.parallel()</span>)</td>
</tr>
</table>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">💾 9. I/O, NIO, sérialisation</div>
<div>Flux console/fichier (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">InputStream</span>/<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">OutputStream</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Reader</span>/<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Writer</span>), sérialisation d'objets Java (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Serializable</span>), API <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">java.nio.file</span> (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Path</span>, parcours et propriétés de fichiers).</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">🌍 10. Localisation</div>
<div><span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Locale</span> et resource bundles, formatage de messages/dates/heures/nombres (devises, pourcentages).</div>
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-bottom: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">📦 11. Modules &amp; déploiement (JPMS)</div>
<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #eff6ff; color: #1e3a8a; border-bottom: 2px solid #bfdbfe;">
<th style="padding: 8px; text-align: left;">Sous-thème</th>
<th style="padding: 8px; text-align: left;">Points clés</th>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Modules</td>
<td style="padding: 8px;">Définition (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">module-info.java</span>), exposition de contenu (y compris par réflexion), dépendances</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Services</td>
<td style="padding: 8px;">Déclaration de <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">services</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">providers</span>, <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">consumers</span></td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">⭐ Module import declarations</td>
<td style="padding: 8px;">Nouveauté récente : <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">import module java.base;</span> pour importer tous les packages exportés d'un module</td>
</tr>
<tr style="background-color: #fafafa;">
<td style="padding: 8px;">Compilation &amp; exécution</td>
<td style="padding: 8px;">⭐ <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">Compact source files</span> et <span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">instance main methods</span> (Java 21+ preview → 25), programmes multi-fichiers</td>
</tr>
<tr style="background-color: #ffffff;">
<td style="padding: 8px;">Packaging</td>
<td style="padding: 8px;">JARs modulaires et non-modulaires, images runtime (<span style="background-color: #e0e7ff; color: #1d4ed8; padding: 1px 5px; border-radius: 3px;">jlink</span>), migration via unnamed/automatic modules</td>
</tr>
</table>
</div>

<div style="background-color: #fef9c3; border: 1px solid #fde047; color: #713f12; padding: 12px; border-radius: 6px; margin-top: 10px;">
⚠️ Les points marqués ⭐ sont des nouveautés post-Java 21 absentes du programme 1Z0-830 — donc les ressources d'étude ciblant l'ancien examen ne les couvrent généralement pas. À vérifier en priorité si tu utilises des supports plus anciens.
</div>

<div style="background-color: #ffffff; border-radius: 8px; border-left: 4px solid #1d4ed8; padding: 15px; margin-top: 15px;">
<div style="color: #1e40af; font-size: 1.1rem; font-weight: bold; margin-bottom: 10px;">📝 Notes pratiques sur l'examen</div>
<div>50 questions, 120 minutes. Beaucoup de questions combinent plusieurs concepts à la fois (ex. generics + streams + pattern matching dans une même question) — la difficulté vient autant de la largeur du programme que de la profondeur attendue sur chaque sujet.</div>
</div>

</div>

