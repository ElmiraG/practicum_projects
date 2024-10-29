# Модель предсказания оттока клиентов телекоммуникационной компании
Оператор связи «ТелеДом» хочет бороться с оттоком клиентов. Для этого сотрудники компании начнут предлагать промокоды и специальные условия всем, кто планирует отказаться от услуг связи. Чтобы заранее находить таких пользователей, «ТелеДому» нужна модель, которая будет предсказывать, разорвёт ли абонент договор. Команда оператора собрала персональные данные о некоторых клиентах, информацию об их тарифах и услугах. Задача заключается в обучении модели на этих данных для прогноза оттока клиентов.

## Описание данных

Данные хранятся в Sqlite  — СУБД, в которой база данных представлена одним файлом. Она состоит из нескольких таблиц:

- **contract** — информация о договорах;
- **personal** — персональные данные клиентов;
- **internet** — информация об интернет-услугах;
- **phone** — информация об услугах телефонии.

## План по выполнению проекта
1. Загрузка данных
2. Исследовательский и предобработка данных
4. Обучение моделей
5. Тестирование модели
6. Общий вывод

## Вывод

**Лучашя модель CatBoostClassifier с параметрами:**

- random_state=16924,
- verbose=0,
- depth = 5,
- iterations = 550,
- learning_rate = 0.1,
- scale_pos_weight = 1

**Анализ результатов тестирования модели**

- Модель демонстрирует высокую точность в классификации истинно отрицательных ответов (0.83), однако имеет низкую точность в классификации истинно положительных ответов (0.094). Ложноположительные предсказания (0.014) меньше, чем ложноотрицательные (0.062), что указывает на то, что модель чаще ошибается, пропуская истинные положительные случаи.

- Кривая ROC расположена выше кривой случайных ответов. Значение AUC = 0.93.

- Точность составляет 0.87, что означает, что когда модель предсказывает положительный класс, она правильно делает это примерно в 87% случаев. Полнота равна 0.60, что указывает на то, что модель успешно идентифицирует примерно 60% всех реальных положительных случаев в данных. Accuracy: 0.92.

**Анализ важности признаков**

Модель посчитала важными такие признаки как:

- продолжительность договора
- ежемесячная оплата
- тип оплаты: раз в два года
- наличие у клиента супруг(а)
- тип оплаты: раз в год