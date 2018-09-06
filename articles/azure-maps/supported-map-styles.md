---
title: Поддерживаемые стили карт в службе Azure Maps | Документация Майкрософт
description: Стили карт, поддерживаемые в службе Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 08/28/2018
ms.topic: concepts
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 04c8f43e3b484ceeb942ae13ea95baf7f0215b53
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344841"
---
# <a name="azure-maps-supported-map-styles"></a>Стили карт, поддерживаемые в службе Azure Maps
Azure Maps поддерживает четыре различные встроенные стили карт. Ниже перечислены стили и их описания.

## <a name="road"></a>Дорога
Карта **дороги** — это стандартная карта, которая отображает дороги, природные и искусственные объекты, а также ярлыки для этих объектов.

![дорога](./media/supported-map-styles/road.png)

**Применимые API:**
* [Изображение карты](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Фрагмент карты](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Элементы управления картой JS

## <a name="satellite"></a>Спутник 
Стиль **спутник** представляет собой комбинацию спутниковых и аэроснимков.

![спутник](./media/supported-map-styles/satellite.png)

**Применимые API:**
* [Фрагмент спутниковой карты](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Элементы управления картой JS

## <a name="satelliteroadlabels"></a>Satellite_Road_Labels
Этот стиль карты представляет собой гибрид дорог и ярлыков, наложенных поверх спутниковых и аэроснимков.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**Применимые API:**
* Элементы управления картой JS

## <a name="grayscaledark"></a>Grayscale_Dark
**Темно-серый** — это темная версия стиля дорожной карты.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**Применимые API:**
* Элементы управления картой JS 