# Django


### 创建项目

```shell
[user@hostname]$ django-admin startproject MyProject [Path]
```

- `MyProject: `为创建的项目名称
- `[Path]: `为创建指定项目选择路径(可选) : 留空则默认创建到当前目录

**项目结构**

```shell
MyProjectFolder
    ├── MyProject
    │   ├── asgi.py
    │   ├── settings.py
    │   ├── urls.py
    │   ├── wsgi.py
    │   └── __init__.py
    ├── db.sqlite3
    └── manage.py
```

### 创建应用

> `进入项目文件夹`
```shell
[user@hostname]$ python manage.py startapp MyApp
```

- `MyApp: `为创建的项目名称

### 修改项目配置文件

`setting.py`
```python

from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent


# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/5.1/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-b_*3j7h^#hp7=$&8lkxl29b7qap@d!2lp7z!qud)d(9w3f5yvt'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []


# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'demo.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
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

WSGI_APPLICATION = 'demo.wsgi.application'


# Database
# https://docs.djangoproject.com/en/5.1/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}


# Password validation
# https://docs.djangoproject.com/en/5.1/ref/settings/#auth-password-validators

AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]


# Internationalization
# https://docs.djangoproject.com/en/5.1/topics/i18n/

LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_TZ = True


# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/5.1/howto/static-files/

STATIC_URL = 'static/'

# Default primary key field type
# https://docs.djangoproject.com/en/5.1/ref/settings/#default-auto-field

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'

```

- **INSTALLED_APPS: 在其中添加以创建的应用(直接添加即可)**
- **DATABASE: 在其中配置数据库**
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3' # 数据库类型
        'NAME': 'xxxxxxx' # 项目名称
        'USER': 'xxxxxxx' # 数据库名称
        'PASSWORD': 'xxxxxxx' # 数据库密码
        'HOST': 'xxx.xxx.xxx.xxx' # 数据库IP
        'PORT': 'xxxx' # 数据库端口
    }
}
```
- **ENGINE:**
    - **`django.db.backends.sqlite3: `用于连接`SQLite`数据库**
    - **`django.db.backends.postgresql: `用于连接`PostgreSQL`数据库**
    - **`django.db.backends.mysql: `用于连接`MySQL`数据库**
    - **`django.db.backends.oracle: `用于连接`Oracle`数据库**

- **LANGUAGE_CODE: 修改语言及代码**
- **TIME_ZONE: 修改时间及区域**

### 运行项目

```shell
[user@hostname]$ python manage.py runserver [Port]
```
- `[Port]: `此为指定端口(可选)


