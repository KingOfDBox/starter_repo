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
Далее создаем ключ rsa c 2048 битами на неограниченный срок, регистрируем его и подписываем:
```
kingofdbox@DESKTOP-CBS8NC0:~/projects/starter_repo/starter_repo$ gpg --full-generate-key
kingofdbox@DESKTOP-CBS8NC0:~/projects/starter_repo/starter_repo$ gpg --list-secret-keys --keyid-format=long
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
Bisect: поиск регрессии
Запускаем скрипт bisect_demo_init.sh и создем коммит good_baseline
```
serkingofdbox@DESKTOP-5S32NBH:~/tmp/starter_repo/starter_repo$ bash scripts/bisect_demo_init.sh
[main b5e2311] [demo] add calc_good.py
 1 file changed, 6 insertions(+)
 create mode 100644 app/calc_good.py
[main 6d05d64] [demo] introduce bug
 1 file changed, 2 insertions(+), 4 deletions(-)
[ok] created tag good_baseline and bad HEAD
```
Поиск плохого коммита
```
serkingofdbox@DESKTOP-5S32NBH:~/tmp/starter_repo/starter_repo$ git bisect start
status: waiting for both good and bad commits
serkingofdbox@DESKTOP-5S32NBH:~/tmp/starter_repo/starter_repo$ git bisect bad HEAD
status: waiting for good commit(s), bad commit known
serkingofdbox@DESKTOP-5S32NBH:~/tmp/starter_repo/starter_repo$ git bisect good good_baseline
6d05d64bd62aa0cdb1ac3e0f8b152f11d03bc8bc is the first bad commit
commit 6d05d64bd62aa0cdb1ac3e0f8b152f11d03bc8bc
Author: Student <student@example.com>
Date:   Sat Nov 8 10:13:18 2025 +0600

    [demo] introduce bug

 app/calc.py | 6 ++----
 1 file changed, 2 insertions(+), 4 deletions(-)
```
Hooks и pre-commit
Создали файл .pre-commit-config.yaml и установили pre-commit
```
serkingofdbox@DESKTOP-5S32NBH:~/tmp/starter_repo/starter_repo$ pre-commit install
pre-commit installed at .git/hooks/pre-commit
serkingofdbox@DESKTOP-5S32NBH:~/tmp/starter_repo/starter_repo$ pre-commit -a
usage: pre-commit [-h] [-V]
                  {autoupdate,clean,gc,init-templatedir,install,install-hooks,migrate-config,run,sample-config,try-repo,uninstall,validate-config,validate-manifest,help,hook-impl}
                  ...
pre-commit: error: unrecognized arguments: -a
serkingofdbox@DESKTOP-5S32NBH:~/tmp/starter_repo/starter_repo$ pre-commit run -a
An error has occurred: InvalidConfigError:
=====> .pre-commit-config.yaml is not a file
Check the log at /home/serkingofdbox/.cache/pre-commit/pre-commit.log
serkingofdbox@DESKTOP-5S32NBH:~/tmp/starter_repo/starter_repo$ pre-commit run -a
[INFO] Initializing environment for https://github.com/pre-commit/pre-commit-hooks.
[INFO] Installing environment for https://github.com/pre-commit/pre-commit-hooks.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
trim trailing whitespace.................................................Passed
fix end of files.........................................................Failed
- hook id: end-of-file-fixer
- exit code: 1
- files were modified by this hook

Fixing requirements.txt
Fixing README.md
Fixing app/calc.py
Fixing scripts/bisect_test.sh
Fixing docs/adr/README.md
Fixing app/calc_good.py
Fixing app/__init__.py
Fixing tests/test_calc.py
Fixing scripts/mini-smoke.sh
Fixing compose.yaml
Fixing app/calc_buggy.py
Fixing tests/test_smoke.py
Fixing docs/adr/templates/adr-template.md

check yaml...............................................................Passed
check json...........................................(no files to check)Skipped
check for added large files..............................................Passed
```
Добавка gitleaks
<img width="925" height="294" alt="image" src="https://github.com/user-attachments/assets/c6c2fe15-8452-428a-9ff0-49238034881a" />
