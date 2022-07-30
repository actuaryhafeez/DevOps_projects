# Requirements & Recommendations
## install python 3.0.10
    python --version


## “install chocolatey on windows 10 cmd 
## go on https://docs.chocolatey.org/en-us/choco/setup
## install kubernetes on window
## docker-window must be installed on window
    choco install kubernetes -cli
## 
    kubectl version --short
    kubectl get pods
    kubectl get nodes
## install docker
  docker --version

# Install Requirements & Start Django Project
    mkdir ~/dev/django-k8s
    cd ~/dev/django-k8s
# Create the Python Virtual Environment
## initialise virtual env
    python -m venv  venv
## activate
    source venv/Scripts/activate
    
    python -V
    pip --version
    pip install pip --upgrade
    pip --version
    mkdir web
    cd web
## create requirements.txt
    touch requirements.txt
    
    nano requirement.txt
    django>=3.2,<4.0
    gunicorn
    requests
    django-dotenv
    psycopg2-binary
    django-storages
    boto3
    # check jandgo-admin
    django-admin --version
# Start Django Project in web folder
    django-admin startproject django_k8s .
# Environment Variables with a dotenv file
## create .env in web folder 
    touch .env
    # paste
    
    DEBUG=1
    DJANGO_SUPERUSER_USERNAME=admin 
    DJANGO_SUPERUSER_PASSWORD=mydjangopw
    DJANGO_SUERPUSER_EMAIL=hello@teamcfe.com
    DJANGO_SECRET_KEY=fix_this_later

   POSTGRES_DB=dockerdc
   POSTGRES PASSWORD=mysecretpassword 
   POSTGRES_USER=myuser
   POSTGRES_HOST=postgres_db
   POSTGRES_PORT=5433

   REDIS_HOST=redis_db
   REDIS_PORT=6380
## make .gitignore file in outer parent folder django-k8s. copy and paste files that should not be copied to github 
# Setup django dotenv to read our dotenv file. dotenv help to read .env file data
 ## make settings of .env folder in wsgi.py 
    """
    WSGI config for django_k8s project.

    It exposes the WSGI callable as a module-level variable named ``application``.

    For more information on this file, see
    https://docs.djangoproject.com/en/3.2/howto/deployment/wsgi/
    """

    import os
    import pathlib # add this line

    import dotenv # add this line

    from django.core.wsgi import get_wsgi_application

    CURRENT_DIR = pathlib.Path(__file__).resolve().parent # add this line
    BASE_DIR = CURRENT_DIR.parent # add this line
    ENV_FILE_PATH = BASE_DIR / ".env" # add this line

    dotenv.read_dotenv(str(ENV_FILE_PATH)) # add this line

    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'django_k8s.settings')

    application = get_wsgi_application()

## in manage.py file 
    import dotenv
    if __name__ == '__main__':
        dotenv.read_dotenv() # add this line
        main()
## now check 
    python manage.py shell
    import os 
    print(os.environ.get("POSTGRES_DB"))
    exit()
# Update Django settings for Database & Environment Variables
# settings file  
    from pathlib import Path
    import os
    SECRET_KEY = os.environ.get("DJANGO_SECRET_KEY")
    DEBUG = str(os.environ.get('DEBUG')) == "1"

    DB_USERNAME = os.environ.get("POSTGRES_USER")
    DB_PASSWORD= os.environ.get("POSTGRES_PASSWORD")
    DB_DATABASE = os.environ.get("POSTGRES_DB")
    DB_HOST = os.environ.get("POSTGRES_HOST")
    DB_PORT = os.environ.get("POSTGRES_PORT")
    DB_IS_AVAIL = all([
      DB_USERNAME,
      DB_PASSWORD,
      DB_DATABASE,
      DB_HOST,
      DB_PORT
      ])

    if DB_IS_AVAIL:
        DATABASES = {
            "default":{
                "ENGINE":"django.db.backends.postgresql",
                "NAME":DB_DATABASE,
                "USER":DB_USERNAME,
                "PASSWORD":DB_PASSWORD,
                "HOST":DB_HOST,
                "PORT":DB_PORT
            }
        }

    print(DATABASES)
   ## save file
   python manage.py runserver # it shows db.sqlite
# Docker, Dockerfile & dockerignore
# now set .dockerfile in web file that carry file that should not be copied in docker image. copy .gitignore file and paste 
# now make Dockerfile in web paste 
    FROM python:3.9.7-slim

    COPY . /app
    WORKDIR /app

    RUN python3 -m venv /opt/venv

    RUN /opt/venv/bin/pip install pip --upgrade && \
        /opt/venv/bin/pip install -r requirements.txt && \
        chmod +x entrypoint.sh

    CMD ["/app/entrypoint.sh"]
## for understanding purpose only 
## we run django server
    python manage.py runserver
## we run server with unicorn in detach mode
    /C/Users/hafee/dev/django-k8s/venv/Scripts/gunicorn/ manage.py runserver
    
## now create entrypoint.sh file in web folder here we set gunicorn
    touch entrypoint.sh
    #paste
    
    #!/bin/bash
    APP_PORT=${PORT:-8000}
    cd /app/
    /opt/venv/bin/gunicorn --worker-tmp-dir /dev/shm django_k8s.wsgi:application --bind "0.0.0.0:${APP_PORT}"
# Create a migration script
## create migrate.sh file
## paste 
    #!/bin/bash

    SUPERUSER_EMAIL=${DJANGO_SUPERUSER_EMAIL:-"hello@teamcfe.com"}
    cd /app/

    /opt/venv/bin/python manage.py migrate --noinput
    /opt/venv/bin/python manage.py createsuperuser --email $SUPERUSER_EMAIL --noinput || true
# Docker Compose Part 1
## make docker-compose.yaml file in django-k8s folder
## 
# Docker Compose Part 2
