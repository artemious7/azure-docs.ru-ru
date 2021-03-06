---
title: Руководство по созданию проекта классификации изображений с помощью Пользовательской службы визуального распознавания и Python
titlesuffix: Azure Cognitive Services
description: Создайте проект, добавьте теги, загрузите изображения, обучите проект и выполните прогнозирование с использованием конечной точки по умолчанию.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: 14b805a60637a889698132e169d5a41670a8bce0
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363383"
---
# <a name="tutorial-create-an-image-classification-project-using-the-custom-vision-service-with-python"></a>Руководство по созданию проекта классификации изображений с помощью Пользовательской службы визуального распознавания в Python

Сведения о том, как создать проект классификации изображений с помощью Пользовательской службы визуального распознавания и базового скрипта Python. Создав проект, можно добавить теги, загрузить изображения, обучить проект, получить URL-адрес конечной точки прогнозирования проекта по умолчанию и с его помощью программными средствами протестировать изображение. Этот пример с открытым кодом можно применить как шаблон для создания собственного приложения с помощью API Пользовательской службы визуального распознавания.



## <a name="prerequisites"></a>Предварительные требования

- Python 2.7+ или Python 3.5+.
- Средство PIP.

## <a name="get-the-training-and-prediction-keys"></a>Получение ключей обучения и прогнозирования

Чтобы получить ключи, используемые в этом примере, посетите [веб-страницу Пользовательской службы визуального распознавания](https://customvision.ai) и выберите __значок шестеренки__ в правом верхнем углу. В разделе __Accounts__ (Учетные записи) скопируйте значения в полях __Training Key__ (Ключ обучения) и __Prediction Key__ (Ключ прогнозирования).

![Изображение пользовательского интерфейса с ключами](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Установка пакета SDK для Пользовательской службы визуального распознавания

Чтобы установить пакет SDK для Пользовательской службы визуального распознавания, выполните следующую команду.

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Получение примеров изображений

В этом примере используются изображения из каталога `Samples/Images` проекта [https://github.com/Microsoft/Cognitive-CustomVision-Windows](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images). Клонируйте или скачайте и извлеките проект в свою среду разработки.

## <a name="create-a-custom-vision-service-project"></a>Создание проекта Пользовательской службы визуального распознавания

Чтобы создать проект Пользовательской службы визуального распознавания, создайте файл с именем `sample.py`. Используйте следующий код в качестве содержимого файла:

> [!IMPORTANT]
> Установите для `training_key` полученное ранее значение ключа обучения.
>
> Установите для `prediction_key` полученное ранее значение ключа прогнозирования.

```python
from azure.cognitiveservices.vision.customvision.training import training_api
from azure.cognitiveservices.vision.customvision.training.models import ImageUrlCreateEntry

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"

trainer = training_api.TrainingApi(training_key)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My Project")
```

## <a name="add-tags-to-your-project"></a>Добавление тегов в проект

Чтобы добавить в проект теги, добавьте следующий код в конец файла `sample.py`:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Загрузка изображений в проект

Чтобы добавить примеры изображений в проект, вставьте следующий код после создания тегов. Этот код загружает изображение с соответствующим тегом:

> [!IMPORTANT]
>
> Измените путь к изображениям в зависимости от того, куда вы ранее скачали проект Cognitive-CustomVision-Windows.

```python
base_image_url = "https://raw.githubusercontent.com/Microsoft/Cognitive-CustomVision-Windows/master/Samples/"

print ("Adding images...")
for image_num in range(1,10):
    image_url = base_image_url + "Images/Hemlock/hemlock_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ hemlock_tag.id ] ) ])

for image_num in range(1,10):
    image_url = base_image_url + "Images/Japanese Cherry/japanese_cherry_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ cherry_tag.id ] ) ])


# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following:
#
#import os
#hemlock_dir = "Images\\Hemlock"
#for image in os.listdir(os.fsencode("Images\\Hemlock")):
#    with open(hemlock_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ hemlock_tag.id ])
#
#cherry_dir = "Images\\Japanese Cherry"
#for image in os.listdir(os.fsencode("Images\\Japanese Cherry")):
#    with open(cherry_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ cherry_tag.id ])
```

## <a name="train-the-project"></a>Обучение проекта

Для обучения классификатора добавьте следующий код в конец файла `sample.py`:

```python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status != "Completed"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Make it the default project endpoint
trainer.update_iteration(project.id, iteration.id, is_default=True)
print ("Done!")
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Получение и использование конечной точки прогнозирования по умолчанию

Чтобы отправить изображение в конечную точку прогнозирования и выполнить прогнозирование, добавьте следующий код в конец файла `sample.py`:

```python
from azure.cognitiveservices.vision.customvision.prediction import prediction_endpoint
from azure.cognitiveservices.vision.customvision.prediction.prediction_endpoint import models

# Now there is a trained endpoint that can be used to make a prediction

predictor = prediction_endpoint.PredictionEndpoint(prediction_key)

test_img_url = base_image_url + "Images/Test/test_image.jpg"
results = predictor.predict_image_url(project.id, iteration.id, url=test_img_url)

# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following.
#
# Open the sample image and get back the prediction results.
# with open("Images\\test\\test_image.jpg", mode="rb") as test_data:
#     results = predictor.predict_image(project.id, test_data, iteration.id)

# Display the results.
for prediction in results.predictions:
    print ("\t" + prediction.tag_name + ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-example"></a>Выполнение примера

Запустите решение. В консоли отобразятся результаты прогнозирования.

```
python sample.py
```

Выходные данные приложения будут выглядеть приблизительно так:

```
Creating project...
Adding images...
Training...
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```