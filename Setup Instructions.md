# Heart Guesser Gauntlet


A full-stack web application where users can register, log in, and play a timed game that consumes an external API. The application manages user scores, enforces different game mode rules, and displays a leaderboard.
  

## 📋 Prerequisites

Before you begin, ensure you have the following installed on your system:

### Required Software

1. **Docker Desktop** (version 20.10 or higher)

- Download from: https://www.docker.com/products/docker-desktop/
- Required for running PostgreSQL and Redis containers

1. **Python 3.11 or higher**

- Download from: https://www.python.org/downloads/
- Verify: `python3 --version`

3. **uv** (Python package manager)

- Install: `pip install uv`
- Or via curl: `curl -LsSf https://astral.sh/uv/install.sh | sh`
- Verify: `uv --version`
  
4. **Node.js and npm** (version 18 or higher)

- Download from: https://nodejs.org/
- Verify: `node --version` and `npm --version`


## 🚀 Complete Setup Guide
  

Follow these steps **in order** to set up and run the HeartGame application.
### Step 1️⃣: Download the zip file and extract

### Step 2️⃣: Backend Setup


#### 2.1 Navigate to Backend Directory

```bash

cd Backend

```


#### 2.2 Install Docker (if not already installed)  

**On Ubuntu/Debian:**

```bash

sudo apt-get update

sudo apt-get install docker.io docker-compose-plugin

sudo systemctl start docker

sudo systemctl enable docker

```


**On macOS/Windows:**

- Download and install Docker Desktop from https://www.docker.com/products/docker-desktop/

- Start Docker Desktop application


**Verify Docker is running:**

```bash

docker --version

docker compose version

```


#### 2.3 Start PostgreSQL and Redis with Docker Compose


```bash

docker compose up -d

```
  

This will start:

- **PostgreSQL** database on port `5432`

- **Redis** cache on port `6379`


**Verify containers are running:**

```bash

docker compose ps

```

  
You should see both `heartgame_postgres` and `heartgame_redis` containers with status "Up".

  
#### 2.4 Create Python Virtual Environment


```bash

uv venv

```

  
**Activate the virtual environment:**


On **Linux/macOS**:

```bash

source .venv/bin/activate

```

  
On **Windows**:

```bash

.venv\Scripts\activate

```

  
#### 2.5 Install Python Dependencies


```bash

uv pip install -e .

```


This will install all required packages.

  
#### 2.6 Run Database Migrations


```bash

alembic upgrade head

```


This creates all necessary database tables in PostgreSQL.


**Expected output:**

```

INFO [alembic.runtime.migration] Running upgrade -> abc123, Initial schema

```

#### 2.7 Start the Backend Server


```bash

uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

```

**OR**
 
```bash

make run

```

if you have make installed.


**Backend is now running!**

- API: http://localhost:8000

- Swagger Docs: http://localhost:8000/docs

- ReDoc: http://localhost:8000/redoc

  
### Step 3️⃣: Frontend Setup


**Open a new terminal** (keep the backend running in the previous terminal)

#### 3.1 Navigate to Frontend Directory


```bash

cd Frontend

```

#### 3.2 Install Node Dependencies


```bash

npm install

```


#### 3.3 Start the Development Server

  
```bash

npm run dev

```

  
**Frontend is now running!**

- Application: http://localhost:5173


## 🎮 Using the Application

  
1. **Open your browser** and navigate to http://localhost:5173

2. **Register** a new account

3. **Login** with your credentials

4. **Select a game mode**:

- **Easy Mode**: Practice mode with forgiving rules

- **Hard Mode**: Competitive mode with scores saved to leaderboard

5. **Play the game** by counting hearts and submitting your answers

6. **View the leaderboard** to see top scores

  
## 🛠️ Troubleshooting

### Docker Issues


**Problem:** "Cannot connect to the Docker daemon"

```bash

# Start Docker service (Linux)

sudo systemctl start docker

  

# On Mac/Windows: Start Docker Desktop application

```


**Problem:** "Port already in use"

```bash

# Check what's using the port

sudo lsof -i :5432 # PostgreSQL

sudo lsof -i :6379 # Redis

  

# Stop existing containers

docker compose down

docker compose up -d

```


### Database Issues

**Problem:** "Connection refused" to PostgreSQL

```bash

# Check if PostgreSQL container is running

docker compose ps

  
# View PostgreSQL logs

docker compose logs postgres


# Restart containers

docker compose restart

```


**Problem:** Alembic migration fails

```bash

# Check database connection in .env file

# Reset database (WARNING: deletes all data)

docker compose down -v

docker compose up -d

alembic upgrade head

```

### Backend Issues

**Problem:** "Module not found" errors

```bash

# Ensure virtual environment is activated

source .venv/bin/activate # Linux/Mac

.venv\Scripts\activate # Windows

  

# Reinstall dependencies

uv pip install -e .

```


**Problem:** Backend won't start

```bash

# Check if port 8000 is already in use

lsof -i :8000 # Linux/Mac

netstat -ano | findstr :8000 # Windows

  

# Kill the process or use a different port

uvicorn app.main:app --reload --port 8001

```

### Frontend Issues


**Problem:** npm install fails

```bash

# Clear npm cache

npm cache clean --force


# Delete node_modules and reinstall

rm -rf node_modules package-lock.json

npm install

```


