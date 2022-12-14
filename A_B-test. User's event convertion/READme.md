## Описание проекта

Мы работаем с данными о поведении пользователей при А/В-тесте на сайте. Информация хранится в разных таблицах, которые необходимо сопоставить между собой. Цель теста — увеличить конверсии пользователей не менее чем на 10% по каждому событию после внедрения новой платежной системы. 

**Наши задачи:**
- оценить корректность проведенного теста по техническому заданию;
- проанализировать полученные результаты.

**Данные, которые у нас есть:**
1. *календарь маркетинговых событий* (название события, регионы, где была кампания, даты начала и конца);
2. *данные обо всех пользователях, которые зарегистрировались на сайте с 7 декабря по 4 января* (идентификатор, дата регистрации, регион пользователя, устройство для входа);
3. *все события новых пользователей в период с 7 декабря по 4 января* (идентификатор, совершенные действия на сайте, дата и время действия, дополнительные данные о действии);
4. *таблица участников теста* (идентификатор, название теста и группа пользователя)

## Ход выполнения проекта

1. **Исследование каждого датасета и подготовка данных:** изменение типов данных, поиск пропущенных значений и дубликатов; <br>
2. **Изучение данных на предмет соответствия ТЗ:** соединение таблиц, фильтрация по нужным параметрам и "сверка" данных по пунктами ТЗ; <br>
3. **Исследовательский анализ данных:** оценка распределений количества событий на одного пользователя и количества событий в дни А/В-теста;<br>
4. **Анализ воронки событий:** подготовка данных для построения воронки и визуализации результатов конверсии пользователей по событиям по группам теста;
4. **Проверка статистических гипотез:** использование z-теста и корректировка уровня значимости для сравнения для долей пользователей, совершавших действия на сайте.<br>

## Используемые библиотеки
- Для работы с массивами данных и обработки значений: *pandas*, *numpy*, *re*, *datetime*;<br>
- Для визаулизации данных: *matplotlib.pyplot*, *seaborn*, *plotly.graph_objects*;<br>
- Для проверки статистических гипотез: *scipy.stats*, *math*.<br>

```
! pip install pandas
! pip install numpy
! pip install re
! pip install datetime
! pip install matplotlib.pyplot
! pip install seaborn
! pip install plotly.graph_objects
! pip install scipy.stats
! pip install math
```

## Результаты

1. Мы исследовали данные об А/В-тесте, проходившем с 7 декабря по 4 января (24 дня). В ходе оценки корректности данных составленному ТЗ были обнаружены следующие особенности:<br>
- частично тест совпал с промо-кампанией, которая затронула последние дни теста;<br>
- кол-во новых пользователей из Европы не соответствует требованиям;<br>
- группы теста не равны. Пользователей из группы А значительно больше, чем из группы В;<br>
- дополнительно хотелось бы выделить формунировку цели теста, так как не до конца понятно: исследовался новый рекомендательный механизм или новая платежная воронка.<br>
<br>
2. При исследовательском анализе данных было замечено, что в тесте присутствуют пользователи, которые не совершали никаких действий на сайте (около половины от теста). Вероятно, это связно с тем, что они не входили в систему и поэтому не получалось смотреть за их действиями (ведь раз они есть в базе, значит, они регистрировались). Учитывая эту особенность мы посмотрели на распределение ненулевых значений по пользователям, совершивших хотя бы одно дейсвтие. В обоих группах были выбросы (более 16 событий). После их отсечения мы убедились, что в среднем пользователи из группы А совершали на 1 действие больше. При этом характер распределения данных похож, если делать скидку на то, что наблюдений за пользователями из группы А больше;<br>
<br>
3. Исследование распределения событий по датам показывает, что <b>наибольший разрыв в событиях между группами происходит с 14 по 21 декабря</b>. Вероятно, это период, когда пользователи активизировались с покупками к праздникм (Рождество и Новый год в Европе). Это может объяснить также и дальнейшее падение активности. При этом если у группы А был резкий "скачок", то для группы В график событий более "плавный", без резких "скачков". <b>Предположим, что это может быть связано с неравномерным распределением пользователей по группам</b>;<br>
<br>
4. <b>Результаты анализа воронки:</b><br>
- Путь пользователя должен был быть «логин — просмотр продукта — просмотр корзины — покупка». Однако у пользователей группы А наблюдался переход от просмотра продуктка сразу к покупке, без просмотра корзины; <br>
- Больше всего потерь пользователей происходит в обеих группах на этапе перехода от просмотра карточки товара к просмотру корзины, так как многие просто пропускают этот этап переходя к покупкам;<br>
- При этом у пользователей из группы В кол-во пользователей посмотревшиз корзину и совершивших покупку почти не отличается. В этой группе, вероятно, тестирование новой воронки дало свои улучшения на этом этапе. Однако по всем остальным показателям, пользователи группы В совершали меньше действий, поэтому назвать тест успешным не получается;<br>
- Для более корректного анализа результатов необходимо сделать выборки пользователей равными по размерам.<br>
<br>
5. <b>Результат z-теста для проверки статистической значимой разницы:</b><br>
Сравнение экспериментальной группы с контрольной показывает, что у первой есть статистически значимая разницы в поведении пользователей. Это говорит о том, что изменение воронки поменяло поведение людей и, судя по показателям воронки, в худшую сторону. <b>Значит, эксперимент нельзя считать удачным</b>
"""
