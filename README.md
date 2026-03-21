# TurnRPG – Turn-Based RPG Prototype (React & JavaScript)

TurnRPG is a prototype designed for game developers who want to build turn-based games using a simple, pre-structured foundation that supports scalability and extensibility.

## Demonstration

If you want to see how the base project behaves in its current state, you can access the playable version on GameJolt:
https://gamejolt.com/games/turnRPGprototype/1040406

## Table of Contents

* About
* Features
* Technologies
* Installation
* Usage
* Architecture
* License

## About

This prototype was developed as a final course project (TCC) for a Frontend Programming course using React and JavaScript.

The goal of the project was to create something challenging and relevant to my interests. Since I was already interested in game development and found the chosen tools accessible, I decided to build a base prototype for turn-based games.

The main problem addressed in this project is the rigidity of many game implementations, which makes them difficult to extend or modify. In order to be useful as a starting point for other developers, the system needed to support scalability and allow the addition of new features without requiring major changes to the existing codebase.

To solve this, I designed a modular system with a simple and flexible execution flow.

## Features

### GameJolt API Integration

The project includes integration with the GameJolt API, supporting automatic authentication and trophy unlocking.
The integration logic is implemented in `gamejolt.js`, located at `stten/src/services/Gamejolt`.

#### Automatic Authentication

Authentication works in two scenarios:

* When running inside the GameJolt platform, authentication is handled automatically.
* When running locally (development environment), the developer must provide credentials manually.

To enable local authentication, credentials must be set in `Credentials.js`, located at `stten/src/components/DEV`.

#### Trophy System

To unlock a trophy, the function `unlockTrophy(TrophyId)` must be called from `gamejolt.js`, where `TrophyId` corresponds to the trophy ID defined on GameJolt.

---

### Modular Systems

Core systems such as health recovery, damage handling, and enemy AI are implemented as modular functions.

These systems are located in:
`stten/src/components/Game/Systems`

They are imported and used by `BattleSystem.jsx`.

Each system:

* Receives the current state as input
* Processes and modifies relevant values
* Returns the updated state with the same structure

This approach allows systems to be modified or extended independently without tightly coupling the logic.

---

### Enemy AI

The enemy AI is also implemented as a modular system.

There are currently two predefined behavior profiles:

* **Dumb**
* **Tactical**

These profiles influence decision-making parameters.

The decision process works as follows:

* The system retrieves the current state of both the player and the enemy (health and posture).
* It evaluates these values and calculates an action priority score based on the AI profile.
* Based on this priority and a randomized factor, the AI decides whether to attack or defend.

#### Fear System

To prevent predictable behavior, the AI includes a fear mechanic:

* When the enemy deals damage, its fear decreases, making it more aggressive.
* When the enemy receives damage, its fear increases, making it more defensive.

This dynamic creates less predictable and more varied behavior patterns.

## Technologies

* React
* JavaScript (ES6+)
* Vite
* HTML5
* CSS3
* GameJolt API
  
## Installation

To start development, simply clone the repository:

```bash
git clone <repository-url>
cd <project-folder>
npm install
```

---

## Usage

### Running in Development Mode

To run the project locally in development mode, navigate to the project directory and execute:

```bash
npm run dev
```

After running this command, a local URL will be provided in the terminal to access the application.

### Building the Project

To generate a production build, run:

```bash
npm run build
```

---

## Architecture

The project uses `App.jsx` as the main scene controller, managing navigation between `Menu.jsx` and `Game.jsx`.

* `Menu.jsx` provides a simple entry interface.
* The core game logic is implemented in `Game.jsx` and `BattleSystem.jsx`.

### Game.jsx

`Game.jsx` acts as the main game controller.

Its responsibilities include:

* Managing the game scene
* Handling user input from the UI
* Passing data to the battle logic
* Receiving processed results and updating the UI

It also stores the global game state, which is shared across components.

---

### BattleSystem.jsx

`BattleSystem.jsx` is responsible for orchestrating the battle logic.

It:

* Controls the battle flow
* Processes game states
* Executes the different phases of the turn system

---

### Battle Flow

The battle system is divided into phases, controlled by a `phase` state:

* `creating`
* `queue`
* `awaiting_input`
* `action`
* `end_turn`
* `analysing`

These phases are managed using `useEffect`, meaning each phase is triggered whenever the `phase` state changes.

Depending on the dependencies of each phase, the system may transition to the next phase or re-execute logic as needed.

---

### Entity Instantiation

Players and enemies are instantiated and stored in arrays.
All entities share the same base structure to ensure compatibility with the modular systems.

Enemies are created from a predefined library:
`stten/src/components/Game/Enemies/EnemyList.json`

These instances are stored in global arrays within `Game.jsx`, allowing the UI to access and render their data.

---

### Data Flow and Modular Systems

The system follows a structured data flow:

1. The full game state is passed to a modular system
2. The system processes and modifies relevant values
3. The updated full state is returned

This ensures:

* Consistency of data structure
* Decoupling between systems
* Easier extensibility

---

## License

This project allows you to expand, modify, and commercialize your own game based on it.

Attribution is appreciated if you use this project as a base.

For more details, see the `LICENSE.md` file.
