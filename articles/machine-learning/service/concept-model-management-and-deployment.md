---
title: Регистрация, развертывание и отслеживание моделей машинного обучения, а также управление ими
titleSuffix: Azure Machine Learning service
description: Узнайте, как развертывать, отслеживать и администрировать модели с помощью службы "Машинное обучение Azure", чтобы постоянно улучшать их. Модели, обученные с помощью службы "Машинное обучение Azure", можно развертывать на локальном компьютере или из других источников.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 1/23/2019
ms.custom: seodec18
ms.openlocfilehash: 2cd2d328d33744854bc525e5ecf1dfa3b6e4bcc8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59275448"
---
# <a name="manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>Администрирование, развертывание и мониторинг моделей с помощью службы "Машинное обучение Azure"

В этой статье описано, как развертывать, отслеживать и администрировать модели с помощью службы "Машинное обучение Azure", чтобы постоянно улучшать их. Модели, обученные с помощью службы "Машинное обучение Azure", можно развертывать на локальном компьютере или из других источников. 

На следующей схеме показан полный рабочий процесс развертывания. [![Рабочий процесс развертывания для машинного обучения Azure](media/concept-model-management-and-deployment/deployment-pipeline.png)](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

Рабочий процесс включает в себя следующие шаги:
1. **Регистрация модели** в реестре, размещенном в рабочей области службы "Машинное обучение Azure"
1. **Регистрация образа**, который связывает модель со сценарием оценки и зависимостями в переносимом контейнере. 
1. **Развертывание** образа в виде веб-службы в облаке или на пограничных устройствах.
1. **Мониторинг и сбор данных**.
1. **Обновление** развертывания для использования нового образа.

Каждый шаг может выполняться независимо друг от друга или как часть одной команды развертывания. Кроме того, развертывание можно интегрировать в **рабочий процесс CI/CD** как показано на следующем рисунке.

[![«Машинное обучение azure непрерывной интеграции и цикл развертывания (CI/CD)»](media/concept-model-management-and-deployment/model-ci-cd.png)](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)

## <a name="step-1-register-model"></a>Шаг 1. Регистрация модели

Регистрация модели позволяет хранить и изменять модели в облаке Azure в рабочей области. Модель реестра позволяет легко организовывать и отслеживать ваши обученные модели.
 
Зарегистрированные модели идентифицируются по имени и версии. При регистрации модели с уже существующим именем реестр увеличивает номер версии. Во время регистрации также можно предоставить дополнительные теги метаданных, которые можно использовать при поиске моделей. Служба машинного обучения Azure поддерживает любые модели, которые можно скачать с помощью Python 3. 

Невозможно удалить модели, которые используются образом.

Дополнительные сведения см. в разделе "Регистрация модели" статьи [Развертывание моделей с помощью Службы машинного обучения Azure](how-to-deploy-and-where.md#registermodel).

Пример регистрации модели, сохраненной в сериализованном формате, см. в статье [Руководство. Развертывание модели классификации изображений в Экземплярах контейнеров Azure](tutorial-deploy-models-with-aml.md).

Сведения об использовании моделей ONNX см. в статье [ONNX и Машинное обучение Azure: создание и развертывание моделей ИИ с возможностью взаимодействия](how-to-build-deploy-onnx.md).

## <a name="step-2-register-image"></a>Шаг 2. Регистрация образа

Образы обеспечивают надежное развертывание модели, а также всех компонентов, необходимых для ее использования. Образ содержит следующие элементы:

* Модель
* модуль оценки;
* приложение или файл для оценки;
* все зависимости, необходимые для оценки модели.

Образ также может включать компоненты пакета SDK для ведения журнала и мониторинга. Данные журналов пакета SDK можно использовать для точной настройки или повторного обучения модели, включая входные и выходные данные модели.

Служба "Машинное обучение Azure" поддерживает самые популярные платформы, как правило все платформы, на которые можно установить pip.

При создании рабочей области также было создано несколько других используемых ею ресурсов Azure.
Все объекты, используемые для создания образа по умолчанию хранятся в учетной записи хранения Azure в рабочей области. При создании образа можно предоставить дополнительные теги метаданных. Теги метаданных также хранятся в реестре образов, их можно запрашивать для поиска образа.

Можно также использовать пользовательские образы, которые могут быть отправлены в реестр контейнеров Azure и с помощью службы машинного обучения Azure.

Дополнительные сведения см. в разделах о настройке и регистрации образа в статье [Развертывание моделей с помощью Службы машинного обучения Azure](how-to-deploy-and-where.md#configureimage).

## <a name="step-3-deploy-image"></a>Шаг 3. Развертывание образа

Зарегистрированные образы можно развернуть в облаке или на пограничных устройствах. В процессе развертывания создаются все ресурсы, необходимые для отслеживания, балансировки нагрузки и автомасштабирования модели. Доступ к развернутым службам может быть защищен с помощью аутентификации на основе сертификатов. Для этого во время развертывания нужно предоставить ресурсы безопасности. Чтобы использовать новый образ, можно также обновить имеющееся развертывание.

Развертывания веб-служб также доступны для поиска. Например, можно найти все развертывания конкретной модели или образа.

[![Целевые объекты выводов](media/concept-model-management-and-deployment/inferencing-targets.png)](media/concept-model-management-and-deployment/inferencing-targets.png#lightbox)

Образы можно развернуть на следующих целевых объектах развертывания в облаке:

* Экземпляр контейнера Azure
* Служба Azure Kubernetes
* компьютеры Azure FPGA;
* устройство Azure IoT Edge.

По мере развертывания службы запрос на вывод автоматически балансируется по нагрузке, а кластер масштабируется, если нужно справиться с пиковыми нагрузками. [Данные телеметрии о службе](how-to-enable-app-insights.md) можно получить через службу Azure Application Insights, связанную с рабочей областью.

Дополнительные сведения см. в разделе о развертывании статьи [Развертывание моделей с помощью Службы машинного обучения Azure](how-to-deploy-and-where.md#deploy).

## <a name="step-4-monitor-models-and-collect-data"></a>Шаг 4. Мониторинг моделей и сбор данных

Доступен пакет SDK для ведения журналов модели и записи данных, чтобы вы могли отслеживать выходные данные, входные данные и другие релевантные данные модели. Данные хранятся в виде большого двоичного объекта в учетной записи хранения Azure для рабочей области.

Чтобы использовать пакет SDK с моделью, импортируйте его в приложение или сценарий для оценки. Затем пакет SDK можно использовать для ведения журнала данных, таких как параметры, результаты и сведения о входных данных.

Если вы настроили включение сбора данных модели для всех развертываний образа, сведения, необходимые для записи данных, такие как учетные данные в личном хранилище BLOB-объектов, подготавливаются автоматически.

> [!Important]
> Корпорация Майкрософт не отслеживает данные, собираемые из модели. Данные отправляются напрямую в учетную запись хранения Azure для рабочей области.

Дополнительные сведения см. в статье [Сбор данных для моделей в рабочей среде](how-to-enable-data-collection.md).

## <a name="step-5-update-the-deployment"></a>Шаг 5. Обновление развертывания

Обновления модели не регистрируются автоматически. Аналогичным образом регистрация нового образа не обновляет автоматически развертывания, созданные с предыдущей версией образа. Вместо этого необходимо вручную зарегистрировать модель, образ и затем обновить модель. Дополнительные сведения см. в разделе об обновлении статьи [Развертывание моделей с помощью Службы машинного обучения Azure](how-to-deploy-and-where.md#update).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о том, как и где можно развертывать модели с помощью службы "Машинное обучение Azure", см. в [этой статье](how-to-deploy-and-where.md). Пример развертывания, см. в разделе [руководства: Развернуть модель классификации изображений в экземплярах контейнеров Azure](tutorial-deploy-models-with-aml.md).

Узнайте, как создавать клиентские приложения и службы, которые [используют модель, развернутую в качестве веб-службы](how-to-consume-web-service.md).
