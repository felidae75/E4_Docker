FROM python:3.9-alpine3.17

WORKDIR /usr/src/app

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN apk update \
	&& apk add postgresql-dev gcc python3-dev musl-dev


RUN pip install --upgrade pip
COPY ./requirements.txt .
RUN pip install -r requirements.txt


RUN mkdir /usr/src/app/static
# copy project
COPY . .

CMD python manage.py migrate \
    && python manage.py shell -c "from django.contrib.auth import get_user_model; User = get_user_model(); User.objects.filter(username='root').exists() or User.objects.create_superuser('root', 'root@example.com', 'root')" \
    && python manage.py collectstatic --no-input \
    && gunicorn dj.wsgi:application --bind 0.0.0.0:8000 --log-level info

