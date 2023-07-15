# git

`touch .gitignore`

echo -e '...\n' >> .gitignore

notepad .gitignore

git config --global user.name "John Doe"

git config --global user.email johndoe@example.com

Если вы хотите проверить используемые настройки

git config --list

git config user.name

Клонирование существующего репозитория - создаст каталог myfolder

&#x20;   git clone git://github.com/schacon/grit.git myfolder

Просмотр истории коммитов

&#x20;   git log

&#x20;   \-p показывает дельту (разницу/diff)

&#x20;   \-2 что ограничит вывод до 2-х

&#x20;   \--word-diff получить дельту по словам вместо обычной дельты по строкам

&#x20;   \--stat выводит под каждым коммитом список изменённых файлов

Отмена изменений  &#x20;

Изменение последнего коммита

&#x20;   git commit --amend

тмена индексации файла

&#x20;   git reset HEAD <файл>... для исключения из индекса  &#x20;

Отмена изменений файла    до состояния предыд коммита

&#x20;   git checkout -- \<file>...

$ git remote

&#x20;   origin

Чтобы посмотреть, какому URL соответствует сокращённое имя в Git, можно указать команде опцию -v

&#x20;   $ git remote -v

Чтобы добавить новый удалённый Git-репозиторий под именем-сокращением, к которому будет проще обращаться

&#x20;   git remote add \[сокращение] \[url]

для получения данных из удалённых проектов, следует выполнить

&#x20;   $ git fetch \[имя удал. сервера]

&#x20;    git pull

извлекает (fetch) данные с сервера, с которого вы изначально склонировали, и автоматически пытается слить (merge) их с кодом, над которым вы в данный момент работаете

Когда вы хотите поделиться своими наработками, вам необходимо отправить (push) их в главный репозиторий

&#x20;   git push \[удал. сервер] \[ветка]

Основы ветвления

&#x20;   $ git checkout -b iss53

&#x20;   Switched to a new branch "iss53"

&#x20;   или

&#x20;   $ git branch iss53

&#x20;   $ git checkout iss53

слить (merge) изменения назад в ветку master, чтобы включить их в продукт. Это делается с помощью команды git merge:

&#x20;   $ git checkout master

&#x20;   $ git merge hotfix

&#x20;   Updating f42c576..3a0874c

&#x20;   Fast forward

&#x20;   README |    1 -

&#x20;   1 files changed, 0 insertions(+), 1 deletions(-)

Вы можете удалить ветку с помощью опции -d к git branch:

&#x20;   $ git branch -d hotfix

&#x20;   Deleted branch hotfix (3a0874c).gir

```
ssh-keygen -t rsa -C "your@email.ru"

На вопрос “Enter file in which to save the key” введите id_rsa и нажмите “Enter”

3. Далее введите любой пароль, и подтвердите его (лучше пропустить, потому что каждый раз при соединении с github, придется вводить его)

4. Скопируйте созданные в директории пользователя файлы “id_rsa” и “id_rsa.pub” в папку c:\Documents and Settings\Username\.ssh (XP) или c:\Users\Username\.ssh (Vista)

6. Добавьте содержимое публичного ключа из файла “id_rsa.pub”, воспользовавшись ссылкой “add public key” в разделе “SSH Public Keys”

7. Проверьте работоспособность ключа, введя в Git Bash:
ssh git@github.com
```

```
Можно сделать до какого то определенного коммита по хешу
git reset --hard HEAD hash
```
