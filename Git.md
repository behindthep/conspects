скопировать файл из одной ветки или из другого коммита.

git checkout <from-branch-name> <path-to-file-or-dir>

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

скопировать файл myDir/second.txt из ветки second в master. Для копирования файла, перейдите в ветку, в которую хотите скопировать файл или директорию из другой ветки.

git checkout master

После можно выполнить команду копирования:

git checkout second ./myDir/second.txt

из ветки second будет скопирован файл, при этом сохранится путь до файла. То есть в master будет лежать файл в папке mydDir.

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

git branch newImage
The branch newImage now refers to the actual commit

Choose the branch for commiting
git checkout newImage
(git switch)

Create a new branch and check it out at the same time
git checkout -b name

Merging branch into main* (staying on main branch) - creates new merge commit on main*
git merge bugFix

current branch - git merge branch that you want merge with

Placed on bugFix aand merge with main

git checkout bufFix; git merge main


For combining commits with linear sequence. With bugFix selected
git rebase main

Now bugFix copies of commits move at the top of main branch (rebased onto main), so history is linear

Checked out on the main and rebase onto bugFix
Move the main branch reference forward on history

HEAD points to the current branch on the recent (last) commit

Detaching HEAD - attaching it to a commit instead of a branch.

Was:
HEAD -> main -> Commit1

Git checkout Commit1

Now:
HEAD -> Commit1

Changes are not effect of any branches

Relative refs
allow us use HEAD or branch name to moving on history

git checkout main^.  Find the parent of the specified commit. Now HEAD points on commits parent. This detach HEAD
Git cheackout commit2;
git checkout HEAD~3.   Num times moves HEAD to parents

-f option (not allowed for current branch) reasign a branch to a commit
git branch -f main HEAD~3
Moves by force the main branch to three parents behind HEAD

Rrvering changes

Revers changes by moving a branch reference backwars in time to an older commit.
git reset HEAD~1

Reverse last commit changes and share those with others - create commit with reversed changes:
git revert HEAD

git cherry-pick C1 C2 ...
Copy commits below HEAD (after - make them lasts)

Git Interactive Rebase
Git cherry-pick is great when you know which commits you want (and their hashes).

you don't know what commits you want? interactive rebasing -- review a series of commits you're about to rebase.

All interactive rebase - Git using rebase command with -i.
open up a UI to show you which commits are about to be copied below the target of the rebase.

reorder commits.
choose to keep all commits or drop specific ones. When the dialog opens, each commit is set to be included.
squashing (combining) commits, amending commit messages, editing the commits themselves.

git rebase -i HEAD~4

Git copied down commits in the same way you specified through the UI.

Locally stacked commits
track down a bug but it is quite elusive. In order to aid in my detective work, I put in a few debug commands and a few print statements.

All of these debugging / print statements are in their own commits. Finally I track down the bug, fix it, and rejoice!

need to get my bugFix back into the main branch. If I fast-forwarded main, then main get all my debug statements which is undesirable.

copy only one of the commits over.

git rebase -i
git cherry-pick

Juggling Commits
changes (newImage) and another set of changes (caption) that are related, so they are stacked on top of each other in your repository (aka one after another).

make a small modification to an earlier commit. change newImage, even though that commit is way back in history!

re-order the commits so the one we want to change is on top with git rebase -i
git commit --amend to make modification
Then re-order the commits back to how they were previously with git rebase -i
Finally move main to this updated part of the tree

Once the commit we wanted to change was on top, we could --amend it and re-order back to our preferred order.

