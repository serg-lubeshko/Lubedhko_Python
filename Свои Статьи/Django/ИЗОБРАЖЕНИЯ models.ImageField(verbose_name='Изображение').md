# ИЗОБРАЖЕНИЯ models.ImageField(verbose_name='Изображение')

В setting.py необходимо указать маршрут к статитике и медиа

**STATICFILES_DIRS**  - дериктории, где  Джанго будет искать статические файлы, чтобы переместить в папку static 

```python
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')

STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'lsrproject/static'),
]

#emblem просто указал вместо media 
MEDIA_ROOT = os.path.join(BASE_DIR, 'emblem')
MEDIA_URL = '/emblem/'
```

Также в urls.py пропишем функцию static

```python
....
if settings.DEBUG:
     urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
     urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
    
#MEDIA_URL -  константа, которую мы создали в settings.py
```

##### В админке валидирует размер

Код в админке валидирует размер изображения на уровне форм, задает цвет help_text

```python
from PIL import Image
from django.contrib import admin
from django.core.exceptions import ValidationError
from django.forms import ModelChoiceField, ModelForm
from django.utils.safestring import mark_safe
from .models import *
from django import forms

class NotebookAdminForm(ModelForm):
    MIN_RESOLUTION = (100, 100)
    MAX_RESOLUTION = (800, 800)

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.fields['image'].help_text = mark_safe(
            '<span style= "color:red">Загружайте изображения с минимальным разрешением {}x{}</span>'.format(
                *self.MIN_RESOLUTION))

    def clean_image(self):
        images = self.cleaned_data['image']
        im = Image.open(images)
        width, height = im.size
        min_width, min_height = self.MIN_RESOLUTION
        max_width, max_height = self.MAX_RESOLUTION
        if width < min_width or height < min_height:
            raise ValidationError('Размер изображения должен быть больше 100х100')
        if width > max_width or height > max_height:
            raise ValidationError('Размер изображения должен быть меньше 800х800')
```

##### Валидируем изображение в модели

Валидируем изображение в модели. ValidationError там не работает, он для форм, пишем кастомный Exception и переопределяем метод save

```python
class MinResolutionImages(Exception):
    pass


class MaxResolutionImages(Exception):
    pass

class Product(models.Model):
    MIN_RESOLUTION = (100, 100)
    MAX_RESOLUTION = (800, 800)
    .....

	def __save__(self):
        images = self.cleaned_data['image']
        im = Image.open(images)
        width, height = im.size
        min_width, min_height = self.MIN_RESOLUTION
        max_width, max_height = self.MAX_RESOLUTION
        if width < min_width or height < min_height:
            raise MinResolutionImages('Размер изображения должен быть больше 100х100')
        if width > max_width or height > max_height:
            raise MaxResolutionImages('Размер изображения должен быть меньше 800х800')
```

##### Обрежем изображение в модели:

```python

```

Проверить

```python
from django.db import models
from django.core.exceptions import ValidationError

def validate_image(fieldfile_obj):
    filesize = fieldfile_obj.file.size
    megabyte_limit = 5.0
    if filesize > megabyte_limit*1024*1024:
        raise ValidationError("Max file size is %sMB" % str(megabyte_limit))

class UFOPicture(models.Model):
    name = models.CharField(max_length=128)
    photo = models.ImageField(upload_to='photos/%Y/%m/%d', height_field='height_field', width_field='width_field', validators=[validate_image])
    height_field = models.PositiveIntegerField(default=0)
    width_field = models.PositiveIntegerField(default=0)
    

    def delete(self, *args, **kwargs):
        storage, path = self.photo.storage, self.photo.path
        storage.delete(path)
        super(UFOPicture, self).delete(*args,**kwargs)

    class Meta:
        app_label = 'myapp'
```

