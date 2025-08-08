# Java Networked Trivia Game

This project is a networked multiplayer trivia game written in Java. It consists of a server and multiple clients that interact over TCP and UDP sockets. The game features real-time buzzing, question distribution, answer submission, scoring, and a graphical user interface (GUI) for players.

---

## Overview: How It Works

### Architecture

- **Server**: Manages questions, buzz-ins, scoring, and client connections. Handles communication via TCP (for questions and answers) and UDP (for "buzz" events).
- **Client**: Displays questions in a GUI, lets users buzz in, select answers, and view scores.

### Game Flow

1. **Startup**
   - Server loads a pool of questions and waits for clients to connect.
   - Clients read configuration (such as server IP, ports, and client ID) and establish TCP/UDP connections to the server.

2. **Question Distribution**
   - The server periodically sends trivia questions to all clients using TCP.
   - Each question contains four answer options (A/B/C/D).

3. **Buzz-In (Polling)**
   - Players "buzz in" using a button on the client GUI (sends a UDP packet to the server).
   - The server tracks who buzzed in first, sending an "ACK" to the fastest client (enabling answer selection), and "NACK" to others.

4. **Answer Submission**
   - The buzzed-in client submits their answer via TCP.
   - The server checks correctness, updates the client's score (+10 for correct, -10 for wrong), and sends feedback.
   - Scores are tracked and updated per client.

5. **Scoring and Game Over**
   - The server maintains scores and sends updates after each question.
   - When all questions are completed, the server announces the winner and leaderboard.

6. **GUI Features**
   - The client window displays the current question, possible answers, a timer, the player’s score, and buzz/submit buttons.
   - Background music plays in the client (can be paused/resumed).
   - Visual feedback (colors, timers) enhance user experience.

---

## Main Components

### Server Side

- **TriviaServer**: Handles incoming connections, question distribution, answer processing, buzz events, and scores.
- **ClientThread**: Manages a single client’s TCP connection.
- **BuzzMessage**: Encapsulates buzz-in data (address, port, client ID).
- **QuestionPool**: Loads questions from resources and checks correct answers.

### Client Side

- **ClientWindow**: The main GUI class. Handles the display, networking, and game logic for a player.
- **ClientWindowTest / TriviaClient**: Entry points for launching the client.

---

## Setup & Running

### Prerequisites

- Java 8 or above
- A compatible audio file (`song.wav`) in the `client` package for background music
- Questions stored as text files in `resources/questions/q1.txt` to `q20.txt` (on server side)
- Configuration file (`config.properties`) in the `client` package

### Configuration Example (`config.properties`)
```
server.ip=11.111.111.11
server.tcp.port=6000
server.udp.port=5000
client.id=2
```

### Running the Server

1. Compile the server files:
    ```bash
    javac server/*.java shared/*.java
    ```
2. Start the server:
    ```bash
    java server.TriviaServer
    ```

### Running Clients

1. Compile the client files:
    ```bash
    javac client/*.java
    ```
2. Start a client:
    ```bash
    java client.ClientWindowTest
    ```
   or
    ```bash
    java client.TriviaClient
    ```

---

## Example Gameplay

- The client GUI shows:
    - The current question (e.g., "Q1: What is the capital of France?")
    - Four answer options (A/B/C/D)
    - A timer counting down for answer submission
    - Player’s current score
    - Buttons for Buzzing in and Submitting an answer

- **Buzz In:** Only the fastest player can answer. Others are locked out for that question.
- **Answer:** Submit the answer; receive instant feedback ("Correct! +10 points" or "Wrong answer! -10 points").
- **Game Over:** Final scores and winner are displayed.

---

## Extending & Customizing

- Add more questions by placing new files in `resources/questions/`.
- Change music by replacing `song.wav` in the client package.
- Adjust game timings (question interval, answer timer) in server/client code.

---
## Contact

Open an issue or contact [LMD200](https://github.com/LMD200) for questions, bug reports, or suggestions.