git cherry-pick will plop down a commit from anywhere in the tree onto HEAD (as long as that commit isn't an ancestor of HEAD).


permanently mark historical points in your project's history. For things like major releases and big merges, mark these commits with something more permanent than a branch?

Git tags permanently mark certain commits as "milestones" that you can then reference like a branch.

You can't "check out" a tag and then complete work on that tag -- tags exist as anchors in the commit tree that designate certain spots.

git tag v1 C1
(version 1)


tags serve as such great "anchors" in the codebase, git has a command to describe where you are relative to the closest "anchor" (tag).

get your bearings after you've moved many commits backwards or forwards in history; after you've completed a git bisect (a debugging search).

git describe <ref>

<ref> ('main' for example) is anything git can resolve into a commit. If you don't specify a ref, git uses where you're checked out right now (HEAD).

The output of the command:

<tag>-<numCommits>-g<hash>

tag is the closest ancestor tag in history, numCommits is how many commits away that tag is, and <hash> is the hash of the commit being described.

Rebasing Multiple Branches
rebase all the work from these branches onto main.

git rebase main bugFix
bugFix встанет за main
git rebase c2
ветка станет на коммит c2

Specifying Parents
Like the ~ modifier, the ^ modifier also accepts an optional number after it.

Rather than specifying the number of generations to go back (what ~ takes), the modifier on ^ specifies which parent reference to follow from a merge commit. merge commits have multiple parents, so the path to choose is ambiguous.

Git will follow the "first" parent upwards from a merge commit, but specifying a number with ^ changes this default behavior.

have a merge commit. If we checkout main^ without the modifier, we will follow the first parent after the merge commit.

git checkout main^ первый родитель (обычно ветка, в которую вливали изменения)
git checkout main^2 второй родитель (обычно ветка, которую влили)

HEAD or main эквивалентно можно использовать в командах

git branch -f three C2
ветку три переместить на коммит

git fetch удалённый репозиторий, содержит два коммита, отсутствующих в локальном.
Коммиты C2 и C3 скачаны в локальный репозиторий, удалённая ветка o/main отобразила эти изменения.

связывается с удалённым репозиторием и забирает данные проекта, которых ещё нет
должны появиться ссылки на все ветки из удалённого репозитория (o/main)
git fetch синхронизирует локальное представление удалённых репозиториев с тем, что является актуальным на текущий момент.

удалённые ветки отображают состояние удалённых репозиториев на тот момент когда вы 'общались' с ними в последний раз.
git fetch 'общается' с удалёнными репозиториями посредством Интернета (через протоколы http:// или git://).

git fetch забирает данные в локальный репозиторий, но не сливает их.

обновим работу, отобразить изменения!
локальный коммит, соединить с другой веткой.
git cherry-pick o/main
git rebase o/main
git merge o/main

Процедура скачивания (fetching) изменений с удалённой ветки и объединения (merging) - git pull.

git fetch; и git merge origin/main; - скачали C3 с fetch и объединяем наработки с git merge o/main. ветка main отображает изменения с удалённого репозитория. git pull делает тоже самое.


история версий «Revision history». зафиксированное изменение в системе контроля версий ревизией.

Фиксация изменений создаёт ревизию, ревизия может содержать внутри либо дельту изменений, либо снимок.

процесс переключения между ревизиями. загружаем ревизию, переключаемся на неё (checkout).

Между ревизиями выявлять различия в случае, если СКВ использует снимки, демонстрирует Word.

diff index.js index2.js > index.patch

patch index.js -i index.patch -o index2.js



git grep Hexlet $(git rev-list --all) # Поиск по истории. список хешей `rev-list`

откат изменений, в рабочей директории, но еще не попали в коммит.

добавили новые файлы в репозиторий, что они вам не нужны.
# удалит все неотслеживаемые файлы -f – force, -d – directory
git clean -fd

# Отменяем Измененные файлы в рабочей директории
git restore INFO.md

git revert aa600a4
# В проект вернулся файл PEOPLE.md

git revert — безопасный способ отмены коммитов. Он создаёт новый коммит, отменяет изменения предыдущего. не меняет историю коммитов.
git reset — опасный инструмент. отменять коммиты, но при этом может изменить или удалить изменения:

git commit --amend --no-edit

--amend не добавляет изменения в сущ коммит. откату коммита через git reset и выполнению нового коммита с новыми данными. один коммит, git commit выполнялась два раза (первый раз — когда сделали ошибочный коммит).

перед коммитом смотреть git diff --staged.


Переключимся на момент, когда был выполнен коммит с сообщением add INFO.md.

git checkout e6f625c


В Bash вывод местоположения редактированию переменной окружения $PS1
https://ru.hexlet.io/blog/posts/kak-prisoedinitsya-k-rabote-nad-opensorsom-chto-takoe-ps1-i-drugie-voprosy-otvechaet-razrabotchik-heksleta-andrey-moshkov#chto-takoe-ps1-i-dlya-chego-ispolzuetsya

git rm file -- cached - удалить фалй из репозитория(гит перестанет его отслеживать), файл останется в рабочей директории.




'сдвинуть' свои наработки - через перебазировку или rebasing. Давайте посмотрим, как это выглядит.

перебазируемся прежде чем публиковать изменения
git fetch; git rebase o/main; git push

обновили наш локальный образ удалённого репозитория средствами git fetch. Ещё мы перебазировали наши наработки, чтобы они отражали все изменения с удалённого репозитория, и опубликовали их с помощью git push.

обновить мои наработки к тому моменту, как удалённый репозиторий был обновлён? Конечно есть!

git merge не передвигает ваши наработки (а всего лишь создаёт новый коммит, в котором Ваши и удалённые изменения объединены), этот способ помогает указать git-у на то, что вы собираетесь включить в состав ваших наработок все изменения с удалённого репозитория. Это значит, что ваш коммит отразится на всех коммитах удалённой ветки, поскольку удалённая ветка является предком вашей собственной локальной ветки.

объединим (merge) вместо перебазирования (rebase)
git fetch; git merge o/main; git push

обновили  локальное представление удалённого репозитория с  git fetch, объединили ваши новые наработки с нашими наработками 

git pull, которая является аналогом и более кратким аналогом для совместных fetch и merge. А команда git pull --rebase - аналог для совместно вызванных fetch и rebase!

git pull --rebase; git push
git pull; git push



выполнять всю свою работу в так называемых фича-бранчах (вне main). как только работа выполнена, разработчик интегрирует всё, что было им сделано. за исключением одного шага, похоже на предыдущий урок (там, где мы закачивали ветки на удалённый репозиторий)

Ряд разработчиков делают push и pull лишь на локальную ветку main - ветка main всегда синхронизирована с тем, что находится на удалённом репозитории (o/main).

интеграцию фича-бранчей в main
закачку (push) и скачку (pull) с удалённого репозитория

обновить main и закачать выполненную работу.
git pull --rebase; git push
перебазировали нашу работу на новенький коммит, пришедший с удалённого репозитория, и
закачали свои наработки в удалённый репозиторий

Текущая задача является обильной - здесь представлена общая схема выполнения:

Есть три фича-бранчи (фича-ветки) - side1 side2 и side3
закачать каждую из них по очереди на удалённый репозиторий
удалённый репозиторий хранит в себе какие-то наработки, которые также следует скачать к себе

Чтобы закачать (push) новые изменения в удалённый репозиторий, всё, что вам нужно сделать - это смешать последние изменения из удалённого репозитория. Это значит, что вы можете выполнить rebase или merge на удалённом репозитории (например, o/main).

Rebasing делает дерево коммитов более чистым и читабельным, потому что всё представляется единой прямой линией.
Метод rebasing явно изменяет историю коммитов в дереве.
Например, коммит C1 может быть перебазирован после C3. Соответственно, в дереве работа над C1' будет отображаться как идущая после C3, хотя на самом деле она была выполнена раньше.

Некоторые разработчики любят сохранять историю и предпочитают слияние (merging). Другие (такие как я) предпочитают иметь чистое дерево коммитов, и пользуются перебазировкой (rebasing).

как git знает, что ветка main соответствует o/main. Конечно, эти ветки имеют схожие имена и связь между локальной и удалённой ветками main выглядит вполне логично, однако, эта связь наглядно продемонстрирована в двух сценариях:

Во время операции pull коммиты скачиваются в ветку o/main и затем соединяются в ветку main. Подразумеваемая цель слияния определяется исходя из этой связи.
Во время операции push наработки из ветки main закачиваются на удалённую ветку main (которая в локальном представлении выглядит как o/main). Пункт назначения операции push определяется исходя из связи между main и o/main.

связь между main и o/main объясняется не иначе как свойство "удалённое отслеживание" веток. Ветка main настроена так, чтобы следить за o/main -- это подразумевает наличие источника для merge и пункта назначения для push в контексте ветки main.

как это отслеживание появилось на ветке main, если мы не запускали ни одной специфической команды. На самом деле, когда вы клонируете репозиторий, это слежение включается автоматически.

В процессе клонирования git локально создаёт удалённые ветки для каждой ветки с удалённого репозитория (такие как o/main). Затем он - git - создаёт локальные ветки, которые отслеживают текущую, активную ветку на удалённом репозитории. В большинстве случаев - это main.

git clone завершит своё выполнение, у вас будет лишь одна локальная ветка (так что вы ещё не сильно перегружены), но, если вам будет интересно, вы сможете увидеть все удалённые ветки (при желании).

после клонирования вы видите в консоли надпись:

local branch "main" set to track remote branch "o/main" (локальная ветка "main" теперь следит за удалённой веткой "o/main")


сказать любой из веток, чтобы она отслеживала o/main, и если вы так сделаете, эта ветка будет иметь такой же пункт назначения для push и merge как и локальная ветка main. Это значит, что вы можете выполнить git push, находясь на ветке totallyNotMain, и все ваши наработки с ветки totallyNotMain будут закачены на ветку main удалённого репозитория!

выполнить checkout для новой ветки, указав удалённую ветку в качестве ссылки. Для этого необходимо выполнить команду

git checkout -b totallyNotMain o/main

создаст новую ветку с именем totallyNotMain и укажет ей следить за o/main.

checkout для новой ветки foo и укажем ей, чтобы она отслеживала main с удалённого репозитория.

git checkout -b foo o/main; git pull

o/main, чтобы обновить ветку foo. Обратите внимание, как обновился main!!

git checkout -b foo o/main; git commit; git push

Мы закачали наши наработки на ветку main нашего удалённого репозитория. При том, что наша локальная ветка называется абсолютно по-другому.

указать ветке отслеживать удалённую ветку — это просто использовать команду git branch -u. Выполнив команду

git branch -u o/main foo

вы укажете ветке foo следить за o/main. А если вы ещё при этом находитесь на ветке foo, то её можно не указывать:

git branch -u o/main

второй способ указать слежение за веткой намного быстрее...

git branch -u o/main foo; git commit; git push


как следить за удалёнными ветками, мы можем начать изучение того, что скрыто под занавесом работы команд git push, fetch и pull.

git находит удалённый репозиторий и ветку, в которую необходимо push-ить, благодаря свойствам текущей ветки, на которой мы находимся. поведение без аргументов, однако команда git push может быть также использована и с аргументами.

git push <удалённый_репозиторий> <целевая_ветка>

Что за такой параметр <целевая_ветка>?

git push origin main

Перейди в ветку с именем "main" в моём локальном репозитории, возьми все коммиты и затем перейди на ветку "main" на удалённом репозитории "origin.". На эту удалённую ветку скопируй все отсутствующие коммиты, которые есть у меня, и скажи, когда ты закончишь.

Указывая main в качестве аргумента "целевая_ветка", мы тем самым говорим git-у, откуда будут приходить и куда будут уходить наши коммиты. Аргумент "целевая_ветка" или "местонахождение" - это синхронизация между двумя репозиториями.

сказали git-у всё, что ему необходимо (указав оба аргумента), ему - git-у - абсолютно всё равно, что вы зачекаутили до этого!

git checkout c0; git push origin main

обновили main на удалённом репозитории, принудительно указав аргументы в push.

не указывали эти аргументы, при этом используя тот же алгоритм?

git checkout c0; git push

команда не выполнилась, так как HEAD потерялся и не находится на удалённо-отслеживаемой ветке.


в качестве аргумента ветку main для команды git push, мы указали совместно источник, откуда будут приходить коммиты, и пункт назначения (получатель), куда коммиты будут уходить.

мои источник и получатель коммитов были различными? Что, если мы хотим запушить коммиты из локальной ветки foo в ветку bar на удалённом репозитории?

разделить источник и получатель аргумента <пункт назначения>, соедините их вместе, используя двоеточие:

git push origin <источник>:<получатель>

Refspec — определения местоположения, которое git может распознать (например, ветка foo или просто HEAD~1)

указали источник и получатель независимо друг от друга, вы можете довольно причудливо и точно использовать команды для работы с удалёнными ветками и репозиториями. Давайте взглянем на демонстрацию!

источник -  местоположение, которое git должен понять:

git push origin foo^: main

git видит в foo^ не что иное, как местоположение, закачивает все коммиты, которые не присутствуют на удалённом репозитории, и затем обновляет получателя

пункт назначения, в который вы хотите запушить, не существует? Без проблем! Укажите имя ветки, и git сам создаст ветку на удалённом репозитории для вас.

git push origin main:newBranch


указали источник и получатель независимо друг от друга, вы можете довольно причудливо и точно использовать команды для работы с удалёнными ветками и репозиториями.



пункт назначения в команде git fetch, например так, как в следующем примере:

git fetch origin foo

Git отправится в ветку foo на удалённом репозитории, соберёт с собой все коммиты, которые не присутствуют локально, и затем поместит их в локальную ветку под названием o/foo.

Указывая пункт назначения...

git fetch origin foo

мы скачиваем только коммиты с ветки foo и помещаем их в o/foo.

зачем git поместил эти коммиты в ветку o/foo вместо того, чтобы разместить их в локальной ветке foo ? Ведь я думал о параметре <пункт назначения>, как о месте, ветке, которая существует в обоих - локальном и удалённом репозитории. Верно?

На самом деле, в данном случае git делает исключение, потому что вы, возможно, работаете над веткой foo, которую не хотите привести в беспорядок!! Об этом упоминалось в ранних уроках по git fetch - эта команда не обновляет ваши локальные 'не удалённые', она лишь скачивает коммиты (соответственно, вы можете инспектировать / объединять их позже).

"Что же тогда произойдёт, если я явно укажу оба параметра: и источник и получатель, пользуясь синтаксисом <источник>:<получатель> ?"

закачать коммиты прямиком в вашу локальную ветку, тогда да, вы можете явно указать источник и получатель через двоеточние. Вы можете воспользоваться таким приёмом лишь для ветки, на которой вы не находитесь в настоящий момент checkout.

<источник> - это место на удалённом репозитории, а <получатель> - место в локальном репозитории, в который следует помещать коммиты. Аргументы в точности до наоборот повторяют git push, и немудрено, ведь теперь мы переносим данные в обратном направлении!

разработчики редко используют такой подход на практике. Целью демонстрации этой возможности было показать, насколько схожи концептуально fetch и push. Их отличие лишь в направлении переноса данных.

git fetch origin C2:bar

git распознал C2 как место в origin и затем скачал эти коммиты в bar, которая является локальной веткой.

ветка-получатель не существует на момент запуска команды? Давайте ещё раз взглянем на предыдущий слайд, но на этот раз ветки bar ещё не существует.

git fetch origin C2:bar

поведение совсем такое же, как и у git push. Git создал ветку-получатель локально прежде чем скачивать данные. Всё как и в случае, когда git создаёт получателя в удалённом репозитории, когда мы закачиваем изменения (если получатель не существует).

Если команда git fetch выполняется без аргументов, она скачивает все-все коммиты с удалённого репозитория и помещает их в соответствующие удалённо-локальные ветки в локальном репозитории...

Странный <источник>
Git использует параметр <источник> странным образом. Странность заключается в том, что Вы можете оставить пустым параметр <источник> для команд git push и git fetch:

git push origin :side
git fetch origin :bugFix
Посмотрим, что же из этого выйдет...

с веткой, на которую мы делаем git push с пустым аргументом <источник>? Она будет удалена!

git push origin :foo

удалили ветку foo в удаленном репозитории, попытавшить протолкнуть(git push) в неё "ничего".

притянуть изменения(git fetch) из "ничего" к нам в локальный репозиторий, то это создаст у нас новую ветку

git fetch origin :bar

Аргументы для git pull не покажутся вам чем-то новым, учитывая, что вы уже знакомы с аргументами для git fetch и git push :)

Как мы помним, git pull сначала выполняет git fetch, а следом сразу git merge с той веткой, в которую притянулись обновления командой fetch. Другими словами, это все равно, что выполнить git fetch с теми же аргументами, которые вы указали для pull, а затем выполнить git merge с веткой, указанной в аргументе <приемник> команды pull.

git pull origin foo это то же самое, что сделать:

git fetch origin foo; git merge o/foo

git pull origin bar:bugFix то же, что:

git fetch origin bar:bugFix; git merge bugFix

Как видно, git pull используется, чтобы за одну команду выполнить fetch + merge.

выполнится fetch с аргументом указанным к pull, а merge выполняется с теми изменениями, которые будут скачаны командой fetch.

git pull origin main

указали main, поэтому как обычно все обновления притянулись на ветку o/main. Затем мы слили (merge) обновленную ветку o/main с веткой, на которой мы находимся.

указать <источник> и <приемник>? 

git pull origin main:foo

Мы создали новую ветку foo в локальном репозитории, скачали на неё изменения с ветки main удаленного репозитория, а затем слили эту ветку с веткой bar, на которой мы находились!

привести дерево к аналогичному в примере. Нужно скачать несколько изменений, создать несколько новых веток, слить одни ветки в другие, но постарайтесь использовать как можно меньше команд.



fetch получить информацию с удаленного репозитория, получить новые ветки, теги и уже после получить содержимое.

Если на удаленном репозитории появилась новая ветка, локально мы об этом не знаем. Для обновления информации о новых ветках, коммитах, тегах в локальном репозитории fetch.

неявно, если используется IDE или git клиент. Но попробуйте локально переключится на удаленную ветку - ничего не выйдет, пока не выполнится fetch.

состояние удаленного и локального репозитория.

с клонирования репозитория в удаленном репе одна ветка main. Локально еще не получили репозиторий.

remote local |-main

Клонируем репозиторий и локально получаем главную ветку по-умолчанию:

remote local |-main |-main

другой разработчик написал код в новой ветки:

remote local |-main |-main |-feat-1

Локально мы не знаем об этой ветки, это можно проверить выполнив команду просмотра локальных веток git branch и получить:

main
локально у нас одна ветка и мы находимся на ней, о чем говорит *. в удаленном репозитории две ветки. список удаленных веток командой git branch -r, получим:

origin/HEAD -> origin/main origin/main

получим только состояние репозиторий в момент клонирования, ведь это локальные данные. И если мы попробуем переключиться на ветку feat-1 - у нас ничего не выйдет git checkout feat-1:

error: pathspec 'feat-1' did not match any file(s) known to git

получим свежие данные из репозитория.

git fetch запросит с удаленного репозитория список изменений и запишет в локальный репозиторий. Но при этом ни происходит попыток слияния, только получение новых данных.

После выполнения команды git fetch получим вывод:

From gitlab.com:sendel/fetch-ex

[new branch] feat-1 -> origin/feat-1
получили информацию о новое ветку в удаленном репозитории. Сейчас состояние репозиториев выглядит так:

remote local |-main |-main |-feat-1 |- feat-1

Информация есть о удаленной ветки есть, но не зря она как приведение. Локально этой ветки еще не существует. Команда git branch -r выведет новую ветку, а список локальных git branch так и останется содержать одну main.

переключится на ветку feat-1, будет создана локальная ветка и она будет указывать на feat-1 ветку удаленного репозитория. Выполним команду перехода на ветку feat-1 git checkout feat-1:

Branch 'feat-1' set up to track remote branch 'feat-1' from 'origin'. Switched to a new branch 'feat-1' Теперь локально создалась ветка feat-1 и связалась с удаленной веткой feat-1.

remote local |-main |-main |-feat-1 |-feat-1

git branch -a, таким образом получим список всех веток, как локальных, так и удаленных:

feat-1 main remotes/origin/HEAD -> origin/main remotes/origin/feat-1 remotes/origin/main
git fetch неизбежно требуется для получения любых новых сведений из удаленного репозитория.

При выполнении git pull, вызывается fetch и далее уже полученные данные сливаются с выбранной веткой.

git pull -v (verbose) выводит информацию о ходе выполнения команды:

remote: Enumerating objects: 5, done. remote: Counting objects: 100% (5/5), done. remote: Compressing objects: 100% (2/2), done. remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 Unpacking objects: 100% (3/3), 287 bytes | 95.00 KiB/s, done. From gitlab.com:sendel/fetch-ex e033308..4bbebd7 feat-2 -> origin/feat-2 = [up to date] feat-1 -> origin/feat-1 = [up to date] main -> origin/main Updating e033308..4bbebd7 Fast-forward feat-2 | 2 +- 1 file changed, 1 insertion(+), 1 deletion(-)

Первой выполняется fetch, и далее уже слияние.
