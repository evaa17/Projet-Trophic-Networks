# Trophic Network — Simulateur de réseaux trophiques

> Projet académique  — Décembre 2024  
> Stack : C (C11) · Graphviz (DOT) · Gnuplot

## Présentation

**Trophic Network** est un simulateur en ligne de commande de **réseaux trophiques** (chaînes alimentaires) modélisés sous forme de graphes orientés. Le programme permet d'analyser la structure d'un écosystème, de simuler l'évolution des populations et d'étudier l'impact de perturbations écologiques.

Ces réseaux modélisent les interactions entre espèces vivantes et ressources (lumière, sol, etc.) : les **sommets** représentent les espèces ou ressources, et les **arcs** indiquent les relations de prédation ou d'échange.

### Objectifs principaux
- **Création de réseaux trophiques** : 3 écosystèmes réels avec complexité croissante (Océan Indien, Savane, Désert du Sahara)
- **Étude structurelle** : connexité, niveaux trophiques, importance relative des espèces (centralité, impact d'une disparition)
- **Étude fonctionnelle** : modélisation de la dynamique des populations (prédation, ressources, régulation naturelle)
- **Simulation** : évolution des populations avec paramètres ajustables, export Gnuplot

---
<img width="1920" height="1080" alt="synthese" src="https://github.com/user-attachments/assets/39e7d9d5-d900-4310-b054-fed888b2b441" />


---

## Les 3 écosystèmes

### Réseau 1 — Océan Indien
Écosystème marin avec récifs coralliens et eaux profondes. La chaîne alimentaire part du phytoplancton vers les grands prédateurs (requins, orques). Un sous-réseau terrestre (vache, chèvre, herbe) a été ajouté **volontairement** pour créer un graphe non connexe et tester la détection de composantes.

### Réseau 2 — Savane
Écosystème tropical africain avec herbivores (éléphants, girafes) et carnivores (lions, guépards). Les décomposeurs et ressources ont été omis volontairement pour se concentrer sur les chaînes alimentaires et leurs impacts.

### Réseau 3 — Désert du Sahara
Écosystème désertique fragile avec davantage de décomposeurs et ressources pour étudier leur rôle. Particulièrement adapté aux simulations d'impacts extérieurs (sécheresse, activité humaine).

---

## Visualisation des graphes (format DOT)
<img width="1920" height="1080" alt="vue" src="https://github.com/user-attachments/assets/c0c009e2-4303-4fe7-bf67-750d797b56cd" />


Les réseaux sont visualisables graphiquement via Graphviz. Le code couleur des nœuds représente le niveau trophique :

| Couleur   | Niveau |
|---        |---                    |
| 🟢 Vert   | Producteurs primaires |
| 🟣 Violet | Consommateurs primaires |
| 🔵 Bleu   | Consommateurs secondaires |
| 🟠 Orange | Consommateurs tertiaires |
| 🔴 Rouge  | Carnivores au sommet |
| 🟤 Marron | Décomposeurs |
| 🟡 Jaune  | Ressources |


<img width="936" height="525" alt="Reseau 2" src="https://github.com/user-attachments/assets/64925bb9-ea89-4cf1-8b9e-92f645721627" />
<img width="847" height="517" alt="Reseau 1" src="https://github.com/user-attachments/assets/38b2efb7-2253-4095-8a0e-6fde53b7eb80" />

### Format fichier texte (`.txt`)

```
Animaux:
1, Orque, 4
2, Requin-tigre, 3
6, Crevette, 1
9, Bacterie, -2      ← niveau < 0 = décomposeur
10, Soleil, -1       ← niveau < 0 = ressource

Relations:
6 3     ← la Crevette (6) est consommée par la Pieuvre (3)
8 7     ← le Phytoplancton (8) est consommé par la Méduse (7)
```
<img width="1920" height="1080" alt="construction" src="https://github.com/user-attachments/assets/becde2f9-b8c4-4fa3-871c-29a6e738b175" />


**Remarque :** les niveaux trophiques `-1` et `-2` (ressources et décomposeurs) sont traités séparément pour ne pas être considérés comme des proies ou prédateurs dans les analyses.


## Résultats — Complexité des réseaux

<img width="1920" height="1080" alt="recap" src="https://github.com/user-attachments/assets/471cc0ca-e2cd-4746-99db-f25049f0cf9a" />


## Algorithmes implémentés

| Algorithme | Objectif | Complexité |
|---|---|---|
| BFS | Parcours par niveaux, connexité, chaînes alimentaires | O(N+M) |
| DFS | Parcours récursif, détection de cycles, tri topologique | O(N+M) |
| Kosaraju | Composantes fortement connexes (CFC) | O(N+M) |
| PERT | Planification du projet, chemin critique | O(N+M) |

---

## Structure du projet

```
trophic-network/
├── main.c              # Menu principal (17 options)
├── cc.c / cc.h         # Chargement du graphe, DFS, Kosaraju
├── vv.c / vv.h         # Sommets particuliers, niveaux trophiques, centralité
├── fonctionnel.c / .h  # Simulation des populations
├── extension.c / .h    # Analyses étendues
├── simu.c / .h         # Simulation de suppression d'espèce
├── anthropique.c / .h  # Impacts humains (braconnage, pêche)
├── CMakeLists.txt
├── desert.dot          # Écosystème désert (Graphviz)
├── ocean.dot           # Écosystème océan (Graphviz)
├── savane.dot          # Écosystème savane (Graphviz)
└── source              # Références bibliographiques
```

---

## Compilation et exécution

### Prérequis
- GCC ou MinGW (Windows)
- CMake ≥ 3.26
- Graphviz (optionnel, pour visualiser les `.dot`)
- Gnuplot (optionnel, pour les graphes d'évolution)

### Avec CLion
1. Ouvrir le dossier racine dans CLion
2. CLion détecte automatiquement `CMakeLists.txt`
3. Cliquer sur **Run** ▶

### Avec CMake en ligne de commande
```bash
mkdir build && cd build
cmake ..
cmake --build .
./untitled9
```

### Utilisation
Au lancement, entrer le nom du fichier d'écosystème :
```
Entrez le nom du fichier:
> desert.dot
```
Fichiers disponibles : `desert.dot`, `ocean.dot`, `savane.dot`

---

## Ce que j'ai appris

- Modélisation de problèmes réels (écologie) sous forme de graphes orientés pondérés
- Implémentation from scratch de DFS, BFS, Kosaraju en C
- Calcul de métriques de centralité (radiale, intermédiarité)
- Simulation de systèmes dynamiques (modèle proie-prédateur)
- Organisation d'un projet C multi-fichiers avec CMake
- Collaboration en groupe avec Git/GitHub et Notion (gestion de tâches)

---

## Auteurs

Projet réalisé en groupe dans le cadre du cours de **Théorie des Graphes**  
Décembre 2024  
Eva Martins et son équipe
