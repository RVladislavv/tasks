## Описание существующего функционала

Cross-check (дальше - кросс-чек) - взаимопроверка студентами работ друг-друга.  
После дедлайна выполнения задания (как правило 23:59 UTC - 3 часа ночи по Минску) на следующее утро админ или автор таска заходит в rs app и вручную запускает кросс-чек, о чём пишет уведомление в анонсах.  
Продолжительность кросс-чека чаще всего три дня.  
Перед дедлайном кросс-чека админ или автор таска пишет об этом в анонсах и на следующее утро заходит в rs app и вручную закрывает кросс-чек.  
При этом студенты, которые отправили свою работы на проверку и проверили все присланные работы получают баллы за свою работу по результатам кросс-чека.  
Раньше баллы рассчитывались как среднее арифметичное по результатам всех оценок за кросс-чек, потом решили в случае, если работу проверили все четверо проверяющих, минимальный балл отбрасывать и выставлять среднее из трёх самых высоких оценок.

## Предлагаемый функционал

### 1. Автоматический старт и дедлайн кросс-чека

- В форму создания и редактирования задания, которое проверяется кросс-чеком, добавить ещё одно поле **Start Cross-check - End Cross-check** [скриншот](images/1.jpg)
- Дату и время в это поле подставлять автоматически после того, как указаны дата и время дедлайна таска  
  Старт кросс-чека немедленно после дедлайна таска, дедлайн кросс-чека через 3дня после старта
- Предусмотреть возможность изменить дату и время старта и дедлайна кросс-чека вручную в ходе создания или редактирования таска
- Дата и время дедлайна кросс-чека таска автоматически добавляются в расписание курса
- После старта кросс-чека происходит автоматическое распределение работ на проверку, как это сейчас происходит при клике по кнопке `Cross-check: Distribute`
- После дедлайна кросс-чека баллы за работы попадают в скор, как это сейчас происходит при клике по кнопке `Cross-check: Complete`
- После дедлайна кросс-чека нельзя сабмитить результаты проверки работ студентов (сейчас такая возможность есть, это похоже на баг)

## Доработка процесса отправки работы на проверку

- в форме отправки работы на проверку видны только первые несколько тасков [скриншот](images/2.jpg). Когда тасков становится много, последние скрываются и без прокрутки их не видно, а иногда и самой полосы прокрутки тоже не видно. Решением может быть или размер поля достаточный чтобы отобразить все таски, или размещение тасков снизу вверх, чтобы последние выданные таски были вверху списка. Таски, сабмит которых ещё доступен, должны выделяться стилем.
- в форму создания или редактирования таска, проверяемого кросс-чеком, добавить радиокнопку чтобы автор таска мог выбрать что должен студент сабмитить в этом таске. Всего должно быть три варианта: 1) сабмитится ссылка на pull request, 2) сабмитится ссылка на деплой, 3) не проверять сабмит. В соответствии с выбранной опцией, при сабмите студентом ссылки нужно выводить сообщение что должно сабмититься, и проверять ту ли ссылку сабмитит студент
- сделать возможным изменение засабмиченной ссылки после дедлайна таска, на случай, если оказалось что студент засабмитил нерабочую ссылку и хочет её поменять. При смене ссылки выводить сообщение, что изменение ссылки после дедлайна приводит к потере права на апелляцию. То, что ссылка на работу менялась после дедлайна должно быть видимым для студента и для админов

## Интеграция формы кросс-чека в rs app

