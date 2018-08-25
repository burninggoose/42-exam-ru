# Вступление

Экзаменационная система дает вам задания по одному и проверяет их по вашему требованию.

Экзамен - набор уровней, каждый уровень имеет набор разных заданий, которые выбираются случайно. У вашего соседа, скорее всего, будут другие задания. Жизнь несправедлива, смиритесь.

За каждый уровень вы будете получать очки, максимум - 100.

## Перед экзаменом

Перед началом экзамена вам нужно зарегистрироваться (или попробовать еще раз) как минимум на один из них в Интранете. Вы можете подписаться на столько, сколько хотите, потому что никаких ограничений нет.

Как только вы подписались как минимум на один экзаменационный проект, вы сможете получить доступ к нему через `examshell`, но только в режиме практики.

Если вы хотите сдать экзамен по-настоящему, вам нужно подписаться на экзаменационную сессию, прийти на нее ВОВРЕМЯ и залогиниться как "exam" в экзаменационном кластере. Только после этого вы сможете сдать экзамен по-настоящему.

## Режимы экзамена

Есть два режима экзамена :
* Режим практики, доступ к которому вы сможете получить из собственной сессии, откуда угодно, когда угодно. В режиме практики все будет работать как надо, но результаты не будут учтены нигде. Вы можете практиковаться так часто, как захотите.
* Реальном режим, доступ к которому открывается только с "exam" сессии, в экзаменационном кластере, во время экзаменационного ивента. В этом режиме ваша оценка БУДЕТ учтена в Интранете, и как только вы закончите, ваша оценка останется неизменной.

## Examshell

Вы будете проходить экзамен в специальной оболочке, используя программу под названием `examshell`.

Вам нужно запустить ее самостоятельно в терминале не позже 10 минут после начала экзамена. Если вы случайно остановите ее, не пугайтесь, вы можете запустить ее заново использя команду `examshell` в терминале. Весь процесс востановится с момента, где вы закончили. 

# Короткая версия

1. Вы открываете examshell
2. Examshell показывает вам разные экзамены, к которым вы имеете доступ
3. Вы выбираете экзамен и запускаете уровень
4. Система создает Git репозиторий для вас и клонирует его в BASEDIR/rendu
5. Система выбирает случайное задание для вас из текущего уровня
6. Examshell загружает описание задания в BASEDIR/subjects/ASSIGNMENT_NAME/
7. Examshell говорит, какое задание вам нужно сделать, сколько вы должны получить поинтов, чтобы выиграть и как это все упаковать
8. Вы работаете над заданием
9. Вы "пушите" решение в Git репозиторий как обычно
10. Когда вы уверены, что закончили: вы просите examhell оценить вас, используя команду `grademe`
11. Система оценивает ваше решение
12. Examshell сохраняет процесс проверки в BASEDIR/traces/ASSIGNMENT_TRACE_DIR
13. Если у вас получилось, вы получите данные и идете на уровень выше. Если на экзамене есть еще уровни, перейдите на шаг 5, иначе экзамен закончен
14. Если вы провалились, вы остаетесь на том же уровне, теряете 5 потенциальных поинтов и возвращаетесь на 5. Если задаиний больше нет, экзамен окончен.

Не поняли? Прочитайте длинную версию!

# Длинная версия

Мы сделаем пример для этой документации, запустив все в режиме практики. В реальном режиме вы будете работать в `~/`, но в режиме практики мы будем работать в `~/exam-basedir/`.

## Шаг 1: запускаем examshell

```
zaz@blackjack ~ $ examshell
examshell v0.1.1

WARNING
Your exam files will be stored in ~/exam-basedir
THIS DIRECTORY WILL BE ENTIRELY EMPTIED BEFORE YOU START
So, if you do have important things there, Ctrl-C NOW and back them up before running this.
(Press Enter to continue...)
```

Это предупреждение абсолютно нормально. Оно сделано, чтобы убедиться, что у вас еще не директории `~/exam-basedir/` с вещами, которые вам нужны, потому что examshell полностью сотрет их. Это для того чтобы у нас была "пустая" директория, в которой мы будем работать.

Если бы мы делали экзамен в реальном режиме, предупреждения не было бы.

## Шаг 2: Выбор экзамена

После логина испльзуйте examshell для выбора нужного экзамена и уровня, с которого хотите начать. Вы можете можете начать только с того уровня, до которого уже дошли ранее.

```
[...]
Getting current exam session from server...

The following projects are available to you at the moment:

  0 : Exam C
      Maximum start level    : 4 (Best grade you got is 75)
      Real mode available    : no (You must subscribe to an exam event)
      Practice mode available: yes (Ends in 4 hours, 0 minutes and 0 seconds)

There is only one project you can do right now.
Automatically selecting project 0: "Exam C"

This project can only be done in practice mode
In 'practice' mode, your grade will not be taken into account, and you can only access your repository from your regular (non-exam) session

Please select the level at which you would like to start your session
Be aware that if you select a level higher than 0, you will only gain however many points the previous levels would have given you IF you complete your selected starting level !

Desired start level (0-4):
```

