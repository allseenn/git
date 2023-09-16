## Итоговая работа по курсу "Углубленный контроль версий"

Данное домашнее задание является продолжением [домашнего задания](https://github.com/allseenn/bash), которое вы выполняли на предыдущем семинаре в репозитории с собственным проектом.

В этом задании предлагаем каждому из вас побыть в роли тимлида.

#### 1. Пригласите в свой проект кого-то из коллег по обучению, дайте им доступ к своему репозиторию (кроме ветки master).

Для выполнения данного задания буду использовать два своих github аккаунта:

- allseenn (в роли тимлида)
- strogino (в роли разраба)

Т.к. в задании не сказано с помощью какого инструмента выполнять работу (веб или консоль), то рискну воспользоваться консольным набором curl + GitHub REST API. Полный [список GitHub API](https://docs.github.com/ru/rest/repos)

**allseenn приглашает strogino в репозиторий bash:**

```
curl \
 -X PUT \
 -H "Accept: application/vnd.github+json" \
 -H "Authorization: Bearer <PAT>" \
 https://api.github.com/repos/allseenn/bash/collaborators/strogino \
 -d '{"permission":"write"}'
```

**allseenn ограничил доступ к ветке main репозитория bash:**

```
curl -L -X PUT \
 -H "Accept: application/vnd.github+json" \ 
 -H "Authorization: Bearer <PAT_Beta>" 
  https://api.github.com/repos/allseenn/bash/branches/main/protection \
 -H "X-GitHub-Api-Version: 2022-11-28" \
 -d '{"required_status_checks":null,"enforce_admins":null,"required_pull_request_reviews":{"required_approving_review_count":1},"restrictions":null}'
```

**allseen может снять защиту ветки main командой:**

```
curl -L \
  -X DELETE \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <PAT_Beta>" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  https://api.github.com/repos/allseen/bash/branches/main/protection
```

**PAT** - Personal Access Token - персональный токен. Можно задать один раз в личном кабинете. Т.к. с 13.08.2021 github отключил авторизацию по паролю. Использую классический вариант токена на стороне strogino.

Для того чтобы получить PAT - необходимо перейти по ссылке https://github.com/settings/tokens/new. Можно задать неограниченный срок действия токена и выбрать права.

**PAT_Beta** - усиленный PAT, с большими правами. https://github.com/settings/tokens?type=beta. На стороне allseenn.

Приглашение присоединиться к проекту приходит на почту, но мы можем посмотреть приглашения с помощью api.

**strogino смотрит список приглашений:**

```
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <PAT>" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  https://api.github.com/user/repository_invitations

```

**strogino по ID приглашения принимает его:**

```
curl -L \
  -X PATCH \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <PAT>" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  https://api.github.com/user/repository_invitations/232323096
```

где, 232323096 - номер приглашения

#### 2. Поставьте ему в GitHub задачу по своему проекту, попросите её выполнить в отдельной ветке, а после выполнения — создать pull request и перевести задачу обратно на вас.

**allseenn открывает issue для strogino с просьбой, что-то сделать в отдельной ветке, а потом создать пулл-реквест:**

```
curl -X POST \
 -H "Accept: application/vnd.github+json" \
 -H "Authorization: Bearer <PAT_Beta> \
 https://api.github.com/repos/allseenn/bash/issues \
-d '{"title":"Add some text to README.md","body":"Make it in dev-branch and pull request me with new issue","assignee":"strogino"}'
```

<a href=https://github.com/allseenn/git/blob/main/03.Tasks/pics/01.png>
<img src=pics/01.png width=500>
</a>

*Рис1. Результат выполнения api по созданию issue, если посмотреть через браузер*

<div style="page-break-after: always;"></div>

**strogino получает issue:**

Все новые задачи приходят на почту, почтовая служба легко может быть задействована в процессе автоматизации.
Но, и на этот случай поможет API:

```
curl -X GET \
 -H "Accept: application/vnd.github+json" \
 -H "Authorization: Bearer <PAT>" \
 https://api.github.com/repos/allseenn/bash/issues \
 -d '{"state":"open","assignee":"strogino"}'
```

**strogino после прочтения задачи создал ветку dev изменил файли и отправил на удаленный репозиторий:**

```
git checkout -b dev
git push -u origin dev
```

**strogino создает pull-request для слияния ветки dev в main:**

```
curl -L -X POST \
 -H "Accept: application/vnd.github+json" \
 -H "Authorization: Bearer <PAT>" \
 -H "X-GitHub-Api-Version: 2022-11-28" \
 https://api.github.com/repos/allseenn/bash/pulls \
 -d '{"title":"New dev branch","body":"Please merge my work to main branch","head":"dev","base":"main"}'
```

<a href=https://github.com/allseenn/git/blob/main/03.Tasks/pics/02.png>
<img src=pics/02.png width=500>
</a>

*Рис2. Результат выполнения api по созданию pull-request, если посмотреть через браузер*

#### 3. Проверьте выполнение задачи, примите pull request и удалите ветку, в которой решалась данная задача.

Пользователь allseenn также получит письмо о pull request. Но, его можно посмотреть с помощью api.

**allseenn читает pull request:**

```
curl -L -X GET \
-H "Accept: application/vnd.github+json" \
-H "Authorization: Bearer <PAT_Beta>" \
-H "X-GitHub-Api-Version: 2022-11-28" \
https://api.github.com/repos/allseenn/bash/pulls
```

Вышеописанный api вернет список всех открытых запросов, можно выбрать нужный и подставить его в конец url.

В нашем случае это второй запрос: https://api.github.com/repos/allseenn/bash/pulls/2 - этот адрес вставим в следующею команду.

<a href=https://github.com/allseenn/git/blob/main/03.Tasks/pics/03.png>
<img src=pics/03.png width=500>
</a>

*Рис3. Тело pull-request, если посмотреть через браузер*

<a href=https://github.com/allseenn/git/blob/main/03.Tasks/pics/04.png>
<img src=pics/04.png width=500>
</a>

*Рис4. Различия веток main и dev, если посмотреть через браузер*

**allseenn принимает запрос и сливает ветки:**

```
curl -L -X PUT \
 -H "Accept: application/vnd.github+json" \
 -H "Authorization: Bearer <PAT_Beta>" \
 -H "X-GitHub-Api-Version: 2022-11-28" \
 https://api.github.com/repos/allseenn/bash/pulls/2/merge \
 -d '{"commit_title":"Merge pull request #1 from allseenn/dev","commit_message":"New dev branch","merge_method":"merge"}'
```

Слияние произошло успешно. Осталось удалить ветку dev. Это можно сделать через консоль:

```
git branch -d dev
git push origin --delete dev
```

Или с помощью апи

**allseenn удаляет ветку dev:**

```
curl -L -X DELETE \
-H "Accept: application/vnd.github+json" \
-H "Authorization: Bearer <PAT_Beta>" \
-H "X-GitHub-Api-Version: 2022-11-28" \
https://api.github.com/repos/allseenn/bash/git/refs/heads/dev
```

Ветка удалена успешно.

<a href=https://github.com/allseenn/git/blob/main/03.Tasks/pics/05.png>
<img src=pics/05.png width=500>
</a>

*Рис5. Результат вливания ветки dev в main, если посмотреть через браузер*

**Задачу можно закрыть**

```
curl -X POST \
 -H "Accept: application/vnd.github+json" \
 -H "Authorization: Bearer <PAT or PAT_Beta>" \
https://api.github.com/repos/allseenn/bash/issues/1 \
 -d '{"state":"closed"}'
```

В итоге имеем:

- [Закрытую задачу (issue)](https://github.com/allseenn/bash/issues/1)
- [Выполненный pull-request](https://github.com/allseenn/bash/pull/2)
