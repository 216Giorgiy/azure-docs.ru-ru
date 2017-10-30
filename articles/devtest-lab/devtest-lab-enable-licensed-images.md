---
title: "Активация лицензированных образов в лаборатории в Azure DevTest Labs | Документация Майкрософт"
description: "Узнайте, как активировать лицензированные образы в Azure DevTest Labs на портале Azure."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 221390d2-8d3b-4e1f-b454-43d33f8072b7
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: tarcher
ms.openlocfilehash: a74eff05285602574e45703dbe5b6caf074adecd
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="enable-a-licensed-image-in-your-lab-in-azure-devtest-labs"></a>Активация лицензированных образов в лаборатории в Azure DevTest Labs

В Azure DevTest Labs лицензированные образы содержат условия (обычно третьих сторон), которые необходимо принять, прежде чем образы станут доступными для пользователей в лаборатории. Следующие разделы содержат сведения о работе с лицензированными образами таким образом, чтобы они были доступны для использования при создании виртуальных машин.

## <a name="determining-whether-a-licensed-image-is-available-to-users"></a>Определение доступности лицензированного образа для пользователей
Чтобы создавать виртуальные машины на основе лицензированных образов, сначала необходимо убедиться, что были приняты условия их использования. Ниже показано, как можно просмотреть состояние предложения лицензированных образов и, если необходимо, принять условия их использования.

1. Выполните вход на [портал Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Щелкните **Другие службы**, а затем выберите в списке **DevTest Labs**.

1. Из списка лабораторий выберите нужную лабораторию.  

1. На панели слева в разделе **Параметры** выберите **Configuration and policies** (Конфигурация и политики).

1. На панели слева в разделе **VIRTUAL MACHINE BASES** (Базы виртуальных машин) выберите **Marketplace images** (Образы Marketplace). 

    ![Пункт меню образов Marketplace](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-marketplace-images.png)

    Список доступных образов Marketplace отображается, включая **состояние предложения** для каждого образа.

    ![Список образов Marketplace с состоянием предложения для каждого образа](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-offer-status.png)

    Для лицензированного образа отображаются следующие состояния предложения: 
    
    - **Terms accepted** (Условия приняты). Лицензированный образ доступен пользователям для создания виртуальных машин. 
    - **Terms review needed** (Требуется проверка условий). Лицензированный образ сейчас недоступен для пользователей. Нужно принять условия лицензии, прежде чем пользователи лабораторий смогут использовать их для создания виртуальных машин. 

## <a name="making-a-licensed-image-available-to-lab-users"></a>Предоставление пользователям лабораторий доступа к лицензированному образу
Чтобы убедиться, что лицензированный образ доступен для пользователей лабораторий, сначала владелец лаборатории с разрешениями администратора должен принять условия для этого лицензированного образа. При активации программного развертывания для подписки, связанной с лицензированным образом, будут автоматически приняты условия и заявления о конфиденциальности для этого образа. Дополнительные сведения о программном развертывании образов Marketplace см. в статье [Working with Marketplace Images on Azure Resource Manager](https://azure.microsoft.com/blog/working-with-marketplace-images-on-azure-resource-manager/) (Работа с образами Marketplace в Azure Resource Manager).

Выполните следующие действия, чтобы включить программное развертывание для лицензированных образов:

1. На [портале Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) в списке **образов Marketplace** определите лицензированный образ, к которому пользователи должны иметь доступ, но условия которого не приняты. Например, вы можете видеть виртуальную машину для обработки и анализа данных, для которой отображается состояние **Terms accepted** (Условия приняты) или **Terms review needed** (Требуется проверка условий).

    ![Настройка окна программного развертывания](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-licensed-images.png)

   > [!NOTE]
   > Виртуальные машины обработки и анализа данных — это образы виртуальных машин Azure, которые предварительно установлены, настроены и протестированы с помощью нескольких популярных инструментов, которые обычно используются для анализа данных, машинного обучения и обучения ИИ. Дополнительные сведения о виртуальных машинах для обработки и анализа данных см. в статье [Общие сведения о виртуальных машинах Linux и Windows для обработки и анализа данных](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview).
   >
   >

1. В столбце **состояния предложения** для образа выберите **Terms review needed** (Требуется проверка условий).

1. В окне настройки программного развертывания выберите **Включить**.

    ![Настройка окна программного развертывания](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-enable-programmatic-deployment.png)

   > [!IMPORTANT]
   > В окне настройки программного развертывания может отображаться несколько подписок. Убедитесь, что программное развертывание включено только для предполагаемой подписки.
   >
   >


1. Щелкните **Сохранить**. В списке образов Marketplace образ имеет состояние **Terms accepted** (Условия приняты) и доступен для создания виртуальных машин пользователями.

## <a name="related-blog-posts"></a>Связанные записи в блогах

- [Custom images or formulas? (Пользовательские изображения или формулы?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Копирование пользовательских образов между лабораториями для разработки и тестирования Azure)](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Дальнейшие действия

- [Добавление виртуальной машины с артефактами в лабораторию в Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md)
