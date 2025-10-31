# ETAP 1: Budowanie
FROM python:3.12-slim
# Używamy lekkiego oficjalnego obrazu Pythona 3.12 — idealny do prostych aplikacji Flask.

WORKDIR /app
# Ustawiamy katalog roboczy, w którym znajdzie się kod frontendu.

# Kopiujemy plik z zależnościami, jeśli istnieje.
COPY requirements.txt requirements_test.txt* ./

# Instalujemy zależności projektu (oraz testowe, jeśli są).
RUN python -m pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt && \
    if [ -f "requirements_test.txt" ]; then pip install --no-cache-dir -r requirements_test.txt; fi

# Kopiujemy cały kod aplikacji (HTML, statyczne pliki, app.py itd.)
COPY . .

# ETAP 2: Uruchomienie aplikacji

# Ustawienia środowiskowe dla Pythona
ENV PYTHONUNBUFFERED=1 PYTHONDONTWRITEBYTECODE=1

# Informujemy Dockera, że aplikacja nasłuchuje na porcie 8000
EXPOSE 8000

# Uruchamiamy aplikację Flask
CMD ["python", "app.py"]
