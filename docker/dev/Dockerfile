FROM python:3.10-slim-bookworm

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

RUN apt-get update && apt-get install -y --fix-missing \
	python3-dev \
	default-libmysqlclient-dev \
	gcc \
	&& rm -rf /var/lib/apt/lists/*

WORKDIR /project

COPY requirements.txt /project/

RUN pip install -r requirements.txt

COPY . /project/

RUN python manage.py migrate
RUN python manage.py collectstatic --noinput

CMD ["python", "manage.py", "runserver", "0.0.0.0:8080"]
