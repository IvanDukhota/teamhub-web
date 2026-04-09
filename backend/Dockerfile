# 1. Використовуємо офіційний легкий образ Python
FROM python:3.12-slim

# 2. Встановлюємо системні залежності для psycopg2 (роботи з Postgres) та збірки пакетів
RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# 3. Встановлюємо робочу директорію в контейнері
WORKDIR /app

# 4. Налаштовуємо змінні оточення
# Python не буде створювати файли .pyc
ENV PYTHONDONTWRITEBYTECODE=1
# Вивід логів відразу в консоль без буферизації
ENV PYTHONUNBUFFERED=1

# 5. Копіюємо requirements і встановлюємо залежності
# Робимо це окремим кроком для кешування шарів Docker
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

# 6. Копіюємо весь код бекенду в контейнер
COPY . /app/

# 7. Відкриваємо порт, на якому працює Django
EXPOSE 8000

# 8. Команда для запуску (використовуємо gunicorn для проду або manage.py для розробки)
# Для CI/CD та продакшну краще gunicorn
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
