Практика 1 — Эталонная среда разработки и Git

Базовый стенд проекта (Docker):
```
kingofdbox@DESKTOP-CBS8NC0:~$ cd projects
kingofdbox@DESKTOP-CBS8NC0:~/projects$ git clone https://github.com/KingOfDBox/starter_repo
Cloning into 'starter_repo'...
remote: Enumerating objects: 26, done.
remote: Counting objects: 100% (26/26), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 26 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (26/26), 6.32 KiB | 3.16 MiB/s, done.
kingofdbox@DESKTOP-CBS8NC0:~/projects$ cd starter_repo
kingofdbox@DESKTOP-CBS8NC0:~/projects/starter_repo$ cd starter_repo
kingofdbox@DESKTOP-CBS8NC0:~/projects/starter_repo/starter_repo$ docker compose up -d --build
[+] Running 5/5
  ✔ app Pulled                                                                         35.1s
    ✔ 19fb8589da02 Pull complete                                                       31.5s
    ✔ 38513bd72563 Pull complete                                                       30.7s
    ✔ a9ffe18d7fdb Pull complete                                                       30.8s
    ✔ e73850a50582 Pull complete                                                       31.5s
[+] Running 2/2
  ✔ Network starter_repo_default  Created                                               0.1s
  ✔ Container starter_repo-app-1  Started                                               1.3s
kingofdbox@DESKTOP-CBS8NC0:~/projects/starter_repo/starter_repo$ docker compose exec app python -m -pytest -q
/usr/local/bin/python: No module named -pytest
kingofdbox@DESKTOP-CBS8NC0:~/projects/starter_repo/starter_repo$ docker compose exec app python -m pytest -q
...                                                                                  [100%]
3 passed in 0.01s
```

Git: обязательный минимум
Конфигурация и подпись коммитов
```
git config --global user.name "my username"
kingofdbox@DESKTOP-CBS8NC0:~/projects/starter_repo/starter_repo$ git config --global user.email "my email"
```
Далее создаем ключ rsa c 2048 битами, регистрируем его и подписываем:
```
kingofdbox@DESKTOP-CBS8NC0:~/projects/starter_repo/starter_repo$ gpg --full-generate-key
git config --global user.signingkey KEYID
git config --global commit.gpgsign true
```
Ветвление и rebase
```
kingofdbox@DESKTOP-CBS8NC0:~/projects/starter_repo/starter_repo$ git checkout -b feature/setup
Switched to a new branch 'feature/setup'
kingofdbox@DESKTOP-CBS8NC0:~/projects/starter_repo/starter_repo$ git fetch origin && git rebase origin/main
Current branch feature/setup is up to date.
```
