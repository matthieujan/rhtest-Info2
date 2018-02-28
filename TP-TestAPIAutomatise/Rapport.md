# Resultat TP-TestAPIAutomatisé

## Documents
- Ce Rapport, compilant les analyses
- Le referentiel, voir TP-ReferentielExigence
- Result.png, présentant les résultats de la suite de test

## Analyse
La suite de test, nommé LaunchAll, contient 11 éléments, dont deux (le premier et le dernier) de remise à zéro des données.
Le dernier test, cas eval OOB, passe au vert si le test précédent est supprimé

### Post - Cas nominal
On va ici rentrer des informations classiques pour voir si on peut ajouter un salarié.
- Données : SAL3 / John / Doe / 10000 / 1
- Oracle : Code HTTP 201
- Resultat : OK
La persistance est verifié plus tard dans la suite de test.

### Post - Cas même matricule
On va ici vérifier que recreer un salarié avec le matricule SAL3 renvoi une erreur
- Données : SAL3 / John / Doe / 10000 / 1
- Oracle : Code HTTP 409, contains "matricule"
- Resultat : OK
L'ajout a donc correctement échoué

### Post - Cas only matricule
On va ici vérifier que l'on peut ajouter un salarié avec uniquement un matricule
- Donnée : SAL4
- Oracle : Code HTTP 201
- Resultat : OK
La persistance est verifié plus tard dans la suite de test

## Get recherche all
On va ici verifier que la recherche fonctionne correctement, et affiche nos deux salariés.
De plus, on verifie que SAL4 est affiché en premier.
- Donnée : mode=all
- Oracle : Code HTTP 200 / Contains, in order "SAL4","SAL3","John","Doe","10000","1".
- Resultat : OK
On valide donc ici la  persistance de nos ajouts, et le tri des salariés par date de modification.

## Get recherche nominal
On va ici verifier que la recherche par nom fonction
- Donnée : name=John
- Oracle : Code HTTP 200 / Contains "SAL3" ...
- Resultat : OK
La recherche fonctionne donc correctement

### Post - Cas nominal (modify)
On va ici rentrer des informations classiques pour voir si on peut modifier un salarié.(ici son nom de famille)
- Données : SAL3 / John / Doey / 10000 / 1
- Oracle : Code HTTP 200
- Resultat : OK
La persistance est verifié plus tard dans la suite de test.

### Recherche testModify
On verifie ici avec une recherche (déjà testé) que notre modification a fonctionné
- Données : name=John
- Oracle : Code HTTP 200 / contains "SAL3" ...
- Resultat : OK

### Post cas salaire float
On verifie ici que seul un salaire entier peut être renseigné
- Données : SAL3 / John / Doe / 10000.5 / 1
- Oracle : Code HTTP 409 / contains "entier"
- Résultat : KO
On voit qu'ici, alors que les specs et l'api nous indique que seul des entiers sont traités, on peut renseigner des décimaux

### Post cas eval OOB
On verifie que seul les valeurs comprises entre -10 et 10 fonctionnement pour l'éval
- Données : SAL3 / John / Doe / 10000 / 11
- Oracle : Code HTTP 409 / contains "10"
- Resultat OK
