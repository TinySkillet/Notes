This document outlines the complete project plan and technical specifications for the "Heart Guesser Gauntlet" web application. It is intended for use by AI agents to guide development.
## 1. Project Goal

The primary goal is to build a full-stack web application where users can register, log in, and play a timed game that consumes an external API. The application must manage user scores, enforce different game mode rules, and display a competitive leaderboard.

## 2. Technology Stack

*   **Backend:** Python with the **FastAPI** framework.
*   **Database:** PostgreSQL, managed with **SQLAlchemy** (for the ORM) and **Alembic** (for database migrations).
*   **Cache:** **Redis**, for caching pre-fetched game questions.
*   **Frontend:** **React**.
*   **Authentication:** **JWT** (JSON Web Tokens) for managing user sessions.

## 3. Directory Structure

The project root contains two main directories. All backend code must be placed within `Backend/`, and all frontend code within `Frontend/`.
```
.
├── Backend/          # All FastAPI source code, models, routers, etc.
├── Frontend/         # All React source code.
├── AGENTS.md         # This file.
├── .cursor/
└── .cursorignore
```

## 4. External Dependency: The Heart Game API

The core gameplay relies on an external, public API. To ensure performance, our backend will be the sole consumer of this API; the frontend will **only** communicate with our own backend.

*   **Base URL:** `http://marcconrad.com/uob/heart/api.php`
*   **Authentication:** None required.

### API Usage Strategy

To ensure instant image rendering, our backend will **always** request the image data in Base64 format.

**Example Request from our Backend to the External API:**
`GET http://marcconrad.com/uob/heart/api.php?out=json&base64=yes`

**Example JSON Response Received by our Backend:**
```json
{
  "question": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUg...",
  "solution": 6,
  "carrots": 5
}
```

## 5. High-Performance Game Architecture

To provide a seamless, low-latency user experience, we will implement a caching and predictive pre-fetching strategy.

### 5.1. Backend: Pre-fetching & Caching

The backend will run a continuous background process to keep a ready-to-use pool of game questions, decoupling our game's performance from the external API's latency.

*   **Background Worker:** A task initiated on FastAPI startup will continuously:
    1.  Check the number of questions in the Redis cache.
    2.  If the count is low, it calls the external API (using the `base64=yes` parameter) to fetch new questions.
    3.  Each fetched question is assigned a unique ID (e.g., UUID) and stored in Redis with its solution.
*   **Cache Store (Redis):** A Redis list will serve as a high-speed queue (`question_queue`). Each item in the list will be a JSON object containing the `question_id`, the Base64 `image_data`, and the `solution`.

### 5.2. Frontend: Predictive Pre-fetching

The frontend will ensure instantaneous transitions between rounds by always fetching one question ahead of time.

*   **State Management:** The React app will manage two core state variables: `currentQuestion` and `nextQuestion`.
*   **Game Flow:**
    1.  **On Game Start:** Fetch two questions from our backend. The first is stored in `currentQuestion`, the second in `nextQuestion`.
    2.  **On Answer Submission:** The timer is paused. The app will initiate two asynchronous requests **in parallel**:
        *   A request to our backend to **verify the answer** for `currentQuestion`.
        *   A request to our backend to **fetch a new question** for the subsequent round.
    3.  **On Correct Answer:** Once verification is confirmed, the data from `nextQuestion` is moved to `currentQuestion`, the newly fetched question is placed in `nextQuestion`, and the next round begins instantly.

## 6. Detailed Development Plan

### 6.1. User Authentication & Identity (Backend)

*   **User Model:** Create a `User` model in SQLAlchemy with fields for `id`, `username` (unique), and `password_hash`.
*   **Password Security:** Use `passlib` with `bcrypt` to hash passwords.
*   **API Endpoints:**
    *   `POST /api/auth/register`: Creates a new user.
    *   `POST /api/auth/login`: Verifies credentials and returns a JWT.

### 6.2. Game Logic Endpoints (Backend)

These are the primary authenticated endpoints for gameplay.

*   `GET /api/game/next-question`:
    *   Pops one question object from the Redis cache.
    *   Returns a JSON object containing the `question_id` and the Base64 `image_data`, but **omits the solution**.
*   `POST /api/game/verify-answer`:
    *   Accepts a body like `{ "question_id": "...", "guess": 5 }`.
    *   Retrieves the correct solution from the Redis cache using the `question_id`.
    *   Returns `{ "correct": true }` or `{ "correct": false }`.

### 6.3. Score and Leaderboard Logic (Backend)

*   **Score Model:** Create a `Score` model in SQLAlchemy with fields for `id`, `user_id`, `score`, and `achieved_at`.
*   **API Endpoints:**
    *   `POST /api/scores`: Authenticated. Accepts `{ "score": 15 }`. Saves the score for the logged-in user.
    *   `GET /api/leaderboard/all-time`: Returns top 10 scores of all time.
    *   `GET /api/leaderboard/daily`: Returns top 10 scores from the last 24 hours.

### 6.4. Frontend Logic (React)

*   **Views/Components:** Build React components for: Login/Register, Main Menu, Game Screen, and Leaderboard.
*   **Authentication Flow:** Use `localStorage` to persist the JWT. Use an Axios interceptor or similar pattern to attach the `Authorization` header to all authenticated requests.
*   **Game Modes:**
    *   **Easy Mode:** Timer starts at 15s. Incorrect guesses are allowed. On correct guess, score increments, and the next round's timer is reduced by 10%. Game ends on time-out. Scores are not saved.
    *   **Hard Mode:** Timer starts at 15s. One incorrect guess ends the game. On correct guess, score increments, and the next round's timer is reduced by 20%. On game over, the final score is sent to `POST /api/scores`.
*   **Leaderboard View:** Fetch and display data from the leaderboard endpoints, with UI to toggle between "All-Time" and "Daily" views.


## 7. Implementation Order

1.  **Backend Setup:** Initialize the FastAPI project in `Backend/`. Set up SQLAlchemy, Alembic, Redis, and the database connection. Use Docker with a `docker-compose.yml` to run the PostgreSQL and Redis services.
2.  **Models & Migrations:** Create the necessary SQLAlchemy models. Use Alembic to generate and run the initial database migration.
3.  **Authentication Endpoints:** Build and test the user registration and login endpoints.
4.  **Backend Caching & Game Logic:** Implement the Redis integration. Create the background worker to populate the cache. Build and test the `/api/game/next-question` and `/api/game/verify-answer` endpoints.
5.  **Score & Leaderboard Endpoints:** Build and test the endpoints for submitting scores and retrieving the two leaderboard views.
6.  **Frontend Foundation:** Initialize the React project in `Frontend/`. Set up components, routing, and global state management.
7.  **Frontend Game Logic:** Implement the core game loop, including the `currentQuestion`/`nextQuestion` state logic, timer, and communication with the `/api/game/` endpoints.
8.  **Frontend Integration:** Connect the remaining parts: user authentication, score submission, and leaderboard display.