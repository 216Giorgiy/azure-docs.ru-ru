---
title: "Как параметризовать файлы конфигурации в Azure Service Fabric | Документация Майкрософт"
description: "Описывается, как параметризовать файлы конфигурации в Service Fabric."
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: 1e7d59ecb231440711b8ed3dc0b27a2b105890c4
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Как параметризовать файлы конфигурации в Service Fabric

В этой статье описывается, как параметризовать файл конфигурации в Service Fabric.

## <a name="procedure-for-parameterizing-configuration-files"></a>Процедура параметризации файлов конфигурации

В этом примере с помощью параметров в развертывании приложения переопределяется значение конфигурации.

1. Откройте файл Config\Settings.xml.
1. Задайте параметра конфигурации, добавив приведенный ниже код XML.

    ```xml
      <Section Name="MyConfigSection">
        <Parameter Name="CacheSize" Value="25" />
      </Section>
    ```

1. Сохраните и закройте файл.
1. Откройте файл `ApplicationManifest.xml` .
1. Добавьте элемент `ConfigOverride`, ссылающийся на пакет конфигурации, раздел и параметр.

      ```xml
        <ConfigOverrides>
          <ConfigOverride Name="Config">
              <Settings>
                <Section Name="MyConfigSection">
                    <Parameter Name="CacheSize" Value="[Stateless1_CacheSize]" />
                </Section>
              </Settings>
          </ConfigOverride>
        </ConfigOverrides>
      ```

1. Затем в файле ApplicationManifest.xml можно указать этот параметр в элементе `Parameters`.

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" />
      </Parameters>
    ```

1. После этого определите `DefaultValue`.

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" DefaultValue="80" />
      </Parameters>
    ```

> [!NOTE]
> В случае добавления ConfigOverride служба Service Fabric всегда выбирает параметры приложения или значение по умолчанию, указанное в манифесте приложения.
>
>

При публикации приложения из Visual Studio с помощью профиля публикации Cloud.xml служба будет использовать порт 80. Если приложение развертывается без указания параметра MyWebAPI_PortNumber, то служба использует порт 8080.

## <a name="next-steps"></a>Дополнительная информация
Чтобы узнать больше о некоторых основных понятиях, рассмотренных в данной статье, ознакомьтесь с разделом [Управление параметрами приложения для нескольких сред](service-fabric-manage-multiple-environment-app-configuration.md).

Сведения о других возможностях управления приложениями, доступными в Visual Studio, см. в статье [Использование Visual Studio для упрощения создания приложений Service Fabric и управления ими](service-fabric-manage-application-in-visual-studio.md).