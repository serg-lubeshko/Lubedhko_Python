# [подключиться к нужному репозиторию github?](https://ru.stackoverflow.com/questions/1069192/Как-подключиться-к-нужному-репозиторию-github)

```git
Это делается командой git remote add, но делать её можно только на существующем репозитории, так что вам понадобится ещё и git init. Потом вам понадобится получить из репозитория коммиты командой git fetch и сделать git reset чтобы вносить свои изменения не с нуля, а начиная с головы репозитория.

git init
git remote add origin url_репозитория
git fetch origin
git reset --mixed origin/master
git add измененные файлы
git commit -m "комментарий к коммиту"
git push -u origin master
URL репозитория можно получить нажав на ту самую кнопку Clone в интерфейсе github.
```

```
2.Открой терминал, затем зайди в папку, где у тебя лежит скачанный файл. Теперь пропиши эти команды по порядку:

git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/твой_аккаунт/имя_репозитория.git
git push -u origin master
```