Если доступно больше одного экзамена, очевидно, вы выбираете нужный вам.

Для примера мы начнем на уровне 0. Если программа не спрашивает, с кого уровня начинать, не паникуйте, это просто означает, что все уровни кроме 0 для вас закрыты.

```
[...]
Desired start level (0-4): 0

You are about to start the project "Exam C", in practice mode, at level 0.
You would have 4 hours, 0 minutes and 0 seconds to complete this project
Confirm ? [y/n]
```

Если по какой-то причине вы не уверены, что хотите сделать, просто введите `n` и вы вернетесь на этап выбора.

Учтите, что если вам нужно будет перезапустить exmashell по какой-либо причине после выбора экзамена, он не будет справшивать вам выбрать еще раз. Мы перейдем к шагу 3.


## Шаг 3: Подготовка рабочей директории

После подтверждения examshell подготовит рабочую директорию (`~/exam-basedir/` в режиме практики и `~/` в реальном режиме), клонируйте нужный Git репозиторий и т.д.

```
[...]
Confirm ? [y/n] y
Selecting project...
Creating required directories...
Ensuring your Git repository for this exam is present and correct...
Git repository is not cloned yet. Cloning...
Cloning into '/Users/zaz/exam-basedir/rendu'...
vogsphere: (INFO) Transaction ID : 8ed938b3-fe1e-4eb6-b561-0f6622e12b82
vogsphere: (INFO) Please mention this ID in any ticket you create concerning this transaction
vogsphere: (INFO) This transaction has been started at 2015-05-28 11:07:49, server time.
vogsphere: (INFO) Rights will be determined using this time, so do NOT cut the connection.
vogsphere: (INFO) It appears you are mmontinet. If that's not true, check your Kerberos tickets (klist)
vogsphere: (INFO) You have read and write rights on this repository
warning: You appear to have cloned an empty repository.

Your git repository was successfully cloned to ~/exam-basedir/rendu

The following commands are available to you:
  status: Displays the status of your session, including information about
    your current assignment, and the exam history.
  grademe: Asks the server to grade your current assignment. If you
    have done it right, you will gain the points of the current assignment, go
    up a level, and try the next one. If you fail, however, you will have
    another assignment of the same level to do, and it will potentially bring
    you less points on your grade ... So be sure of yourself before you launch
    this command !
  finish: Tells the server you are finished with your exam.

You can log out at any time. If this program tells you you earned points,
then they will be counted whatever happens.

(Press Enter to continue...)
```

## Шаг 4: Получение задания

Examshell теперь скачает ваше задание из системы.

Он автоматически сохранит все материалы в директорию `subjects`. Вы можете открыть ее из терминала.

```
[...]
================================================================================
You are currently at level 0
You are running in practice mode (Your grade does not count)
Your current grade is 0/100
Assignments:
  Level 0:
    0: max for 16 potential points (Current)

Your current assignment is max for 16 potential points
It is assignment 0 for level 0
The subject is located at: ~/exam-basedir/subjects/max
You must turn in your files in a subdirectory of your Git repository with the
same name as the assignment (~/exam-basedir/rendu/max).
Of course, you must still push...

The end date for this exam is: 28/05/2015 15:07:47
You have 3 hours, 54 minutes and 11 seconds remaining
================================================================================
You can now work on your assignment. When you are sure you're done with it,
push it to vogsphere, and then use the "grademe" command to be graded.
examshell>
```

Эта информация всегда доступна через команду `status`.

## Шаг 5: Работа над заданием

Теперь, ну, вы работаете над заданием.

Заметьте, что вы ДОЛЖНЫ перейти в директорию, которую вам показал examshell, которая является поддерикторией Git репозитория с таким же именем, как и задание. Никаких знаков, ничего. Если вы используете неверную директорию, вы провалите задание, и это никак не отменить. Это очень плохо.

В нашем примере мы положим наш файл `max.c` в `~/exam-basedir/rendu/max/`.

Вы ДОЛЖНЫ запушить всю работу после окончания, как и обычный проект, так что не забудьте!

## Шаг 6: Запрос проверки

Как только вы УВЕРЕНЫ, что ваша работа сделана верно, что вы запушили все на vogsphere, вы можете использовать команду `grademe` чтобы запросить проверку вашего задания:

```
examshell> grademe

Before continuing, please make ABSOLUTELY SURE that you have pushed your files,
that they are in the right directory, that you didn't forget anything, etc...
If your assignment is wrong, you will have another assignment at the same level,
but with less potential points to earn!

Are you sure? [y/N]
```

