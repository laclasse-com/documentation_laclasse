# Spécifications fonctionnelles 
# Intégration de l'import CSV des élèves et des profs dans l'annuaire ENT.

## objectif et contours fonctionnels
L'annuaire ENT est le service qui réaliser l'analyse et l'importation des comptes fournis par le système d'information de l'académie de LYON. Elle réceptionne les données poussées par l'académie tout les jours, parse le XML et consolide une base tampon de comptes d'élèves, de parents et de personnels de l'éducation nationale. Cette base alimente ensuite à la demande (manuellement ou automatiquement les ENT v2 et V3, selon le choix de chaque établissement).

### Schéma général de fonctionement

![../../interfaces/alimentation-academique.png](../../interfaces/alimentation-academique.png)

L'objectif est de développer une interface permettant le chargement et le traitement de fichiers CSV pour la création de comptes d'élèves et de profs dans les écoles primaires.
Le fichier demandé aux administrateurs et à destination de l'annuaire ENT est issu du système d'information académique via une extraction au format CSV de l'application BASE_ELEVES.

Cette interface est branchée au plus haut niveau de gestion des comptes, l'annuaire ENT, qui ensuite dispatche les comptes dans les 2 versions de l'ENT V2 et V3. L'alimentation CSV devient alors une source autoritaire au même titre que l'alimentation académique.

*Le traitement prendra en entrée les données des fichiers CSV et les transformera dans les formats utilisées par les api d'insertion/modification de base de données qui sont déjà alimentée par l'analyse des fichiers XML.* Cette partie est spécifiée plus bas dans ce document.

![./images/interfacage-csv.png](./images/interfacage-csv.png)

Les développements devront s'intégrer dans l'existant, en utilisant au maximum les fonctions, commandes et classes existantes. Il n'est pas demandé dans ce travail de refactorer l'ensemble de l'application, néanmoins des propositions de refactoring pourront être faites en fin de projet.

## L'existant
L'interface web de l'annuaire ENT est existante. Il faut donc intégrer la fonctionnalité CSV dans l'existant.
L'enchaînement des écrans est aussi déjà intégré, mais nécessitera d'être revue afin d'évoluer avec les developpements demandé. Aucun traitement n'a encore été ecrit, l'existant ne représente donc qu'une maquette d'enchaînement des écrans.

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

### Le fichiers PROFS

Le fichier prof ne sera pas issu de BASE_ELEVE, mais constitué de toute pièce avec les attributs suivants.
- civilite
- nom
- prenom
- mailAcademique
- niveau   (On doit trouve dans ce champs le niveau de la classe CP, CE1 , CE2, CM1 , CM2)

## Structure de l'annuaire ENT
### l'UID unique
A chaque utilisateur est affecté un uid unique normé par l'annexe du SDET concernant l'annuaire ENT. Une fonction php permet de récupérer un UID en fonction de certains paramètres, notamment l'identifiant de jointure.

### Règles de validation des données
Une classe PHP permet de créer des règles de validation des données et ainsi de pouvoir afficher en face de chaque compte l'état des données. Ces états sont au nombre de 3 : 
- OK : le compte peut être créé, les données reçues sont valides.
- WARN : certaines données sont invalides, mais cela n'empêche pas la création du compte
- ERROR : certaines données sont invalides et c'est bloquant pour la création du compte.

Ces règles sont exécutées à l'insertion dans la base de données, et leur résultat est stocké dans un attribut de la table concernée.
Par exemple, pour les élèves, le traitement d'insertion est `insert_eleves([])` Ce traitement insert toutes les lignes d'élèves du tableau passé en paramètre dans la table `eleve`, et à chaque insertion, les règles d'analyse sont exécutées et leur résutat stocké dans l'attribut `etat_previsu` de la table `eleve`.
Cela siginfie que les données, même si elles ne sont pas correctes, sont insérées dans la base tampon. Elles ne seront de toutes façon pas exportées vers les ENT si elle sont marquées 'ERROR'.
Ce choix a été fait initialement, pour permettre aux administrateurs d'établissement de concerver un état consistant des erreurs à corriger, donné sur simple appel de la page de bilan, du fait que la mise à jour des données ne dépendait pas d'eux et était cadencée toutes les nuits.

## Correspondances des données

