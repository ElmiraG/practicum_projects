# Анализ сервиса аренды самокатов GoFast
## Описание проекта
Анализ сервиса аренды самокатов GoFast. В проекте данные о некоторых пользователях из нескольких городов, а также об их поездках. Необходимо проанализировать данные и проверить гипотезы.

Чтобы совершать поездки по городу, пользователи сервиса GoFast пользуются мобильным приложением. Сервисом можно пользоваться:
- **без подписки**
    - абонентская плата отсутствует;
    - стоимость одной минуты поездки — 8 рублей;
    - стоимость старта (начала поездки) — 50 рублей;
- **с подпиской Ultra**
    - абонентская плата — 199 рублей в месяц;
    - стоимость одной минуты поездки — 6 рублей;
    - стоимость старта — бесплатно.

## Описание данных
В основных данных есть информация о пользователях, их поездках и подписках.
- **Пользователи** — users_go.csv:
    - **user_id** — уникальный идентификатор пользователя;
    - **name** — имя пользователя;
    - **age** — возраст;
    - **city** — город;
    - **subscription_type** — тип подписки (free, ultra).

- **Поездки** — rides_go.csv:
    - **user_id** — уникальный идентификатор пользователя;
    - **distance** — расстояние, которое пользователь проехал в текущей сессии (в метрах);
    - **duration** — продолжительность сессии (в минутах) — время с того момента, как пользователь нажал кнопку «Начать   поездку» до момента, как он нажал кнопку «Завершить поездку»;
    - **date** — дата совершения поездки.

- **Подписки** — subscriptions_go.csv:
    - **subscription_type** — тип подписки;
    - **minute_price** — стоимость одной минуты поездки по данной подписке;
    - **start_ride_price** — стоимость начала поездки;
    - **subscription_fee** — стоимость ежемесячного платежа.

## Вывод

**1. Предобработка данных:**
   
- В датафрейме rides столбец date преобразован в формат datetime, добавлен столбец с номером месяца.
- В датафрейме users удалены дублирующие строки.

**2. Выводы по исследовательскому анализу данных:**
   
3.1) Пользователи сервиса из восьми городов: Пятигорск, Екатеринбург, Ростов-на-Дону, Краснодар, Сочи, Омск, Тюмень, Москва. Частота встречаемости городов варьируется от 11% до 14.3%. Топ-3 города: Пятигорск (14,3%), Екатеринбург (13,3%), Ростов-на-Дону (12,9%). Москва - самый реже встречаемый (11%).

3.2) 54,4% пользователей без подписки, 45,6% с подпиской.

3.3) Возраст пользователей от 12 до 43 лет, средний возраст - 25 лет. Возрастные категории: 18-25 лет - 51,0%, 25-30 лет - 32,5%, 30-35 лет - 10,6%, до 18 лет - 5,1%, от 35 лет - 0,8%.

3.4) Средняя дистанция поездок - 3,07 км. Короткие дистанции (до 1,5 км) - ≈700 метров, длительные (от 1,5 км до 7 км) - ≈3,2 км. 90% пользователей предпочитают длительные дистанции.

3.5) Средняя продолжительность поездок - 17,7 минут. Процент ошибочно завершенных поездок у пользователей без подписки (до 0,5 мин) - 0,5%.

**4. Объединены данные о пользователях, поездках и подписках в один датафрейм.**

Созданы отдельные датафреймы для пользователей с подпиской и без. Визуализированы расстояние и время поездок для обеих категорий. Средняя продолжительность поездок пользователей с подпиской на 0,05% больше, чем у пользователей без подписки.

**5. Создан датафрейм datagain с агрегированными данными о поездках и помесячной выручкой от каждого пользователя.**

Продолжительность каждой поездки округляется до следующего целого числа.

**6. Проверка гипотез:**

6.1) Есть основания считать, что пользователи с подпиской тратят больше времени на поездки.

6.2) Есть основания говорить, что помесячная выручка от пользователей с подпиской выше, чем от пользователей без подписки. Анализ показывает, что продолжительность поездок с подпиской на 0,05% больше, а средняя месячная выручка для пользователей без подписки на 0,09% выше. Разница составляет лишь десятую долю процента.
  
6.3) Есть основания говорить, что расстояние, преодолеваемое пользователями с подпиской, оптимально с точки зрения износа.