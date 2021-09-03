# MODEL

https://django.fun/docs/django/ru/3.1/ref/models/fields/#django.db.models.Field Справочник типов полей

https://django.fun/docs/django/ru/3.1/ref/models/instances/#model-instance-reference Справочник по методам модели

> Избегайте использования [`null`](https://django.fun/docs/django/ru/3.1/ref/models/fields/#django.db.models.Field.null) в строковых полях, таких как [`CharField`](https://django.fun/docs/django/ru/3.1/ref/models/fields/#django.db.models.CharField) и [`TextField`](https://django.fun/docs/django/ru/3.1/ref/models/fields/#django.db.models.TextField). Cоглашение Django заключается в использовании пустой строки, а не `NULL`. Единственное исключение - когда a [`CharField`](https://django.fun/docs/django/ru/3.1/ref/models/fields/#django.db.models.CharField) имеет оба значения: `unique=True` и `blank=True`. В этой ситуации требуется `null=True`, чтобы избежать нарушений ограничений при сохранении нескольких объектов с пустыми значениями.
>
> 

**Модель** — это класс, описывающий одну из таблиц в базе данных и предоставляю­щий инструменты для работы с ней: выборки записей, их фильтрации, сортиров­ки и т.д.

Модели объявляются на уровне отдельного приложения, в модуле models.py пакета этого приложения.

Чтобы модели были успешно обработаны программным ядром Django приложение должно быть зарегистрировано в списке приложений проекта.

### Параметры, поддерживаемые полями всех типов

- *verbose name* - "понятное пользователю” название поля, которое будет выводиться на веб­страницах. Если не указано, то будет выводиться имя поля.

* *help_tex* - поясняющий текст, выводимый на экран.

* *default* - значение по умолчанию, записываемое в поле, если в него явно не было занесено никакого значения. Может быть указано двумя способами:

  - как обычное значение любого неизменяемого типа:

    ```python
    is_active = models.BooleanField(default=True)
    ```

    

  - как ссылка на функцию, вызываемую при создании каждой новой записи и возвращающую в качестве результата заносимое в поле значение (например, если значение по умолчании выступает список или словарь):

    ```python
    def is_active_default(): 
        return not is_all_posts_passive 
    is_active = models.BooleanField(default=is_active_default)
    ```

* *unique* - если True, то в текущее поле может быть занесено только уникальное в пределах таблицы значение (уникальное поле). Если вносишь повторное значение будет возбуждено исключение `IntegrityError` из модуля `django.db.`Значение по умол­чанию — False;

  - *unique for date* - если в этом параметре указать представленное в виде строки имя поля даты (DateFieid) или даты и времени (DateTimeFieid), то текущее поле может хранить только значения, уникальные в пределах даты, которая хранится в указанном поле;

    ```python
    title = models.CharField(max_length=50, unique_for_date=’published’) 
    published = models.DateTimeFieid()
    #позволит сохранить в поле title только значения, уникальные в пределах даты, хранящейся в поле published;
    ```

  - *unique for month* - то же самое, что и unique for date, но в расчет принимается не всё значение даты, а лишь месяц;

  - *unique fоr year* — то же самое, что и unique for date, но в расчет принимается не всё значение даты, а лишь год;

* *null* — если True, то поле в таблице базы данных может хранить значение null и, таким образом, являться необязательным к заполнению. Если False, то поле в таблице должно иметь какое-либо значение, хотя бы пустую строку. Значение ПО умолчанию — False. **Чтобы упростить обработку отсутствия значения в таком поле, его не стоит де­лать <u>необязательным к заполнению</u> ~~blank = True~~.**

* *blank* - если True, то Django позволит занести в поле пустое значение (тем са­мым сделав поле необязательным к заполнению). По умолчанию — False. Если этому параметру дано значение True для строко­вого поля — внесет пустую строку;

* *db index* — если True, то по текущему полю в таблице будет создан индекс, если False — не будет. По умолчанию — False;

* *primary key* (**в модели может присутствовать только одно ключевое поле**) — если True, то текущее поле станет ключевым. Такое поле будет помечено как обязательное к заполнению и уникальное (null = False, unique = True). По нему будет создан ключевой индекс. Если ключевое поле в модели не было задано явно, сам фреймворк создаст в ней целочисленное автоинкрементное ключевое поле с именем id;

* *editable* — если True, то поле будет выводиться на экран в составе формы, если False — не будет (даже если явно создать его в форме). По умолчанию — True;

* *db_column* — имя поля таблицы в виде строки. Если не указано, то поле таблицы получит то же имя, что и поле модели.

### Поля

|               |                                                              |      |
| ------------- | ------------------------------------------------------------ | ---- |
| DateTimeField | *auto now add* конструктора значение True, мы предпишем Django при создании новой записи заносить в это поле текущие дату и время<br />*db index* при присваивании ему значения True укажет создать для этого поля индекс |      |
|               |                                                              |      |
|               |                                                              |      |

### class Meta:

| атрибуты класса               | Описание                                                     |      |
| ----------------------------- | ------------------------------------------------------------ | ---- |
| **verbose_name**              | название модели в единственном числе.                        |      |
| **ordering**                  | последовательность полей, по которым по умолчанию будет выпол­няться сортировка записей. |      |
| **verbose_name_plural**       | название модели во множественном числе;                      |      |
| **managed = False**           | если модель представляет существующую таблицу или представление базы данных, созданное каким-либо другим способом. Не позволяет вносить изменения в таблицу. Вродк так |      |
| **db_table = 'user_account**' | Название таблицы в базе данных для этой модели               |      |



### Методы модели, которые переопределим:

| Метод                                           | Описание                                    |
| ----------------------------------------------- | ------------------------------------------- |
| def__ str__ (self) :<br/>      return self.name | Возвращающий строковое представление класса |
|                                                 |                                             |
|                                                 |                                             |
|                                                 |                                             |

### Своя функция

```python
 ... 
facilities = models.ManyToManyField("Facilities")
number_room = models.CharField(max_length=3, verbose_name='№ комнаты', blank=True)

   def save(self, *args, **kwargs):
       self.number_room = self.title[0:3]
       super(Room, self).save()
```



### USER при создании модели

```python
from django.contrib.auth import get_user_model
User = get_user_model() #(говорим Джанго что мы хотим использовать юзера, который указан в settings.AUTH_USER_MODEL )

```



### Полиморфные связи  ContentType

Полиморфная связь создается в классе вторичной модели. Для ее установления необходимо объявить там три сущности:

* поле для хранения типа модели, связываемой с записью. Оно должно иметь тип ForeignKey (т. е. внешний ключ для связи "один-со-многими"), устанавливать СВЯЗЬ С моделью ContentType из модуля django. contrib. contenttypes .models (там хранится перечень всех моделей проекта) и выполнять каскадное удаление. Обычно такому полю дается имя `content type;`
* поле для хранения значения ключа связываемой записи. Оно должно иметь целочисленный тип — обычно PositiveintegerFieid. Как правило, этому полю дается имя `object id;`
* поле полиморфной Связи, реализуемое экземпляром класса GenericForeignKey из модуля django.contrib.contenttypes. fields. Вот формат вызова его конструктора:
  `GenericForeignKey ([ct_field=’ content_type ’ ] [, ] [fk_field=’object_id' ][, ] [for_concrete_model=True] )`

Конструктор принимает следующие параметры:
  `ct_fieid`— указывает имя поля, хранящего тип связываемой модели, если   это имя отличается от content type;
  `fk_fieid` — указывает имя поля, хранящего ключ связываемой записи, если   это имя отличается от object id;
  `for_concrete_model`— следует дать значение False, если необходимо устанавливать связи, в том числе и с прокси-моделями (о них будет рассказано
  позже).  

```python
from django.contrib.contenttypes.fields import GenericForeignKey
from django.contrib.contenttypes.models import ContentType
#Экземпляр ContentType представляет и хранит информацию о моделях, использующихся в вашем проекте, и новые экземпляры модели ContentType создаются автоматически при добавлении новых моделей в проект.
class CartProduct(models.Model):
    user = models.ForeignKey('Customer', verbose_name='Покупатель', on_delete=models.CASCADE)
    cart = models.ForeignKey('Cart', verbose_name='Корзина', on_delete=models.CASCADE, related_name='related_products')
    content_type = models.ForeignKey(ContentType, on_delete=models.CASCADE)
    object_id = models.PositiveIntegerField()
    content_object = GenericForeignKey('content_type', 'object_id')
    qty = models.PositiveIntegerField(default=1)
    final_price = models.DecimalField(max_digits=9, decimal_places=2, verbose_name='Общая цена')

# p = NotebookProduct.objects.get(pk=1)
# cp = CartProduct.objects.create(content_object=p). content_type и object_id создадутся автоматически. object_id станет 1, content_type модель Nooteproduct
    
    def __str__(self):
        return "Продукт: {} (для корзины)".format(self.content_object.title)

    def save(self, *args, **kwargs):
        self.final_price = self.qty * self.content_object.price
        super().save(*args, **kwargs)

```