### L'établissement
Lorsqu'on arrive sur l'interface d'alimentation par CSV, l'établissement est déjà choisi (son code UAI est un paramètre requis pour afficher cet écran).
Par conséquent, l'établissement existe déjà dans le référentiel de l'annuaire ENT (Si ce n'est pas le cas, charge aux administrateurs de l'ENT de l'y ajouter).
Il n'y a donc pas à modifier le référentiel, concernant l'établissement.

### Les élèves

#### Table *eleve*
Le problème majeur est que l'extraction CSV ne comporte aucun identifiant unique de la personne. Ce qui veut dire que l'on est obligé de générer cet identifiant unique sur la base des données fournies les plus pérennes. 

Pour l'élève, il s'agit de son *nom*, son *prénom* et sa *date de naissance*. 
En effet, au sein d'une même académie, il est rare, voir quasiment impossible de tomber sur des doublons en associant ces 3 attributs.

La contrainte supplémentaire consiste à générer un identifiant de type `entier` car ceux envoyés par l'académie pour les comptes de collèges sont de ce type, mais dans des plages relativement basse (largement inférieurs à 10 000 000).
Enfin la dernière contrainte est bien évidemment d'éviter les collisions entre les identifiants issus de l'académie et ceux générés en interne dans l'annuaire ENT pour les comptes issus des fichiers CSV.

Une fonction php permet de générer un entier sur la base d'une chaîne de caractères. Elle sera utilisée pour la génération de cet identifiant unique.
```php
function get64BitHash($str)
{
  return gmp_strval(gmp_init(substr(md5($str), 0, 16), 16), 10);
}
```
exemple d'usage : 
```php
echo get64BitHash("PIERRE-GILLESLEVALLOIS03/07/1970690078K");
// Affiche 2721088881606737241
```


| Table aaf.eleve         | Données du fichiers CSV | Génération                     | Transformation       | Commentaire                                                 |
|-------------------------|-------------------------|--------------------------------|----------------------|-------------------------------------------------------------|
| ENTPersonJointure       | -                       | génération en interne sur la base de la fonction get64BitHash()                          |                      |                                                             |
| categoriePersonne       | -                       | null                           |                      |                                                             |
| ENTPersonDateNaissance  | Date naissance          |                                |                      | au format ISO 8601                                         |
| ENTPersonNomPatro       | Nom Elève               |                                |    Majuscules    |                                                             |
| sn                      | Nom Elève               |                                |                      |                                                             |
| givenName               | Prénom Elève            |                                |                      |                                                             |
| ENTPersonAutresPrenoms  | Prénom Elève            |                                |                      |                                                             |
| personalTitle           | Sexe                    |                                | Formatage M. ou Mlle |                                                             |
| ENTEleveBoursier        | -                       |                                |                      |                                                             |
| ENTEleveRegime          | -                       |                                |                      |                                                             |
| ENTEleveTransport       | -                       |                                |                      |                                                             |
| ENTEleveStatutEleve     | -                       |                                |                      |                                                             |
| ENTEleveMEF             | Classe                  |                                |                      |  Code MEF trouvé dans la table des MEF en fonction de la classe (CP,CE1, CE2, CM1, CM2)                                                      |
| ENTEleveLibelleMEF      | Classe                  |                                |                      | Libellé trouvé dans la table des MEF, en fonction du code   |
| ENTEleveNivFormation    | Niveau                  |                                |                      |                                                             |
| ENTEleveFiliere         | -                       |                                |                      |                                                             |
| ENTPersonStructRattach  | -                       | Identifiant de l'établissement |                      |                                                             |
| ENTEleveStructRattachId | -                       |                                |                      |                                                             |
| etat_previsu            | -                       | verifier_eleve()               |                      | Ce champs est renseigné automatiquement lors de l'insertion |
| date_last_maj           | -                       |                                |                      | Ce champs est renseigné automatiquement lors de l'insertion |
| uid                     | -                       | fonction get_uid(ENTPersonJointure)             |                      | ENTPersonJointure est l'dentifiant généré avec la fonction get64BitHash                                         |

Note sur les MEF : 
Les codes de Module Elémentaire de Formation au format "Education Nationale" font partie du référentiel de l'annuaire ENT. Pour l'école, ils sont les suivants : 
- 00110002110 : CP
- 00210002210 : CE1
- 00210002220 : CE2
- 00310002210 : CM1
- 00310002220 : CM2

Il faudra coder une fonction qui permet d'dentifier les MEF au format officiel (11 chiffres) et de les associer avec les champs du fichier CSV  Cycle,	Niveau,	Classe, qui bien évidemment ne sont pas formattés de la même manière (ça aurait été trop facile).

Voici un exemple de ce qu'on peut trouver dans le fichier csv :(

| Cycle | Niveau | Classe | Code MEF à Associer |
|-------|--------|--------|---------------------|
| CYCLE II	 | CP	 | 01 COURS PREPARATOIRE-COURS ELEMENTAIRE 1 | 00110002110 |
| CYCLE II	 | CE1 | 02 COURS ELEMENTAIRE 1 ET 2 | 00210002210 |
| CYCLE II	 | CE1 | 01 COURS PREPARATOIRE-COURS ELEMENTAIRE 1 | 00210002220 |
| CYCLE III | CE2 | 03 COURS ELEMENTAIRE 2 COURS MOYEN 1 | 00310002210 |
| CYCLE III | CM2 | 04 COURS MOYEN 1 ET 2 | 00310002220 |


#### Table *classe*
Dans cette table, chaque classe n'est insérée qu'une seule fois.

| Table aaf.classe     | Données du fichiers CSV | Génération                     | Transformation          | Commentaire            |
|---------------------|-------------------------|--------------------------------|-------------------------|------------------------|
| ClasseStructRattach | -                       | identifiant de l'établissement |                         |                        |
| ClasseNom           | Niveau                  |                                |                         |                        |
| ClasseLibelleMEF    | Classe                  |                                |                         |                        |
| ClasseCodeMEF       | Classe                  |                                | Code MEF issu de la table des MEF |                    |
| ClasseMEFRattach    | -                       |                                |                         | code issu de la table MEF |
| date_last_maj       | -                       |                                |                         |  automatiquement renseigné à l'insertion                      |

#### Table des *eleve_classe*
C'est la table de relation entre les élèves et les classes. Cette table est automatiquement renseignée à l'insertion d'un élève.

### Les profs

#### table `pers_educ_nat`
Ici aussi, le fichier CSV ne fournira pas d'identifiant unique. il faudra donc le générer sur la base de la fonction get64BitHash(), à laquelle on passera l'adresse email académique.

| Table aaf.pers_educ_nat        | Données du fichiers CSV | Génération                     | Transformation          | Commentaire     |
|--------------------------------|-------------------------|--------------------------------|-------------------------|-----------------| 
| categoriePersonne              | - |  |  |  | 
| ENTPersonJointure              | - | génération en interne sur la base de la fonction get64BitHash() |  |  | 
| ENTPersonDateNaissance         | - |  |  |  | 
| ENTPersonNomPatro              | nom |  |  |  | 
| sn                             | nom |  |  |  | 
| givenName                      | prenom |  |  |  | 
| personalTitle                  | civilite |  |  |  | 
| mail                           | mailAcademique |  |  |  | 
| ENTAuxEnsCategoDiscipline      | - |  |  |  | 
| PersEducNatPresenceDevantEleves| - | 'O' |  |  | 
| ENTPersonAdresse               | - |  |  |  | 
| ENTPersonCodePostal            | - |  |  |  | 
| ENTPersonVille                 | - |  |  |  | 
| ENTPersonPays                  | - |  |  |  | 
| ENTAuxEnsClassesMatieres       |  Niveau |  |  [idEtablissement]$[Niveau de la classe]$023500  | Ce champ doit être généré avec l'identifiant de l'établissement, le niveau de la classe et le xode matière 023500, concaténés et spéparés par le caractère '$'              | 
| ENTAuxEnsClassesPrincipal      | - | 'O' |  |  | 
| ENTPersonStructRattach         | - | [idEtablissement] |  |  | 

## Règles de gestion

### Pour les élèves
Pour toutes ces mises à jour, il suffit de générer une structure php aux normes de ce que comprend l'api d'insertion.
Le traitement d'API prend en paramètre un tableau structuré (initiallement issue du parsing XML des fichiers académiques).
Il est à noter que certains attributs sont multivalués, aisni donc, chaque attribut prend comme valeur un potentiel tableau php.
Cette structure comprend l'intégralité des élèves parsés dans le fichier CSV.
La structure attendue pour l'appel à `insert_eleve`est la suivante :

```php
t [0] => {
    'categoriePersonne' => 		{ [0] => ... }
    'ENTPersonJointure' => 		{ [0] => ... }
    'ENTEleveStructRattachId' =>{ [0] => ... }
    'ENTPersonDateNaissance' => { [0] => ... }
    'ENTPersonNomPatro' => 		{ [0] => ... }
    'sn' => 					{ [0] => ... }
    'givenName' => 				{ [0] => ... }
    'ENTPersonAutresPrenoms' => { [0] => ... }
    'personalTitle' => 			{ [0] => ... }
    'ENTEleveBoursier' => 		{ [0] => ... }
    'ENTEleveRegime' => 		{ [0] => ... }
    'ENTEleveTransport' => 		{ [0] => ... }
    'ENTEleveStatutEleve' => 	{ [0] => ... }
    'ENTEleveMEF' => 			{ [0] => ... }
    'ENTEleveLibelleMEF' => 	{ [0] => ... }
    'ENTEleveNivFormation' => 	{ [0] => ... }
    'ENTEleveFiliere' => 		{ [0] => ... }
    'ENTPersonStructRattach' => { [0] => ... }
    'ENTEleveClasses' => { 
    		[0] => {
    		'ClasseStructRattach' => 	{ [0] => ... } 
    		'ClasseNom' => 				{ [0] => ... }
    		'ClasseLibelleMEF' => 		{ [0] => ... } 
    		'ClasseCodeMEF' => 			{ [0] => ... } 
    		'ClasseMEFRattach' => 		{ [0] => ... }
    		'date_last_maj' => 			{ [0] => ... }
    	}
    }
  },
t[1] => ...

```
### Pour les Prof (personnels de l'éducation nationale).
La struction à générer à  a passer à l'api d'insertion `insert_pers_educ_nat` est la suivante.
A noter qu'un certain nombre de champs ne seron pas renseignés ou seront renseigné avec des valeurs en dur. C'est le cas pour la metière par exemple. La raison est que le référentiel des matières est adapté au secondaire, mais pas au primaire.

```php
"categoriePersonne" => { [0] => ... }
"ENTPersonJointure" => { [0] => ... }
"ENTPersonDateNaissance" => { [0] => ... }
"ENTPersonNomPatro" => { [0] => ... }
"sn" => { [0] => ... }
"givenName" => { [0] => ... }
"personalTitle" => { [0] => ... }
"mail" => { [0] => ... }
"ENTAuxEnsCategoDiscipline" => { [0] => ... }
"PersEducNatPresenceDevantEleves" => { [0] => 'O' }
"ENTPersonAdresse" => { [0] => ... }
"ENTPersonCodePostal" => { [0] => ... }
"ENTPersonVille" => { [0] => ... }
"ENTPersonPays" => { [0] => ... }
"PersEducNatPresenceDevantEleves" => { [0] => 'O' }
"ENTAuxEnsClassesMatieres" => { [0] =>  [idEtablissement]$[Niveau de la classe]$023500 }
"ENTAuxEnsClassesPrincipal" => { [0] => 'O'] }
"ENTPersonStructRattach" => { [0] => [idEtablissement] }
```

## écrans d'import
Point d'entrée : *?action=csv&rne=[un code uai de collège]*
Le breadcrumb est renseigné automatiquement pour cette action.

### Zonning global
Ce zonning global est déjà existant. Il est simple : une zone de travail (le déroulement des étapes) et une zone d'aide contextuelle. Le texte de la zone d'aide contextuelle sera complété poar Erasme.
![images/zonning.png](./images/zonning.png)

### Etape 1 : Choix du profil d'utilisateurs à importer
Url d'accès : *?action=csv&rne=[un code uai de collège]*
Cette étape permet de choisir le type de fichier à importer (eleves, ou profs).

### Etape 2 : upload du fichier
Url d'accès : *?action=csv&rne=[un code uai de collège]&profil=[eleve, prof]*
Cette étape fait l'upload du fichier, la vérification de sa structure et l'analyse de conformité par rapport aux données attendues.
Si le fichier n'est pas conforme, un écran d'erreur est affiché, expliquant quelles sont les conditions qui ne sont pas remplies.
En revanche si le fichier est conforme, le traitement de création des comptes est réalisé.

### Etape 3 : Compte rendu d'activité
Ce compte rendu d'activité est donné par l'url suivante : *?action=rapport&rne=[code UAI]&profil=[eleve, prof]*
Le travail côté serveur est donc de rediriger vers cette page qui affiche un état des comptes élèves ou profs créés.

## Logging du traitement
Une classe de log permet d'instrumentaliser simplement le traitement de création de comptes afin qu'il soit possible d'en relire le déroulement à postériori.

_exemple d'usage de la classe de logs_
```php
/* 
* Classe simple et efficace de gestion de log, création de fichier, écriture, formatage.
* Les logs sont au format HTML par défaut, comme ça ils sont consultables depuis le web.
* 14/06/2009 : PGL.
*
* Usage :
*/
$monLog = new log("Interface CSV");
$monLog->section("Création du compte...")
$monLog->info("texte texte");
$monLog->warn("texte texte");
... 
$monLog->error("texte texte");
...
$monLog->panic("texte texte");
...
$monLog->end();
...
```

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
