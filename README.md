# La compagnie d'aventuriers

Une compagnie d'aventuriers part explorer un donjon ! Dans l'équipe, il y a : un barbare qui adore taper, une magicienne dont les sorts ratent une fois sur deux, un nain qui compte chaque pièce d'or et une elfe qui trouve tout le monde trop bruyant. 

Ta mission : écrire cette compagnie en Java.

Tu as la matinée.

## Démarrer

```
git clone https://github.com/peacepilou/java-adventurers.git
```

Dans IntelliJ, **Open**, puis choisis le dossier `java-adventurers` (celui qui contient `pom.xml`). Dans **File > Project Structure > Project**, choisis le JDK 21. Tes classes vont dans `src/main/java/`, dans un package `party`.

## Les règles du jeu

- **Personne n'existe dans un état absurde.** Exemple : un aventurier sans nom, un niveau 0, des points de vie négatifs... la compagnie doit refuser de créer de tels objets. 
- **Ce qui ne change pas ne se modifie pas.** Si un nom ne change jamais, rien dans ta classe ne permet de le changer.
- **Une erreur se dit.** Quand une règle est enfreinte, une exception part, avec un message qui dit quoi et pourquoi. Exemple : `Niveau invalide pour Grumli : 0`.
- **Sans IA**, du début à la fin.

---

## 1 · L'aventurier

Écris la classe `Adventurer`.

Un aventurier a un **nom**, un **niveau**, des **points de vie** et une bourse de **pièces d'or**.

- Le nom n'est ni vide ni fait d'espaces. Le niveau va de 1 à 20. Les points de vie de départ sont strictement positifs. La bourse commence à 0.
- Une règle enfreinte à la création lève une `InvalidAdventurerException`, que tu écris toi aussi.
- `takeDamage(int damage)` retire des points de vie. Ils ne descendent jamais sous 0. Un dégât négatif est refusé.
- `isAlive()` répond `true` tant qu'il lui reste des points de vie.
- `earnGold(int amount)` ajoute des pièces. Un montant négatif est refusé.
- `toString()` affiche l'aventurier en une ligne : `Grumli (niveau 3) : 24 PV, 12 pièces d'or`.

Avant de coder, sur une feuille : la liste des attributs, et pour chacun, qui a le droit de le lire et de le modifier.

## 2 · Les classes de personnage

On ne recrute pas « un aventurier ». On recrute un barbare, une magicienne, un nain, une elfe. `Adventurer` devient **abstraite**, et chaque classe de personnage en hérite.

Chacun attaque à sa façon, avec une méthode `attack()` qui renvoie les dégâts infligés :

| Classe      | Ce qu'elle a en plus        | Ce que renvoie `attack()`                                                                                   |
| ----------- | --------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `Barbarian` | une **force**               | sa force + son niveau                                                                                       |
| `Mage`      | une réserve de **mana**     | 3 × son niveau, et chaque sort coûte 5 de mana. Sans mana, elle lève une `OutOfManaException`               |
| `Dwarf`     | une **hache**, qui a un nom | son niveau × 2. Le nain gagne 10 % de pièces d'or en plus à chaque `earnGold`, arrondi à l'entier inférieur |
| `Elf`       | un **arc**, et des flèches  | 4 + son niveau, et chaque attaque consomme une flèche. Sans flèche, elle ne fait aucun dégât                |

Les règles de la partie 1 valent pour tous, sans que tu les réécrives dans chaque classe. La force, le mana et les flèches ne sont jamais négatifs.

## 3 · Ceux qui soignent

Certains aventuriers savent soigner. Pas tous.

- Écris une interface `Healer`, avec une méthode `heal(Adventurer target)`.
- La magicienne et l'elfe savent soigner. Le barbare et le nain, non.
- La magicienne soigne de 2 × son niveau, pour 5 de mana. L'elfe soigne de 3 points, sans rien dépenser.
- On ne soigne pas un mort. On ne dépasse pas les points de vie de départ.

## 4 · La compagnie

Écris la classe `Party`, qui garde ses membres dans une liste.

- `recruit(Adventurer adventurer)` ajoute un membre. Une compagnie compte **6 membres au plus** : au-delà, une `PartyFullException`. Deux membres ne portent jamais le même nom.
- `getAliveCount()` renvoie le nombre de membres encore debout.
- `getTotalGold()` renvoie l'or de toute la compagnie.
- `shareLoot(int gold)` partage un butin entre les membres **vivants**, à parts égales, en pièces entières. Le reste de la division va au nain, s'il y en a un. Sinon, il est perdu (personne n'est d'accord sur qui le garde).
- `fight(int monsterDamage)` : chaque membre vivant attaque une fois, et la somme des dégâts est renvoyée. Puis le monstre frappe chaque membre vivant de `monsterDamage`.

## 5 · L'aventure

Écris une classe `Main`, avec une méthode `main`, qui raconte une aventure dans la console :

1. Une compagnie de quatre : un barbare, une magicienne, un nain, une elfe.
2. Trois combats contre un monstre qui frappe de plus en plus fort.
3. Un soin entre deux combats.
4. Un butin de 101 pièces d'or, partagé.
5. L'état de la compagnie après chaque étape, avec `toString()`.

Puis, dans le même `main`, provoque **chacune de tes exceptions** au moins une fois. Attrape-la avec un `try` / `catch`, et affiche son message. Si une règle ne lève rien quand tu l'enfreins, c'est qu'elle n'est pas protégée.

---

## Cet après-midi

Correction : trois ou quatre compagnies présentées, choisies pour leurs approches différentes. Apporte ta sortie console, et la liste des règles que tu as eu du mal à faire respecter.

Bonne aventure. Et compte bien ton or : le nain, lui, compte.
