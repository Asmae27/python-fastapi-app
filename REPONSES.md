
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
