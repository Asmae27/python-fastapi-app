
# TP 7 – Security Notes

## Q3. Pourquoi faut-il épingler une version (@v4) plutôt que @main ?

La référence @main pointe vers une branche qui peut être modifiée à tout moment. Une modification du code peut donc changer le comportement de notre workflow sans intervention de notre part.

La référence @v4 permet d'utiliser une version majeure précise et offre une meilleure stabilité.

Pour renforcer davantage la sécurité, il est préférable d'utiliser un SHA complet de commit, qui identifie précisément le code exécuté.

## Q4. Le badge « créateur vérifié » garantit-il qu'une action est sûre ? Justifiez.

Non. Ce badge indique que GitHub a vérifié l'identité du créateur, mais il ne garantit pas l'absence de vulnérabilités ou de code malveillant.

Il reste nécessaire d'examiner le code source, les dépendances et le comportement de l'action.

## Q5. Citez deux vérifications à faire avant d'utiliser, en production, une action d'un fournisseur inconnu.

1. Vérifier le code source, les permissions demandées et la manière dont l'action utilise les secrets et les données du dépôt.

2. Vérifier la réputation du fournisseur, l'activité de maintenance, les dépendances et les éventuelles vulnérabilités connues.
