Посмотрите связи с удалённым репозиторием
```
cd repo
git remote -v
```
Удалите связь с текущим репозиторием
```
git remote remove origin
```
Подключите свой локальный репозиторий к существующему удалённому
```
git remote add -f origin https://github.com/allseenn/remote-test-2
```
Отправьте изменения в новый удалённый репозиторий
```
git push -u origin main
```
Создаём связь со вторым репозиторием (зеркалом)
git remote add mirror https://github.com/allseenn/remote-test-1
Просматриваем связи и убеждаемся, что все установились
git remote -v
git remote show mirror
Вносим в удалённых репозиториях изменения в разные файлы
Затягиваем изменения: git fetch --all