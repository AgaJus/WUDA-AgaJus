# ADD YOUR OWN DOCKERFILE

# ETAP 1: Budowanie

FROM python:3.12-slim
# Używa lekkiego oficjalnego obrazu Pythona w wersji 3.12.
# Służy do zainstalowania zależności i uruchomienia testów.

WORKDIR /app
# Ustawia katalog roboczy w kontenerze na /app,
# żeby wszystko było w jednym miejscu.

# KOpiuje pliki z listą zależności do obrazu.
# Dzięki temu Docker użyje cache, jeśli requirements się nie zmieniły.
COPY requirements.txt requirements_test.txt* ./

# Instaluje wszystkie wymagane biblioteki.
# --no-cache-dir = nie zapisuj tymczasowych plików (mniejszy obraz)
# Instaluje także pakiety testowe, jeśli są w drugim pliku.
RUN python -m pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt && \
    if [ -f "requirements_test.txt" ]; then pip install --no-cache-dir -r requirements_test.txt; fi


# Kopiuje cały kod projektu do obrazu (app.py, database.py, itp.).
COPY . .

# ETAP 2: Uruchomienie aplikacji

# Uruchamiamy buforowanie logów, żeby były widoczne od razu w konsoli. Zapobiega generowaniu plików .pyc w obrazie
ENV PYTHONUNBUFFERED=1 PYTHONDONTWRITEBYTECODE=1

# Użytownik musi podac dane srodowiskowe przez plik .env lub flagę --env-file

# Otwórz port 8000 — to informacja dla Dockera, że aplikacja nasłuchuje na tym porcie.
EXPOSE 8080

# Uruchamia aplikację. Ten plik (app.py) startuje backend.
# W przyszłosci linijka do zmiany przy rozbudowie np. serwerze
CMD ["python", "app.py"]

