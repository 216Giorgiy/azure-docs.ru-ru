---
title: Поддержка диспетчера рендеринга — пакетная служба Azure
description: Использование Azure для рендеринга с использованием интеграции диспетчера рендеринга в пакетную службу Azure
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: bcc66a73e3d7986b177b13eb309ad664a006b960
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2019
ms.locfileid: "57790067"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Использование пакетной службы Azure с диспетчерами фермы рендеринга

Если вы используете существующую локальную ферму рендеринга, скорее всего для управления ее емкостью и заданиями рендеринга настроен диспетчер рендеринга.

Azure предоставляет встроенную поддержку или поддержку через надстройки для многих популярных диспетчеров рендеринга. Это позволяет добавлять и удалять виртуальные машины Azure, включая содержащие лицензии приложений с оплатой по мере использования и (или) низкоприоритетные виртуальные машины.

Поддерживаются следующие диспетчеры рендеринга:

* [PipelineFX Qube!](https://www.pipelinefx.com/);
* [Royal Render](http://www.royalrender.de/);
* [Thinkbox Deadline](https://deadline.thinkboxsoftware.com/).

## <a name="using-azure-with-pipelinefx-qube"></a>Использование Azure с PipelineFX Qube

Соответствующие скрипты и инструкции по использованию виртуальных машин пула пакетной службы Azure в качестве рабочих ролей Qube вы можете получить в [репозитории GitHub](https://github.com/Azure/azure-qube).

## <a name="using-azure-with-royal-render"></a>Использование Azure с Royal Render

Для Royal Render в Azure и пакетной службе Azure реализована встроенная поддержка интеграции, что позволяет расширить ферму рендеринга за счет виртуальных машин на платформе Azure. Краткое описание этого варианта вы найдете в [файлах справки](http://www.royalrender.de/help8/index.html?Cloudrendering.html).

Пример клиента Royal Render с интеграцией в Azure можно изучить в статье [об истории клиента Jellyfish Pictures](https://customers.microsoft.com/story/jellyfishpictures).

## <a name="using-azure-with-thinkbox-deadline"></a>Использование Azure с Thinkbox Deadline

Соответствующие скрипты и инструкции по использованию виртуальных машин пула пакетной службы Azure в качестве ведомых узлов Deadline вы можете получить в [репозитории GitHub](https://github.com/Azure/azure-deadline).

## <a name="next-steps"></a>Дальнейшие действия

Попробуйте настроить интеграцию пакетной службы Azure с используемым диспетчером рендеринга, применяя соответствующий подключаемый модуль и инструкции на сайте GitHub, если потребуется.