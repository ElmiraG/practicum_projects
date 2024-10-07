# Прогнозирование оттока клиентов в сети отелей «Как в гостях»
**Описание проекта**
Заказчик этого исследования — сеть отелей «Как в гостях». 


Чтобы привлечь клиентов, эта сеть отелей добавила на свой сайт возможность забронировать номер без предоплаты. Однако если клиент отменял бронирование, то компания терпела убытки. Сотрудники отеля могли, например, закупить продукты к приезду гостя или просто не успеть найти другого клиента.
Чтобы решить эту проблему, вам нужно разработать систему, которая предсказывает отказ от брони. Если модель покажет, что бронь будет отменена, то клиенту предлагается внести депозит. Размер депозита — 80% от стоимости номера за одни сутки и затрат на разовую уборку. Деньги будут списаны со счёта клиента, если он всё же отменит бронь.
**Бизнес-метрика и другие данные:**

Основная бизнес-метрика для сети отелей — прибыль. Прибыль отеля — это разница между стоимостью номера за все ночи и затраты на обслуживание: как при подготовке номера, так и при проживании постояльца. 


В отеле есть несколько типов номеров. В зависимости от типа номера назначается стоимость за одну ночь. Есть также затраты на уборку. Если клиент снял номер надолго, то убираются каждые два дня.

Стоимость номеров отеля:
 - категория A: за ночь — 1 000, разовое обслуживание — 400;
 - категория B: за ночь — 800, разовое обслуживание — 350;
 - категория C: за ночь — 600, разовое обслуживание — 350;
 - категория D: за ночь — 550, разовое обслуживание — 150;
 - категория E: за ночь — 500, разовое обслуживание — 150;
 - категория F: за ночь — 450, разовое обслуживание — 150;
 - категория G: за ночь — 350, разовое обслуживание — 150.

В ценовой политике отеля используются сезонные коэффициенты: весной и осенью цены повышаются на 20%, летом — на 40%.
Убытки отеля в случае отмены брони номера — это стоимость одной уборки и одной ночи с учётом сезонного коэффициента.
На разработку системы прогнозирования заложен бюджет — 400 000. При этом необходимо учесть, что внедрение модели должно окупиться за тестовый период. Затраты на разработку должны быть меньше той выручки, которую система принесёт компании.

# План по выполнению проекта
1. Загрузка данных. 
2. Предобработка и исследовательский анализ данных.
3. Вычислите бизнес-метрику. Оцените прибыль отеля без внедрения депозитов.
4. Разработка модели ML.
   - Обучить разные модели и оцените их качество кросс-валидацией. Выбрать лучшую модель и проверить её на тестовой выборке. 
   - Выберать метрику для обучения.
   - Оценить прибыль, которую принесёт выбранная модель за год.
5. Выявить признаки «ненадёжного» клиента.
   - На основе исследовательского анализа данных описать клиента, склонного к отказу от брони.
6. Написать общий вывод.

# Описание данных
**Пути к файлам:**
- /datasets/hotel_train.csv — данные для обучения модели.
- /datasets/hotel_test.csv — данные для тестирования модели.

**Описание данных:**

В таблицах hotel_train и hotel_test содержатся одинаковые столбцы:
- **id** — номер записи;
- **adults** — количество взрослых постояльцев; 
- **arrival_date_year** — год заезда;
- **arrival_date_month** — месяц заезда;
- **arrival_date_week_number** — неделя заезда;
- **arrival_date_day_of_month** — день заезда;
- **babies** — количество младенцев;
- **booking_changes** — количество изменений параметров заказа;
- **children** — количество детей от 3 до 14 лет;
- **country** — гражданство постояльца;
- **customer_type** — тип заказчика:
  - **Contract** — договор с юридическим лицом;
  - **Group** — групповой заезд;
  - **Transient** — не связано с договором или групповым заездом;
  - **Transient-party** — не связано с договором или групповым заездом, но связано с бронированием типа Transient.
- **days_in_waiting_list** — сколько дней заказ ожидал подтверждения;
- **distribution_channel** — канал дистрибуции заказа;
- **is_canceled** — отмена заказа;
- **is_repeated_guest** — признак того, что гость бронирует номер второй раз;
- **lead_time** — количество дней между датой бронирования и датой прибытия;
- **meal** — опции заказа:
  - **SC** — нет дополнительных опций;
  - **BB** — включён завтрак;
  - **FB** — включён завтрак, обед и ужин.
  - **HB** — включён завтрак и обед;
