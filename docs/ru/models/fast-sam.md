---
comments: true
description: Исследуйте FastSAM, базирующуюся на CNN сегментацию объектов в реальном времени на изображениях. Улучшенное взаимодействие с пользователем, высокая вычислительная эффективность и применимость к различным задачам компьютерного зрения.
keywords: FastSAM, машинное обучение, решение на базе CNN, сегментация объектов, решение в реальном времени, Ultralytics, задачи компьютерного зрения, обработка изображений, применение в промышленности, пользовательское взаимодействие
---

# Модель Fast Segment Anything (FastSAM)

Модель Fast Segment Anything (FastSAM) - это новаторское решение на базе CNN, предназначенное для решения задачи сегментации объектов в реальном времени. Эта задача разработана для сегментации любого объекта на изображении на основе различных возможных запросов пользователя. FastSAM значительно снижает вычислительные требования, при этом сохраняя конкурентоспособность работы, что делает ее практически подходящей для различных задач компьютерного зрения.

![Обзор архитектуры модели Fast Segment Anything (FastSAM)](https://user-images.githubusercontent.com/26833433/248551984-d98f0f6d-7535-45d0-b380-2e1440b52ad7.jpg)

## Обзор

FastSAM разработана для преодоления ограничений [модели Segment Anything (SAM)](sam.md), тяжелой модели Transformer, требующей значительных вычислительных ресурсов. FastSAM разделяет задачу сегментации объектов на два последовательных этапа: сегментация всех экземпляров и выбор, основанный на запросах пользователя. На первом этапе используется [YOLOv8-seg](../tasks/segment.md) для создания сегментационных масок всех экземпляров на изображении. На втором этапе модель выводит область интереса, соответствующую запросу.

## Основные особенности

1. **Решение в реальном времени:** Благодаря эффективности вычислений на базе CNN, FastSAM обеспечивает решение задачи сегментации объектов в реальном времени, что делает ее ценной для применения в промышленных приложениях, требующих быстрых результатов.

2. **Эффективность и производительность:** FastSAM обеспечивает значительное снижение вычислительных и ресурсных требований, не ухудшая качество работы. Она достигает сопоставимой производительности с моделью SAM, но требует значительно меньше вычислительных ресурсов, что позволяет использовать ее в реальном времени.

3. **Сегментация на основе запросов пользователя:** FastSAM может выполнять сегментацию любого объекта на изображении, основываясь на различных возможных запросах пользователя, что обеспечивает гибкость и приспособляемость к различным сценариям.

4. **Основана на YOLOv8-seg:** FastSAM основана на модели [YOLOv8-seg](../tasks/segment.md), которая является детектором объектов с ветвью сегментации экземпляров. Это позволяет ей эффективно создавать сегментационные маски всех экземпляров на изображении.

5. **Высокие результаты на показателях:** При выполнении задачи предложения объектов на наборе данных MS COCO FastSAM достигает высоких показателей производительности при значительно большей скорости работы, чем [SAM](sam.md) на одном графическом процессоре NVIDIA RTX 3090, что свидетельствует о ее эффективности и способности.

6. **Практическое применение:** Предложенный подход предоставляет новое практическое решение для большого количества задач компьютерного зрения с очень высокой скоростью, в десятки или сотни раз превышающей скорость существующих методов.

7. **Возможность сжатия модели:** FastSAM демонстрирует возможность существенно снизить вычислительные затраты, введя искусственное преимущество в структуру модели, открывая новые возможности для создания крупномасштабных архитектур моделей для общих задач компьютерного зрения.

## Доступные модели, поддерживаемые задачи и режимы работы

В следующей таблице представлены доступные модели с их конкретными заранее обученными весами, поддерживаемые задачи и совместимость с различными режимами работы, такими как [Вывод](../modes/predict.md), [Валидация](../modes/val.md), [Обучение](../modes/train.md) и [Экспорт](../modes/export.md), обозначенные значками ✅ для поддерживаемых режимов и значками ❌ для неподдерживаемых режимов.

| Тип модели | Заранее обученные веса | Поддерживаемые задачи                          | Вывод | Валидация | Обучение | Экспорт |
|------------|------------------------|------------------------------------------------|-------|-----------|----------|---------|
| FastSAM-s  | `FastSAM-s.pt`         | [Сегментация экземпляров](../tasks/segment.md) | ✅     | ❌         | ❌        | ✅       |
| FastSAM-x  | `FastSAM-x.pt`         | [Сегментация экземпляров](../tasks/segment.md) | ✅     | ❌         | ❌        | ✅       |

## Примеры использования

Модели FastSAM легко интегрировать в ваши приложения на Python. Ultralytics предоставляет удобный пользовательский интерфейс API и команды CLI для упрощения разработки.

### Использование для предсказаний

Для выполнения обнаружения объектов на изображении используйте метод `predict`, как показано ниже:

!!! Example "Пример"

    === "Python"
        ```python
        from ultralytics import FastSAM
        from ultralytics.models.fastsam import FastSAMPrompt

        # Определение исхода вывода
        source = 'путь/к/фото_автобуса.jpg'

        # Создание модели FastSAM
        model = FastSAM('FastSAM-s.pt')  # или FastSAM-x.pt

        # Выполнение вывода на изображение
        результаты = model(source, device='cpu', retina_masks=True, imgsz=1024, conf=0.4, iou=0.9)

        # Создание объекта Prompt Process
        prompt_process = FastSAMPrompt(source, результаты, device='cpu')

        # Вывод всего
        ann = prompt_process.everything_prompt()

        # Прямоугольная область по умолчанию [0,0,0,0] -> [x1,y1,x2,y2]
        ann = prompt_process.box_prompt(bbox=[200, 200, 300, 300])

        # Текстовый запрос
        ann = prompt_process.text_prompt(text='фотография собаки')

        # Запрос точки
        # Точки по умолчанию [[0,0]] [[x1,y1],[x2,y2]]
        # метка точки по умолчанию [0] [1,0] 0:фон, 1:передний план
        ann = prompt_process.point_prompt(points=[[200, 200]], pointlabel=[1])
        prompt_process.plot(annotations=ann, output='./')
        ```

    === "CLI"
        ```bash
        # Загрузка модели FastSAM и сегментация всего объекта на нем
        yolo segment predict model=FastSAM-s.pt source=путь/к/фото_автобуса.jpg imgsz=640
        ```

В этом фрагменте кода демонстрируется простота загрузки предобученной модели и выполнения предсказаний на изображении.

### Использование для валидации

Валидацию модели на наборе данных можно выполнить следующим образом:

!!! Example "Пример"

    === "Python"
        ```python
        from ultralytics import FastSAM

        # Создание модели FastSAM
        model = FastSAM('FastSAM-s.pt')  # или FastSAM-x.pt

        # Валидация модели
        результаты = model.val(data='coco8-seg.yaml')
        ```

    === "CLI"
        ```bash
        # Загрузка модели FastSAM и ее валидация на примере набора данных COCO8 при размере изображения 640
        yolo segment val model=FastSAM-s.pt data=coco8.yaml imgsz=640
        ```

Пожалуйста, обратите внимание, что FastSAM поддерживает только обнаружение и сегментацию единственного класса объектов. Это означает, что модель будет распознавать и сегментировать все объекты как один и тот же класс. Поэтому при подготовке набора данных вам нужно преобразовать все идентификаторы категорий объектов в 0.

## Официальное использование FastSAM

FastSAM также доступна непосредственно из репозитория [https://github.com/CASIA-IVA-Lab/FastSAM](https://github.com/CASIA-IVA-Lab/FastSAM). Вот краткий обзор типичных шагов для использования FastSAM:

### Установка

1. Клонируйте репозиторий FastSAM:
   ```shell
   git clone https://github.com/CASIA-IVA-Lab/FastSAM.git
   ```

2. Создайте и активируйте виртуальное окружение Conda с Python 3.9:
   ```shell
   conda create -n FastSAM python=3.9
   conda activate FastSAM
   ```

3. Перейдите в каталог склонированного репозитория и установите требуемые пакеты:
   ```shell
   cd FastSAM
   pip install -r requirements.txt
   ```

4. Установите модель CLIP:
   ```shell
   pip install git+https://github.com/openai/CLIP.git
   ```

### Пример использования

1. Скачайте [файл контрольной точки модели](https://drive.google.com/file/d/1m1sjY4ihXBU1fZXdQ-Xdj-mDltW-2Rqv/view?usp=sharing).

2. Используйте модель FastSAM для выполнения вывода. Примеры команд:

    - Сегментация всего объекта на изображении:
      ```shell
      python Inference.py --model_path ./weights/FastSAM.pt --img_path ./images/dogs.jpg
      ```

    - Сегментация определенных объектов с использованием текстового запроса:
      ```shell
      python Inference.py --model_path ./weights/FastSAM.pt --img_path ./images/dogs.jpg --text_prompt "желтая собака"
      ```

    - Сегментация объектов в пределах ограничивающей рамки (укажите координаты рамки в формате xywh):
      ```shell
      python Inference.py --model_path ./weights/FastSAM.pt --img_path ./images/dogs.jpg --box_prompt "[570,200,230,400]"
      ```

    - Сегментация объектов, находящихся близко к определенным точкам:
      ```shell
      python Inference.py --model_path ./weights/FastSAM.pt --img_path ./images/dogs.jpg --point_prompt "[[520,360],[620,300]]" --point_label "[1,0]"
      ```

Кроме того, вы можете опробовать FastSAM с помощью [демонстрационного ноутбука Colab](https://colab.research.google.com/drive/1oX14f6IneGGw612WgVlAiy91UHwFAvr9?usp=sharing) или на [веб-демо HuggingFace](https://huggingface.co/spaces/An-619/FastSAM) для визуального опыта.

## Цитирование и благодарности

Мы хотели бы выразить благодарность авторам FastSAM за их значительный вклад в области сегментации объектов в реальном времени:

!!! Quote ""

    === "BibTeX"

      ```bibtex
      @misc{zhao2023fast,
            title={Fast Segment Anything},
            author={Xu Zhao and Wenchao Ding and Yongqi An and Yinglong Du and Tao Yu and Min Li and Ming Tang and Jinqiao Wang},
            year={2023},
            eprint={2306.12156},
            archivePrefix={arXiv},
            primaryClass={cs.CV}
      }
      ```

Оригинальная статья FastSAM доступна на [arXiv](https://arxiv.org/abs/2306.12156). Авторы сделали свою работу общедоступной, и код можно получить на [GitHub](https://github.com/CASIA-IVA-Lab/FastSAM). Мы ценим их усилия по развитию отрасли и доступу к их работе для широкого круга пользователей.