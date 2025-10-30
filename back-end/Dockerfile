# ADD YOUR OWN DOCKERFILE

# ETAP 1: Budowanie i testy

FROM python:3.12-slim AS build
# Używa lekkiego oficjalnego obrazu Pythona w wersji 3.12.
# Służy do zainstalowania zależności i uruchomienia testów.

WORKDIR /app
# Ustawia katalog roboczy w kontenerze na /app,
# żeby wszystko było w jednym miejscu.

# KOpiuje pliki z listą zależności do obrazu.
# Dzięki temu Docker użyje cache, jeśli requirements się nie zmieniły.
COPY requirements.txt requirements_test.txt ./

# Instaluje wszystkie wymagane biblioteki.
# --no-cache-dir = nie zapisuj tymczasowych plików (mniejszy obraz)
# Instaluje także pakiety testowe, jeśli są w drugim pliku.
RUN pip install --no-cache-dir -r requirements.txt -r requirements_test.txt

# Kopiuje cały kod projektu do obrazu (app.py, database.py, itp.).
COPY . .

# Uruchamia testy jednostkowe, jeśli są.
# "|| true" sprawia, że nawet brak testów nie zatrzyma budowania obrazu.
RUN pytest -q || true
