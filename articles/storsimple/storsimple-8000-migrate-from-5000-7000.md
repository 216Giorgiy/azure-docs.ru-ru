---
title: Перенос данных с устройства StorSimple серий 5000–7000 на устройство серии 8000 | Документация Майкрософт
description: Предоставляет обзор и предварительные условия для функции миграции.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2018
ms.author: alkohli
ms.openlocfilehash: 967c03f3c4201bdcf1529fdda93717b6eb74e771
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2019
ms.locfileid: "55495859"
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-8000-series-device"></a>Перенос данных с устройства StorSimple серий 5000–7000 на устройство серии 8000

> [!IMPORTANT]
> - 31 июля 2019 года поддержка устройств StorSimple серий 5000/7000 будет прекращена. Мы рекомендуем клиентам устройств StorSimple серий 5000 и 7000 перейти на один из альтернативных вариантов, описанных в этом документе.
> - Сейчас миграции — это интерактивная операция. Если вы намерены перенести данные с устройства StorSimple 5000–7000 на устройство серии 8000, вам необходимо запланировать миграцию с помощью службы поддержки Майкрософт. Затем служба технической поддержки Майкрософт разрешит миграцию для вашей подписки. Дополнительные сведения см. в статье [Обращение в службу поддержки Майкрософт](storsimple-8000-contact-microsoft-support.md).
> - После того, как вы подадите запрос на обслуживание, на реализацию плана миграции и саму миграцию может потребоваться несколько недель.
> - Прежде чем обращаться в службу поддержки Майкрософт, обязательно просмотрите и выполните [предварительные требования для миграции](#migration-prerequisites), указанные в статье.

## <a name="overview"></a>Обзор

В этой статье описывается функция миграции, позволяющая клиентам устройств серии StorSimple 5000–7000 переносить свои данные на физическое устройство серии StorSimple 8000 или облачное устройство серии 8010 или 8020. В этой статье также содержится ссылка на скачиваемое руководство по шагам, необходимым для переноса данных с устаревшего устройства серий 5000–7000 на физическое или облачное устройство серии 8000.

Эта статья применима как к локальному устройству 8000, так и к облачному устройству StorSimple.


## <a name="migration-feature-versus-host-side-migration"></a>Функция миграции и миграция на стороне узла

Вы можете перемещать свои данные с помощью функции миграции или путем миграции на стороне узла. В этом разделе описаны особенности каждого метода, в том числе преимущества и недостатки. Эти сведения помогут вам выбрать подходящий метод миграции данных.

Функция миграции имитирует процесс аварийного восстановления (DR) с устройств серий 7000/5000 на устройство серии 8000. Эта функция позволяет перенести данные из формата серии 5000/7000 в формат серии 8000 в Azure. Процесс миграции запускается с использованием средства миграции StorSimple. Средство запускает скачивание и преобразование метаданных резервных копий на устройство серии 8000, а затем использует последнюю резервную копию для отображения томов на устройстве.

|      | Преимущества                                                                                                                                     |Недостатки                                                                                                                                                              |
|------|-------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | При миграции сохраняется журнал резервных копий, созданных на устройствах серии 5000/7000.                                               | Когда пользователи пытаются получить доступ к данным, процесс миграции будет загружать данные из Azure, что приведет к расходам за скачивание данных.                                     |
| 2.   | Данные на стороне узла не переносятся.                                                                                                     | Для процесса требуется время простоя между началом резервного копирования и последней резервной копией серии 8000 (можно рассчитать с помощью инструмента миграции). |
| 3.   | Этот процесс сохраняет все политики, шаблоны полосы пропускания, шифрование и другие настройки на устройствах серии 8000.                      | Пользователь получит доступ только к тем данным, доступ к которым получали пользователи. Весь набор данных не будет восстановлен.                                                  |
| 4.   | Этот процесс требует дополнительного времени для преобразования всех старых резервных копий в Azure, которые выполняются асинхронно, не влияя на производительность. | Миграция может выполняться только на уровне конфигурации облака.  Отдельные тома в конфигурации облака нельзя перенести независимо.                       |

Миграция на стороне узла позволяет отдельно настраивать устройство серии 8000 и копировать данные с устройств серии 5000/7000 на устройство серии 8000. Это эквивалентно переносу данных с одного устройства хранения данных на другое. Для копирования данных используются различные средства, такие как Diskboss или robocopy.

|      | Преимущества                                                                                                                      |Недостатки                                                                                                                                                                                                      |
|------|---------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Миграцию можно выполнять поэтапно по одному тому за раз.                                               | Предыдущие резервные копии (сделанные на устройствах серии 5000/7000) не будут доступны на устройстве серии 8000.                                                                                                       |
| 2.   | Позволяет консолидировать данные в одной учетной записи хранения в Azure.                                                       | На создание первой резервной копии в облаке на устройстве серии 8000 уйдет больше времени, так как все данные этого устройства нужно скопировать в Azure.                                                                     |
| 3.   | После успешной миграции все данные находятся на устройстве локально. При доступе к данным нет задержек. | Использование ресурсов хранилища Azure будет увеличиваться до тех пор, пока данные не будут удалены с устройства серии 5000/7000.                                                                                                        |
| 4.   |                                                                                                                           | Если устройство серии 7000/5000 имеет большой объем данных, во время миграции эти данные необходимо скачать из Azure, что приведет к повышению затрат и задержек, связанных с загрузкой данных из Azure. |

В этой статье основное внимание уделяется функции миграции с устройства 5000/7000 на устройство серии 8000. Дополнительные сведения о миграции на стороне узла см. в этом [документе](https://download.microsoft.com/download/9/4/A/94AB8165-CCC4-430B-801B-9FD40C8DA340/Migrating%20Data%20to%20StorSimple%20Volumes_09-02-15.pdf).

## <a name="migration-prerequisites"></a>Предварительные требования к миграции

Ниже перечислены требования к миграции с устаревшего устройства серии 5000/7000 на устройство StorSimple серии 8000.

> [!IMPORTANT]
> Перед отправкой запроса на обслуживание в службу поддержки Майкрософт просмотрите и выполните необходимые условия для миграции.

### <a name="for-the-50007000-series-device-source"></a>Для устройства серии 5000/7000 (источник)

Перед началом миграции убедитесь, что у вас есть следующие компоненты:

* У вас есть исходное устройство серии 5000/7000. Устройство может работать или быть отключено.

    > [!IMPORTANT]
    > Мы рекомендуем наличие последовательного доступа к этому устройству на протяжении всего процесса миграции. При возникновении каких-либо проблем с устройством последовательный доступ может помочь в устранении неполадок.

* На исходном устройстве серии 5000/7000 должно использоваться программное обеспечение версии 2.1.1.518 или более поздней. Более ранние версии не поддерживаются.
* Версию на вашем устройстве серии 5000/7000 можно увидеть в верхнем правом углу веб-интерфейса. Там отобразится версия программного обеспечения на вашем устройстве. Для миграции на вашем устройстве серий 5000/7000 должна быть запущена версия 2.1.1.518.

    ![Проверка версии программного обеспечения устаревшего устройства](media/storsimple-8000-migrate-from-5000-7000/check-version-legacy-device1.png)

    * Если версия отличается от 2.1.1.518, выполните обновление системы до минимальной требуемой версии. Подробные инструкции см. в статье [Software Patch Upgrade Guide v2.1.1.518](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518) (Руководство по обновлению программного обеспечения до версии 2.1.1.518).
    * Если вы используете версию 2.1.1.518, перейдите в веб-интерфейс, чтобы узнать, появились ли какие-либо уведомления о сбоях при восстановлении реестра. Если не удалось восстановить реестр, запустите восстановление реестра. Дополнительные сведения см. в разделе о [восстановлении резервной копии реестра](http://onlinehelp.storsimple.com/111_Appliance/2_User_Guides/1_Current_(v2.1.1)/1_Web_UI_User_Guide_WIP/2_Configuration/4_Cloud_Accounts/1_Cloud_Credentials#Restoring_Backup_Registry).
    * При наличии неработающего устройства с версией ПО 2.1.1.518 выполните отработку отказа на другое устройство, работающее под управлением ПО версии 2.1.1.518. Подробные инструкции см. в документации по аварийному восстановлению устройства StorSimple серии 5000/7000.
    * Создайте резервную копию данных для своего устройства, сделав моментальный снимок облака.
    * Проверьте наличие других активных заданий резервного копирования, которые выполняются на исходном устройстве. Сюда входят задания на узле StorSimple Data Protection Console. Дождитесь завершения текущих заданий.


### <a name="for-the-8000-series-physical-device-target"></a>Для физических устройств серии 8000 (целевой объект)

Перед началом миграции убедитесь, что у вас есть следующие компоненты:

* Зарегистрировано и запущено целевое устройство серии 8000. Дополнительные сведения см. в статье [Развертывание локального устройства StorSimple (с обновлением 3 и более поздней версии)](storsimple-8000-deployment-walkthrough-u2.md).
* На устройстве StorSimple серии 8000 установлена ​​последняя версия обновления 5 и устройство работает под управлением ПО 6.3.9600.17845 или более поздней версии. Если на вашем устройстве не установлены последние обновления, установите их, прежде чем продолжить миграцию. Дополнительные сведения см. в статье [Установка обновления 5 на устройство StorSimple](storsimple-8000-install-update-5.md).
* Подписке Azure готова к миграции. Если подписка не включена, обратитесь в службу поддержки Майкрософт, чтобы включить ее в план миграции.

### <a name="for-the-80108020-cloud-appliance-target"></a>Для облачного устройства 8010/8020 (целевой объект)

Перед началом миграции убедитесь, что у вас есть следующие компоненты:

* Целевое облачное устройство зарегистрировано и запущено. Дополнительные сведения см. в статье [Развертывание и администрирование облачного устройства StorSimple в Azure (обновление 3 и более поздние версии)](storsimple-8000-cloud-appliance-u2.md).
* В вашем облачном устройстве работает последняя версия программного обеспечения StorSimple 8000 Series (обновление 5 6.3.9600.17845). Если на вашем облачном устройстве не установлено обновление 5, создайте облачное устройство с обновлением 5, прежде чем продолжить миграцию. Дополнительные сведения см. в статье [Развертывание и администрирование облачного устройства StorSimple в Azure (обновление 3 и более поздние версии)](storsimple-8000-cloud-appliance-u2.md).

### <a name="for-the-computer-running-storsimple-migration-tool"></a>Для компьютера со средством миграции StorSimple

Средство миграции StorSimple — это средство на основе пользовательского интерфейса, которое позволяет выполнить миграцию данных с устройства StorSimple серии 5000/7000 на устройство серии 8000. Чтобы установить средство миграции StorSimple, используйте компьютер, отвечающий следующим требованиям.

Убедитесь, что компьютер подключен к Интернету, а также:

* На нем установлена следующая операционная система:
    * Windows 10;
    * Windows Server 2012 R2 (или более поздней версии) для установки средства миграции StorSimple.
* Установлена платформа .NET 4.5.2.
* На нем есть минимум 5 ГБ свободного места для установки и использования средства.

> [!TIP]
> Если устройство StorSimple подключено к узлу Windows Server, вы можете установить средство миграции на главном компьютере Windows Server.

#### <a name="to-install-storsimple-migration-tool"></a>Установка средства миграции StorSimple

Выполните следующие шаги для установки средства миграции StorSimple на своем компьютере.

1. Скопируйте папку _StorSimple8000SeriesMigrationTool_ на компьютер Windows. Убедитесь, что на диске, куда копируется программное обеспечение, достаточно свободного места.

    Откройте файл конфигурации средства _StorSimple8000SeriesMigrationTool.exe.config_ в папке. Ниже приведен фрагмент файла.
    
    ```xml
        <add key="UserName" value="username@xyz.com" />
        <add key="SubscriptionName" value="YourSubscriptionName" />
        <add key="SubscriptionId" value="YourSubscriptionId" />
        <add key="TenantId" value="YourTenantId" />
        <add key="ResourceName" value="YourResourceName" />
        <add key="ResourceGroupName" value="YourResourceGroupName" />

    ```
2. Измените значения, соответствующие ключам и замените на следующее:

    * `UserName` — имя пользователя для входа на портал Azure.
    * `SubscriptionName and SubscriptionId` — имя и идентификатор вашей подписки Azure. На целевой странице службы диспетчера StorSimple в разделе **Общие** щелкните **Свойства**. Скопируйте имя и идентификатор подписки, связанный с вашей службой.
    * `ResourceName` — имя службы диспетчера устройств StorSimple на портале Azure. Отображается также в поле свойства службы.
    * `ResourceGroup` — имя группы ресурсов, связанной со службой диспетчера устройства StorSimple на портале Azure. Отображается также в поле свойства службы.
    ![Проверка свойств службы для целевого устройства](media/storsimple-8000-migrate-from-5000-7000/check-service-properties1.png)
    * `TenantId` — клиент Azure Active Directory на портале Azure. Войдите в Microsoft Azure с правами администратора. На портале Microsoft Azure щелкните **Azure Active Directory**. В разделе **Управление** щелкните **Свойства**. Идентификатор клиента отображается в поле **Идентификатор каталога**.
    ![Проверка идентификатора клиента Azure Active Directory](media/storsimple-8000-migrate-from-5000-7000/check-tenantid-aad.png).

3.  Сохраните изменения, внесенные в файл конфигурации.
4.  Запустите _StorSimple8000SeriesMigrationTool.exe_ для запуска средства. При запросе учетных данных укажите учетные данные, связанные с подпиской на портале Azure. 
5.  Отобразится пользовательский интерфейс средства миграции StorSimple.
  

## <a name="next-steps"></a>Дополнительная информация
Скачайте пошаговые инструкции о том, как [перенести данные с устройства StorSimple серии 5000–7000 на устройство серии 8000](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b).
