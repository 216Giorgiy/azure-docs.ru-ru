---
title: "Указание настраиваемого образа в шаблоне масштабируемого набора Azure | Документация Майкрософт"
description: "Узнайте, как добавить настраиваемый образ в существующий шаблон масштабируемого набора виртуальных машин Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/10/2017
ms.author: negat
ms.openlocfilehash: cf52fc9e95267c4bc5c0106aadf626685ddd5c24
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Добавление настраиваемого образа в шаблон масштабируемого набора Azure

В этой статье объясняется, как изменить [шаблон минимального приемлемого масштабируемого набора](./virtual-machine-scale-sets-mvss-start.md), чтобы выполнить развертывание с помощью настраиваемого образа.

## <a name="change-the-template-definition"></a>Изменение определения шаблона

Шаблон минимального приемлемого масштабируемого набора доступен [здесь](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), а шаблон для развертывания масштабируемого набора с помощью настраиваемого образа — [здесь](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json). Давайте рассмотрим DIFF-файл, с помощью которого можно постепенно создать этот шаблон (`git diff minimum-viable-scale-set custom-image`).

### <a name="creating-a-managed-disk-image"></a>Создание образа управляемого диска

Если вы уже создали настраиваемый образ управляемого диска (ресурс типа `Microsoft.Compute/images`), можете пропустить этот раздел.

Сначала добавьте параметр `sourceImageVhdUri`, который представляет собой URI универсального большого двоичного объекта в службе хранилища Azure, содержащего настраиваемый образ для развертывания.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "The source of the generalized blob containing the custom image"
+      }
     }
   },
   "variables": {},
```

Затем добавьте ресурс типа `Microsoft.Compute/images`, который является образом управляемого диска, основанном на универсальном большом двоичном объекте, расположенном в URI `sourceImageVhdUri`. Этот образ должен находиться в том же регионе, что и масштабируемый набор, который его использует. В свойствах образа укажите тип ОС, расположение большого двоичного объекта (из параметра `sourceImageVhdUri`) и тип учетной записи хранения:

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2016-04-30-preview",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

В ресурс масштабируемого набора добавьте предложение `dependsOn`, которое ссылается на настраиваемый образ, чтобы создать образ до того, как масштабируемый набор попытается выполнить развертывание из этого образа:

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a>Изменение свойств масштабируемого набора для использования управляемого образа

В свойстве `imageReference` масштабируемого набора `storageProfile` вместо издателя, предложения, номера SKU и версии образа платформы укажите `id` ресурса `Microsoft.Compute/images`:

```diff
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
-              "publisher": "Canonical",
-              "offer": "UbuntuServer",
-              "sku": "16.04-LTS",
-              "version": "latest"
+              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

В этом примере используется функция `resourceId`, чтобы получить идентификатор ресурса образа, созданного в том же шаблоне. Если образ управляемого диска создан заранее, укажите идентификатор этого образа. Этот идентификатор должен иметь следующий формат: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Дальнейшие действия

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
