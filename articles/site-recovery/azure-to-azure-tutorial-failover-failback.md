---
title: Отработка отказа и повторное включение защиты виртуальных машин Azure, реплицированных в дополнительный регион Azure, в случае аварийного восстановления с помощью службы Azure Site Recovery.
description: Узнайте, как выполнять отработку отказа и повторное включение защиты виртуальных машин Azure, реплицированных в дополнительный регион Azure, в случае аварийного восстановления с помощью службы Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 96e3c0b761a9ed4c5f84d8ece1ba504bd5aacf6f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59797573"
---
# <a name="fail-over-and-reprotect-azure-vms-between-regions"></a>Отработка отказа и повторная защита виртуальных машин Azure между регионами

Служба [Azure Site Recovery](site-recovery-overview.md) помогает реализовать стратегию аварийного восстановления за счет управления процессами репликации, отработки отказа и восстановления локальных компьютеров и виртуальных машин Azure.

Этот учебник содержит сведения об отработке отказа виртуальной машины Azure в дополнительный регион Azure. После выполнения отработки отказа нужно повторно включить защиту виртуальной машины. Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Отработка отказа виртуальной машины Azure
> * Выполните повторную защиту дополнительной виртуальной машины Azure, чтобы обеспечить ее репликацию в основной регион.

> [!NOTE]
> Этот учебник содержит самый простой путь с параметрами по умолчанию и минимальной настройкой. Сведения о более сложных сценариях см. в соответствующих руководствах по виртуальным машинам Azure.

## <a name="prerequisites"></a>Предварительные требования

- Обязательно выполните [отработку аварийного восстановления](azure-to-azure-tutorial-dr-drill.md), чтобы убедиться в надлежащей работе всех компонентов.
- Перед запуском тестовой отработки отказа проверьте свойства виртуальной машины. Виртуальная машина должна соответствовать [требованиям Azure](azure-to-azure-support-matrix.md#replicated-machine-operating-systems).

## <a name="run-a-failover-to-the-secondary-region"></a>Запуск отработки отказа в дополнительный регион

1. В разделе **Реплицированные элементы** выберите виртуальную машину, для которой требуется отработка отказа, а затем выберите **Отработка отказа**

   ![Отработка отказа](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. В разделе **Отработка отказа** выберите **точку восстановления**, в которую будет выполнена отработка отказа. Можно выбрать один из следующих вариантов:

   * **Последние** (по умолчанию): Выполняет обработку всех данных в службе Site Recovery и обеспечивает наименьшую целевую точку восстановления (RPO).
   * **Последняя обработанная**. Отменяет изменения для виртуальной машины до последней точки восстановления из числа обработанных службой Site Recovery.
   * **Настраиваемая**. Отработка отказа выполняется до определенной точки восстановления. Этот параметр удобен для выполнения тестовой отработки отказа.

3. Выберите **Shut down machine before beginning failover** (Завершение работы виртуальной машины перед началом отработки отказа), чтобы служба Site Recovery попыталась выключить исходные виртуальные машины, прежде чем запускать отработку отказа. Отработка отказа продолжится, даже если их не удастся отключить. Site Recovery не поддерживает очистку источника после отработки отказа.

4. На странице **Задания** можно следить за ходом выполнения отработки отказа.

5. После отработки отказа войдите на виртуальную машину, чтобы проверить ее работу. Если вы хотите использовать для виртуальной машины другую точку восстановления, выберите команду **Изменить точку восстановления**.

6. Когда вы убедитесь, что после отработки отказа виртуальная машина работает правильно, **зафиксируйте** отработку отказа.
   При фиксации удаляются все доступные в службе точки восстановления. Теперь вы не сможете изменить точку восстановления.

## <a name="reprotect-the-secondary-vm"></a>Повторная защита дополнительной виртуальной машины

После отработки отказа виртуальной машины нужно снова защитить ее, чтобы она реплицировалась в основной регион.

1. Убедитесь, что виртуальная машина находится в состоянии **Отработка отказа зафиксирована**, основной регион доступен и вы можете создавать и использовать новые ресурсы в нем.
2. В разделе **Хранилище** > **Реплицированные элементы** щелкните виртуальную машину, для которой проводилась отработка отказа, щелкните правой кнопкой мыши и выберите **Защитить повторно**.

   ![Щелкните правой кнопкой мыши для повторной защиты](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Убедитесь, что направление защиты — из дополнительного региона в основной — уже выбрано.
3. Просмотрите информацию о **группе ресурсов, сети, хранилище и группах доступности**. Все ресурсы, помеченные как новые, создаются во время повторного включения защиты.
4. Нажмите кнопку **ОК**, чтобы запустить задание повторной защиты. Оно заполняет целевой сайт актуальными данными, а затем реплицирует отличия в основной регион. Теперь виртуальная машина находится в защищенном состоянии.

## <a name="next-steps"></a>Дополнительная информация
- После повторного включения защиты [узнайте, как](azure-to-azure-tutorial-failback.md) восстановить размещение в основном регионе в тех ситуациях, где это возможно.
- Подробнее о [процессе применения повторной защиты](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection).
