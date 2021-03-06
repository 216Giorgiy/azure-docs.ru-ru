---
title: Известные проблемы с Azure Data Lake Storage 2-го поколения | Документация Майкрософт
description: Сведения об ограничениях и известных проблемах с Azure Data Lake Storage 2-го поколения
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: normesta
ms.openlocfilehash: d56fb411eb032e5e6227d68cd8abe02c0e30850b
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006868"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Известные проблемы с Azure Data Lake Storage 2-го поколения

Эта статья содержит известные проблемы и временных ограничений с Gen2 хранилища Озера данных.

## <a name="sdk-support-for-data-lake-storage-gen2-accounts"></a>Поддержка пакета SDK для учетных записей Gen2 хранилища Озера данных

Отсутствуют SDK, которые будут работать с учетными записями Gen2 хранилища Озера данных.

## <a name="blob-storage-apis"></a>API хранилища BLOB-объектов

API-интерфейсы хранилища BLOB-объектов, пока недоступны для учетных записей Gen2 хранилища Озера данных.

Эти API пока отключены, чтобы избежать случайных проблем с доступом к данным, которые могут возникать из-за временной несовместимости между API службы хранилища BLOB-объектов и API Azure Data Lake Storage 2-го поколения.

Если вы использовали эти API-интерфейсы для загрузки данных, прежде чем они были отключены, и существует требование рабочей среды для доступа к этим данным, обратитесь в службу поддержки Майкрософт, предоставив следующие сведения:

* идентификатор подписки (уникальный идентификатор, а не имя);

* имена учетных записей хранения;

* сведения о том, были ли вы активно вовлечены в производство и, если это так, то для какой учетной записи хранения;

* сведения о том, необходимо ли копировать данные в другую учетную запись хранения и, если это так, то по каким причинам (укажите эти данные, даже если вы не были активно вовлечены в производство).

В таком случае мы можем восстановить доступ к API BLOB-объектов на ограниченный период времени, чтобы вы смогли скопировать эти данные в учетную запись хранения, в которой не включена функция поддержки иерархического пространства имен.

Диски неуправляемых виртуальных машин зависят от отключенного API хранилища BLOB-объектов. Если вы хотите использовать в учетной записи хранения иерархические пространства имен, то можете разместить неуправляемые диски виртуальных машин в учетной записи хранения, для которой не включена функция иерархического пространства имен.

## <a name="api-interoperability"></a>Взаимодействие между API

API хранилища BLOB-объектов и Azure Data Lake Storage 2-го поколения несовместимы друг с другом.

Если у вас есть инструменты, службы, приложения или скрипты, которые используют API BLOB-объектов, и вы используете их для работы с содержимым, отправленным в учетную запись, не включайте иерархическое пространство имен на учетной записи хранения BLOB-объектов, пока API BLOB-объектов не получит возможность взаимодействия с API Azure Data Lake 2-го поколения. С помощью учетной записи хранения без иерархическое пространство имен означает, что затем у вас нет доступа к некоторым функциям Gen2 хранилища Озера данных, таких как каталоги и файлы системный список управления доступом.

## <a name="azure-storage-explorer"></a>обозреватель хранилищ Azure

Чтобы просмотреть учетные записи Data Lake Storage 2-го поколения или управлять ими с помощью Обозревателя службы хранилища Azure, необходимо иметь по крайней мере версию `1.6.0` средства, которое можно [бесплатно скачать](https://azure.microsoft.com/features/storage-explorer/).

Обратите внимание, что версия обозревателя службы хранилища, внедренный в портале Azure в настоящее время не поддерживает просмотр и управление учетными записями Gen2 хранилища Озера данных с включенной функцией иерархического пространства имен.

## <a name="blob-viewing-tool"></a>Средство просмотра больших двоичных объектов

BLOB-объектов, средстве просмотра на портале Azure только ограниченную поддержку Gen2 хранилища Озера данных.

## <a name="third-party-applications"></a>Сторонние приложения

Сторонние приложения могут не поддерживать Gen2 хранилища Озера данных.

Такая поддержка предоставляется по усмотрению поставщиков сторонних приложений. В настоящее время API-интерфейсы хранилища BLOB-объектов и API-интерфейсов Gen2 хранилища Озера данных не может использоваться для управления тем же содержимым. Так как мы работаем, чтобы включить такую возможность взаимодействия, вполне возможно, что многие сторонние средства будет автоматически поддерживать Gen2 хранилища Озера данных.

## <a name="azcopy-support"></a>Поддержка AzCopy

AzCopy версии 8 не поддерживает Gen2 хранилища Озера данных.

Вместо этого используйте последнюю предварительную версию AzCopy ( [AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json) ), так как он поддерживает конечные точки Gen2 хранилища Озера данных.

## <a name="azure-event-grid"></a>Сетка событий Azure

[Сетка событий Azure](https://azure.microsoft.com/services/event-grid/) не получает события из учетных записей Azure Data Lake Storage 2-го поколения, так как эти учетные записи пока их не создают.  

## <a name="soft-delete-and-snapshots"></a>Обратимое удаление и моментальные снимки

Удаление с возможностью восстановления и моментальных снимков недоступны для учетных записей Gen2 хранилища Озера данных.

Все возможности управления версиями, в том числе [моментальные снимки](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) и [обратимое удаление](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete), пока недоступны для учетных записей хранилища с функцией поддержки иерархического пространства имен.

## <a name="object-level-storage-tiers"></a>Уровни хранилища на уровне объектов

Уровни хранилища на уровне объектов ("горячий", "холодный" и "архивный") пока недоступны для учетных записей Azure Data Lake Storage 2-го поколения, но доступны для учетных записей хранилища без функции поддержки иерархических пространств имен.

## <a name="azure-blob-storage-lifecycle-management-policies"></a>Политики управления жизненным циклом хранилища BLOB-объектов Azure

Политики управления жизненным циклом Azure хранилище BLOB-объектов, пока недоступны для учетных записей Gen2 хранилища Озера данных.

Эти политики доступны для учетных записей хранилища без функции поддержки иерархических пространств имен.

## <a name="diagnostic-logs"></a>Журналы диагностики

Журналы диагностики недоступны для учетных записей Gen2 хранилища Озера данных.
