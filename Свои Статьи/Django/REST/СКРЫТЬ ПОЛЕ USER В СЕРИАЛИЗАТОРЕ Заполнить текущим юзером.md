# СКРЫТЬ ПОЛЕ USER В СЕРИАЛИЗАТОРЕ Заполнить текущим юзером

1. serializers.py

   ```python
   class CarDetailSerializer(serializers.ModelSerializer):
       user = serializers.HiddenField(default=serializers.CurrentUserDefault())
       class Meta:
           model = Car
           fields = '__all__'
   ```

   