- **previous_bookings_not_canceled** — количество подтверждённых заказов у клиента;
- **previous_cancellations** — количество отменённых заказов у клиента;
- **required_car_parking_spaces** — необходимость места для автомобиля;
- **reserved_room_type** — тип забронированной комнаты;
- **stays_in_weekend_nights** — количество ночей в выходные дни;
- **stays_in_week_nights** — количество ночей в будние дни;
- **total_nights** — общее количество ночей;
- **total_of_special_requests** — количество специальных отметок.

## Общий вывод
1.
- Пропущенных значений в данных нет. Дубликатов в таблиц нет. 
- В столбцах 'is_canceled' и 'is_repeated_guest' изменен тип данных на bool. 
- В столбцах  'adults', 'children', 'babies' тип данных выбран неверно: вместо float изменен на integer.
- Размеры таблиц 'hotel_train' и 'hotel_test' делятся в соотношении примерно 2:1

- В двух таблицах есть бронирования без взрослых, некоторые из них даже с детьми. Причем, не все из этих бронирований были отменены. Так что эти строки были удалены.
 
- При удалении id появилось много дублирующихся столбцов, удалены.
- Удалены из 'hotel_train' значения в столбце babies с количеством детей 9 и 10, так как их всего по одному и это напоминает аномалию, возможно это приведет к тому, что модель машинного обучения будет некорректно предсказывать отказ от брони. Вариантов с детьми больше двух тоже очень мало, так что удалены и они. Признак переименован в has_babies, чтобы обозначить его теперь уже бинарную природу. Аналогичным образом обработан и признак с количеством парковочных мест
- по той же причине удалены строки в столбце required_car_parking_spaces со значением больше 2. 
- В стобце meal есть неявный дубликат в опции SC, из-за лишних пробелов в данном столбце, аналогичные лишние пробелы есть и в столбце reserved_room_type. 
- Типы данных выбранных столбцов успешно изменены. 
- Судя по описанию, должна быть связь между тремя признаками: previous_cancellations, is_repeated_guest и previous_bookings_not_canceled: если клиент бронирует номер не в первый раз, то у него должны быть либо отмененные заказы, либо подтвержденные. Эти условия не соблюдаются, поэтому добавлены вместо них два новых бинарных признака: has_cancellations и has_successful_bookings.

- Использовалось OHE кодирование категориальных признаков и StandardScaler для численных признаков для будущих моделей.

- Целевой признак несбалансирован. В таблице hotel_train процент отказа от брони составляет 25.4%, в hotel_test - 31.5%. В дальнейшем будет применена балансировка классов. 

- В основном, бронирование номеров происходит летом, дальше идут весенние и осенние месяцы, традиционный спад в январе из-за новогодних праздников. Ценовая политика отеля с использованием сезонных коэффициенты (весной и осенью цены повышаются на 20%, летом — на 40%) - рациональна.

2. Создана таблица с ключевыми бизнес-метриками, разработана функция для расчета прибыли отеля
- Прибыль с января по август 2017 г. без внедрения депозита составила 32.6 млн.руб.

3. Для проекта выбраны три модели машинного обучения:

- DecisionTreeClassifier
- RandomForestClassifier
- LogisticRegression

Также использованы две техникии балансировки классов undersampling и oversampling:

- Недостаточная выборка с использованием Tomek links (undersampling)
- SMOTETomek - Комбинация over- and under-sampling.

Лучшая модель - RandomForestClassifier с гиперпараметрами:

- random_state=12345,
- n_estimators = 251,
- max_depth = 20,
- class_weight = 'balanced_subsample'

- С использованием балансировки классов SMOTETomek from imblearn.over_sampling

- Истинно отрицательные ответы TN = 0.56 (клиент остался), истинно положительные ответы (клиент ушел) TР = 0.17.
- Ложно отрицательных предсказаний больше ложно положительных: 0.15 и 0.12, соответственно.

Модель разделяет классы, но класс клиент ушел определяет гораздо хуже, чем клиент остался.

- Кривая ROC расположена выше кривой случайных ответов. Значение AUC = 0.78

- f1 = 0.56
- accuracy = 0.73
- precision = 0.59
- recall = 0.53

При использование GridSearchCV можно будет подобрать более подходящие гиперпараметры и улучшить характеристикии модели

**Прибыль после обучения моделисоставила 36.77 млн.руб. Прибыль от использования модели увеличится на 4.47 млн.руб за 8 месяцев**

4. Ненадежный клиент: 

- Бронирует номер сильно заранее
- Не оставляет специальных отметок
- Не резервирует парковочные места
- Чем дольше планирует остаться в отеле, тем больше вероятность, что он откажется от брони
- Часто отказывался от брони раньше