**Problem:** Frontend can't connect to backend

- Ensure backend is running on http://localhost:8000

- Check CORS settings in backend

- Verify `VITE_API_URL` in frontend configuration

## Project Structure

```

HeartGame/

├── Backend/

│ ├── app/

│ │ ├── api/ # API endpoints

│ │ │ └── v1/ # API version 1

│ │ │ ├── auth.py

│ │ │ ├── game.py

│ │ │ ├── leaderboard.py

│ │ │ ├── scores.py

│ │ │ └── users.py

│ │ ├── cache/ # Redis cache logic

│ │ │ ├── question_cache.py

│ │ │ └── redis_client.py

│ │ ├── core/ # Security & config

│ │ │ ├── security.py

│ │ │ └── exceptions.py

│ │ ├── daos/ # Data access layer

│ │ │ ├── user_dao.py

│ │ │ └── score_dao.py

│ │ ├── models/ # SQLAlchemy models

│ │ │ ├── user.py

│ │ │ └── score.py

│ │ ├── schemas/ # Pydantic schemas

│ │ │ ├── user.py

│ │ │ └── score.py

│ │ ├── services/ # Business logic

│ │ │ ├── auth_service.py

│ │ │ ├── game_service.py

│ │ │ └── question_service.py

│ │ ├── workers/ # Background workers

│ │ │ └── question_worker.py

│ │ ├── config.py # App configuration

│ │ ├── database.py # Database setup

│ │ ├── dependencies.py # FastAPI dependencies

│ │ └── main.py # Application entry point

│ ├── alembic/ # Database migrations

│ │ ├── versions/ # Migration scripts

│ │ └── env.py # Alembic configuration

│ ├── docker-compose.yml # PostgreSQL & Redis

│ ├── pyproject.toml # Python dependencies

│ └── alembic.ini # Alembic configuration

│

├── Frontend/

│ ├── src/

│ │ ├── components/ # React components

│ │ │ ├── auth/ # Authentication components

│ │ │ │ ├── Login.jsx

│ │ │ │ ├── Register.jsx

│ │ │ │ └── Auth.css

│ │ │ ├── game/ # Game components

│ │ │ │ ├── GameScreen.jsx

│ │ │ │ ├── QuestionDisplay.jsx

│ │ │ │ ├── Timer.jsx

│ │ │ │ ├── GameScreen.css

│ │ │ │ ├── QuestionDisplay.css

│ │ │ │ └── Timer.css

│ │ │ ├── leaderboard/ # Leaderboard components

│ │ │ │ ├── Leaderboard.jsx

│ │ │ │ └── Leaderboard.css

│ │ │ └── menu/ # Menu components

│ │ │ ├── MainMenu.jsx

│ │ │ └── MainMenu.css

│ │ ├── context/ # React Context providers

│ │ │ ├── AuthContext.jsx

│ │ │ ├── EventContext.jsx

│ │ │ └── GameContext.jsx

│ │ ├── factories/

│ │ │ └── gameModeFactory.js

│ │ ├── services/ # API client services

│ │ │ ├── api.js

│ │ │ ├── auth.js

│ │ │ ├── game.js

│ │ │ ├── leaderboard.js

│ │ │ ├── scores.js

│ │ │ └── users.js

│ │ ├── utils/ # Event emitter & utilities

│ │ │ └── eventEmitter.js

│ │ ├── App.jsx # Root component

│ │ ├── App.css

│ │ ├── main.jsx

│ │ └── index.css

│ ├── public/

│ ├── package.json

│ └── vite.config.js

│

└── README.md

```


## Key Features

### Technology Stack

- **Backend**: FastAPI (Python 3.10+)

- **Database**: PostgreSQL 15 (via Docker)

- **Cache**: Redis 7 (via Docker)

- **Frontend**: React 18 + Vite

- **Authentication**: JWT (Access + Refresh Tokens)

- **Package Management**: `uv` (Python), `npm` (Node.js)

  
### Design Patterns

- **Repository Pattern**: Clean data access abstraction

- **Service Layer**: Business logic separation

- **Factory Pattern**: Game mode creation

- **Event-Driven Architecture**: Custom event emitter for game events

- **Dependency Injection**: FastAPI's powerful DI system


## API Endpoints


### Authentication

- `POST /api/v1/auth/register` - Register new user

- `POST /api/v1/auth/login` - Login user

- `POST /api/v1/auth/refresh` - Refresh access token

  
### Game

- `GET /api/v1/game/next-question` - Get next question (requires auth)

- `POST /api/v1/game/verify-answer` - Verify answer (requires auth)


### Scores & Leaderboard

- `POST /api/v1/scores` - Submit score (requires auth)

- `GET /api/v1/leaderboard/all-time` - All-time top 10

- `GET /api/v1/leaderboard/daily` - Daily top 10


## Game Modes

### Easy Mode

- Initial timer: 15 seconds

- Timer bonus: +3 seconds per correct answer

- Incorrect answers allowed (shows "Try Again" feedback)

- Game ends when timer runs out

- **Scores are NOT saved**


### Hard Mode

- Initial timer: 10 seconds

- Timer bonus: +1 second per correct answer

- **One incorrect answer ends the game**

- Game ends on timer expiration or wrong answer

- **Scores ARE saved to leaderboard**

