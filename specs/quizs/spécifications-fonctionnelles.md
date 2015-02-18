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

## Exécuter/Jouer un Quiz
	Il s'agit de l'interface de visualisation ou de rendu d'un Quiz, qui permet de l'exécuter.

### IHM
	
### Gestion des sessions
	Une session représente le fait qu'un utilisateur est en train d'exécuter le quiz. Elle démarre au lancement du quiz et se termine lorsque l'utilisateur le quitte, qu'il soit arrivé à la fin ou pas.

	Seules les sessions des élèves sont conservées et peuvent être consulter par le profs qui leur a publié, il n'est pas forcé d'être l'auteur du quiz.

	Les sessions des autres utilisateurs que les élèves sont détruites un fois terminées.

## Créer / Modifier un quiz

### Paramètres généraux

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

## Calcul des scores

### Pour une question

### score global



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
