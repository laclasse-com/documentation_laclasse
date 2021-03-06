# Module de Quiz / Laclasse.com /V3.
# Spécifications fonctionnelles

## Introduction

Le module de quizs en ligne est un composant de l'ancienne version de laclasse.com. Il doit être ré-écrit pour la version 3.
Ce module comporte 3 points d'entrées : 

- L'exécution des quizs
- La création/modification (back-office)
- L'examen des sessions et résultats des élèves.

Il sera ré-écrit quasiment à iso-fonctionnalité de la version existante, en ajoutant toutefois la prise en charge des contenus multimédia (Audio, Images, Vidéos) dans les questions, les éléments de réponse et les commentaires de corrections.

Les des questions sont au nombre de 3 : 

- QCM / QCU
- Appariement ou Association
- Textes à trous

L'application sera faite au maximum en mode "Single Page App" en utilisant les fonctionnalités d'Angular JS.
Dans les principes généraux de navigation, il sera demandé de veiller à ce que chaque action de destruction de données soit confirmée par l'utilisateurs.
Elle devra être compatible avec les tablettes (responsive design, ergonomie adaptée pour les boutons et les cases à cocher).

## Description fonctionnelle

### Exécuter/Jouer un Quiz
Il s'agit de l'interface de visualisation ou de rendu d'un Quiz, qui permet de l'exécuter.
Le quiz se déroule question après question, une par page, la validation du formulaire d'une question entraîne le passage à la question suivante, avec selon le paramétrage du quiz, un possibilité de retour en arrière ou pas, un affichage ou pas de la correction, et du score pour la question.

Le premier écran récapitule le paramétrage du quiz pour préciser à l'élève dans quelles conditions il va travailler.

![images/014-reponse-quizs-lancement.jpg](./images/014-reponse-quizs-lancement.jpg)

#### Rendu des QCM

![images/015-reponse-quiz-qcm.jpg](./images/015-reponse-quiz-qcm.jpg)

#### Rendu des associations

![images/016-reponse-quiz-association.jpg](./images/016-reponse-quiz-association.jpg)

Tous les fichiers multimédia sont affichés en fenêtre modale.

![images/017-reponse-quiz-vu-media.jpg](./images/017-reponse-quiz-vu-media.jpg)

![images/018-reponse-quiz-association2.jpg](./images/018-reponse-quiz-association2.jpg)

#### Rendu des textes à trous

La liste des propositions de réponse (les trous) est triées dans l'ordre alphabétique.

![images/019-reponse-quiz-txt-trous.jpg](./images/019-reponse-quiz-txt-trous.jpg)

### Gestion des sessions
Une session représente le fait qu'un utilisateur est en train d'exécuter le quiz. Elle démarre au lancement du quiz et se termine lorsque l'utilisateur le quitte, qu'il soit arrivé à la fin ou pas.

Seules les sessions des élèves sont conservées et peuvent être consulter par le profs qui leur a publié, il n'est pas forcé d'être l'auteur du quiz.

Les sessions des autres utilisateurs que les élèves sont détruites un fois terminées.

#### Examiner les sessions
L'examen des sessions permet au prof de voir la progression de ses élèves sur les quizs qu'ils leur a publiés.
L'écran d'examen des sessions est une liste classée par ordre alphabétique de toutes les sessions regroupées par élève, liées à un quiz dans une classe.
Le paramètre d'entrée est donc l'identifiant du quiz.
Depuis cette IHM il doit être possible d'accéder au détail des réponses de chaque session de quiz d'un élève.

#### Liste des sessions
Cette liste contient antant de lignes que de sessions de quiz réalisées par chaque élève.
La liste affiche les attributs suivants :

- nom et prénom de l'élève, classe
- date et heure de la session
- score obtenu affiché graphiquement
- lien vers le détail de ses réponses (sur la ligne complète )
- lien vers un traitement de suppression de la session (non affiché sur le mockup mais nécessaire, sur la droite )

![images/023-suivi-resultats-eleves.jpg](./images/023-suivi-resultats-eleves.jpg)

Un filtre permet de réduire la liste des données affichées selon les critères suivants :
 - Derniers résultats (les + récents)
 - Premiers résultats (les + anciens)
 - Selection d'une classe


### Calcul et gestion des scores
Le score est un taux de réussite. Il borné et peut aller de 0 à 100%.

Afin de ne pas encourager la triche (!) il n'est pas possible d'obtenir le score maximum si l'on coche toutes les réponses à une question. Le calcul du score se fait donc en pondérant le nombre de réponses faites justes et fausses par rapport au nombre de réponses attendues.

