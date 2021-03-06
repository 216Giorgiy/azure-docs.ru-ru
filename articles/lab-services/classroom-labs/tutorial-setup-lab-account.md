---
title: Настройка учетной записи лаборатории с помощью Служб лабораторий Azure | Документация Майкрософт
description: С помощью этого руководства вы настроите учетную запись лаборатории с помощью Служб лабораторий Azure.
services: devtest-lab, lab-services, virtual-machines
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
ms.date: 03/29/2019
ms.author: spelluru
ms.openlocfilehash: 962b69a97b8116b82878a0a82960c9159091a9a7
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652626"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Руководство по Настройка учетной записи лаборатории с помощью Служб лабораторий Azure
В Службах лабораторий Azure учетная запись лаборатории является центральной учетной записью, в которой управляются все лаборатории организации. В учетной записи лаборатории разрешите другим пользователям создавать лаборатории и задайте политики, которые применяются ко всем лаборатория под учетной записью лаборатории. В этом руководстве описано, как в качестве администратора создадите учетную запись лаборатории. 

Вот какие действия выполняются в этом руководстве:

> [!div class="checklist"]
> * создание учетной записи лаборатории;
> * Добавление пользователя к роли создателя лаборатории
> * Выбор образов Marketplace, доступных для владельцев лаборатории

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="create-a-lab-account"></a>Создание учетной записи лаборатории
Ниже показано, как на портале Azure создать учетную запись лаборатории с помощью Служб лабораторий Azure. 

1. Войдите на [портале Azure](https://portal.azure.com).
2. В меню слева выберите **Все службы**. Выберите **Службы лабораторий** в разделе **DevOps**. При выборе звездочки (`*`) рядом с полем **Службы лабораторий** оно добавляется в раздел **Избранное** в меню слева. В следующий раз выберите **Службы лабораторий** в разделе **Избранное**.

    ![Все службы -> Службы лабораторий](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. На странице **Службы лабораторий** на панели инструментов выберите **Добавить**. 

    ![Выбор "Добавить" на странице "Учетные записи лабораторий"](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. На странице **Учетная запись лаборатории** сделайте следующее: 
    1. В поле **Lab account name** (Имя учетной записи лаборатории) введите имя. 
    2. Выберите **подписку Azure**, в которой нужно создать учетную запись лаборатории.
    3. Для **группы ресурсов** выберите **Создать** и введите имя группы ресурсов.
    4. В поле **Расположение** выберите расположение или регион, в котором нужно создать учетную запись лаборатории. 
    5. В меню **Одноранговая виртуальная сеть** выберите одноранговую виртуальную сеть (VNet) для сети лаборатории. Лаборатории, созданные в этой учетной записи, подключаются к выбранной виртуальной сети и получают доступ к ресурсам в выбранной виртуальной сети. 
    6. В поле **Allow lab creator to pick lab location** (Разрешить создателю лаборатории выбирать расположение лаборатории) укажите, нужно ли предоставить создателям лабораторий возможность выбирать расположение для лаборатории. По умолчанию этот параметр отключен. В таком случае создатели лабораторий не могут указать расположение для лабораторий, которые они создают. Лаборатории создаются в географическом расположении, которое находится ближе всего к учетной записи лаборатории. Если этот параметр включен, создатель лаборатории может выбрать расположение при создании лаборатории. 
    7. Нажмите кнопку **Создать**. 

        ![Окно создания учетной записи лаборатории](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. На панели инструментов (**Уведомления**) выберите **значок колокольчика**, подтвердите, что развертывание прошло успешно, а затем выберите **Перейти к ресурсу**. 

    Кроме того, на странице **Учетные записи лабораторий** выберите **Обновить** и выберите созданную учетную запись лаборатории. 

    ![Окно создания учетной записи лаборатории](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Отобразится следующая страница **учетной записи лаборатории**:

    ![Страница учетной записи лаборатории](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Добавление пользователя к роли создателя лаборатории
Чтобы настроить лабораторию для аудитории в учетной записи лаборатории, вы должны быть членом роли **Создатель лаборатории** в учетной записи лаборатории. Учетная запись, использованная для создания учетной записи лаборатории, автоматически добавляется в эту роль. Этот шаг можно пропустить, если вы планируете использовать ту же учетную запись для создания лаборатории для аудитории. Чтобы использовать другую учетную запись для создания лаборатории аудитории, сделайте следующее: 

Чтобы предоставить преподавателям разрешение на создание лаборатории для классов, добавьте их в роль **Создатель лаборатории**.

1. На странице **Учетная запись лаборатории** выберите **Управление доступом (IAM)** и на панели инструментов щелкните **+ Добавить** и выберите **+ Добавить назначение ролей**. 

    !["Управление доступом" -> кнопка "Добавить назначение ролей"](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. На странице **Добавление назначения ролей** для **роли** выберите **Создатель лаборатории**, выберите пользователя, которого нужно добавить к роли создателя лаборатории, а затем нажмите кнопку **Сохранить**. 

    ![Добавление создателя лаборатории](../media/tutorial-setup-lab-account/add-lab-creator.png)

## <a name="specify-marketplace-images-available-to-lab-creators"></a>Выбор образов Marketplace, доступных для создателей лаборатории
Являясь владельцем учетной записи лаборатории, вы можете указать образы Marketplace, которые можно использовать при создании лабораторий в учетной записи лаборатории. 

1. В меню слева выберите **Образы Marketplace**. По умолчанию отображается полный список образов (включенных и отключенных). В раскрывающемся списке вверху можно отфильтровать список, чтобы увидеть только включенные или отключенные образы, выбрав **Только включенные**/**Только отключенные**. 
    
    ![Страница с образами Marketplace](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    В списке отображаются только образы Marketplace, которые соответствуют следующим условиям:
        
    - позволяют создать одну виртуальную машину;
    - используют диспетчер ресурсов Azure для подготовки виртуальных машин;
    - не требуют приобретения дополнительного плана лицензирования.
2. Чтобы **отключить** включенный образ Marketplace, выполните одно из следующих действий: 
    1. Щелкните **... (многоточие)** в последнем столбце и выберите **Отключить образ**. 

        ![Отключение одного образа](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Выберите один или несколько образов в списке, установив флажки перед их именами, и щелкните **Отключить выбранные образы**. 

        ![Отключение нескольких образов](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Аналогично, чтобы **включить** отключенный образ Marketplace, выполните одно из следующих действий: 
    1. Щелкните **... (многоточие)** в последнем столбце и выберите **Включить образ**. 
    2. Выберите один или несколько образов в списке, установив флажки перед их именами, и щелкните **Включить выбранные образы**. 

## <a name="next-steps"></a>Дополнительная информация
В рамках этого руководства вы создали учетную запись лаборатории. Сведения о создании лаборатории для аудитории см. в следующем руководстве:

> [!div class="nextstepaction"]
> [Руководство по настройке лаборатории для аудитории](tutorial-setup-classroom-lab.md)

