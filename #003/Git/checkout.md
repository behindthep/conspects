скопировать файл из одной ветки или из другого коммита. обновили конфиг и надо у себя в ветке тоже его использовать без слияний и прочих лишних действий.

git checkout <from-branch-name> <path-to-file-or-dir>

репозиторий с двумя ветками и в ветке second  будет файл в директории.

git init -b master&& \
echo "this is txt file" > main.txt && \
git add main.txt && \
git commit -m "Initial commit" && \
git checkout -b second && \
mkdir myDir && \
echo "must be copied" > myDir/second.txt && \
git add myDir/second.txt && \
git commit -m "added second.txt"

скопировать и выполнить все команды за один раз.

Допустим, нам требуется скопировать файл myDir/second.txt из ветки second в master. Для копирования файла, перейдите в ветку, в которую хотите скопировать файл или директорию из другой ветки.

git checkout master

После можно выполнить команду копирования:

git checkout second ./myDir/second.txt

После этого из ветки second будет скопирован файл, при этом сохранится путь до файла. То есть в master будет лежать файл в папке mydDir.

Если файл уже есть в текущей ветке - он будет перезаписан!

файл у нас скопировался:

git status

On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   myDir/second.txt

файл уже подготовлен к коммиту.

из коммита скопировать файл, просто замените название ветки на хэш коммита.

скопировать файл по другому пути? Так тоже можно, для этого можно использовать другую команду git show.

повторить действия - удалите директорию и снова создайте репозиторий с двумя ветками, использую код выше.

перейдем в master и создадим папку config и в нее позже скопируем файл second.txt.

git checkout master
mkdir config

git show <branch-name-or-hash>:<path-to-copy> > <path-to-paste>

указываем ветку или хэш коммита, далее источник файла для копирования и путь до нового файла в текущей ветке.

show “показывает” файл из другой ветки. А мы этот вывод пишем в новый файл у себя, для этого и указываем вывод через >

git show second:myDir/second.txt > config/config.txt

файл скопирован:

git status -uall

On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	config/config.txt

nothing added to commit but untracked files present (use "git add" to track)

-uall покажет файлы в новых директориях, без этого параметра не увидим содержимое config

Файл на месте. он скопирован в рабочую директорию и к коммиту не подготовлен и не отслеживается гитом.
