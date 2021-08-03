# test-case
Для реализации тестового задания был выбран Google Colab (Блокноты Colab – это блокноты Jupyter, которые размещены в сервисе Colab).
Пройдя авторизацию начинаем выполнение примеров тестового задания:
Первым делом необходимо выяснить какое среднее количество транзакций совершали пользователи из имеющегося датасета:
# 1 What is the average number of transactions per purchaser?
%%bigquery --project vigilant-result-285814 
SELECT AVG(totals.transactions)
FROM bigquery-public-data.google_analytics_sample.ga_sessions_*
В качестве результата получаем значение равное 1.048736149584488, что говорит нам о том, что пользователи в среднем совершают лишь одну транзакцию в исследуемом магазине.
Далее посчитаем сколько в среднем транзаций было совершенно в апреле 2017 года:
# 2 Calculate the average number of transactions per purchaser for April 2017
%%bigquery --project vigilant-result-285814
SELECT AVG(totals.transactions)
FROM bigquery-public-data.google_analytics_sample.ga_sessions_*
WHERE
_TABLE_SUFFIX BETWEEN '20170401' AND '20170431'
Результат также приблежен к 1 транзакции - 1.033405172413793
Для того, чтобы понять с какого дбраузера больше всего было совершено транзакций, продюсируем следующий запрос:
# 3 What is the total number of transactions generated per device browser?
%%bigquery --project vigilant-result-285814
SELECT device.browser, 
SUM(totals.transactions) as total_transactions
FROM bigquery-public-data.google_analytics_sample.ga_sessions_* 
GROUP BY device.browser
<img width="721" alt="Снимок экрана 2021-08-03 в 19 13 22" src="https://user-images.githubusercontent.com/88386830/128049913-3e360f68-48b4-4e6c-b22a-5a3492cbf7f2.png">
<img width="686" alt="Снимок экрана 2021-08-03 в 19 14 40" src="https://user-images.githubusercontent.com/88386830/128050179-6ee9541b-1755-4799-82c3-b6cc32af5ca2.png">
<img width="679" alt="Снимок экрана 2021-08-03 в 19 14 55" src="https://user-images.githubusercontent.com/88386830/128050140-9315df71-3c1f-4749-b433-cdb1a8a1929d.png">
Топ 5 браузеров, согласно проведенной аналитике являются:
<img width="682" alt="Снимок экрана 2021-08-03 в 19 16 49" src="https://user-images.githubusercontent.com/88386830/128050410-107b07a8-b4f1-4adf-b6b9-44bcf7c13d30.png">
Причем, согласно диаграмме, что представлена ниже, Ghrome занимает около 88% всех транзакций:
![Unknown-2](https://user-images.githubusercontent.com/88386830/128050974-15560aa2-d667-42fb-a9de-41a162879896.png)
Если говорить об общем времени проведенном юзерами на анализируемом сайте, то имеем показатель равен 118672851 временных единиц.
А вот в среднем каждый пользователь проводит около 262.6121413428814 временных единиц.
Чтобы понять с какого континента наши самые активные юзеры выполняем следующий запрос:
# 7
%%bigquery --project vigilant-result-285814 df1
SELECT geoNetwork.continent,
SUM(totals.timeOnSite) as timeOnSite
FROM bigquery-public-data.google_analytics_sample.ga_sessions_*
GROUP BY geoNetwork.continent
Имеем результат:
<img width="694" alt="Снимок экрана 2021-08-03 в 19 26 28" src="https://user-images.githubusercontent.com/88386830/128051754-d2a978cd-641d-4e9b-9e2f-43098111fe29.png">
Чтобы визуально было удобнее понимать какой континент лидирует, загоняем все в "пандовский" фреймворк и выводим столбчастую гистограмму:
![Unknown-3](https://user-images.githubusercontent.com/88386830/128051873-f4d81982-cc67-4053-85b8-faaf3b9d7a20.png)
Лидером является Америка 🙂
Для подробного понимания пос транам выводим таблицу ТОП-10: 
<img width="683" alt="Снимок экрана 2021-08-03 в 19 28 28" src="https://user-images.githubusercontent.com/88386830/128052378-4b915cd2-b956-4429-af2a-0a3a1f967a4b.png">

CONCLUSIONS:
Проведя быструю EDA можно говорить о том, что магазин не пользуется особым спросом, так как на одного пользователя приходиться лишь одна транзакция, таким образом необходимо провести маркетинговую работу направленную на увеличении конверсии сайта ( целевых действий пользователей, то есть совершение успешных транзакций).

Однако, анализ показал, что в среднем на сайте за весь период времени пользорователи проводят около 262 часов, что равняется 11 дням в год, в сравнительной характеристики это приличный показатель.
Стоит отметить также тот факт, что в большинстве случаев для просмотра сайта и совершения транзакций пользователи выбирают Google Chrome, при этом большинство из них находится в Америке, а именно в США.
