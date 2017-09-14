## Djadock
Simple template for Django + PostgreSQL applications.

## Create new Django project
In env.dev replace environment settings for db service.

```bash
cd docker/
docker-compose -f docker-compose.dev.yml build web
docker-compose -f docker-compose.dev.yml run web django-admin.py startproject <your_app_name> .
```

Go to the root folder of your application and change the ownership of the new files:
```bash
sudo chown -R $USER:$USER .
```

In <your_app_name>/settings.py replace the DATABASES = ... with the following:
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'HOST': os.environ.get('DB_SERVICE', ''),
        'PORT': os.environ.get('DB_PORT', 5432),
        'NAME': os.environ.get('POSTGRES_DB', ''),
        'USER': os.environ.get('POSTGRES_USER', ''),
        'PASSWORD': os.environ.get('POSTGRES_PASSWORD', ''),
    }
}
```

Run application:
```bash
cd docker/
docker-compose -f docker-compose.dev.yml up -d
```

To perform console commands inside web container:
```bash
docker-compose -f docker-compose.dev.yml exec web bash
```
