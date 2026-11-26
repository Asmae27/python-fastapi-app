
# TP 6 – Variables d'environnement, Inputs et Outputs

## Q1. Pourquoi la variable n'est-elle pas disponible dans la step qui l'a définie, mais seulement dans les suivantes ?

GITHUB_ENV est un fichier traité par GitHub Actions à la fin de chaque step. Les variables ajoutées deviennent disponibles uniquement dans les steps suivantes du même job.

## Q2. Que constatez-vous dans le job verify-env ? Quelle est la portée de GITHUB_ENV ?

La variable APP_NAME n'est pas disponible dans le job verify-env. Sa valeur est vide, car GITHUB_ENV possède une portée limitée au job dans lequel elle est définie.

## Q3. Que se passe-t-il si vous écrivez > au lieu de >> dans GITHUB_OUTPUT alors que deux outputs sont définis dans la même step ?

L'opérateur > remplace le contenu précédent du fichier GITHUB_OUTPUT. Ainsi, le premier output est écrasé par le deuxième. L'opérateur >> permet d'ajouter plusieurs outputs sans supprimer les précédents.

## Q4. Pourquoi GITHUB_ENV ne suffit-il pas ici, alors que GITHUB_OUTPUT fonctionne entre jobs ?

GITHUB_ENV permet uniquement de partager des variables entre les steps d'un même job. GITHUB_OUTPUT permet de produire des outputs qui peuvent être exposés au niveau du job et transmis à un autre job grâce à needs.

## Q5. Que se passe-t-il si vous retirez needs: build du job deploy ?

Le job deploy ne dépend plus de build et peut donc s'exécuter indépendamment, éventuellement en parallèle. De plus, le contexte needs.build ne sera pas disponible correctement pour récupérer l'output du job build.



# TP 7 – Actions tierces et actions personnalisées

## Q1. Pourquoi actions/checkout n'a-t-il pas besoin de with alors que setup-node en a besoin ?

actions/checkout utilise des paramètres par défaut pour récupérer le dépôt courant. En revanche, actions/setup-node utilise l'input node-version pour connaître la version de Node.js à installer. Dans ce TP, nous devons donc préciser node-version: 20 avec with.

## Q2. Pourquoi le job verify ne retrouve-t-il pas report.txt sans l'artefact, alors que les deux jobs partagent le même dépôt ?

Chaque job GitHub Actions s'exécute dans un environnement isolé avec son propre espace de travail. Les fichiers créés dans le job build ne sont donc pas automatiquement accessibles dans verify. L'artefact permet de transférer explicitement le fichier entre les jobs.

## Q6. Pourquoi actions/checkout doit-il s'exécuter avant l'appel d'une action locale ?

L'action locale se trouve dans le dépôt courant. actions/checkout récupère le contenu du dépôt dans l'espace de travail du runner. Il faut donc exécuter checkout avant d'appeler cette action.

## Q7. Pourquoi shell: bash est-il obligatoire sur les steps run d'une action composite ?

Dans une action composite, chaque step utilisant run doit préciser le shell à utiliser. shell: bash indique au runner d'exécuter les commandes avec Bash.



# TP 8 – Stratégies GitHub Actions

## Q1. Combien de jobs sont générés ? Donnez la liste des combinaisons.

La matrice initiale génère 6 jobs :

1. ubuntu-latest avec Python 3.11
2. ubuntu-latest avec Python 3.12
3. ubuntu-latest avec Python 3.13
4. windows-latest avec Python 3.11
5. windows-latest avec Python 3.12
6. windows-latest avec Python 3.13

## Q2. Combien de jobs au total après exclude et include ?

Il y a 6 jobs au total.

La matrice initiale contient 6 combinaisons. L'exclusion de Windows avec Python 3.11 réduit le nombre à 5. Ensuite, include ajoute Ubuntu avec Python 3.14, ce qui donne 6 jobs.

## Q3. Que se passerait-il si vous tentiez d'exclure la combinaison 3.14 ? Pourquoi ?

La combinaison Python 3.14 ajoutée avec include ne serait pas supprimée par cette règle. En effet, exclude est appliqué avant include. La combinaison 3.14 est ajoutée après l'application des exclusions.

## Q4. Quelle est la différence entre fail-fast true et false ?

Avec fail-fast: true, l'échec d'un job peut provoquer l'annulation des autres jobs de la matrice encore en cours ou en attente.

Avec fail-fast: false, les autres jobs continuent leur exécution même lorsqu'un job échoue.

J'utiliserais true pour arrêter rapidement un pipeline en cas d'échec et false pour obtenir les résultats de toutes les configurations testées.

## Q5. Donnez deux raisons d'utiliser max-parallel en entreprise.

1. Limiter la consommation des ressources et maîtriser les coûts des runners.
2. Éviter de surcharger l'infrastructure CI/CD en contrôlant le nombre de jobs exécutés simultanément.
