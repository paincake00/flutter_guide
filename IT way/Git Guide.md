
Чтобы исключить файл из индексации в Git, но сохранить изменения локально (не отображались при `git status`):

```sh
git update-index --assume-unchanged <file>
```

Если вы хотите отменить это действие и снова начать отслеживать изменения в файле:

```sh
git update-index --no-assume-unchanged <file>
```

