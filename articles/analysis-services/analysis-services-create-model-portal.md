---
title: "Создание табличной модели с помощью конструктора веб-приложений Azure Analysis Services | Документация Майкрософт"
description: "Узнайте, как создать табличную модель Azure Analysis Services с помощью конструктора веб-приложений на портале Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/14/2018
ms.author: owend
ms.openlocfilehash: ff2f1dd89796a30d4e0fee0810ff45ff57f5f8bf
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="create-a-model-in-azure-portal"></a>Создание модели на портале Azure

Функция веб-конструктора Azure Analysis Services (предварительная версия) на портале Azure позволяет легко и быстро создавать табличные модели и запрашивать данные модели прямо в браузере. 

Помните, что веб-конструктор находится на этапе **предварительной версии**. В него постоянно добавляются новые функции, но на этапе предварительной версии функциональные возможности ограничены. Для разработки и тестирования более сложных моделей лучше использовать Visual Studio (SSDT) и SQL Server Management Studio (SSMS).

## <a name="before-you-begin"></a>Перед началом работы

- Сервер Azure Analysis Services уровня Standard или Developer. Новые модели, созданные с помощью конструктора веб-приложений, используют DirectQuery, что поддерживается только этими уровнями.
- База данных SQL Azure, хранилище данных SQL Azure или PBIX-файл Power BI Desktop в качестве источника данных. Новые модели, созданные из файлов Power BI Desktop, поддерживают в качестве источников данных Базу данных SQL Azure, хранилище данных SQL Azure, Oracle и Teradata.
- Учетная запись SQL Server и пароль для подключения к источникам данных в Базе данных SQL Azure или хранилище данных SQL Azure.

## <a name="sign-in-to-the-azure-portal"></a>Выполните вход на портал Azure.

Войдите на [портале Azure](https://portal.azure.com/).

## <a name="to-create-a-new-tabular-model"></a>Создание табличной модели

1. На сервере последовательно выберите **Обзор** > **Дизайнер веб-страниц** и щелкните **Открыть**.

    ![Создание модели на портале Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. Выбрав **Web designer** (Конструктор веб-приложений)  >  **Модели**, нажмите кнопку **+ Добавить**.

    ![Создание модели на портале Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. В диалоговом окне **Новая модель** введите имя модели и выберите источник данных.

    ![Диалоговое окно "Новая модель" на портале Azure](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. В диалоговом окне **Подключение** введите свойства подключения. Необходимо указать имя пользователя и пароль учетной записи SQL Server.

     ![Диалоговое окно "Подключение" на портале Azure](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. В диалоговом окне **Таблицы и представления** выберите таблицы для добавления в модель и нажмите кнопку **Создать**. Связи между таблицами создаются автоматически с помощью пары ключей.

     ![Выбор таблиц и представлений](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Новая модель отобразится в браузере. На данном этапе можно сделать следующее:   

- Запрашивать данные модели, перетаскивая поля в конструкторе запросов и добавляя фильтры.
- Создавать новые меры в таблицах.
- Изменять метаданные модели с помощью редактора JSON.
- Открыть модель в Visual Studio (SSDT), Power BI Desktop или Excel.

![Выбор таблиц и представлений](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> При редактировании метаданных модели или создании мер в браузере вносимые изменения сохраняются в модели в Azure. Если вы также работаете с моделью в SSDT, Power BI Desktop или Excel, то она может оказаться несинхронизированной.


## <a name="next-steps"></a>Дополнительная информация 
[Управление ролями и пользователями базы данных](analysis-services-database-users.md)  
[Подключение с помощью Excel](analysis-services-connect-excel.md)  


