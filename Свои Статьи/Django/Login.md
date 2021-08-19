# LOGIN

#### Настройки в SETTING.py

```python
AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
        'OPTIONS': {
            'min_length': 9,
        }
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]
```

Этот пример включает все четыре включенных валидатора:

- `UserAttributeSimilarityValidator`, который проверяет схожесть пароля и набора атрибутов пользователя.
- `MinimumLengthValidator`, который проверяет, соответствует ли пароль минимальной длине. Этот валидатор настроен с настраиваемой опцией: теперь ему требуется, чтобы минимальная длина составляла девять символов вместо восьми по умолчанию.
- `CommonPasswordValidator`, который проверяет, входит ли пароль в список общих паролей. По умолчанию он сравнивается с включенным списком из 20 000 общих паролей.
- `NumericPasswordValidator`, который проверяет, не является ли пароль полностью числовым.

Для `UserAttributeSimilarityValidator`и `CommonPasswordValidator`в этом примере мы используем настройки по умолчанию. `NumericPasswordValidator` не имеет настроек.

Тексты справки и любые ошибки от валидаторов паролей всегда возвращаются в том порядке, в котором они перечислены [`AUTH_PASSWORD_VALIDATORS`](https://docs.djangoproject.com/en/3.2/ref/settings/#std:setting-AUTH_PASSWORD_VALIDATORS).

#### Пробую Crispy

1. ```python
   from django.contrib.auth.forms import UserCreationForm
   ```

2. Создадим  форму используя [Django Crispy Forms](https://django-crispy-forms.readthedocs.io/en/latest/index.html). Сторонняя библиотека [Django Crispy Forms](https://django-crispy-forms.readthedocs.io/en/latest/index.html) позволяет контролировать внешний вид Django-форм и создана в соответствии с принципами [DRY-way](https://ru.wikipedia.org/wiki/Don't_repeat_yourself).

