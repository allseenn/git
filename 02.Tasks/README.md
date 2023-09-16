Данное домашнее задание является продолжением [домашнего задания](https://github.com/allseenn/git/tree/main/01.Tasks), которое вы выполняли на предыдущем семинаре в репозитории с [собственным проектом](https://github.com/allseenn/bash).

1. Просмотрите историю коммитов в своём проекте и выберите три случайных коммита. Просмотрите изменения, которые были в них сделаны.

```
git log --graph
git log --pretty=oneline | shuf -n 3
```

2. Верните эти изменения командой git revert последовательно, чтобы в итоге получилось тоже три коммита.

```
git revert 612aaf4a1e71eebbe313dc84b3b1af15c187c570
git revert -m 2 916d87e4b940a48b2c337a182aecb57aab919b2d
git revert 1c9ec0d95bc207b331e0d2ce5369959f49bdd607

```

3. Попробуйте отменить эти три коммита:
* последний — командами git reset --soft и git restore;
* предпоследний — командой git reset --mixed и git restore;
* первый — командой git reset --hard.

```
git reset --soft HEAD~1
git reset --mixed HEAD~1
git reset --hard HEAD~1
```
