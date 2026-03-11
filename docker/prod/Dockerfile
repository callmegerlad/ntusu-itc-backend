FROM python:3.10-slim-bookworm

ENV PYTHONUNBUFFERED=1

RUN apt-get update && apt-get install -y --fix-missing \
    python3-dev \
    default-libmysqlclient-dev \
    gcc \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /project

COPY requirements.txt /project/

RUN pip install --no-cache-dir -r requirements.txt

COPY . /project/

RUN python manage.py migrate
RUN python manage.py collectstatic --noinput

CMD ["gunicorn", "--bind", "0.0.0.0:8080", "SUITC_Backend.wsgi:application"]
