---
title: Расширение интерфейса командной строки для Машинного обучения
titleSuffix: Azure Machine Learning service
description: Узнайте о расширении интерфейса командной строки для службы Машинного обучения Azure. Azure CLI — это кроссплатформенная программа командной строки, которая позволяет работать с ресурсами в облаке Azure. Расширение Машинного обучения позволяет работать со службой "Машинное обучение Azure".
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: d7baa4faf718852e5bddd89f99e4ffc1248a403c
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249737"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Использование расширения интерфейса командной строки для Службы машинного обучения Azure

CLI для Машинного обучения Azure является расширением кроссплатформенного интерфейса командной строки для платформы Azure ([Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)). Это расширение предоставляет команды для работы со службой "Машинное обучение Azure" из командной строки. Оно позволяет создавать скрипты для автоматизации выполнения рабочих процессов машинного обучения. Например, можно создать скрипты, которые выполняют следующие действия:

+ выполнять эксперименты для создания моделей машинного обучения;

+ регистрировать модели машинного обучения для использования клиентами;

+ упаковывать, развертывать и отслеживать жизненный цикл моделей машинного обучения.

Интерфейс командной строки не является заменой пакета SDK для Машинного обучения Azure. Это дополнительное средство, которое оптимизировано для обработки очень параметризованных задач, таких как:

* Создание вычислительных ресурсов.

* Отправка параметризованного эксперимента.

* регистрация модели;

* Создание образа.

* Развертывание службы

## <a name="prerequisites"></a>Предварительные требования


* Для использования интерфейса командной строки необходима подписка Azure. Если у вас еще нет подписки Azure, создайте бесплатную учетную запись Azure, прежде чем начинать работу. Опробуйте [бесплатную или платную версию Службы машинного обучения Azure](http://aka.ms/AMLFree).

* [Интерфейс командной строки Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="install-the-extension"></a>Установка расширения

Чтобы установить расширение интерфейса командной строки Машинного обучения, выполните следующую команду:

```azurecli-interactive
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-1.0.6-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

При появлении запроса выберите `y`, чтобы установить расширение.

Чтобы убедиться, что расширение установлено, используйте следующую команду для отображения списка подкоманд Машинного обучения:

```azurecli-interactive
az ml -h
```

> [!TIP]
> Чтобы обновить расширение, необходимо __удалить__, а затем __установить__ его. При этом устанавливается последняя версия.

## <a name="remove-the-extension"></a>Удаление расширения

Чтобы удалить расширение интерфейса командной строки, используйте следующую команду:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Управление ресурсами

Следующие команды демонстрируют использование интерфейса командной строки для управления ресурсами, используемыми службой "Машинное обучение Azure".


+ Создайте рабочую область Службы машинного обучения Azure.

    ```azurecli-interactive
    az ml workspace create -n myworkspace -g myresourcegroup
    ```

+ Задайте рабочую область по умолчанию:

    ```azurecli-interactive
    az configure --defaults aml_workspace=myworkspace group=myresourcegroup
    ```

+ Создайте управляемый целевой объект вычислений для распределенного обучения.

    ```azurecli-interactive
    az ml computetarget create amlcompute -n mycompute --max_nodes 4 --size Standard_NC6
    ```

* Обновите управляемый целевой объект вычислений.

    ```azurecli-interactive
    az ml computetarget update --name mycompute --workspace –-group --max_nodes 4 --min_nodes 2 --idle_time 300
    ```

* Подключите неуправляемый целевой объект вычислений для обучения или развертывания.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myrg -w myworkspace
    ```

## <a name="experiments"></a>Эксперименты

Следующие команды демонстрируют, как использовать интерфейс командной строки для работы с экспериментами:

* Вложите проект (конфигурация запуска) перед отправкой эксперимента.

    ```azurecli-interactive
    az ml project attach --experiment-name myhistory
    ```

* Запустите выполнение эксперимента. При использовании этой команды укажите имя файла `.runconfig`, который содержит конфигурацию запуска. Целевой объект вычисления использует конфигурацию запуска для создания среды обучения для модели. В этом примере конфигурация запуска загружается из файла `./aml_config/myrunconfig.runconfig`.

    ```azurecli-interactive
    az ml run submit -c myrunconfig train.py
    ```

    По умолчанию файлы `.runconfig` с именами `docker.runconfig` и `local.runconfig` создаются при подключении проекта с помощью команды `az ml project attach`. Возможно, вам придется изменить их, прежде чем использовать для обучения модели. 

    Вы также можете создать конфигурацию запуска программным способом с помощью класса RunConfiguration, описанного [здесь](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py). После создания вы можете использовать метод `save()`, чтобы создать файл `.runconfig`.

* Просмотрите список отправленных экспериментов.

    ```azurecli-interactive
    az ml history list
    ```

## <a name="model-registration-image-creation--deployment"></a>Регистрация модели, создание и развертывание образа

Следующие команды демонстрируют регистрацию обученной модели, а затем ее развертывание в качестве рабочей службы:

+ Зарегистрируйте модель с помощью Машинного обучения Azure.

  ```azurecli-interactive
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Создайте образ для размещения модели и зависимостей машинного обучения. 

  ```azurecli-interactive
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Разверните образ в целевом объекте вычислений:

  ```azurecli-interactive
  az ml service create aci -n myaciservice --image-id myimage:1
  ```
