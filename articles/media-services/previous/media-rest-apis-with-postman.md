---
title: Настройка Postman для вызовов REST API служб мультимедиа Azure
description: Узнайте, как настроить Postman для вызовов REST API служб мультимедиа Azure
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: da52827107fcf1672047644d2d5ac5ed2e6beed5
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58007746"
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Настройка Postman для вызовов REST API служб мультимедиа  

В этом руководстве описано, как настроить **Postman** для вызова REST API служб мультимедиа Azure (AMS). В руководстве также показано, как импортировать файлы среды и коллекции в **Postman**. Коллекция содержит сгруппированные определения HTTP-запросов, которые вызывают REST API служб мультимедиа Azure (AMS). Файл среды содержит переменные, которые используются коллекцией.

Среда и коллекция описаны в руководствах по выполнению различных задач с помощью REST API служб мультимедиа Azure.

## <a name="prerequisites"></a>Технические условия

- Установите клиент REST [Postman](https://www.getpostman.com/) для выполнения REST API, как показано в некоторых руководствах по REST AMS. 

    Мы используем **Postman**, но подойдет любое средство REST. Другие варианты включают: **Visual Studio Code** с подключаемым модулем REST или **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>Настройка среды 

1. Создайте JSON-файл, содержащий переменные среды, используемые в руководствах по AMS. Назовите файл (например, **AzureMediaServices.postman_environment.json**). Откройте файл и вставьте код, который определяет среду Postman из [этого примера кода](postman-environment.md). 
2. Откройте **Postman**.
3. В правой части экрана выберите параметр **Управление средой**.

    ![Отправка файла](./media/media-services-rest-upload-files/postman-create-env.png)
4. В диалоговом окне **Управление средой** нажмите кнопку **Импорт**.
5. Найдите и выберите файл **AzureMediaServices.postman_environment.json**.
6. Будет добавлена среда **AzureMedia**.
7. Закройте диалоговое окно.
8. Выберите среду **AzureMedia**.

    ![Отправка файла](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Настройка коллекции

1. Создайте JSON-файл, содержащий коллекцию **Postman** со всеми операциями, которые необходимы для передачи файла в службы мультимедиа. Назовите файл (например, **AzureMediaServicesOperations.postman_collection.json**). Откройте файл и вставьте код, который определяет коллекцию **Postman** из этого [примера кода](postman-collection.md).
2. Нажмите кнопку **импорта**, чтобы импортировать файл коллекции.
3. Выберите файл **AzureMediaServicesOperations.postman_collection.json**.

    ![Отправка файла](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Дальнейшие действия

См. руководство по [передаче файлов](media-services-rest-upload-files.md).  