So, yeah, let's say we're sure.

```
[...]

Are you sure? [y/N] y
OK, making grading request to server now.

We will now wait for the job to complete.
Please be patient, this CAN take several minutes...
(10 seconds is fast, 30 seconds is expected, 3 minutes is a maximum)
waiting...
```

Теперь мы ждем, пока система проверит нас. Это и правда МОЖЕТ занять какое-то время, но это ожидаемо, так что не паникуйте. Если это занимает больше 1-2 минут, позовите админа, это легкий фикс.

### Успех!

```
[...]
waiting...
>>>>>>>>>> SUCCESS <<<<<<<<<<
You have successfully completed the assignment and earned 16 points!
Trace saved to ~/exam-basedir/traces/0-0_max.trace

(Press Enter to continue...)
```

In this case, we succeeded. The grading trace is automatically saved for us to read if we want.

The next status message will show that:

* We earned the points in play for the assignment (16)
* We have gone up a level (now at 1)
* We have a new assignment

```
[...]
(Press Enter to continue...)

================================================================================
You are currently at level 1
You are running in practice mode (Your grade does not count)
Your current grade is 16/100
Assignments:
  Level 0:
    0: max for 16 potential points (Success)
  Level 1:
    0: wdmatch for 16 potential points (Current)

Your current assignment is wdmatch for 16 potential points
It is assignment 0 for level 1
The subject is located at: ~/exam-basedir/subjects/wdmatch
You must turn in your files in a subdirectory of your Git repository with the
same name as the assignment (~/exam-basedir/rendu/wdmatch).
Of course, you must still push...

The end date for this exam is: 28/05/2015 15:07:47
You have 3 hours, 42 minutes and 27 seconds remaining
================================================================================
You can now work on your assignment. When you are sure you're done with it,
push it to vogsphere, and then use the "grademe" command to be graded.
examshell>
```

Если это был последний уровень экзамена, examshell сообщит об этом и зкажет, что экзамен окончен.

### Провал :(

Давайте провалим задание, чтобы посмотреть что случится:

```
[...]
examshell> grademe

Before continuing, please make ABSOLUTELY SURE that you have pushed your files,
that they are in the right directory, that you didn't forget anything, etc...
If your assignment is wrong, you will have another assignment at the same level,
but with less potential points to earn!

Are you sure? [y/N] y
OK, making grading request to server now.

We will now wait for the job to complete.
Please be patient, this CAN take several minutes...
(10 seconds is fast, 30 seconds is expected, 3 minutes is a maximum)
waiting...
>>>>>>>>>> FAILURE <<<<<<<<<<
You have failed the assignment.
Trace saved to ~/exam-basedir/traces/1-0_wdmatch.trace

(Press Enter to continue...)

================================================================================
You are currently at level 1
You are running in practice mode (Your grade does not count)
Your current grade is 16/100
Assignments:
  Level 0:
    0: max for 16 potential points (Success)
  Level 1:
    0: wdmatch for 16 potential points (Failure)
    1: ft_strrev for 11 potential points (Current)

Your current assignment is ft_strrev for 11 potential points
It is assignment 1 for level 1
The subject is located at: ~/exam-basedir/subjects/ft_strrev
You must turn in your files in a subdirectory of your Git repository with the
same name as the assignment (~/exam-basedir/rendu/ft_strrev).
Of course, you must still push...

The end date for this exam is: 28/05/2015 15:07:47
You have 3 hours, 41 minutes and 25 seconds remaining
================================================================================
You can now work on your assignment. When you are sure you're done with it,
push it to vogsphere, and then use the "grademe" command to be graded.
examshell>
```

В случае провала:

* Мы НЕ получаем поинты
* Мы НЕ переходим на следующий уровень
* Мы получаем новое задание, но без 5 возможных поинтов за завершение

Если это было последнее возможное задание на этом уровне, examshell сообщит нам об этом, а так же о том, что экзамен завершен.

### Ошибка :<

Есть вариант, что examshell сообщит нам, что проверка закончилась ERROR. Это не значит, что вы провалились, это значит, что система сама крашнулась.

Тем не менее, не паникуйте !

У вас будет возможность попробовать еще раз или отменить проверку. Вы ДОЛЖНЫ попробовать еще хотя бы раз, потому что ошибки в системе оценивания редки и часто временны, так что они решаются еще одной попыткой. Если это не помогло, позовите админа на помощь!

Если все выше не получается, вы можете отменить проерку. Система даст вам новое задание того же уровня, но без потери потенциальных поинтов.

## Шаг 7: Повторяем еще раз

Ну, это все. Вы будете получать новые задания до тех пор, пока не провелите их все, или пока не дойдете до последнего уровня экзамена.

