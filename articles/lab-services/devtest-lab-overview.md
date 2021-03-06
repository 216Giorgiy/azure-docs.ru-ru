---
title: Сведения об Azure DevTest Labs | Документы Майкрософт
description: Узнайте, как DevTest Labs может упростить создание и отслеживание виртуальных машин Azure, а также управление ими.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1b9eed3b-c69a-4c49-a36e-f388efea6f39
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: b7cd6bb1fd0377ca1440d9c667453df922aacbd4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698662"
---
# <a name="about-azure-devtest-labs"></a>Сведения об Azure DevTest Labs
Azure DevTest Labs позволяет разработчикам в командах эффективно Самостоятельное управление виртуальных машин (ВМ) и ресурсов PaaS без ожидания утверждения.

DevTest Labs создает лабораторные занятия, состоящий из предварительно настроенных базовых классов или шаблонов Azure Resource Manager. Они имеют все необходимые инструменты и программное обеспечение, которое можно использовать для создания сред. Через несколько минут, в отличие от нескольких часов или дней, могут создавать среды.

С помощью DevTest Labs, вы можете протестировать последнюю версию приложения, выполнив следующие задачи:

- Быстро Подготовьте среды Windows и Linux с помощью многократно используемых шаблонов и артефактов.
- Для подготовки сред по запросу интегрируйте с DevTest Labs конвейер развертывания.
- Увеличьте нагрузочное тестирование путем предоставления нескольких агентов тестирования и создайте предварительно подготовленные среды для обучения и демонстраций.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]

## <a name="capabilities"></a>Возможности
DevTest Labs предоставляют следующие возможности для разработчиков, работе с виртуальными машинами.

- Быстро Создайте виртуальные машины, следуя меньше пяти простых шагов.
- Выберите проверенный список баз виртуальных Машин, которые настраиваются, утверждения и авторизуется руководитель команды или центрального ИТ.
- Создайте виртуальные машины на основе предварительно созданных пользовательских образов, которые имеют все программное обеспечение и средства, которые уже установлены. 
- Создайте виртуальные машины из формулы, которые по сути пользовательских образов, в сочетании с последних сборок программного обеспечения, которая устанавливается при создании виртуальных машин. 
- Установка артефактов, которые являются расширениями, развернутом на виртуальных машинах, после они подготовлены.
- Настроить автозавершение работы и автоматический запуск расписаний на виртуальных машинах.
- Утверждение предварительно созданной виртуальной Машины, минуя процесс создания.

DevTest Labs предоставляет следующие возможности для разработчиков, работающих с сред PaaS:

- Используйте Resource Manager для быстрого создания сред PaaS, следуя меньше трех простых шагов.
- Выберите проверенный список шаблонов Resource Manager, которые настраиваются и авторизуется руководитель команды или центрального ИТ.
- Запускайте пустую группу ресурсов (песочница) с помощью шаблона диспетчера ресурсов для изучения Azure в контексте лаборатории.

DevTest Labs позволяет центрального ИТ-специалистам управлять пустая трата, оптимизировать затраты на ресурсы и оставаться в пределах бюджета, выполнив следующие задачи:  

- Настройка расписания автоматического завершения работы и автоматического запуска на виртуальных машинах.
- Настройка политик на число виртуальных машин, создаваемых пользователями.
- Настройка политик о размерах виртуальных машин и пользователям выбрать один из образов из коллекции.
- Отслеживания затрат и установив параметр целевых объектов в лаборатории.
- Уведомляющее на высокой прогнозируемых затрат для лабораторных работ, чтобы вы могли использовать необходимые действия.

DevTest Labs предоставляет следующие преимущества в создание, Настройка и управление средами в облаке.

## <a name="cost-control-and-governance"></a>Управление затратами и контроль над
DevTest Labs облегчает контроль расходов, позволяя выполнять следующие задачи:

- [Установка политик для своей лаборатории](devtest-lab-get-started-with-lab-policies.md), например количество виртуальных машин на пользователя или на лабораторию. 
- Создание [политики для автоматического завершения работы](devtest-lab-set-lab-policy.md) и запуск виртуальных машин.
- Отслеживать затраты на ресурсы виртуальных машин и PaaS историях внутри лаборатории, чтобы оставаться в пределах [бюджета](devtest-lab-configure-cost-management.md).
- Оставаться в контексте своей лаборатории, поэтому вы не развертывать ресурсы за пределами их.

## <a name="quickly-get-to-ready-to-test"></a>Быстрый переход в состояние "Готово к тестированию"
DevTest Labs позволяет создавать готовые среды, все, что необходимо вашей команде для разработки и тестирования приложений. Просто [утвердите среды](devtest-lab-add-claimable-vm.md) котором установлена последняя Проверенная версия вашего приложения и начала работы. Или использовать контейнеры для создания среды еще быстрее, экономнее.

## <a name="create-once-use-everywhere"></a>Создайте один раз и используйте везде
Захват и совместное использование PaaS [шаблонов среды](devtest-lab-create-environment-from-arm.md) и [артефакты](add-artifact-repository.md) внутри вашей команды или организации — все в системе управления версиями, легко создавать разработчика и тестовых сред.

## <a name="worry-free-self-service"></a>Самообслуживание без проблем
Лаборатория для разработки и позволяет разработчикам и тест-инженерам быстро и легко [создавать виртуальные машины IaaS](devtest-lab-add-vm.md) и [ресурсы PaaS](devtest-lab-create-environment-from-arm.md) с помощью набора предварительно настроенных ресурсов.

## <a name="use-iaas-and-paas-resources"></a>Использовать ресурсы IaaS и PaaS 
Разработчики могут также быстро ресурсы PaaS, такие как кластеров Azure Service Fabric, компонент веб-приложений службы приложений Azure и ферм SharePoint, с помощью шаблонов Resource Manager. Чтобы приступить к работе на PaaS в лабораториях, используйте шаблоны из [репозитория общедоступной среде](devtest-lab-configure-use-public-environments.md) или [подключение лаборатории в репозиторий Git](devtest-lab-create-environment-from-arm.md#configure-your-own-template-repositories). Вы также можете отслеживать затраты на эти ресурсы, чтобы оставаться в пределах вашего бюджета.

## <a name="integrate-with-your-existing-toolchain"></a>Интеграция с вашей цепочкой инструментов
Используйте готовые подключаемые модули или API для развертывания сред для разработки и тестирования непосредственно из предпочитаемого [средства непрерывной интеграции (CI)](devtest-lab-integrate-ci-cd-vsts.md), интегрированная среда разработки (IDE) или автоматизированного конвейера выпусков. Можно также использовать полнофункциональное средство командной строки.

## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь со следующими статьями:

- Дополнительные сведения о DevTest Labs, см. в разделе [основные понятия DevTest Labs](devtest-lab-concepts.md).
- Пошаговое руководство с пошаговыми инструкциями см. в разделе [руководства: Настройка лаборатории с помощью Azure DevTest Labs](tutorial-create-custom-lab.md).