#### Pour une question
La règle de calcul s'appliquant est donc la suivante : 
```
                          ( nb de réponses justes - ( nb de réponses fausses ) / 2 ) 
Score pour la question =  -----------------------------------------------------------
                                nb de bonnes réponses attendues pour la question
```
On voit ici qu plus il y a de réponses fausses plus le score est bas, s'il est négatif, il est ajusté à 0.

#### Pour le quiz
Le score global est la somme des scores de chaque question au prorata du nombre de questions, sans coéfficient particulier.
Le calcul du score est identique que le quiz ait été complété jusqu'à la dernière question ou pas.

#### Affichage
L'affichage se fait donc sous forme d'une jauge variant entre 0 et 100%.
L'arrondi est à l'unité. Pas de virgule.

## Intégration dans le portail
L'application quiz doit être indépendante du reste afin de respecter les critères de simplicité voulus pour cette version 3 de l'ENT.
Aussi son point d'entrée se fait par le portail par un carré approprié.
Cela permet de mettre en valeur ce module et et d'en proposer une utilisation indépendante de le GED.

![images/intégration-portail.png](./images/intégration-portail.png)

### Gestion de la correction
La correction peut être affichée après l'exécution de chaque question ou à la fin du déroulement du quiz, selon le paramétrage qui a été fait.
Les écrans de corrections sont bien évidemment en lecture seule.

#### Correction QCM

![images/020-correction-qcm.jpg](./images/020-correction-qcm.jpg)

#### Correction textes à trous

![images/022-correction-txt-trous.jpg](./images/022-correction-txt-trous.jpg)

#### Correction Associations.

![images/021-correction-association.jpg](./images/021-correction-association.jpg)


### Point d'entrée Profs / PEN
Le point d'entrée Prof propose un écran qui montre la liste de ses quizs, avec la possibilité d'en créer un nouveau, d'examiner les sessions des élèves, de partager, de publier aux élèves, de supprimer.
Des widgets lui montrent :

- la liste des nouvelles sessions depuis sa dernière connexion, limitée à 5 éléments.
- La liste des nouveaux partages des autres profs limitée à 5 éléments, avec la possibilité de récupérer le quiz (duppliquer et s'approprier le quiz).

Pour éliminer un élément de cette liste, le geste de "glisser vers la gauche" sera utilisé. Ce geste permet de faire dispaaître un élément et de raffraichir la liste avec des  nouveaux éléments non encore affichés.

![images/point-d-entree-prof.png](./images/point-d-entree-prof.png)

#### Créer / Modifier un quiz
Le quiz est un type de document particulier succeptible d'être manipulé dans l'application de gestion documentaire de l'ENT.
Cela permet de le partager facilement.

##### Paramètres généraux
Les paramètres suivants seront définit et accessibles en modification au début du processus de création d'un quiz
Tous ces paramètres prennent les valeurs O/N.

- _mélange des questions_
Si activé, permet de mélanger aléatoirement l'ordre des questions du quiz. Ce calcul est alors effectué à l'initialisation de la session.

- _affichage du score_
Si activé, le score de la question qui vient d'être terminée est affiché sur l'écran de correction (si elle n'est pas masquée *et* qu'on l'affiche après chaque question).

- _refaire le quiz plusieurs fois_
Si activé, l'utilisateur peut revenir sur les questions précédentes et modifier ses réponses.
Sinon, il n'a pas la possibilité de revenir à la question précédente (au passage, il ne faut pas qu'il puisse le faire par les fonctions du navigateur ! ).

- _Masquer la correction_
  - oui : La corrections n'est jamais affichée.
  - non : Possibilité de choisir l'intégration des écrans présentant la correction
    - _Correction après chaque question_
    - _Correction à la fin de l'exercice_

Tous ces paramètres pourrons être initialisés, soit un par un, soit par un mode d'exécution qui contraint les valeurs de plusieurs paramètres à la fois.

Les modes d'exécution du quiz à sa création sont au nombre de 3 :

| Mode         | Melanger | Refaire | Afficher score | afficher correction | corr. à la fin |
| ------------ | -------- | ------- | -------------- | ------------------- | -------------- |
| Entrainement | N | Y | Y | Y | N |
| Exercice     | Y | N | Y | Y | Y |
| Validation   | Y | N | N | N | N |

#### Choix du type de question
##### Données communes à tous les types de questions :
- La question
- L'aide
- Le commentaire de correction
- le type de question.
La première partie de l'écran est dédiée aux données communes sauf commentaire de correction, la deuxième partie du formualire, spécifique à un type de question, n'est affichée que lorsque ce type est choisi par l'utilisateur.
La troisième partie du formulaire est aussi commune à tous les types de question puisqu'elle contient le commentaire de correction.

##### Les QCM / QCU
C'est une question à choix multiples ou choix unique. Le rendu est le même pour ces deux catégories de questions.
Le fomrulaire de création est donc identique.
Par défaut, 8 propositions sont disponibles d'emblée à la saisie. Seuls les champs non vides sont enregistrés.

![images/004-ajouter-qcm.jpg](./images/004-ajouter-qcm.jpg)
![images/005-ajouter-qcm2.jpg](./images/005-ajouter-qcm2.jpg)


##### Les appariements ou associations
Il s'agit d'un mode de questions permettant d'associer plusieurs propositions d'un premier groupe à plusieurs propositions d'un second.
A l'instar du QCM, l'interface de création propose 8 emplacements pour chaque groupe de propositions. 

La création d'une association se déroule en deux temps.  On ajoute d'abord les propositions dans les deux groupes (gauche et droite).
Il n'est pas obligatoire d'avoir le même nombre de propositions dans les 2 groupes, et seuls les champs renseignés sont enregistrés.

![images/007-ajouter-association2.jpg](./images/007-ajouter-association2.jpg)

Dans un second temps, second écran, on réalise les leins entres propositions de gauche et propositions de droite.

![images/008-ajouter-association3.jpg](./images/008-ajouter-association3.jpg)

##### Les textes à trous
Ce sont des textes dans lequels certains mots doivent être complétés par l'élèves. Ces mots sont fournis par une liste fermée pouvant comporter des leurres, c'est-à-dire des réponses fausses.
La liste fermée représente sous forme d'une selectbox constituée de la liste dédoublonnée de tous les mots manquants du textes à laquelle s'ajoute la liste des leurres. Cette liste est ensuite triée par ordre alphabétique.

![images/011-ajouter-texte-trous2.jpg](./images/011-ajouter-texte-trous2.jpg)

#### Publication d'un quiz / notification des élèves
L'action de publier un quiz représente la mise à disposition de cet exercice pour un groupe d'élèves (classes ou regroupements).
Cette action propose donc de choisir la ou les classes/groupes concerner puis une date de début et de fin de publication.

Lors de la mise à disposition pour les élèves, l'application utilisera les API de notification de l'application de publipostage, si celle-ci est activée, pour notifier dans le flux de news du portailles élèves des groupes concernés par la publication.

![images/publication-quiz.png](./images/publication-quiz.png)

#### Partage de quiz
L'action de partager un quiz, le rend visible des autres profs quelque soit leur établissement de rattachement (tous les profs de l'ent qui ont accès au module de quizs).


