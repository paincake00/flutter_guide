
Система контроля версий - система, отслеживающая изменения в файлах в течение времени и позволяющая хранить несколько версий одного и того же файла и вернуться при необходимости к более ранним версиям.

git это одна из таких систем.

### Настройка

![[Pasted image 20251008192106.png]]

```sh
git config --global user.name "John Doe"

git config --global user.email john_doe@at-consulting.ru

git config --global core.editor nano
```


### Команды

#### `--amend`

Позволяет изменять последний коммит (не создавать новый), то есть изменять HEAD:
```sh
git commit --amend
```

#### `--orphan` и `--alow-empty`

Можно создать корневую (сиротскую) ветку и как бы начать новую историю коммитов. Также можно создать пустой коммит для инициализации, чтобы можно было залить на удаленный репозиторий.  
```sh
git checkout --orphan main
git commit --allow-empty -m "Initial empty commit"

git push origin main
```

#### Удалить "мертвые" ссылки на удаленные ветки (origin/branch)

```sh
git remote prune origin
```

### Красивый log

```sh
git log --oneline --graph --decorate --all
```

### Теги

Нужен чтобы пометить коммит.
```sh
git tag 1.0
```

Чтобы исключить файл из индексации в Git, но сохранить изменения локально (не отображались при `git status`):

### Ветка слежения (Tracking branch)

```sh
git fetch origin # получаем ветки

git checkout -b serverfix origin/serverfix
```

### Быстрое переключение на другую ветку с сохранением

Можно использовать `git stash` через `push` и `pop`, но лучше и проще просто создать быстро ветку (прямо в текущей, которые даже не добавлены в индекс через `git add`):

```sh
git checkout -b temp-branch
# потом сохраняем изменения
git add .
git commit -m "suddenly changes"
```
Потом переключаемся на другую ветку и что-то делаем.

Возвращаемся на `temp-branch` и делаем (если изменений не делали в actual-branch - скипайте этот шаг):
```sh
# если сделали изменения в actual-branch
git checkout temp-branch
git rebase actual-branch # или другая, на которой работали изначально
```

Потом:
```sh
# если не делали изменений в асtual-branch
git checkout actual-branch
git rebase temp-branch
```
Всё.

### Merging

git merge
```sh
git checkout main       # куда вливаем
git merge feature          # что вливаем
```

Fast-Forward merge в отличие от трехстороннего мерджа (three-way merge) позволяет оставлять красивую историю (как у git rebase), просто перемещая указатель главной ветки на последний коммит фичевой ветки (по сути перенесди коммиты как в rebase).

```sh
git merge --ff-only <feature> # <feature> - ветка, которую надо влить
```

[![Tutorial Git and GitHub - Fast-forward Merge - 2020|300](https://www.bogotobogo.com/cplusplus/images/Git/Fast_Forward_Merge/FastForwardMerge.png)

### Rebasing

git rebase
```sh
git checkout feature       # что переносим
git rebase main         # куда переносим (крепим) / на базе чего строим

git push --force-with-lease origin feature # обновляем origin/feature
```