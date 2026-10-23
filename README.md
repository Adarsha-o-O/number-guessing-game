# Number Guessing Game

A terminal-based number guessing game built with Bash and PostgreSQL. The game generates a random number, guides the player toward the correct answer, and stores player information and game results in a database.

This project was completed as part of the **[freeCodeCamp Relational Database Certification](https://www.freecodecamp.org/learn/relational-database/)** challenge, **Build a Number Guessing Game**.

## Project Overview

The game generates a secret number between 1 and 1000. Players enter guesses through the terminal and receive hints indicating whether the secret number is higher or lower.

PostgreSQL stores usernames and game results, allowing returning players to see their previous game count and best performance.

## Features

- Generate a random secret number between 1 and 1000.
- Accept guesses through a terminal interface.
- Validate that guesses contain integers.
- Provide higher-or-lower hints after incorrect guesses.
- Register new players by username.
- Recognize returning players.
- Display the number of games played by a returning player.
- Display the player's best game, measured by the fewest guesses.
- Save completed game results to PostgreSQL.

## Technologies Used

- **Bash** — game logic and terminal interaction
- **PostgreSQL** — persistent storage of usernames and game results
- **SQL** — retrieving player statistics and saving results
- **Git and GitHub** — version control and project hosting

## Repository Structure

```text
number-guessing-game/
├── number_guess.sh
├── number_guess.sql
└── README.md
```

| File | Description |
|---|---|
| `number_guess.sh` | Contains the game logic, input validation, player lookup, and result storage. |
| `number_guess.sql` | PostgreSQL database dump defining the tables, sequences, and constraints. |
| `README.md` | Project documentation and setup instructions. |

## Database Design

The database is named `number_guess` and contains two tables.

### 1. `users`

Stores player information.

| Column | Description |
|---|---|
| `user_id` | Primary key identifying each user. |
| `username` | Player's username, limited to 22 characters and required to be unique. |

### 2. `games`

Stores completed game results.

| Column | Description |
|---|---|
| `game_id` | Primary key identifying each game. |
| `user_id` | Foreign key referencing the player in `users`. |
| `guesses` | Number of guesses required to find the secret number. |

The relationship between `users` and `games` allows one player to have multiple recorded games.

## How It Works

### 1. Generate the secret number

The script generates a random integer between 1 and 1000:

```bash
SECRET_NUMBER=$(( RANDOM % 1000 + 1 ))
```

### 2. Identify the player

The game asks for a username and checks whether it exists in the `users` table.

- **New player:** The script welcomes the player and inserts their username into the database.
- **Returning player:** The script retrieves the player's previous game count and lowest guess count, then displays those statistics.

### 3. Play the game

The player enters guesses until the secret number is found.

- If the input is not an integer, the game asks for another guess.
- If the guess is too high, the game says the secret number is lower.
- If the guess is too low, the game says the secret number is higher.
- If the guess is correct, the game displays the number of tries and saves the result.

### 4. Save the result

After a successful game, the script inserts the player's ID and guess count into the `games` table.

The best game is calculated using the minimum value of `guesses` for that player.

## Getting Started

### Prerequisites

Install or have access to:

- PostgreSQL
- `psql`, the PostgreSQL command-line client
- Bash
- Git

The commands below assume a Linux environment, such as GitHub Codespaces, and the database configuration used in the challenge.

### 1. Clone the repository

```bash
git clone https://github.com/Adarsha-o-O/number-guessing-game.git
cd number-guessing-game
```

### 2. Restore the database

**Warning:** The `number_guess.sql` dump contains `DROP DATABASE number_guess;`. Restoring it can delete an existing database with that name. Back up any important data before proceeding.

If you understand the effect and have the required PostgreSQL permissions, restore the database with:

```bash
psql -U postgres -d postgres -f number_guess.sql
```

The dump recreates the `number_guess` database, defines the `users` and `games` tables, and configures their sequences and constraints.

The database owner and connection settings must match your PostgreSQL environment.

### 3. Make the script executable

```bash
chmod +x number_guess.sh
```

### 4. Start the game

```bash
./number_guess.sh
```

The script connects using the configuration defined in the file:

```bash
psql --username=freecodecamp --dbname=number_guess
```

Ensure that the PostgreSQL server is running and that the `freecodecamp` role has the necessary database permissions.

## Example Gameplay

```text
Enter your username:
adhi
Welcome, adhi! It looks like this is your first time here.
Guess the secret number between 1 and 1000:
500
It's lower than that, guess again:
250
It's higher than that, guess again:
375
You guessed it in 3 tries. The secret number was 375. Nice job!
```

The secret number and guesses shown above are illustrative; the actual secret number is generated randomly.

## Concepts Practiced

This project provided practice with:

- Bash variables, arithmetic, loops, and conditional statements.
- Random number generation.
- Reading and validating terminal input.
- Connecting Bash scripts to PostgreSQL.
- Writing SQL queries and inserting records.
- Using primary keys, foreign keys, and unique constraints.
- Retrieving statistics with `COUNT()` and `MIN()`.
- Persisting player data across game sessions.
- Using Git and GitHub to manage a project.

## Current Scope and Possible Improvements

The current implementation stores player usernames and completed-game guess counts.

Possible future improvements include:

- Validate username length and handle empty usernames.
- Validate that guesses fall within the range 1–1000.
- Use safer SQL input handling instead of directly interpolating user input into SQL commands.
- Improve database error handling.
- Add difficulty levels or configurable number ranges.
- Add more player statistics, such as average guesses per game.

These are potential improvements, not existing features.

## Author

**Adarsha**

- GitHub: [@Adarsha-o-O](https://github.com/Adarsha-o-O)

---

Completed as part of the freeCodeCamp Relational Database Certification curriculum.
