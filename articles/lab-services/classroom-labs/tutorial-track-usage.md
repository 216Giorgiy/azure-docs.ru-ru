---
title: Отслеживание использования лаборатории в Службах лабораторий Azure | Документация Майкрософт
description: В этом руководстве вы, как создатель или владелец лаборатории, отследите использование своей лаборатории.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: e2831191905da1b9e0ad55131be9eaa7aa13950e
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2019
ms.locfileid: "55894366"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Руководство по Отслеживание использования лаборатории в Службах лабораторий Azure
В этом руководстве показано, как создатель или владелец лаборатории может отследить ее использование.

Вот какие действия выполняются в этом руководстве:

> [!div class="checklist"]
> * Просмотр пользователей, зарегистрированных в лаборатории.
> * Просмотр сведений об использовании виртуальных машин в лаборатории
> * Управление виртуальными машинами учащихся 


## <a name="view-users-registered-with-the-lab"></a>Просмотр пользователей, зарегистрированных в лаборатории

1. Перейдите на [веб-сайт Служб лабораторий Azure](https://labs.azure.com). 
2. Выберите **Вход** и введите свои учетные данные. Службы лабораторий Azure поддерживают учетные записи организации и учетные записи Майкрософт.
3. На странице **My labs** (Мои лаборатории) выберите лабораторию, использование которой вы хотите отследить. 
4. В меню слева выберите **Пользователи** или щелкните плитку **Пользователи**. Вы увидите учащихся, зарегистрированных в вашей лаборатории. Выберите **ссылку для регистрации**, скопируйте ее и отправьте любому учащемуся, который еще не зарегистрирован в вашей лаборатории. 

    ![Зарегистрированные пользователи](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Просмотр сведений об использовании виртуальных машин в лаборатории 

1. В меню слева выберите **Виртуальные машины**. 
2. Убедитесь, что вы видите состояние виртуальных машин и количество часов их работы. Время, которое владелец лаборатории тратит на виртуальную машину учащегося, не учитывается в значении времени использования, указанном в последнем столбце. 

    ![Использование виртуальных машин](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Управление виртуальными машинами учащихся 
Когда вы наведете указатель мыши на строку в списке виртуальных машин, вы увидите элементы управления для выполнения следующих задач (как показано на рисунке в предыдущем разделе): 

- Подключение к виртуальной машине
- Запуск виртуальной машины
- Остановка виртуальной машины
- Удаление виртуальной машины


![Элементы управления виртуальной машиной](../media/tutorial-track-usage/vm-controls.png)

Также можете использовать кнопки панели инструментов для запуска, остановки или удаления виртуальной машины. 



## <a name="next-steps"></a>Дополнительная информация
См. дополнительные сведения о [лабораториях для аудитории](how-to-manage-lab-accounts.md).
