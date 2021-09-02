# LIST Посмотреть все записи

1. serializers

   ```python
   from rest_framework import generics
   from cars.serializers import CarDetailSerializer
   from cars.models import Car #
   
   ```

2. model

   ```python
   class CarsListView(generics.ListAPIView):
       serializer_class = CarListSerializer
       queryset = Car.objects.all()
   ```

   

