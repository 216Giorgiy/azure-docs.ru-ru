---
title: Управление учетными записями лабораторий в Службе лабораторий Azure | Документация Майкрософт
description: Подробные сведения о том, как создавать, просматривать и удалять учетные записи лабораторий в подписке Azure.
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
ms.date: 02/07/2018
ms.author: spelluru
ms.openlocfilehash: f1194d8385d1e7ddcb906d0c8c3a2b56648e2547
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58120828"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Управление учетными записями лабораторий в Службах лабораторий Azure 
В службах Azure лаборатории учетной записи лаборатории — это контейнер для лаборатории управляемых типов, таких как лабораторные занятия в аудитории. Администратор задает учетную запись лаборатории с помощью Служб лабораторий Azure и предоставляет доступ владельцам лабораторий, которые могут создать лаборатории в учетной записи. В этой статье содержатся подробные сведения о том, как создавать, просматривать и удалять учетные записи лабораторий.

## <a name="create-a-lab-account"></a>Создание учетной записи лаборатории
Ниже показано, как на портале Azure создать учетную запись лаборатории с помощью Служб лабораторий Azure. 

1. Войдите на [портале Azure](https://portal.azure.com).
2. В меню слева выберите **Все службы**. Выберите **Учетные записи лабораторий** в разделе **DEVOPS**. При выборе звездочки (`*`) рядом с полем **Учетные записи лабораторий**, добавляется в раздел **Избранное** в меню слева. В следующий раз выберите **Учетные записи лабораторий** в разделе **Избранное**.

    !["Все службы" -> "Учетные записи лабораторий"](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. На странице **Учетные записи лабораторий** на панели инструментов выберите **Добавить**. 

    ![Выбор "Добавить" на странице "Учетные записи лабораторий"](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. На странице **Учетная запись лаборатории** сделайте следующее: 
    1. В поле **Lab account name** (Имя учетной записи лаборатории) введите имя. 
    2. Выберите **подписку Azure**, в которой нужно создать учетную запись лаборатории.
    3. Для **группы ресурсов** выберите **Создать** и введите имя группы ресурсов.
    4. В поле **Расположение** выберите расположение или регион, в котором нужно создать учетную запись лаборатории. 
    5. Для **одноранговой виртуальной сети**, выберите одноранговой виртуальной сети (VNet) для лабораторной сети. Лабораторий, созданных в этой учетной записи подключения к выбранной виртуальной сети и иметь доступ к ресурсам в выбранной виртуальной сети. 
    7. В поле **Allow lab creator to pick lab location** (Разрешить создателю лаборатории выбирать расположение лаборатории) укажите, нужно ли предоставить создателям лабораторий возможность выбирать расположение для лаборатории. По умолчанию этот параметр отключен. В таком случае создатели лабораторий не могут указать расположение для лабораторий, которые они создают. Лаборатории создаются в географическом расположении, которое находится ближе всего к учетной записи лаборатории. Если этот параметр включен, создатель лаборатории может выбрать расположение при создании лаборатории.      
    8. Нажмите кнопку **Создать**. 

        ![Окно создания учетной записи лаборатории](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. На панели инструментов (**Уведомления**) выберите **значок колокольчика**, подтвердите, что развертывание прошло успешно, а затем выберите **Перейти к ресурсу**. 

    Кроме того, на странице **Учетные записи лабораторий** выберите **Обновить** и выберите созданную учетную запись лаборатории. 

    ![Окно создания учетной записи лаборатории](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Отобразится следующая страница **учетной записи лаборатории**:

    ![Страница учетной записи лаборатории](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Добавление пользователя к роли создателя лаборатории
Чтобы настроить лабораторию для аудитории в учетной записи лаборатории, вы должны быть членом роли **Создатель лаборатории** в учетной записи лаборатории. Учетная запись, использованная для создания учетной записи лаборатории, автоматически добавляется в эту роль. Этот шаг можно пропустить, если вы планируете использовать ту же учетную запись для создания лаборатории для аудитории. Чтобы использовать другую учетную запись для создания лаборатории аудитории, сделайте следующее: 

Чтобы предоставить преподавателям разрешение на создание лаборатории для классов, добавьте их в роль **Создатель лаборатории**.

1. На странице **Учетная запись лаборатории** выберите **Управление доступом (IAM)** и на панели инструментов щелкните **+ Добавить назначение ролей**. 

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

## <a name="configure-the-lab-account"></a>Настройка учетной записи лаборатории
1. На **учетной записи лаборатории** выберите **конфигурация Labs** в меню слева.

    ![Страница конфигурации лабораторий](../media/how-to-manage-lab-accounts/labs-configuration-page.png) 
1. Для **одноранговой виртуальной сети**выберите **включено** или **отключено**. Значение по умолчанию — **отключено**. Чтобы включить для одноранговой виртуальной сети, выполните следующие действия: 
    1. Щелкните **Включено**.
    2. Выберите **виртуальной сети** из раскрывающегося списка. 
    3. На панели инструментов щелкните **Сохранить**. 
    
        Для выбранной виртуальной сети подключены лабораторий, созданных в этой учетной записи. Они получат доступ к ресурсам в выбранной виртуальной сети. 
3. Для **разрешить создатель лаборатории для выбора расположения лаборатории**выберите **включено** Если вы хотите, чтобы создатель лаборатории, чтобы иметь возможность выбрать расположение для лаборатории. Если она отключена, лабораторные работы автоматически создаются в том же расположении, в котором существует учетной записи лаборатории. 

## <a name="view-lab-accounts"></a>Просмотр учетных записей лаборатории
1. Войдите на [портале Azure](https://portal.azure.com).
2. В меню выберите **Все ресурсы**. 
3. Выберите **Учетные записи лабораторий** для **типа**. 
    Вы также можете выполнять фильтрацию по подписке, группе ресурсов, расположению и тегам. 

    ![Все ресурсы -> Учетные записи лабораторий](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)

## <a name="view-and-manage-labs-in-the-lab-account"></a>Просмотр и управление лабораториями в учетной записи лаборатории

1. На странице **Учетная запись лаборатории** в меню слева выберите **Лаборатории**.

    ![Лаборатории учетной записи](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. В учетной записи вы увидите **список лабораторий** с указанной следующей информацией: 
    1. Имя лаборатории.
    2. Дата создания лаборатории. 
    3. Адрес электронной почты пользователя, создавшего лабораторию. 
    4. Максимальное число пользователей, которые могут работать в лаборатории. 
    5. Статус лаборатории. 



## <a name="delete-a-lab-in-the-lab-account"></a>Удаление лаборатории в учетной записи лаборатории
Следуйте инструкциям в предыдущем разделе, чтобы просмотреть список лаборатории в учетной записи лаборатории.

1. Выберите **... (многоточие)** и выберите **Удалить**. 

    ![Кнопка удаления лаборатории](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. Нажмите в предупреждающем сообщении **Да**. 

    ![Подтверждение удаления лаборатории](../media/how-to-manage-lab-accounts/confirm-lab-delete.png)

## <a name="delete-a-lab-account"></a>Удаление учетной записи лаборатории
Следуйте инструкциям из предыдущего раздела, в котором учетные записи лаборатории отображаются в виде списка. Чтобы удалить учетную запись лаборатории, сделайте следующее: 

1. Выберите **учетную запись лаборатории**, которую нужно удалить. 
2. На панели инструментов выберите **Удалить**. 

    ![Учетные записи лабораторий -> кнопка "Удалить"](../media/how-to-manage-lab-accounts/delete-button.png)
1. Для подтверждения действия щелкните **Да**.
1. Нажмите кнопку **Удалить**. 

    ![Подтверждение удаления учетной записи лаборатории](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)



## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь со следующими статьями:

- [Создание и администрирование учетных записей лаборатории (для владельцев лаборатории)](how-to-manage-classroom-labs.md)
- [Настройка и публикация шаблонов (для владельцев лаборатории)](how-to-create-manage-template.md)
- [Настройка, администрирование и контроль использования лаборатории (для владельцев лаборатории)](how-to-configure-student-usage.md)
- [Доступ к лабораториям для аудитории (для пользователей лаборатории)](how-to-use-classroom-lab.md)