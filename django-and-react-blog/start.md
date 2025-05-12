---
icon: python
---

# Start

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Create directory Backend and Frontend

Buat dalam dir backend, `pip install django==4.2`&#x20;

Install django latest, atau versi 4.2

Buat Freeze

pip freeze > requirements.txt

```pip-requirements
asgiref==3.7.2
Django==4.2
django-anymail==9.1
django-ckeditor==6.7.0
django-ckeditor-5==0.2.10
django-cors-headers==3.14.0
django-dotenv==1.4.2
django-environ==0.9.0
django-import-export==3.2.0
django-jazzmin==2.6.0
django-rest-framework==0.1.0
djangorestframework==3.14.0
djangorestframework-simplejwt==5.2.2
drf-yasg==1.21.7
humanize==4.6.0
requests==2.31.0
shortuuid==1.0.11
sqlparse==0.4.4
tzdata==2023.3
```

pip install -r requirements.txt

## Start Project

Buat project&#x20;

`django-admin startproject backend .`

Cek, apakah sudah berhasil..

`python manage.py runserver`

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

dengan susunan file sebagai berikut

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

## Start App

`python manage.py startapp api`

```python
    # Costums apps
    'api',

    # Third-party apps
    'corsheaders',
    'rest_framework',
    'rest_framework_simplejwt.token_blacklist',
    'drf_yasg',
```

Tambahkan costumes apps and thirds-party apps to settings.py\


add `'corsheaders.middleware.CorsMiddleware', #tambahkan ini`

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'corsheaders.middleware.CorsMiddleware', #tambahkan ini
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

and adding `'DIRS': [os.path.join(BASE_DIR, 'templates')], #tambahkan ini`

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')], #tambahkan ini
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

```

and..adding.

```python

# tambahkan ini
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    )
}


SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=5),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=50),
    'ROTATE_REFRESH_TOKENS': True,
    'BLACKLIST_AFTER_ROTATION': True,
    'UPDATE_LAST_LOGIN': False,

    'ALGORITHM': 'HS256',

    'VERIFYING_KEY': None,
    'AUDIENCE': None,
    'ISSUER': None,
    'JWK_URL': None,
    'LEEWAY': 0,

    'AUTH_HEADER_TYPES': ('Bearer',),
    'AUTH_HEADER_NAME': 'HTTP_AUTHORIZATION',
    'USER_ID_FIELD': 'id',
    'USER_ID_CLAIM': 'user_id',
    'USER_AUTHENTICATION_RULE': 'rest_framework_simplejwt.authentication.default_user_authentication_rule',

    'AUTH_TOKEN_CLASSES': ('rest_framework_simplejwt.tokens.AccessToken',),
    'TOKEN_TYPE_CLAIM': 'token_type',
    'TOKEN_USER_CLASS': 'rest_framework_simplejwt.models.TokenUser',

    'JTI_CLAIM': 'jti',

    'SLIDING_TOKEN_REFRESH_EXP_CLAIM': 'refresh_exp',
    'SLIDING_TOKEN_LIFETIME': timedelta(minutes=5),
    'SLIDING_TOKEN_REFRESH_LIFETIME': timedelta(days=1),
}

CORS_ALLOWED_ALL_ORIGINS = True
```

and.. create new directory.. `media`and `templates`

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

