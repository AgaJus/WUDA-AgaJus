# WUDA - Mini Applications Setup

This repository contains two main applications: the back-end (server) and the front-end (user interface). This guide explains how to set up and run them locally.

---

## 🖥️ Back-End Setup

1. **Install dependencies:**
```bash
cd back-end
pip install -r requirements.txt
```
2. Create .env file in backend dir with given values:


```.env
# PostgreSQL database name
POSTGRES_DB=your_database_name

# PostgreSQL user
POSTGRES_USER=your_database_user

# PostgreSQL password
POSTGRES_PASSWORD=your_database_password

# Optional: PostgreSQL host (default is localhost)
POSTGRES_HOST=localhost

# Optional: PostgreSQL port (default is 5432)
PORT=5432

# Secret key for JWT
JWT_KEY=your_secret_jwt_key

# Port where the backend will run (default is 5000)
APP_PORT=5000

```

3. In order to run app use command
``` bash
python3 app.py
```

## 🌐 Front-End Setup

1. **Install dependencies:**
   ```bash
   cd front-end
   pip install -r requirements.txt

2. Create .env file

```.env

# Hostname of the back-end server (default: localhost)
BACKEND_HOSTNAME=localhost

# Port of the back-end server (default: 5000)
BACKEND_PORT=5000

# Secret key for front-end application
SECRET_KEY=your_secret_key_here

# Port where the front-end server will run (default: 8000)
SERVER_PORT=8000
```

3. Run app by following command:


```bash
python3 app.py
```

## Database setup
Required is postgresql. Either run given docker-compose.yaml file or provide setup for backend's `.env` file


```yaml
services:
  postgres:
    image: postgres
    environment:
      POSTGRES_PASSWORD: alamakota
      POSTGRES_USER: board_games
      POSTGRES_DB: games
    ports:
      - 5432:5432
```

## For students.
In `tasks` directory there are clear instructions for each task that you should do in order to pass the course.
Recommended order:
1. [CICD](./tasks/CICD.md)
2. [Kubernetes](./tasks/kubernetes.md)
3. [Monitoring](./tasks/monitoring.md)



-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Dokumnentacja

## Pierwszy punkt kontrolny

W celu wykonania pierwszej części projektu stworzono dwa potoki CI/CD, czyli automatyczne procesy, które mają na celu automatyczne sprawdzenie czy zmiana kodu przez któregoś z użytkowników jest poprawna (nie ma błędów w kodzie) i czy nie ma zagrożenia bezpieczeństwa.

Co zrobiono w naszym potoku:
1. Pobrano kod
2. Sprawdzono jakość kodu (czyli linting) przy pomocy Flake8
3. Analizowano bezpieczeństwo przy pomocy Bandit
4. Sprawdzono biblioteki przy pomocy pip-audit
5. Zbudowano aplikację
6. Wykonano automatyczne testy (pytesty)
7. Wysłano do Github Container Registry (czyli takiego kontenera wersji apliakcji)
8. Sprzątanie


   Jak to wygląda schematycznie (czyli jak uruchomimy pipeline i znajdziemy/nie znajdziemy błędu):
   ```

     Uruchomienie pipeline
               |
               |
       Analizowanie kodu
               |
               |
        -----------------------
        |                     |
   Wykryto błąd       Nie wykryto błędu
        |                     |
   Zatrzymanie            Kontynuacja pipeline
   pipeline               aż do samego końca
        |
   Brak publikacji
   dockerowego obrazu

   Nie wysyła się nic
   do GHCR

   Powiadomienie
   o błędzie
   ```
   
   Jak to wygląda schematycznie, jeśli nie został znaleziony żaden błąd:

```
   Zmiana kodu na GitHub
               |
               |
   ------------------------
   |                      |
FRONT-END             BACK-END
Analiza kodu        Analiza kodu
(Flake8)              (Flake8)
   |                      |
Bezpieczeństwo     Bezpieczeństwo
(Bandit)              (Bandit)
   |                      |
Testowanie           Testowanie
   |                      |
Obraz Dockerowy   Obraz Dockerowy
(budowa)              (budowa)
   |                      |
   ------------------------
      |             |
    Wysłanie obrazu do GHCR
              |
              |
        Możliwe wdrożenie
   

```
