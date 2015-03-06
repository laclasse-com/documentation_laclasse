# Spécifications fonctionnelles 
# Intégration de l'import CSV des élèves et des profs dans l'annuaire ENT.

## objectif et contours fonctionnels
L'annuaire ENT est le service qui réaliser l'analyse et l'importation des comptes fournis par le système d'informations de l'académie de LYON. Elle réceptione les donnéess poussées par l'académie tout les jours, parse le XML et consolide une base tampon de comptes d'élèves, de parents et de personnels de l'éducation nationale. Cette base alimente ensuite à la demande (manuellement ou automatiquement les ENT v2 et V3, selon le choix de chaque établissement).

### Schéma général de fonctionement

![../../interfaces/alimentation-academique.png](../../interfaces/alimentation-academique.png)

L'objectif est de développer une interface permettant le chagement et le traitement de fichiers CSV pour la création de comptes d'élèves et de profs dans les écoles primaires.
Le fichiers demandé aux administrateurs et  à destination de l'annuaire ENT est issu du système d'informations académique via une extraction au format CSV de l'application BASE_ELEVES.

Cette interface est branchée au plus haut niveau de gestion des comptes, l'annuaire ENT, qui ensuite dispatche les comptes dans les 2 versions de l'ENT V2 et V3.

*Le traitement prendra en entrée les données des fichiers CSV et les transformera dans les formats utilisées par les api d'insertion/modification de base de données.* Cette partie est spécifiée plus bas dans ce document.

Les développements devront s'intégrer dans l'existant, en utilisant au maximum les fonctions, commandes et classes existantes. Il n'est pas demandé dans ce travail de refactorer l'ensemble de l'application, néanmoins des propositions de refactoring pourront être faites en fin de projet.

## L'existant
L'interface web de l'annuaire ENT est existante. Il faut donc intégrer la fonctionnalité CSV dans l'existant.
L'enchaînement des écrans est aussi déjà intégré, mais nécessitera d'être revue afin d'évoluer avec les developpements demandé. Aucun traitement n'a encore été ecrit, l'existant représente donc les mockups d'enchaînement des écrans.

## Structure du fichier issu de BASE_ELEVES
### Le fichier ELEVES
L'extraction CSV de BASE_ELEVES contient les champs suivants :

- Nom Elève
- Nom d'usage Elève
- Prénom Elève
- Date naissance
- Sexe
- Adresse1
- Cp1
- Commune1
- Pays1
- Adresse2
- Cp2
- Commune2
- Pays2
- Cycle         (Il s'agit du cycle d'apprentissage : cycle II, ou III)
- Niveau        (On trouve dans ce champs le niveau de la classe CP, CE1 , CE2, CM1 , CM2)
- Classe        (On trouve ici la typologie de classe : "01 COURS PREPARATOIRE-COURS ELEMENTAIRE 1", "02 COURS ELEMENTAIRE 1 ET 2", "03 COURS ELEMENTAIRE 2 COURS MOYEN 1", "04 COURS MOYEN 1 ET 2")
- Attestation fournie         (Oui/Non)
- Autorisations associations  (Oui/Non)
- Autorisations photos        (Oui/Non)
- Décision de passage         (Oui/Non)

Les quatres derniers champs fournis ne sont pas utilisés par le processus d'alimention à mettre en place.

### Le fichiers des parents
La structure du fichiers des arents est plus complexe puisque la relation 1-n parents <-> enfants est mise à plat dans le fichier.
Cela signifie que la séquences des derniers champs peut se répéter autant de fois sur la même ligne qu'il y a d'enfants. Ce fichier a donc une structure variable.

- Civilité Responsable    (3 valeurs possibles : M., MME, MLLE)
- Nom usage responsable
- Nom responsable
- Prénom responsable
- Adresse responsable
- CP responsable
- Commune responsable
- Pays
- Courriel
- Téléphone domicile
- Téléphone travail
- Numéro de poste
- Téléphone portable
Puis les 4 champs répétés autant de fois que d'enfants dans l'école.
- Nom d'usage enfant
- Nom de famille enfant
- Prénom enfant
- Classes enfants

### Le fichiers PROFS

## Structure de l'annuaire ENT
### l'UID unique
A chaque utilisateur est affecté un uid unique normé par l'annexe du SDET concernant l'annuaire ENT. Une fonction php permet de récupérer un UID en fonction de certains paramètres.

### Règles de validation des données
Une classe PHP permet de créer des règles de validation des données et ainsi de pouvoir afficher en face de chaque compte l'état des données. Ces états sont au nombre de 3 : 
- OK : le compte peut être créé, les données reçues sont valides.
- WARN : certaines données sont invalides, mais cela n'emêche pas la création du compte
- ERROR : certaines données sont invalides et c'est bloquant pour la création du compte.

Ces règles sont exécutées à l'insertion dans la base de données, et leur résultat est stocké dans un attribut de la table concernée.
Par exemple, pour les élèves, le traitement d'insertion est `insert_eleves([])` Ce traitement insert toutes les lignes d'élèves du tableau passé en paramètre dans la table `eleves`, et à chaque insertion, les règles d'analyse sont exécutées et leur résutat stocké dans l'attribut `etat_previsu` de la table `eleves`.
Cela siginfie que les données, même si elle ne sont pas correctes, sont insérées dans la base tampon. Elles ne seront de toutes façon pas exportées vers les ENT si elle sont marquées 'ERROR'.
Ce choix a été fait initialement, pour permettre aux administrateurs d'établissement de concerver un état consistant des erreurs à corriger, donné sur simple appel de la page de bilan.

## Correspondances des données

### L'établissement
Lorsqu'on arrive sur l'interface d'alimentation par CSV, l'établissement est déjà choisi (son code UAI est un paramètre requis pour afficher cet écran).
Par conséquent, l'établissement existe déjà dans le référentiel de l'annuaire ENT (Si ce n'est pas le cas, charge aux administrateur de l'ENT de l'y ajouter).
Il n'y a donc pas à modifier le référentiel, concernant l'établissement.

### Les élèves

### Les parents

### Les profs

## Règles de gestion

## écrans d'import
Les paramètres requis pour accéder à l'écran d'import sont : 
- action=csv
- rne=[un code uai de collège]
Le breadcrumb est renseigné automatiquement pour cette action.

### Zonning global
Ce zonneing global est déjà existant. Il est simple : une zone de travail (le déroulement des étapes) et une zone d'aide contextuelle.
![images/zonning.png](./images/zonning.png)

### Etape 1 : Choix du profil d'utilisateurs à importer
### Etape 2 : upload du fichier
### Etape 3 : prévisualisation des changements, application des règles de validation
### Etape 4 : Application des changements
### Etape 5 : Compte rendu d'activité

## Environnement technique 
### Client web
- HTML5 
- Twitter Bootstrap 
- JQuery

### Serveur
- Apache 2.0
- Php 5.3.10

### Base de données
- Mysql
