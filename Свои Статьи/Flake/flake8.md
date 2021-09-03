# flake8

Сам Flake8 — инструмент, позволяющий просканировать код проекта и обнаружить в нем стилистические ошибки и нарушения различных конвенций кода на Python.

```python
pip install flake8
flake8 my_project #or
flake8 .
```

Чтобы увеличить разрешенную длину строки до 120, добавьте файлик `.flake8` в корневую директорию, содержимое файла будет таким:

```python
[flake8] 
max-line-length = 120
exclude = .git,__pycache__,venv
```

Как подключить в pycharm

https://habr.com/ru/company/dataart/blog/318776/

