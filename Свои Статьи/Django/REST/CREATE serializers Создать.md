# CREATE serializers

1. serializers

   ```python
   from rest_framework import serializers
   from cars.models import Car
   
   class CarDetailSerializer(serializers.ModelSerializer):
       class Meta:
           model = Car
           fields = '__all__'
   
       
   ```

   

2. models

   ```python
   from rest_framework import generics
   from cars.serializers import CarDetailSerializer
   
   class CarCreateView(generics.CreateAPIView):
       serializer_class = CarDetailSerializer
          
   
   ```

   

