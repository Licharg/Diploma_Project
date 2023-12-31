# дипломный проект

⏳ **Прогноз расхода энергии по временным рядам**

  В этом репозитории используются данные о потреблении энергии с сайта региональной организации США *PJM*. С их помощью можно построить модель временных рядов, чтобы прогнозировать энергетический расход. Кроме того, эти данные пригодятся, чтобы выявить тенденции расходов по времени суток, праздникам и более длительным срокам.

Данные взяты [отсюда](https://www.kaggle.com/datasets/robikscube/hourly-energy-consumption).

📂 СОДЕРЖАНИЕ<a id="0"></a>

- [0. Содержание](#0)
- [1. Краткий обзор](#1)
- [2. Библиотеки и модули](#2)
- [3. EDA](#3)
  - [3.1 Обработка и очистка данных](#3.1)
  - [3.2 Визуализация очищенных исходных данных](#3.2)
  - [3.3 Графический анализ энергопотребления](#3.3)
  - [3.4 Графический анализ энергопотребления с учётом времени года](#3.4)
  - [3.5 Анализ на гистограммах по временам года](#3.5)
- [4. Статистический анализ данных](#4)
  - [4.1 Сезонная декомпозиция](#4.1)
  - [4.2 Стационарность и гетероскедастичность](#4.2)
  - [4.3 Автокорреляция и частичная автокорреляция](#4.3)
- [5. Моделирование временных рядов](#5)
  - [5.1 Подготовка данных к моделированию](#5.1)
  - [5.2 Модель Baseline - наивный прогноз](#5.2)
  - [5.3 Модель HWES - тройное экспонециальное сглаживание Холта-Винтера](#5.3)
  - [5.4 Модель XGBoost](#5.4)
  - [5.5 Модель Lasso - L1-регрессия](#5.5)
  - [5.6 Модель SARIMA](#5.6)
  - [5.7 Модель Prophet](#5.7)
- [6. Выводы](#6)

📂 КРАТКИЙ ОБЗОР<a id="1"></a>

  *PJM Interconnection LLC (PJM)* - региональная организация передачи данных *(RTO)* в Соединенных Штатах. Это часть сети Восточного межсоединения, управляющая системой передачи электроэнергии, обслуживающей весь или части Делавэра, Иллинойса, Индианы, Кентукки, Мэриленда, Мичигана, Нью-Джерси, Северной Каролины, Огайо, Пенсильвании, Теннесси, Вирджинии, Западной Вирджинии и округа Колумбия.
Данные о почасовом потреблении энергии берутся с веб-сайта *PJM* и выражаются в мегаваттах (МВт*ч).
Регионы менялись с годами, поэтому данные могут отображаться только для определенных дат в каждом регионе.

📂 БИБЛИОТЕКИ И МОДУЛИ<a id="2"></a>

  В ноутбуке в основном использованы следующие библиотеки и модули:

- Numpy
- Pandas
- Matplotlib
- Seaborn
- Statsmodels
- Sklearn
- Xgboost
- Prophet

📂 EDA<a id="3"></a>

📐 Обработка и очистка данных<a id="3.1"></a>

  Читаем данные, выбираем признаки для дальнейшей работы, смотрим статистические данные, определяем и избавляемся от дубликатов и пропусков.

📐 Визуализация очищенных исходных данных<a id="3.2"></a>

  Визуализируем очищенные данные в ежечасовом, ежедневном и еженедельном формате. Делаем предварительные выводы и гипотезы.

![Рис_01](./Images/image_01.png "Потребление энергии PJM")

📐 Графический анализ энергопотребления<a id="3.3"></a>

  Готовим данные и выполняем визуализацию в разрезе: час, день недели, день месяца, неделя года, месяц, квартал, год.

📐 Графический анализ энергопотребления с учётом времени года<a id="3.4"></a>

  Готовим данные и выполняем визуализацию в разрезе: час, день недели, день месяца, неделя года, месяц, квартал, год, но с учётом времени года (зима, весна, лето, осень).

📐 Анализ на гистограммах по временам года<a id="3.5"></a>

  Готовим данные и выполняем визуализацию на гистограммах в разрезе: времени года. Делаем предварительные выводы и гипотезы по выполненным ранее визуализациям.

📂 СТАТИСТИЧЕСКИЙ АНАЛИЗ ДАННЫХ<a id="4"></a>

📐 Сезонная декомпозиция<a id="4.1"></a>

  Раскладываем данные на компоненты:

- Уровень;
- Тенденция;
- Сезонность;
- Шум.

  Визуализируем результаты и делаем предварительные выводы.

📐 Стационарность и гетероскедастичность<a id="4.2"></a>

  Проверяем данные на стационарность (Тест Дики-Фуллера) и гетероскедастичность (гомоскедастичность). Делаем предварительные выводы, предварительно определяем параметры: d, D.

📐 Автокорреляция и частичная автокорреляция<a id="4.3"></a>

  Строим графики ACF и PACF. Предварительно определяем параметры: p, q, P, Q, m. Определяем скользящее среднее значение и стандартное отклонение, визуализируем результаты.

![Рис_14](./Images/image_14.png "Потребление энергии, скользящее среднее и стандартное отклонение")

📂 МОДЕЛИРОВАНИЕ ВРЕМЕННЫХ РЯДОВ<a id="5"></a>

📐 Подготовка данных к моделированию<a id="5.1"></a>

  Определяемся с перечнем моделей, которые будем использовать для моделирования временных рядов. Делим данные на тренировочную и тестовую выборки.

📐 Модель Baseline - наивный прогноз (AR)<a id="5.2"></a>

  В качествет Baseline (наивного прогноза) применяем авторегрессионную модель (AR) — т.е. модель временных рядов, которая описывает, как прошлые значения временного ряда влияют на его текущее значение. Таким образом, авторегрессия представляет собой линейную регрессию на себя.

📐 Модель HWES - тройное экспонециальное сглаживание Холта-Винтера<a id="5.3"></a>

  Модель прогноза Хольта Винтерса — это 3-х параметрическая модель прогноза, которая учитывает:

- Сглаженный экспоненциальный ряд;
- Тренд;
- Сезонность.

📐 Модель XGBoost<a id="5.4"></a>

  XGBoost - это оптимизированная распределенная библиотека градиентного бустинга, предназначенная для эффективного и масштабируемого обучения моделей машинного обучения.

📐 Модель LASSO - L1-регрессия<a id="5.5"></a>

  Регрессия Лассо основана на линейной регрессионной модели, но дополнительно выполняет так называемую L1 регуляризационную процедуру, представляющую собой процесс введения дополнительной информации с целью предотвращения переобучения.

📐 Модель SARIMA<a id="5.6"></a>

  Модель сезонного авторегрессионного скользящего среднего с семью структурными параметрами: (p, d, q)(P ,D ,Q, m).

📐 Модель Prophet<a id="5.7"></a>

  Prophet — это библиотека с открытым исходным кодом от компании Facebook. Она предназначена для прогнозирования временных рядов. По словам разработчиков (команды Core Data Science team) данный инструмент хорошо работает с рядами, которые имеют ярко выраженные сезонные эффекты, а также имеют несколько таких периодов. Prophet устойчив к отсутствию данных и достаточно хорошо справляется с выбросами.

🔍 ВЫВОДЫ<a id="6"></a>

  Результаты расчётов представим в виде таблице. Используемые метрики были:

- Средняя абсолютная погрешность (MAE)
- Средняя абсолютная процентная ошибка (MAPE)
- Среднеквадратичная ошибка (RMSE)
- Коэффициент детерминации (R2)

|Модель|MAE|RMSE|MAPE|R2|
|:---:|:---:|:---:|:---:|:---:|
|Baseline (AR)|69773|94868|9.2|0.21|
|HWES|124601|144758|17.7|-0.84|
|XGBoost|31141|40435|4.2|0.86|
|LASSO|35615|43907|4.8|0.83|
|SARIMA|96365|118034|13.2|-0.22|
|Prophet|61767|83527|8.0|0.39|

Как видим, наиболее оптимальной моделью является XGBoost, у модели LASSO также хорошие результаты метрики, она на втором месте.

![Рис_39](./Images/image_39.png "Результат моделирования")
