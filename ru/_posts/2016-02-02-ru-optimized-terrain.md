---
layout: ru_post
title: Оптимизированный ландшафт
---

В этой объёмной статье речь пойдёт о создании ландшафта для мобильных игр на Unity. Встроенный в движок стандартный ландшафт слишком тяжёлый и использовать его не представляется возможным, поэтому нам пришлось искать альтернативные решения. В нашей игре "Ancient Rivals" мы хотели добавить больше открытых пространств, а не ограничиваться классическими подземельями, поэтому вопрос производительности ландшафта стоял остро.

Лучшее, что мы нашли, это смешивание текстур ландшафта по RGB маске, называемой сплатмапом. Сплатмап являет из себя текстуру из, в данном случае, трёх цветов - красный, зелёный, синий. В Unity затем каждому цвету присваивается своя текстура, скажем, красный заменяется тайлящейся травой. Сам [шейдер](https://www.assetstore.unity3d.com/en/#!/content/17334) мы купили на Asset Store, это оказался единственный ассет, выдающий приемлимый FPS на всех наших мобильных девайсах. Итак, предположим, что мы хотим сделать вот такой ландшафт:

![Alt text](http://i.imgur.com/p19G8A4.jpg)

Прежде всего нам нужно создать ландшафт в самом Unity стандартными средствами. На его основе будем делать нашу оптимизированную версию. Этот ход позволит добиться правильной формы, масштаба и расположения ландшафта на сцене. Точность проработки формы зависит от того, насколько детальным вы хотите получить ландшафт в конечном итоге. Сразу замечу, что чем больше деталей вы хотите сохранить, тем сложнее будет работа. Для этого урока я ограничусь вот таким простеньким холмистым коридорчиком:

![Alt text](http://i.imgur.com/B6mlSY8.jpg)

Следующим шагом будет экспорт меша в ваш любимый 3d-редактор, это может быть хоть Maya, хоть Blender. Я использую Maya LT, имеющую определённые ограничения на полигонаж мешей, который мне придётся обходить, но для экономии места в статье я опущу эти шаги. Для экспорта ландшафта в OBJ файл вы можете использовать скрипты, свободно доступные на Unity wiki. На картинке ниже - импортированная в 3d-редактор модель, даже в таком размере вы можете видеть, насколько она высокополигональная.

![Alt text](http://i.imgur.com/OkgpLsw.jpg)

Зная ракурс игровой камеры, продумайте, какие области ландшафта точно не будут видны и удалите их. Как правило ландшафт на игровой карте занимает много места, и если вы пользуетесь запечённым, а не реалтаймовым освещением, удаление лишних полигонов может избавить вас от нескольких лишних лайтмап-атласов.

Как вы можете видеть, сетка ландшафта неоптимальна. Она имеет одинаковую плотность и на плоских участках, и на высокодетализированных холмах. Есть два метода решения этой проблемы. Первый - ленивый, быстрый, дающий удовлетворительный результат, вписывающийся во все технические гайдлайны. Второй же элегантный, но до неприличия медленный; на выходе позволяет получить идеальный меш.

Давайте сначала остановимся у ленивого метода. Здесь нам потребуется Zbrush, импортируем модель в него:

![Alt text](http://i.imgur.com/HxgPYXf.jpg)

У меня этой программы нет, поэтому я попросил помощи друга 3d-художника. Если у вас такой возможности нет, можно либо воспользоваться триальной версией Zbrush, либо посмотреть, какие возможности автоматической ретопологии предоставляют бесплатные пакеты типа Blender.
В Zbrush мы прогнали меш через Decimation Master, получив нужный полигонаж со страшной, но удовлетворяющей техническим требованиям сеткой. Суть работы этого инструмента в том, что он автоматически определяет, в каких местах меша нужно больше полигонов, а в каких - меньше.

![Alt text](http://i.imgur.com/sMIbN5S.jpg)

Если отключить отображение сетки, то в целом меш выглядит приемлимо и вписывается в наши технические требования:

![Alt text](http://i.imgur.com/GKCbWKc.jpg)

Альтернатива этому подходу - ручная ретопология, позволяющая добиться отличной сетки, но на ландшафт для одной миссии могут уйти дни труда. Вот пример небольшого вручную сделанного кусочка с использованием Quad Draw в Maya LT.

![Alt text](http://i.imgur.com/asL8a4y.jpg)

Один из способов улучшить получившуюся у нас страшную сетку - это инструмент Relax, в том или ином виде доступный во всех пакетах трёхмерного моделирования и / или скульптинга. Во время применения Relax стремится сгладить полигоны так, чтобы все они имели примерно одинаковый размер, таким образом мы избавляемся от чрезмерно вытянутых, слишком маленьких и непозволительно больших полигонов там, где не хотим их видеть.

Сравним меш до Relax'а:

![Alt text](http://i.imgur.com/clIjLvy.jpg)

И после (ракурс не совпадает, но вы сразу заметите, насколько равномернее стало расположение полигонов):

![Alt text](http://i.imgur.com/rBNGoG2.jpg)

Дальше я предпочитаю делить огромный меш, что мы сейчас имеем, на отдельные кусочки. Мой ландшафт небольшой, поэтому двух хватит. В других случаях у вас могут получиться десятки отдельных моделей. Это помогает оптимизировать игру, так как мы не будем единовременно держать в памяти весь здоровый ландшафт, а только его части. Но есть ещё немаловажная причина: такое разделение позволит моделям поместиться  в лайтмап атласы и адекватно там упаковаться. Разрезая меш, старайтесь прятать швы UV-развёртки там, где игрок их точно не заметит, хотя это и не всегда возможно. На скриншоте не заметно, но я провёл разрез модели в лощинах между холмиками, тем самым скрывая шов. К слову, делая развёртку, хорошим правилом будет давать больше текстурного пространства тем местам, которые чаще на виду, и меньше тем, которые в отдалении. Я использую текстуру Checkerboard, с которой отлично видна плотность развёртки. Чем крупнее квадратики, тем меньше текстурного пространства отдаётся этим полигонам.

![Alt text](http://i.imgur.com/cGPQXmE.jpg)

Пришло время экспортировать каждый кусочек ландшафта в отдельный OBJ файл. Не ленитесь давать им понятные названия - ещё не раз окажется полезным. В Maya LT есть ограничения на экспорт больших мешей, но функции Export to Unity и Export to Unreal выручают.

Следующим шагом будет создание маски смешивания. Суть в следующем: шейдер смотрит на цвет текстуры-маски, где видит красный - кладёт поверх ландшафта одну текстуру, например, траву; где видит синий  - камни, зелёный - опавшие листья.

Рисовать маску будем необычным и странным способом! Открываем Photoshop, импортируем первый кусочек ландшафта (да-да, только что подготовленный нами OBJ), и переходим в режим 3d-рисования. Рисование прямо поверх модели удобно в первую очередь тем, что вы наглядно видите, где конкретно проводите штрих, в отличие от рисования по полу-абстрактной двумерной текстурной развёртке.

![Alt text](http://i.imgur.com/HmhP0pI.jpg)

Каждую маску сохраняем отдельным файлом, не забывая подбирать подходящее название, чтобы не запутаться, какая маска какому кусочку ландшафта принадлежит. На моём скриншоте маска чёрно-белая, потому что мы пошли на шаг дальше в плане оптимизации, и смешиваем только две текстуры.

![Alt text](http://i.imgur.com/EjjUnBd.jpg)

Закончив работу над масками, экспортируйте их в PNG. В Unity при импорте поставите компрессию пожестче, чтобы файл весил максимально мало. Точность маски не важна, особенно в нашем случае, когда мы занимаемся жесткой оптимизацией под мобильные устройства. Дальше всё просто, берёте купленный шейдер, про который я говорил в начале статьи, выбираете в нём тип материала - Mobile Fastest. В слот Splatmap вставляете свою маску, в слоты Detail Map - текстуры травы, камней и проч. Частота повторяемости этих текстур будет зависеть от параметра Tiling.

В результате получаем вот такой вот ландшафт, отлично работающий на мобильных устройствах:

![Alt text](http://i.imgur.com/d319z1y.jpg)

![Alt text](http://i.imgur.com/DUbeRSl.jpg)

Нашему ландшафту понадобится коллайдер. По сути я дублирую наши модели, очень сильно уменьшаю их полигонаж, удаляю лишние полигоны, но добавляю вертикальные стены, чтобы блокировать герою путь туда, куда ходить ему не следует. Затем создаю общий Game Object, в котором будут сидеть все наши кусочки ландшафта и все Mesh Colliders. Коллайдеры я предпочитаю разбивать на несколько маленьких кусочков, чтобы столкновения считались с маленькими моделями, а не с одной гигантской.

![Alt text](http://i.imgur.com/7AVpVkQ.jpg)

![Alt text](http://i.imgur.com/CJ2A3ac.jpg)

Это был длинный рассказ. Создание оптимизированного мобильного ландшафта - долгая муторная процедура, но оно того стоит. Наличие земли на ваших игровых уровнях создаёт множество классных творческих возможностей, так что за работу!

P.S. Когда будете удалять исходный Unity ландшафт, в корневой папке проекта не забудьте удалить автоматически созданный им ассет.