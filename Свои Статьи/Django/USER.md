https://django.fun/tips/polzovatelskaya-model-user/

## Создание приложения пользователя

```python
(code) $ python manage.py startapp users
```



1. обновим` AUTH_USER_MODEL`, чтобы Django знал, что нужно использовать нашу новую модель `CustomUser` вместо [модели User](https://django.fun/docs/2.2/ref/contrib/auth.html#user-model) по умолчанию.

2. Откройте `new_project/settings.py` в текстовом редакторе и внесите следующие два изменения:

```python
# new_project/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # Local
    'users.apps.UsersConfig', # новое
]
...
AUTH_USER_MODEL = 'users.CustomUser' # новое
```

## Шаг 3. Модель CustomUser

Мы хотим расширить (или скопировать) существующую модель `User` и назвать ее как-то иначе, в нашем случае `CustomUser`. Это все, что нам нужно сделать. Просто сделайте копию, и тогда мы сможем настроить ее так, как нам нравится, но при этом использовать все преимущества встроенного `User`.

Нам даже не нужно добавлять поле на этом этапе!

```python
# users/models.py
from django.contrib.auth.models import AbstractUser
from django.db import models

class CustomUser(AbstractUser):
    pass
    # add additional fields in here
```

## Шаг 4: Обновление форм

Django использует модель `User` - теперь нашу модель `CustomUser`, поскольку мы указали ее в `AUTH_USER_MODEL` повсюду. Два основных места - это когда создается новый пользователь и когда мы что-то меняем у пользователя. Поэтому мы должны снова расширить встроенные формы для этого и указать их для нашей новой модели `CustomUser`.

Создайте новый файл `users/forms.py` и заполните его следующим текстом:

```python
# users/forms.py
from django import forms
from django.contrib.auth.forms import UserCreationForm, UserChangeForm
from .models import CustomUser

class CustomUserCreationForm(UserCreationForm):

    class Meta(UserCreationForm):
        model = CustomUser
        fields = ('username', 'email')

class CustomUserChangeForm(UserChangeForm):

    class Meta:
        model = CustomUser
        fields = ('username', 'email')
```

## Шаг 5: Обновление admin.py

Модель Django `User` тесно связана с превосходным встроенным приложением администратора `admin`, поэтому мы должны указать Django использовать вместо этого `CustomUser`. Вот как:

```python
# users/admin.py
from django.contrib import admin
from django.contrib.auth import get_user_model
from django.contrib.auth.admin import UserAdmin

from .forms import CustomUserCreationForm, CustomUserChangeForm
from .models import CustomUser

class CustomUserAdmin(UserAdmin):
    add_form = CustomUserCreationForm
    form = CustomUserChangeForm
    model = CustomUser
    list_display = ['email', 'username',]

admin.site.register(CustomUser, CustomUserAdmin)
```

## Всё!!!

И это все. Создайте файл миграции для наших изменений, а затем впервые запустите миграцию, чтобы инициализировать нашу базу данных с помощью `CustomUser` вместо `User`.

```
(code) $ python manage.py makemigrations users
(code) $ python manage.py migrate
```

Поздравляем! Ваш проект Django рассчитан на будущее и вы можете продолжить работу без проблем.

*Перевод статьи https://wsvincent.com/django-tips-custom-user-model/*

