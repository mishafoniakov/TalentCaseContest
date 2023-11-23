# SberTalentCase_C-2
Задание для отборочного тура кейс-чемпионата Sber Talent Case Contest 2023 для команды С-2

## 0. Подготовка рабочей среды
### 0.1. Скачивание нужных модулей
### 0.2. Запуск датасета

## 1. Предобрадотка данных
### 1.1. Очистка первичных данных
Необходимо проверить предложения на их типы: повествовательное, восклицательное и вопросительное. Это определяет последний знак препинания в предложении. Если предложение оканчивается на ".", то оно повествовательное, если на "!" - восклицательное, если на "?", то вопросительное.

Затем необходимо будет очистить предложения и привести их к общему виду:

1. Удалить лишние пробелы
2. Убрать прописные буквы: сделать все буквы строчные
3. Убрать все символы кроме цифр и букв
4. Провести лемматизацию слов в предложении: привести все слова к начальной форме
5. Также дополнительно провести очистку, оставив только цифры и буквы: возможно такие остались после лемматизации
6. Также повторно удалить возможные возникшие дополнительные пробелы
7. Очистить предложения от стоп-слов
8. Провести токенизацию получившихся предложений

### 1.2. Формирование датасета для анализа
Далее необходимо получить cartesian product датасета: сделать сочетание предложений каждое с каждым и переименовать получившиеся колонки для более удобного чтения

1. Делаем cross join нашего датасета
2. Убираем повторяющиеся парные значения
3. Выбираем интересующие нас столбцы
4. Необходимо убрать пары предложений с разными типами, поскольку они не могут быть рерайтами
5. Переименовываем столбцы для более удобного чтения

## 2. Создание модели Word2Vec и алгоритма
### 2.1. Создание модели Word2Vec
Создаём модель Word2Vec c минимальным значением токенизированного предложения 1
### 2.2. Создание алгоритма для поиска рерайтов
Алгоритм предлагаем следующий:

1. Из полученного датасета выводим пару токенизированных предложений
2. Внутри этой пары сравниваем попарно слова из одного и другого токенизированного предложения с присваиванием каждому слову вектор
3. Считаем cosine similarity для такой пары
4. Выводим средний cosine similarity из каждой пары токенизированных предложений
5. Если средний cosine similarity больше или равен 0.3 - то это рерайт

### 2.3. Результат
Выводим получившийся результат и сохраняем его в файл JSON

## Далее предлагаем два алгоритма для поиска рерайтов: c помощью векторной модели Word2Vec и с помощью модели KeyedVectors

## 3. Создание модели KeyedVectors и алгоритма
### 3.1. Формирование модели Word2Vec с помощью модуля KeyedVectors
Создаём модель KeyedVectors для построения векторов

### 3.2. Создание алгоритма для поиска рерайтов
Алгоритм предлагается следующий:

1. Из полученного датасета из каждого предложения каждой пары берём поэлементное среднее и объединяем средние значения
2. Получаем числовой вектор из каждого предложения
3. Считаем cosine similarity для каждой пары предложений
4. Если средний cosine similarity больше или равен 0.3 - то это рерайт

### 3.3. Результат
Выводим получившийся результат и сохраняем его в файл JSON

## 4. Создание алгоритма на основе модели word2vec с использованием токенов и биграммов
Алгоритм предлагается следующий:

Для каждого предложения выводим биграммы. Таким образом, в нашем пайплайне есть две колонки: колонка с токенами и колонка с биграммами

Для каждого биграмма: внутри биграмма для каждого токена находим word2vec вектор. Берём минимальное значение из полученного вектора и получаем коэффициент

Для каждого токена: находим минимальное значение для каждого токена. Берём минимальное значение из полученного вектора и получаем коэффициент

Сравниваем коэффициенты биграммов. Если они равны и при этом не равны 0 - то это копирайт

Если коэффициент у биграмма нулевой - сравниваем коэффициенты токенов. Если коэффициенты у токенов равны - то это копирайт

##4.1. Формирование модели Word2Vec и алгоритма для поиска рерайтов

##4.2. Получение результатов

##4.3. Результат

##5. Создание алгоритма на основе вектора BagOfWords с использованием токенов
##$5.1. Формирование модели BagOfWords и алгоритма для поиска рерайтов

Алгоритм предлагается следующий:

Получаем список уникальных слов из всего датасета

Создаём вектор из этого уникального списка слов:

изначально создаём нулевой вектор

длина вектора каждого предложения равна длине этого списка уникальных слов

если слово встречается в предложении - ставим соответствующую цифру повторов в индексе соответствующему слову в списке слов

#5. Cравниваем вектора каждого преждожения попарно

## 5.1. Создание алгоритма на основе вектора BagOfWords с использованием токенов

## 5.2 Формирование модели BagOfWords и алгоритма для поиска рерайтов

## 5.3. Полчучение результатов

Алгоритм предлагается следующий:
Создаём KeyedVector из каждого предложения: из полученного датасета из каждого предложения каждой пары берём поэлементное среднее и объединяем средние значения и получаем числовой вектор из каждого предложения

Создаём Bag of Words из каждого предложения: изначально создаём нулевой вектор, длина вектора каждого предложения равна длине этого списка уникальных слов, если слово встречается в предложении - ставим соответствующую цифру повторов в индексе соответствующему слову в списке слов, получаем числовой вектор из каждого предложения

Распределяем вектора по 200 кластерам

Сопоставляем пары номеров кластеров по KeyedVectors и BagOfWords

Если по одному или по другому вектору кластера равны - это копирайт
Получаем список уникальных слов из всего датасета

Создаём вектор из этого уникального списка слов:

изначально создаём нулевой вектор

длина вектора каждого предложения равна длине этого списка уникальных слов

если слово встречается в предложении - ставим соответствующую цифру повторов в индексе соответствующему слову в списке слов

Cравниваем вектора каждого преждожения попарно

Считаем схожесть вектора по cosine similarity

6.3.Получение результатоа