Если вы хотите, вы можете закончить сессию в любое время, использовав команду `finish`.

```
examshell> finish
Please confirm that you REALLY want to end your current session.
If you do, you will not be able to do anything with it anymore!
Are you finished? [y/N] y
Your session has been marked as finished. You may now log out.
zaz@blackjack ~ $
```

# FAQ / Частые ошибки

## Я остановил examshell, что мне делать?

Просто запустите его заново.

## Examshell говорит мне "login window expired", что делать ?

У вас есть 10 минут на выбор экзамена после момента старта экзамена, в ином случае вы получете эту ошибку, потому что просто поздно.
Нет, вы ничего с этим не поделаете.

## Examshell говорит мне "Mismatched versions", что делать ?

Машина, за которой вы сидите, скорее всего просто не обновлена. Выберите другую или зайдите в Bocal и попросите обновить ее.

## Почему я не могу использовать реальный режим // режим практики ?

Examshell говорит вам почему, вот возможные причины:

* "Can't practice when logged in as 'exam'"
	* Вам нужно зайти под своей сессией для прохождения практики
* "Project doesn't allow it"
	* Это означает, что в экзамене нет такого режима, вы ничего не можете с этим поделать
* "You must subscribe to an exam event"
	* Как сказано во вступлении, вы должны зарегистрироваться на ивент экзамена. Это можно сделать в календаре на главной странице.
* "You must retry the project on the intranet"
	* Вы не можете использовать реальный режим, если не попробовали его еще раз в интранете, потому что система не сможет дать вам оценку за это.
* "Current event doesn't allow for this exam"
	* Некоторые экзамены закрыты для определенных сессий. Самое заметное во время "бассейна". Вы сможете пройти экзамен только залогинившись под определенной сессией
* "Can't do it from this location"
	* Большинство экзаменов привязаны к определенным местам, чтобы экзамен проходил в определенных условиях. Вам нужно залогиниться в одном их этих мест. Режим практики доступен откуда угодно.
* "Too early, well be OK in XXX" // "Too late, ended at XXX"
	* Означает, что ближайший экзамен, на который во подписаны, еще не начался или уже закончился.
* "Must login as 'exam' to run in real mode"
	* Говорит само за себя. Просто зайдите под логином "exam" и паролем "exam"

## У меня задания не такие же как у моего соседа !

Да. Очень плохо. Смирись.

## Могу я получить репозиторий после экзамена ? А задания ?

Обычно, вы получаете email со всем этим после завершения экзамена.

Если нет, ... ну, ждите. Если вы и правда не получили их, то плохо, жизнь несправедлива, я думаю.

## Examshell говорит мне, что я провалился, но я уверен, что это не так !

Скорее всего вы забили что-то из этого :

* Вы запушили ?
* Точно ?
* Вы использовали правильную директорию ?
* Все ваши файлы с правильными названиями ?
* Вы делали правильное задание ?
* Вы ТОЧНО запушили ?
* и т.д.

Если вы точно, точно, ТОЧНО, АБСОЛЮТНО уверены, что вы все сделали правильно, и что система завалила вас, ну, подойдите к админам ПОСЛЕ ЭКЗАМЕНА. Мы все проверим, но пожалуйста, пожалуйста, пожалуйста подумайте прежде чем подходить к нам: Более вероятно, что это ваша ошшибка, и у нас уйдет очень много времени, что проверить ошибку в задании.

## Ваша система отстой, я хочу старую !

Нет, она не отстой, и нет, вы не можете все вернуть.

## Я нашел баг в системе !

Правда ?

Ну, если это так, то скажи нам. Мы не будем злиться, если ты конечно не использовал его в своих целях.

## Я нашел баг в системе и использовал его, вы никогда не поймаете меня!

Нет, мы узнаем. Как-нибудь! И мы надеемся, вам понравится искать новую школу :)

Серьезно, не будьте идиотом, просто сообщите об этом и двигайтесь дальше, не стоит рисковать исключением ради пары баллов на экзамене.

## Examshell говорит, что я ждал слишком много для выбора экзамена ?

Ничего страшного, просто попробуйте еще раз. Это правда не ошибка, это просто examshell говорит вам, что вы очень нерешительный.

## Я отправил запрос на оценку, но он занимает очень много времени 

Ну, она когда-то завршится ? Если проверка не закончилась через минуту или две, просто позовите дамина, потому что это и правда не нормально, но достаточно просто починить.

## Почему режим практики закрыт для прохождения ?

Потому что я так сказал

## У меня есть еще вопрос, которого тут нет. FAQ бесполезен !

Круто. Просто спроси у нас, и мы добавим его сюда, мы не можем думать обо всем сразу без использования веществ.