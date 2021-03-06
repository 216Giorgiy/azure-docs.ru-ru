---
title: С помощью общего образа коллекции в службы лабораторий Azure | Документация Майкрософт
description: Сведения о настройке учетной записи лаборатории для использования общего образа коллекции, чтобы пользователь может поделиться изображение с другими, и другой пользователь может использовать образ для создания шаблона виртуальной Машины в лаборатории.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: spelluru
ms.openlocfilehash: 93136c7d685bd9fc8ec4bcdea3a900b28029059b
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2019
ms.locfileid: "58653216"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>С помощью общего образа коллекции в службы лабораторий Azure
В этой статье показано, как администратор преподавателей и лабораторий можно сохранить образ шаблона виртуальной машины для нее для повторного использования другими пользователями. Эти образы сохраняются в Azure [общего образа коллекции](../../virtual-machines/windows/shared-image-galleries.md). В качестве первого шага администратор лаборатории присоединяет существующей коллекцией общего образа к учетной записи лаборатории. После присоединения коллекции общих образов лабораторий, созданных в учетной записи лаборатории можно сохранить образы коллекции общих образов. Другие преподавателей можно выбрать образ из коллекции общего образа, чтобы создать шаблон для своих классов. 

## <a name="prerequisites"></a>Технические условия
Создать галерею общего образа с помощью [Azure PowerShell](../../virtual-machines/windows/shared-images.md) или [Azure CLI](../../virtual-machines/linux/shared-images.md).

## <a name="attach-a-shared-image-gallery-to-a-lab-account"></a>Присоединение коллекции общего образа к учетной записи лаборатории
Ниже показано, как подключить к учетной записи лаборатории общего образа коллекции. 

1. Войдите на [портале Azure](https://portal.azure.com).
2. В меню слева выберите **Все службы**. Выберите **службы лабораторий** в **DEVOPS** раздел. При выборе типа "звезда" (`*`) рядом с полем **службы лабораторий**, он добавляется в **"Избранное"** раздел, в меню слева. В следующее время и более поздних версий, выберите **службы лабораторий** под **"Избранное"**.

    ![Все службы -> службы лаборатории](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Выберите учетную запись лаборатории, чтобы просмотреть **учетной записи лаборатории** страницы. 
4. Выберите **коллекции образов Shared** в левом меню и выберите **Attach** на панели инструментов. 

    ![Общие изображение коллекции — кнопка "Добавить"](../media/how-to-use-shared-image-gallery/sig-attach-button.png)
5. На **Присоединение существующей коллекции образов Shared** выберите галерею общего образа и выберите **ОК**.

    ![Выбор существующей коллекцией](../media/how-to-use-shared-image-gallery/select-image-gallery.png)
6. Появится следующий экран: 

    ![Мои коллекции в списке](../media/how-to-use-shared-image-gallery/my-gallery-in-list.png)
    
    В этом примере существует еще изображения не в коллекции общих образов.

Удостоверение службы лабораторий Azure добавляется участник коллекции общего образа, подключенного к лабораторной среде. Он позволяет преподавателям / images ИТ-администратору для сохранения виртуальной машины в коллекцию общих образов. Всех занятий, созданных в этой лаборатории учетной записи имеют доступ к коллекции прикрепленного общего изображения. 

Все образы в коллекции прикрепленного общего изображения включены по умолчанию. Можно включить или отключить выбранные образы, выбрав их в списке и с помощью **включить выбранные образы** или **отключить выбранные образы** кнопки. 

## <a name="detach-a-shared-image-gallery"></a>Отсоединение коллекции общего образа
Только одна коллекция общих изображений можно прикреплять к лаборатории. Если вы хотите подключить другой коллекции общих образов, отсоедините за текущий год, прежде чем подключать новую. Чтобы отсоединить коллекцию общего образа из лаборатории, выберите **отсоединения** на панели инструментов и подтвердите операцию отсоединения. 

## <a name="save-an-image-to-the-shared-image-gallery"></a>Сохранить изображение в галерее общего образа
После присоединения коллекции общего образа преподаватель можно сохранить образ шаблона в коллекцию общих образов, таким образом, он может быть использован другими преподавателей.

![Сохраните образ виртуальной машины в коллекции.](../media/how-to-use-shared-image-gallery/save-virtual-machine.png)

## <a name="use-an-image-from-the-shared-image-gallery"></a>Использование образа из коллекции общего образа
Преподаватель/профессором можно выбрать пользовательский образ доступен в коллекции образов, общих для шаблона во время создания новой лабораторной.

![Использовать образ виртуальной машины из коллекции](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о коллекциях общего образа см. в разделе [общего образа коллекции](../../virtual-machines/windows/shared-image-galleries.md).
