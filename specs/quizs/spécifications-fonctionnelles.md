# Module de Quiz / Laclasse.com /V3.
# Spécifications fonctionnelles

## Introduction
Le module de s en ligne est un composant de l'ancienne version de laclasse.com. Il doit être ré-écrit pour la version 3.
Ce module comporte 3 points d'entrées : 

- L'exécution des quizs
- La création/modification (back-office)
- L'examen des sessions et résultats des élèves.

Il sera ré-écrit quasiment à iso-fonctionnalité de la version existante, en ajoutant toutefois la prise en charge des contenus multimédia (Audio, Images, Vidéos) dans les questions, les éléments de réponse et les points de corrections.

Les des questions sont au nombre de 3 : 

- QCM / QCU
- Appariement ou Association
- Textes à trous

L'application sera faite au maximum en mode "Single Page App" en utilisant les fonctionnalités d'Angular JS.
Dans les principes généraux de navigation, il sera demandé de veiller à ce que chaque action de destruction de données soit confirmée par l'utilisateurs.

## Exécuter/Jouer un Quiz
	Il s'agit de l'interface de visualisation ou de rendu d'un Quiz, qui permet de l'exécuter.

### IHM
	
### Gestion des sessions
Une session représente le fait qu'un utilisateur est en train d'exécuter le quiz. Elle démarre au lancement du quiz et se termine lorsque l'utilisateur le quitte, qu'il soit arrivé à la fin ou pas.

Seules les sessions des élèves sont conservées et peuvent être consulter par le profs qui leur a publié, il n'est pas forcé d'être l'auteur du quiz.

Les sessions des autres utilisateurs que les élèves sont détruites un fois terminées.

## Créer / Modifier un quiz
Le quiz est un type de document particulier succeptible d'être manipulé dans l'application de gestion documentaire de l'ENT.
Cela permet de le partager facilement.

### Paramètres généraux
Les paramètres suivants seront définit et accessible en modification au début du processus de création d'un quiz
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

Tous ces paramètres pourrons être initialisé, soit un par un, soit par un mode d'exécution qui contriant les valeurs de plusieurs paramètres à la fois.

Les modes d'exécution du quiz à sa créationsont au nombre de 3 :

- Entrainement	| MELANGER=N | REFAIRE=Y | AFFICHAGE-SCORE=Y | AFFICHAGE-CORRECTION=Y | CORRECTION-FIN=N
- Exercice	| MELANGER=Y | REFAIRE=N | AFFICHAGE-SCORE=Y | AFFICHAGE-CORRECTION=Y | CORRECTION-FIN=Y
- Validation	| MELANGER=Y | REFAIRE=N | AFFICHAGE-SCORE=N | AFFICHAGE-CORRECTION=N | CORRECTION-FIN=N

### Choix du type de question

### QCM / 

### Appariements

### Textes à trous


## Partager un Quiz


## Récupérer/copier un Quiz



## Examiner les sessions

### Liste des sessions

#### Liste d'une classe

#### Liste d'un élève

## Calcul et gestion des scores

### Pour une question

### score global

### Affichage



## Intégration à la gestion documentaire

### L'extension `.quiz`

### Créer un quiz

### Editer un quiz

### publier un quiz

### examiner les sessions


## Cadre technique

### Serveur

  - Sinatra
  - Ruby

### Backend

  - Mysql

### Client

  - Angular JS

## Résumé des règles fonctionnelles

1. Il y a 3 types de questions QCM/QCU, Appariement, Textes à trous
2. Tout utilisateur peut avoir une session de quiz.
3. Seules les sessions des élèves sont conservées.
4. Les sessions des élèves sur un quiz donné sont consultables/gérables par le prof qui leur a publié.
5. Le mélange des questions est effectué à l'initialisation de la session.
6. Le score la question est affiché sur l'écran de correction si celle-ci n'est pas masquée *&* qu'elle intervvient après chaque question.
7. On peut revenir en arrière sur le quiz *que* si son paramétrage nous l'autorise.
