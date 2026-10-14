# GEI Mobile Robotics — NXP Cup

Dépôt principal du projet de robot mobile développé au département Génie Électrique et Informatique (GEI) de l'INSA Toulouse dans le cadre de la compétition NXP Cup.

## Organisation du dépôt

Ce dépôt regroupe les différents éléments du robot sous forme de sous-modules Git (*Git submodules*).

| Sous-module | Description |
|---|---|
| [`gei-nxpcup-4w-mainboard`](https://github.com/INSA-GEI/gei-nxpcup-4w-mainboard) | Carte électronique principale basée sur la FRDM-IMX8MP |
| [`gei-nxpcup-4w-meca`](https://github.com/INSA-GEI/gei-nxpcup-4w-meca) | Conception et pièces mécaniques du robot |
| [`gei-nxpcup-4w-pwr`](https://github.com/INSA-GEI/gei-nxpcup-4w-pwr) | Carte électronique de puissance |

Chaque sous-module est un dépôt Git indépendant. Le dépôt principal mémorise le commit de référence de chaque sous-module afin de garantir la cohérence de l'ensemble du projet.

## Clonage du dépôt

### Prérequis

- Git installé sur la machine.
- Un accès réseau aux dépôts GitHub.

### Premier clonage

Pour récupérer le dépôt principal sur la branche `develop`, ainsi que les trois sous-modules et leurs éventuels sous-modules, exécuter :

```bash
git clone --branch develop --recurse-submodules \
    https://github.com/INSA-GEI/gei-mobile-robotic-nxpcup.git

cd gei-mobile-robotic-nxpcup
```

L'option `--branch develop` sélectionne la branche de développement du dépôt principal.

L'option `--recurse-submodules` initialise et récupère automatiquement les sous-modules aux versions référencées par le dépôt principal.

### Si le dépôt a déjà été cloné

Si le dépôt principal a été cloné sans récupérer les sous-modules, se placer à sa racine et exécuter :

```bash
git submodule update --init --recursive
```

Cette commande initialise les sous-modules, récupère leur contenu et sélectionne les commits référencés par le dépôt principal.

## Mettre à jour le dépôt

### Récupérer la dernière version de `develop`

Pour récupérer les modifications du dépôt principal et synchroniser les sous-modules avec les versions qu'il référence :

```bash
git switch develop
git pull --ff-only origin develop
git submodule update --init --recursive
```

Cette opération conserve la cohérence des versions entre le dépôt principal et ses sous-modules.

**Attention :** cette commande ne met pas nécessairement chaque sous-module à la dernière version de sa propre branche distante. Elle récupère la version explicitement référencée par le dépôt principal.

### Vérifier l'état des sous-modules

Pour afficher les commits actuellement utilisés par les sous-modules :

```bash
git submodule status
```

Pour vérifier l'état global du dépôt :

```bash
git status
```

## Mettre volontairement à jour les sous-modules

Pour récupérer les dernières versions des branches distantes des sous-modules, il faut effectuer une mise à jour volontaire.

Depuis la racine du dépôt principal :

```bash
git submodule update --init --recursive --remote
```

**Attention :** cette commande peut faire avancer les sous-modules au-delà des commits actuellement référencés par le dépôt principal. Il convient donc de vérifier la compatibilité des modifications avant de les valider.

Si les nouvelles versions sont validées, enregistrer les nouvelles références dans le dépôt principal :

```bash
git status
git diff --submodule
git add gei-nxpcup-4w-mainboard \
        gei-nxpcup-4w-meca \
        gei-nxpcup-4w-pwr
git commit -m "Update submodules"
git push origin develop
```

Le commit du dépôt principal mémorise les nouvelles références des sous-modules. Les autres contributeurs pourront ainsi récupérer exactement les mêmes versions avec `git submodule update --init --recursive`.

## Contribuer au projet

Chaque sous-module possède son propre historique Git et ses propres commits.

Lorsqu'une modification concerne un sous-module :

1. Entrer dans le répertoire du sous-module concerné.
2. Créer ou sélectionner la branche de travail appropriée.
3. Modifier les fichiers, puis committer et pousser les changements dans le dépôt du sous-module.
4. Revenir à la racine du dépôt principal.
5. Commiter et pousser la nouvelle référence du sous-module dans la branche `develop` du dépôt principal.

Cette procédure permet de conserver un historique indépendant pour chaque partie du robot tout en garantissant la reproductibilité de l'ensemble du projet.
