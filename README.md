# 🤖 Bumper Smash — EvalBot Assembly Game

**Bumper Smash** est un jeu embarqué développé en **assembleur ARM** sur un robot **Stellaris LM3S9B92 EvalBot**, dans le cadre d'un projet académique à **ESIEE Paris**.

Inspiré du jeu *Smash Hit*, le projet exploite directement les périphériques matériels du robot — **moteurs, bumpers, LEDs, bouton et haut-parleur** — afin de créer un jeu autonome basé sur la détection de collisions.

<p align="center">
  <img src="docs/bumper-smash.jpg" alt="Bumper Smash sur EvalBot" width="750">
</p>

## 🎥 Démonstration

▶️ [Voir Bumper Smash en fonctionnement sur YouTube](https://youtu.be/Eq4oSJp94r8)

La démonstration présente le jeu exécuté directement sur le robot **Stellaris LM3S9B92 EvalBot**, avec son déplacement, la détection des collisions et les différents retours visuels et sonores.

## 🎮 Principe du jeu

Le joueur démarre une partie à l'aide du bouton de l'EvalBot.

Après un compte à rebours, le robot commence à se déplacer automatiquement.

L'objectif est de réaliser un nombre défini de collisions avec les bumpers avant la fin du temps imparti.

Lorsqu'une collision est détectée, le robot :

1. incrémente le score ;
2. déclenche un retour visuel et sonore ;
3. recule ;
4. change de direction ;
5. reprend son déplacement.

Chaque manche possède un objectif de collisions à atteindre dans un temps limité.

Le jeu se termine par une **victoire** si les différentes manches sont complétées ou par une **défaite** si le temps imparti est écoulé.

## ✨ Fonctionnalités

- Déplacement autonome de l'EvalBot
- Contrôle des moteurs en assembleur
- Détection des collisions via les bumpers gauche et droit
- Système de score
- Objectifs évolutifs selon les manches
- Manches chronométrées
- Compte à rebours avant le démarrage
- Gestion des états de victoire et de défaite
- Animations avec les LEDs de l'EvalBot
- Effets sonores lors des collisions
- Musiques de démarrage, victoire et défaite
- Menu d'attente avant le lancement d'une partie
- Gestion des entrées/sorties GPIO

## 🧠 Fonctionnement

Le programme repose sur plusieurs états successifs :

```text
        Lobby
          │
          ▼
   Appui sur Switch
          │
          ▼
   Compte à rebours
          │
          ▼
      Début manche
          │
          ▼
   Déplacement robot
          │
     ┌────┴────┐
     │         │
 Collision   Timer
     │         │
     ▼         ▼
 Score +1   Temps écoulé
     │         │
     ▼         ▼
 Objectif ?  Défaite
     │
  ┌──┴──┐
  │     │
 Non   Oui
  │     │
  ▼     ▼
Recul  Manche suivante
  │
Rotation
  │
  └──────► Déplacement
```

À chaque collision, le robot recule puis effectue une rotation avant de reprendre son déplacement.

Une manche est remportée lorsque l'objectif de collisions est atteint avant la fin du chronomètre.

## 📈 Difficulté progressive

L'objectif augmente au fil des manches.

Le nombre de collisions nécessaires est calculé selon :

```text
Objectif = 1 + 2 × (Manche - 1)
```

Ce qui donne :

| Manche | Objectif |
|:---:|:---:|
| 1 | 1 collision |
| 2 | 3 collisions |
| 3 | 5 collisions |

Le joueur doit terminer les trois manches pour remporter la partie.

## ⚙️ Programmation bas niveau

Le projet a été développé en **assembleur ARM**, avec une gestion des différents périphériques matériels de l'EvalBot.

Le programme interagit notamment avec :

- les GPIO ;
- les bumpers gauche et droit ;
- le bouton de démarrage ;
- les LEDs ;
- les moteurs gauche et droit ;
- le système audio.

Les différents périphériques disposent de routines d'initialisation et de contrôle appelées depuis le programme principal.

Exemple d'initialisation :

```asm
BL Clock_Enable
BL Sound_Init
BL LED_init
BL BUMPER_init
BL SWITCH_init
BL MOTEUR_INIT
```

Le programme initialise ensuite les variables nécessaires au fonctionnement du jeu avant d'entrer dans le lobby.

## 🔌 Entrées / Sorties

Le projet utilise plusieurs périphériques de l'EvalBot.

### Entrées

| Périphérique | Port | Broche |
|---|:---:|:---:|
| Bumper droit | E | 1 |
| Bumper gauche | E | 0 |
| Switch 1 | D | 6 |

### Sorties

| Périphérique | Port | Broche |
|---|:---:|:---:|
| LED 1 | F | 4 |
| LED 2 | F | 5 |
| Speaker | J | 3 |

## 🛠️ Technologies et matériel

- **Assembleur ARM**
- **Stellaris LM3S9B92 EvalBot**
- **GPIO**
- **Bumpers**
- **Moteurs**
- **LEDs**
- **Speaker**
- **Switch**
- **TCRT5000** — prototype de suivi de ligne

## 📂 Organisation du programme

Le programme principal s'appuie sur plusieurs modules permettant de séparer la logique du jeu de la gestion des périphériques.

```text
Bumper Smash
│
├── Programme principal
│   ├── Lobby
│   ├── Démarrage de la partie
│   ├── Gestion des manches
│   ├── Chronomètre
│   ├── Gestion des collisions
│   ├── Score
│   ├── Transition entre les manches
│   └── Victoire / Défaite
│
├── Bumpers
├── LEDs
├── Switch
├── Moteurs
└── Audio
```

Les différentes routines matérielles sont importées dans le programme principal afin de contrôler les périphériques de l'EvalBot.

## 👥 Auteurs

Projet académique réalisé en équipe par :

- **Kadir Ersoy**
- **Valentin Hodonou**

## 🤝 Répartition du travail

### Kadir Ersoy

- Configuration des bumpers, LEDs et switch
- Tests unitaires des bumpers, LEDs et switch
- Création du lobby
- Gestion du lancement du jeu
- Gestion des collisions avec les bumpers
- Gestion des moteurs
- Système de manches
- Gestion de la fin du jeu

### Valentin Hodonou

- Configuration du système audio
- Tests unitaires de la musique de démarrage
- Tests unitaires de la musique de victoire
- Tests unitaires de la musique de défaite
- Tests unitaires du son de collision
- Musique de démarrage
- Musique de victoire
- Musique de défaite
- Gestion du scénario de défaite

## 🎓 Contexte académique

Projet réalisé à **ESIEE Paris** dans le cadre de l'apprentissage de l'**architecture des systèmes et de la programmation bas niveau**.

L'objectif était de mettre en pratique la programmation en assembleur à travers le contrôle des périphériques d'un système embarqué et la réalisation d'une application interactive complète.
