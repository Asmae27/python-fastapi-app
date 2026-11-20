
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
