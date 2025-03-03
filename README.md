# Анализ данных о поломках оборудования и прогнозирование выживаемости

## Описание проекта

Проект посвящен анализу данных о поломках оборудования с целью выявления закономерностей, прогнозирования будущих поломок и оценки выживаемости различных компонентов.  Анализ включает в себя подготовку данных, статистическое моделирование и визуализацию результатов.


## Подготовка данных

Перед анализом данные были очищены и преобразованы.  Это включало:

•   Добавление столбца "Название детали" с использованием модели LLM (ChatGPT) для извлечения названий деталей из описания поломок, коррекции орфографических ошибок и приведения к единому формату.
•   Преобразование формата дат для удобства работы в Python.
•   Создание нового набора данных для анализа выживаемости, включающего столбцы "Начало простоя", "Длительность поломки" и "Название детали", с добавлением строк для дней без поломок и бинарным целевым признаком (1 - поломка, 0 - без поломки).
•   Выделение наиболее часто заменяемых деталей для фокусировки анализа на наиболее проблемных компонентах.  Это осуществлялось с помощью функции `set()` для определения уникальных названий деталей и подсчета частоты их замен.
•   Визуализация частоты поломок для различных артикулов деталей с помощью сводной гистограммы.

## Анализ данных и прогнозы

Анализ данных фокусируется на выявлении корреляций между различными факторами и частотой поломок, а также на построении моделей для прогнозирования будущих поломок. 

**Тенденции для наиболее уязвимых деталей**

**Тормозная пластина**
1. Частота поломок:
•  Тормозная пластина - №10: Фиксируются поломки как в 2022, так и в 2023 и 2024 годах, что указывает на потенциальную проблему с этой деталью на протяжении нескольких лет.

•  Тормозная пластина - №63: Поломки встречаются в разные годы, начиная с 2020, что указывает на относительно долгосрочную проблему с этой деталью.

2. Сезонность:
•  Март: Поломки тормозных пластин №10 и №63 чаще всего происходят в марте. Это может быть связано с сезонными изменениями погоды (переход от зимы к весне)
•  Весна-Лето: У всех пластин наблюдается повышенная частота поломок в период с весны по лето (апрель-август).

3. Длительность простоя:
•  Тормозные пластины, которые часто ломались (10, 12, 31, 63) также менялись или чинились по два раза за месяц, с периодом 4-10 дней. Первые починки были всегда быстрее. Возможно, это связано с неаккуратной работой при замене или починки. 

•  Для пластины №10 можно наблюдать поломки в каждом квартале, что говорит о необходимости обратить внимание на качество материала, либо на условия эксплуатации

**Дроссель**
1. Парные дроссели: Замена парой обязательна.

При поломке одного дросселя из пары второй, вероятно, сломается в течение месяца. Заменяйте оба, чтобы избежать повторного простоя.

2. Дроссели с >4 заменами: Ресурс 2 года.

Часто заменяемые дроссели (более 4 раз) служат около 2 лет. Планируйте замену с учетом этого срока, либо ищите более надежные аналоги.

3. Дроссели CO2: Летние поломки, 07.12.21 - аномалия.

Дроссели CO2 ломаются в основном с июня по сентябрь. Поломка 07.12.21 - исключение, требующее выяснения причин.

## Анализ выживаемости

**Модель Каплана-Майера**

Медиана выживаемости: (время, к которому вероятность выживания снижается до 50%) = 2-4 дня. 

Вероятность поломок на 10 день от конца наблюдения (начало 2025 год) составляет 75% 

Начиная с 20 дня, вероятность поломки детали стремится к 100%

**Модель Нельсона-Аалена**

Более крутой наклон графика указывает на более высокий риск в этот период. 

Область вокруг линии - доверительный  интервал, показывает, насколько надежна оценка кумулятивной опасности.

Наиболее крутой наклон на графике в первые 10 дней. Это говорит о том, что в данный промежуток наиболее вероятны поломки. Доверительный интервал практически совпадает с линией.

После первых 10 дней линия имеет более пологий наклон, что говорит о меньше вероятности поломки, однако доверительный интервал шире и сама линия может иметь в реальности более крутой наклон (вероятность поломок выше)

## Выводы

Благодаря анализу были обнаружены следующие закономерности для наиболее уязвимых деталей: 

•  Сезонность: 
При дальнейшем анализе других поломок этот фактор может масштабироваться на другие детали
Необходимо установить причину поломок летом (перегрев, нагрузка) и весной (влажность), вероятнее всего тенденция поломок в это время может сократиться.

  •  Взаимосвязь:
Поломка одного дросселя из пары ведет к поломке второго, аналогичные результаты могут быть выявлены и у других деталей.

Наиболее проблемные компоненты: тормозные пластины,  дроссели, наполнители. 

Исходя из анализов выживаемости, удалось спрогнозировать наличие поломок на ближайшие 20 дней. Вероятнее всего, при большем количестве данных прогнозы будут дальше и точнее. 