Для удобства проверки студенты создали форму для кросс-чека [пример](https://momentum-crosscheck.netlify.app/), в которой можно отмечать выполненные и невыполненные пункты и которая сама считает сумму баллов за таск. Эту форму необходимо интегрировать в rs app, чтобы она находилась не где-то на стороннем ресурсе, а в самом приложении.

#### Доработка существующей формы кросс-чека

В целом существующая форма для проверки нас устраивает, но в неё необходимо внести несколько улучшений

- возможность указать количество баллов за пункт "выполнено частично". Сейчас за него считается половина баллов, что неоправданно много за небольшую неточность, которую и не заметить нельзя, и снять половину баллов много.  
Есть несколько вариантов как можно реализовать эту возможность.  
Один из возможных вариантов показан на [скриншоте](images/3.jpg): остаются пункты "выполнено полностью" (100% баллов за пункт требований), "не выполнено" (0 баллов), а в пункте "выполнено частично" находятся десять звёздочек, кликами по которым можно указать количество баллов за пункт требований.  
Другие варианты - `input[type="number"]` в котором можно изменить количество баллов как стрелочками сбоку, так и введя баллы вручную, или `input[type="range"]` в котором можно изменить количество баллов перетаскиванием ползунка
- нужно предусмотреть, что максимальное количество баллов за выполнение всех пунктов требований может быть больше, чем максимально возможное количество баллов за таск, которое идёт в score
- сохранение комментариев проверяющего без необходимости нажимать сочетание клавиш "Ctrl + Enter"
- радиокнопка "Отметить работу как лучшую" с пояснением какие работы считаются лучшими. Функционал кнопки рассматривается ниже

#### Как должна работать форма кросс-чека

Попытка интегрировать форму кросс-чека в rs app уже была, но отображалась форма только у автора работы, который мог с её помощью проверить свою собственную работу. Самопроверка работы по пунктам требований тоже нужна и полезна, но также необходимо, чтобы студенты проверяли присланные на проверку работы по этой же форме. 

У автора работы результат проверки его работы будет отображаться как перечень пунктов требований напротив каждого из которых отображается пять оценок - его собственная и четыре оценки проверяющих.

У проверяющего форма для кросс-чека будет отображаться как перечень пунктов требований, напротив каждого из которых выставлены баллы. После сабмита выставленного проверяющим балла у проверяющего отобразится также самооценка автора работы по пунктам требований.

После окончания (дедлайна) кросс-чека у проверяющего отобразятся оценки не только автора работы, но и всех других проверяющих.

#### Добавление формы кросс-чека в rs app

Сейчас добавление формы для кросс-чека происходит редактированием json-файла [пример](https://github.com/rolling-scopes-school/checklist/blob/master/active-tasks/rss-fancy-weather.json)  
Можно такой способ добавления формы в rs app оставить, он удобный и привычный.  
Если вдруг появится желание сделать веб-интерфейс для добавления формы для кросс-чека, нужно предусмотреть в нём возможность редактирования текущей формы, так как часто нужно поменять всего 1-2 пункта и преписывать ради этого всю форму не очень удобно.

## Обмен сообщениями между проверяемым и проверяющим в ходе кросс-чека

Об этом функционале студенты просят уже очень давно.  
Речь не о полноценном чате, не о мгновенном обмене сообщениями.  
Как сейчас проверяющий отправляет сообщение проверяемому с результатом проверки его задания и комментрием к нему, по той же схеме проверяемый может отправить сообщение проверяющему с просьбой перепроверить работу, обратить внимание на отдельный пункт требований к заданию.

Это могут быть как комментарии в свободной форме, так и сообщения из списка подготовленных фраз "Функционал в пунктах [здесь перечень пунктов] работает, пожалуйста, перепроверьте", "Я считаю, что баллы за пункты [здесь перечень пунктов] не соответствуют моей работе" и т.д).

Уведомления о сообщениях необходимо встроить в rs app, например, будет гореть кружочек возле cross-check review, а также причилать нотификацию в телеграм (о нотификациях будет отдельный пункт ниже)

Переписка между проверяющим и проверяемым должна быть анонимной, то есть студенты не должны видеть ники, гитхабы или другие контакты тех, с кем переписываются

Админы могут видеть переписку между студентами (об этом должно быть уведомление где-то на странице с сообщениями)

Должна храниться история сообщений, новые сообщения не должны затирать предыдущие.

Сейчас проверяющий, повторно отправляющий сообщение с результатом кросс-чека, видит оба отправленных сообщения, а проверяемый только последнее. Это похоже на баг.

## Лучшие работы

В интегрированную в rs app форму для кросс-чека нужно добавить радиокнопку "Отметить работу как лучшую" с пояснением что мы ожидаем от лучших работ.

Для каждого таска должна быть отдельная страница, куда попадают ссылки на работы, отмеченные студентами как лучшие. Доступ к странице с лучшими работами должен быть у админов и у автора таска.

Работы автоматически сортируются по количеству голосов.

Нужна возможность редактирования этой страницы - добавления и удаления работ вручную.

Нужна возможность экспортировать работы в формат `csv`.

Нужна возможность выделить все работы или снять выделение со всех работ, или выделить только некоторые работы, или снять выделение только с некоторых работ.

Для отмеченных работ нужна возможность отправить благодарность в rs app за лучшую работу.

Также эти работы потом будут попадать на страницу с лучшими работами, когда она появился в rs app.

## Благодарности и жалобы

У студента, работу которого проверили, должна быть возможность поблагодарить проверяющего за отличный кросс-чек или пожаловаться за недобросовестный кросс-чек.

Так как кросс-чек анонимный, все благодарности и жалобы приходят со стандартным текстом и сразу в rs app, минуя дискорд

Нужно продумать как сделать чтобы благодарили не за баллы, а за полезные советы и комментарии. Чтобы жаловались не на баллы, которые не нравятся, а на необоснованно сниженные баллы.

По результатам проведённых кросс-чеков можно формировать рейтинг проверяющих.

Жалобы можно рассматривать в ходе апелляции и по результатам рассмотрения отправлять предупреждение или проверяющему за необъективную проверку, или автору работы за необоснованную жалобу

## Нотификация в телеграм

Нужно для каждой нотификации придумать стандартный текст с необходимыми пояснениями и ссылками.

#### Перечень предлагаемых уведомлений:
- выдача таска (в rs app появилась возможность сабмита задания)
- три дня до дедлайна, а работа ещё незасабмичена
- сутки до дедлайна, а работа ещё незасабмичена
- работа была успешно засабмичена и после старта кросс-чека будет отправлена на проверку
- старт кросс-чека, вам нужно проверить работы
- сутки до дедлайна кросс-чека, а присланные на проверку работы ещё не проверены
- все присланные на проверку работы проверены в ходе кросс-чека
- вам прислали сообщение по процесу кросс-чека
- вас поблагодарили за кросс-чек
- на вас пожаловались за кросс-чек
- вашу работу проверили
- в ходе кросс-чека вашу работу отметили как лучшую 

## Аналитика

Раз у нас есть баллы за таск автора работы и четырёх проверяющих, можем установить процент расхождения баллов

Расхождение в пределах 10% считаем полным совпадением и такой кросс-чек считаем идеальным

Если чьи-то баллы отличаются больше чем на 15-20%, эти баллы не учитываем при выставлении балла за таск, автору проверки отправляем предупреждение о необоснованном завышении/занижении баллов. Речь в том числе и про автора работы, если таким студентом окажется он сам

Самые большие отклонения от среднего проверяем в ходе апелляции, по результатам которой отправляются предупреждения студентам, чьи баллы отличаются от результата апелляции больше чем на 15-20%, в том числе и автору работы, если таким студентом окажется он сам

## Изменение существующего функционала

Если предложенные правки будут реализованы можно будет отказаться от
- отбрасывания минимального балла в случае четырёх проверяющих, вместо него отбрасывается балл, который максимально не совпадает с баллами других проверяющих
- радиокнопку "Отображать моё имя" кросс-чек будет полностью анонимным, каким он и задумывался
- продлить продолжительность кросс-чека на день для возможности корректировки результатов в процессе обмена сообщениями. Проверять работы в последний день уже нельзя, но можно корректировать выставленные баллы