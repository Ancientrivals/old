---
layout: ru_post
title: Интерактивный прототип интерфейсов
---

Проектируя интерфейсы, гейм-дизайнеры часто останавливаются на этапе статичных картинок. Всё лучше, чем ничего, но можно пойти на шаг дальше и извлечь из макетов в разы больше пользы.
Достичь этого можно посредством интерактивных макетов, на которых есть “кликабельные” кнопки, имитирующие реально работающий ффункционал. Для этой цели в “Ancient Rivals” мы использовали сервис [Invision App](http://www.invisionapp.com/).

Посмотрите на интерактивный [прототип](http://invis.io/EM32MDNW7), который был сделан для нашего проекта. Почему был выбран именно этот сервис? Мы изучили множество инструментов, но они не подходили: то слишком сложные в освоении, то нельзя тестировать на мобильных устройствах, то нет пробной версии, то невыносимо высокая цена… Короче, всё непросто. И только Invision смог удовлетворить наши запросы.

Но давайте сначала поговорим о том, зачем вообще напрягаться и делать интерактивный макет вместо набора статичных картинок. Вот несколько причин, которые приходят на ум:
- Когда дело доходит до графики, всегда проще показать, чем рассказать. Проверено неоднократно.
- Красивая презентация, подходящая и для самих разработчиков, и для людей от бизнеса.
- Делая статичный макет, вы можете упустить что-то важное. В динамике же все ошибки проектирования полезут на свет.
- Flow. Вы можете ясно видеть, как будет работать ваш интерфейс. В играх интерфейсы нелинейны, имеют много развилок, необычных путей, циклов. Визуализировать всё это будет крайне полезно.
- Юзабилити. Создав прототип, вы можете открыть его на вашем мобильном устройстве и посмотреть, как нажимаются кнопки, адекватно ли их расположение, подходит под палец размер? Статичные макеты не дают и доли той обратной связи, что вы получаете даже от простейшего интерактивного макета.

Теперь же вкратце расскажем, как делали прототип в Invision App. Прежде всего, нужно сделать сами макеты в графическом редакторе, например, в Photoshop, после чего экспортировать в виде отдельных картинок. Представьте, что у вас есть карта мира, на которой присутствует кнопка, вызывающая всплывающее окошко. Вам понадобятся две картинки: просто карта мира и карта мира с висящим над ней всплывающим окошком. В интерактивном прототипе по нажатию той кнопки картинка карты мира заменится картинкой карты мира со всплывающим окошком. По такому же принципу придётся делать все новые окошки и игровые экраны: по одной картинке для каждого.

Сервис читает все основные графические форматы, но я рекомендую заранее подготовить компрессированные JPG, чтобы ваш прототип не занимал много места и грузился молниеносно. Не верьте смешным картинкам из интернета, JPG выглядит отлично, а весит несравнимо меньше, чем PNG.

![Alt text](http://i.imgur.com/rM05Jhc.png)

Когда вы зарегистрируетесь в сервисе, прежде всего вас попросят создать проект, выбрав целевую платформу, например, iPhone, Android, Web.

![Alt text](http://i.imgur.com/yZ6MkFV.png)

После этого вы загружаете свои файлы, и оказываетесь в разделе со списоком ваших картинок. Нажмите на ту, которая станет отправной точкой для вашего интерактивного макета.

В нижней части экрана первым делом зайдите в настройки (иконка шестерёнки), там вы можете указать метод отображения вашего прототипа. Например, Android выглядит так:

![Alt text](http://i.imgur.com/4m6Ghte.png)

Заметьте, что там есть возможность выбрать симулируемое устройство. В данном случае включён Nexus 7. А если зайти в настройки устройства Apple, то там доступен широкий модельный ряд:

![Alt text](http://i.imgur.com/0hg7ZsG.png)

В итоге ваш макет будет выглядеть примерно так:

![Alt text](http://i.imgur.com/th7sfip.png)

Теперь вернёмся к интерактивности. В нижней части экрана есть несколько режимов работы: просмотр, редактирование и комментирование (другие люди смогут оставлять комментарии к вашей работе, если вы включите эту опцию).
В режиме редактирования вы можете делать вот такие синие зоны:

![Alt text](http://i.imgur.com/TtuKql2.png)

Эти зоны отображают точки, на которые можно нажимать. При создании новой точки вам даётся возможность выбрать, какая картинка будет загружаться при клике на точку. Таким образом вы делаете на каждом экране несколько точек, ведущих на другие экраны.

Чтобы поделиться с кем-то интерактивным прототипом, нажмите на кнопку Share в нижнем правом углу экрана. Там будет несколько опций, подбирайте удобную для вас. Что интересно, вы можете запускать интерактивный прототип на вашем мобильном устройстве. Очень удобная функция для тестирования юзабилити на тач экране. Только имейте ввиду, что потребуется установить небольшое бесплатное приложение для просмотра прототипов.

Напоследок немного о денежном вопросе. В бесплатной версии Invision App вы можете создать только 1 проект, но с неограниченным количеством экранов, что уже хорошо, потому что большинство сервисов дают возможность создать только 1 проект всего с двумя-тремя экранами, что не лезет вообще ни в какие ворота.
Чтобы иметь возможность создавать 3 проекта, придётся платить 15 долларов в месяц. Неограниченное количество проектов обойдётся в 25 долларов в месяц. Это достаточно дорого, если вы делаете небольшие прототипы время от времени. Но если вы зарабатываете созданием интерфейсов, веб-дизайном или часто работаете с юзабилити - оно себя окупит.