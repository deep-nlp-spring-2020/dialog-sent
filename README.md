dialog-sent
==============================


## Описание проекта

Проект в рамках курса DL in NLP 2020. Решается задача с [соревнования](http://www.dialog-21.ru/evaluation/2015/sentiment/). Сначала решалась задача E (Aspect Based Sentiment Analysis), а затем по просьбе организаторов переключились на задачу C (Aspect-Target Sentiment Classification).

### ABSA

Задача заключается в том, чтобы определить тональность предложения/текста относительно заданного аспекта.

#### English

Сначала задача решалась для англоязычного датасета с отзывами на рестораны. Аспектами выступали:
* Food
* Interior
* Price
* Whole
* Service

Возможные тональности:
* neutral
* positive
* negative

За основу была взята статья Attention-based LSTM for Aspect-level Sentiment Classification (см. в references/). По статье были реализованы модели:
* AE-LSTM (1-db-ae-lstm.ipynb)
* AT-LSTM (2-db-at-lstm.ipynb)
* ATAE-LSTM (3-db-atae-lstm.ipynb)

Метрика бралась такая же, как и в статье (accuracy), чтобы получить сравнимые результаты. Там смотрели только на тональности: positive, neutral, negative. Результаты получились хуже, чем в статье.

Также на этом этапе была испробована модель на основе BERT (см. reports/fedor_noskov.md).

Результаты:
ДОБАВИТЬ РЕЗУЛЬТАТЫ!!!!

#### Russian

Брался русскоязычный датасет с отзывами на рестораны. Аспектами выступали:
* Food
* Interior
* Price
* Whole
* Service

Возможные тональности:
* positive
* negative
* both

ATAE-LSTM была перенесена на русский язык (4-db-at-atae-lstm-russian.ipynb). Были использованы русскоязычные эмбеддинги из RusVectōrēs с соответствующим препроцессингом текста (лемматизация и разбиение по частям речи).

Модель на основе BERT не успели перенести на русский язык, так как сменилась задача.

Результаты:

Model | F1-Macro
--- | ---
Best competition | 0.458
ATAE-LSTM | 0.446

Best competition -- лучшая модель на соревновании (результаты соревнования см. в references/SentiRuEval_results.xlsx).

### ATSC

Задача заключается в том, чтобы определить тональность предложения/текста относительно заданного набора последовательных токенов (entity).

#### English

Сначала задача решалась для англоязычного датасета с отзывами на рестораны. Тональности:
* positive
* negative
* neutral

Были реализованы модели на основе 
* ELMo 
* BERT 
* ULMFiT 

Результаты сравнивались с результатами статьи Adapt or Get Left Behind: Domain Adaptation through BERT Language Model Finetuning forAspect-Target Sentiment Classification. Подробности по моделям можно найти в отчетах студентов: ELMo -- Бунин Дмитрий, BERT -- Носков Федор, ULMFiT -- Мамонов Кирилл.

Использовались те же метрики, что и в статье выше, чтобы было с чем сравнивать. Приведем результаты лучших моделей каждого типа:

Результаты:
Model | Accuracy | F1-Macro
--- | --- | ---
SOTA | 0.871 | 0.801
ELMoCNN | 0.807 | 0.707
BERT | 0.808 | 0.627

Под SOTA подразумевается результат из вышеобозначенной статьи. Как видим, до SOTA далеко.

Для UMLFiT использовался другой датасет -- [Twitter US Sentiment Data](https://www.kaggle.com/crowdflower/twitter-airline-sentiment).

Результаты:
Model | Accuracy
--- | ---
ULMFiT | 0.627

#### Russian

Тональности:
* positive
* negative
* both

Были перенесены модели из предыдущего пункта на русскоязычный датасет. Подробности по моделям по-прежнему расположены в reports/.

Были получены следующие результаты:

Model | F1-Micro | F1-Macro
--- | --- | ---
Best competition | 0.825 | 0.555
ELMoCNN-1 | 0.855 | 0.520
ELMoCNN-2 | 0.851 | 0.521

Best competition -- лучшая модель на соревновании (результаты соревнования см. в references/SentiRuEval_results.xlsx), она намного превосходила все остальные результаты соревнования.


## Отчеты по каждому участнику

Чтобы понять что делал каждый участник см. reports/

## Дальшейшие планы

* Обучение мультиязычного BERT, и на русском, и на английском датасете
* Использование других эмбеддингов в ELMo. Например, на RusVectōrēs есть эмбеддинги для лемматизированного текста.

## Выводы

Выводы будут даны про ATSC, потому что в итоге мы решали именно ее.

* По метрике f1-macro достичь лучшего результата не соревновании не удалось, проблемы с обработкой дизбаланса классов.
* По метрике f1-micro удалось превзойти лучший результат соревнования.


Структура проекта
------------

    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make data` or `make train`
    ├── README.md          <- The top-level README for developers using this project.
    ├── data
    │   ├── external       <- Data from third party sources.
    │   ├── interim        <- Intermediate data that has been transformed.
    │   ├── processed      <- The final, canonical data sets for modeling.
    │   └── raw            <- The original, immutable data dump.
    │
    ├── docs               <- A default Sphinx project; see sphinx-doc.org for details
    │
    ├── models             <- Trained and serialized models, model predictions, or model summaries
    │
    ├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
    │                         the creator's initials, and a short `-` delimited description, e.g.
    │                         `1.0-jqp-initial-data-exploration`.
    │
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └── figures        <- Generated graphics and figures to be used in reporting
    │
    ├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
    │                         generated with `pip freeze > requirements.txt`
    │
    ├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
    ├── src                <- Source code for use in this project.
    │   ├── __init__.py    <- Makes src a Python module
    │   │
    │   ├── data           <- Scripts to download or generate data
    │   │   └── make_dataset.py
    │   │
    │   ├── features       <- Scripts to turn raw data into features for modeling
    │   │   └── build_features.py
    │   │
    │   ├── models         <- Scripts to train models and then use trained models to make
    │   │   │                 predictions
    │   │   ├── predict_model.py
    │   │   └── train_model.py
    │   │
    │   └── visualization  <- Scripts to create exploratory and results oriented visualizations
    │       └── visualize.py
    │
    └── tox.ini            <- tox file with settings for running tox; see tox.testrun.org

