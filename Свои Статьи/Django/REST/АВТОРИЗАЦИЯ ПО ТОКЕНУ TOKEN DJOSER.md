# АВТОРИЗАЦИЯ ПО ТОКЕНУ TOKEN DJOSER

1. Install djoser

```python
 pip install djoser
```

2. settings.py

   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'demorest.apps.DemorestConfig',
       
       "rest_framework",
       "rest_framework.authtoken",		# авторизация по токену
       "cars",
       "djoser"                          #
   ]
   ```

   

3. view.py  ДОСТУП ПО ТОКЕНУ так решили

   ```python
   from django.shortcuts import render
   from rest_framework import generics
   from cars.serializers import CarDetailSerializer, CarListSerializer
   from cars.models import Car
   from cars.permission import IsOwnerOrReadOnly
   from rest_framework.permissions import IsAuthenticated
   from rest_framework.authentication import TokenAuthentication##
   
   class CarCreateView(generics.CreateAPIView):
       serializer_class = CarDetailSerializer
          
   
   class CarsListView(generics.ListAPIView):
       serializer_class = CarListSerializer
       queryset = Car.objects.all()
       permission_classes = [IsAuthenticated,]   #Посмотрит любой, кто авторизован
   
   class CarDetailView(generics.RetrieveUpdateDestroyAPIView):
       serializer_class = CarDetailSerializer
       queryset = Car.objects.all()
       authentication_classes = [TokenAuthentication]  ## Авторизуемся по токену https://www.youtube.com/watch?v=C6S3dMt1s_M
       permission_classes = [IsOwnerOrReadOnly,]     #Другой пользователь ничего не исправит
   ```

   

