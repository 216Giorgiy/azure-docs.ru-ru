---
title: включение файла
description: включение файла
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: f55bdc774437d280db51fb69f060bea7dc579a26
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203769"
---
## <a name="set-up-your-project"></a>Настройка проекта

Хотите скачать этот пример проекта Android Studio? [Скачайте проект](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) и перейдите к [настройке](#register-your-application), чтобы настроить пример кода перед его выполнением.

### <a name="create-a-new-project"></a>Создание нового проекта

1. Откройте Android Studio, а затем выберите **File** > **New** > **New Project** (Файл > Создать > Новый проект).
2. Назовите приложение и нажмите кнопку **Next** (Далее).
3. Установите флажок **API 21 or newer (Android 5.0)** (API версии 21 или более поздней (Android 5.0)) и нажмите кнопку **Next** (Далее).
4. Оставьте значение **Empty Activity** (Пустое действие) без изменений, нажмите кнопку **Next** (Далее), а затем — кнопку **Finish** (Готово).

### <a name="add-msal-to-your-project"></a>Добавление MSAL в проект

1. В Android Studio выберите **Gradle Scripts** (Скрипты Gradle) > **build.gradle (Module: app)**.
2. В разделе **Dependencies** (Зависимости) вставьте следующий код:

    ```gradle  
    compile ('com.microsoft.identity.client:msal:0.1.+') {
        exclude group: 'com.android.support', module: 'appcompat-v7'
    }
    compile 'com.android.volley:volley:1.0.0'
    ```

<!--start-collapse-->
### <a name="about-this-package"></a>Об этом пакете

Пакет в приведенном выше примере кода устанавливает библиотеку проверки подлинности Майкрософт (MSAL), которая обрабатывает все операции с маркерами, включая получение, кэширование, обновление и удаление. Маркеры необходимы для доступа к API, защищенным платформой удостоверений Майкрософт.
<!--end-collapse-->

## <a name="create-the-apps-ui"></a>Создание пользовательского интерфейса приложения

1. Перейдите в раздел **res** > **layout**, а затем откройте файл **activity_main.xml**.
2. Замените макет действия `android.support.constraint.ConstraintLayout` или другой на `LinearLayout`.
3. Добавьте в узел `LinearLayout` свойство `android:orientation="vertical"`.
4. Вставьте следующий код в узел `LinearLayout`, заменив текущее содержимое:

    ```xml
    <TextView
        android:text="Welcome, "
        android:textColor="#3f3f3f"
        android:textSize="50px"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10dp"
        android:layout_marginTop="15dp"
        android:id="@+id/welcome"
        android:visibility="invisible"/>

    <Button
        android:id="@+id/callGraph"
        android:text="Call Microsoft Graph"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="200dp"
        android:textAllCaps="false" />

    <TextView
        android:text="Getting Graph Data..."
        android:textColor="#3f3f3f"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:id="@+id/graphData"
        android:visibility="invisible"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dip"
        android:layout_weight="1"
        android:gravity="center|bottom"
        android:orientation="vertical" >

        <Button
            android:text="Sign Out"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="15dp"
            android:textColor="#FFFFFF"
            android:background="#00a1f1"
            android:textAllCaps="false"
            android:id="@+id/clearCache"
            android:visibility="invisible" />
    </LinearLayout>
    ```