#### Récupérer/copier un Quiz
Il est intéressant de pouvoir duppliquer un quiz partagé pour le modifier et l'adapater à ses élèves sans que cela n'affecte le quiz source. Cela permet d'amender le-dit quiz sans pour antant changer le quiz source partagé, sur lequel il peut y avoir des sessions en cours ou terminées.
Cette action effectue une dupplication complèete en base de données du quiz choisi, sauf les sessions.

### Point d'entrée Elèves/parents
Le point d'entrée des élèves présente les quizs qu'ils ont à faire par ordre du plus urgent (date de fin de publication la plus proche).
Sur la partie droite de l'écran, il voit la liste de sessions déjà effectuées.


## Cadre technique

### Développements

#### Serveur

  - Sinatra
  - Ruby

#### Backend

  - Mysql

#### Client

  - Angular JS

## Reprise de données
Il faudra re-importer les données des quizs dans la nouvelle structure de base de données. L'extraction des données de l'ancienne base est _à la charge d'ERASME_, elle sera faite au format qui convient le mieux pour permettre une intégration facile dans la nouvelle structure de base de données (JSON ? Insert SQL ?).

## Résumé des règles fonctionnelles importantes

1. Il y a 3 types de questions QCM/QCU, Appariement, Textes à trous
2. Tout utilisateur peut avoir une session de quiz.
3. Seules les sessions des élèves sont conservées.
4. Les sessions des élèves sur un quiz donné sont consultables/gérables par le prof qui leur a publié.
5. Il est possible de détruire une session de Quiz pour le prof qui a publier le quiz.
6. Le mélange des questions est effectué à l'initialisation de la session.
7. Le score la question est affiché sur l'écran de correction si celle-ci n'est pas masquée *&* qu'elle intervvient après chaque question.
8. On peut revenir en arrière sur le quiz *que* si son paramétrage nous l'autorise.
9. Le score est un pourcentage sous forme d'entier.
10. Score pour la question = ( nb de réponses justes - ( nb de réponses fausses ) / 2 ) / nb de bonnes réponses attendues pour la question;
11. Le score global est la somme des scores de chaque question au prorata du nombre de questions, sans coéfficient particulier.
12. Le calcul du score est identique que le quiz ait été complété jusqu'à la dernière question ou pas.
13. Un quiz publié apparaît dans pendant fenêtre de publcation (date de débu et de fin)
14. Les sessions des élèves ne disparraissent pas lorsqu'un quiz cesse d'être dans l'intervalle de publication.


