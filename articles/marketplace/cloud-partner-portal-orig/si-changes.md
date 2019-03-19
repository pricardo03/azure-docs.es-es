---
title: Notas de la versión de Seller Insights | Microsoft Docs
description: Proporciona información sobre los cambios realizados en la característica Seller Insights.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 3/3/2019
ms.author: pbutlerm
ms.openlocfilehash: 7df96d53b02e80283f263901dc7e26ed4edb947d
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57342384"
---
# <a name="seller-insights-release-notes"></a>Notas de la versión de Seller Insights 

(Fecha de lanzamiento: 1 de marzo de 2019)

En este artículo se proporciona información sobre los cambios realizados en la característica Seller Insights de [Cloud Partner Portal](https://cloudpartner.azure.com/#insights).

## <a name="release-highlights-for-march-1-2019"></a>Información destacada de la versión para el 1 de marzo de 2019

* *Tendencia de cliente* agregado al resumen
* *Principales cinco clientes* resumen reflejar todas las suscripciones de Azure tiene un cliente
* *Normaliza la tendencia de uso y tendencias de pedidos activos* resumen movido *mensual de pedidos de un vistazo*
* *Informe de conciliación de pagos* actualizado
* *Principales cinco clientes* en pago reflejar todas las suscripciones de Azure tiene un cliente
* *Informe de uso* actualizado con el Id. de cliente
* *Antigüedad del cliente* en pedidos y uso refleja todas las suscripciones de Azure tiene un cliente


(Fecha de lanzamiento: 28 de julio de 2018)

## <a name="release-highlights-for-july-28-2018"></a>Información destacada de la versión para el 28 de julio de 2018


-   *Precios estimados*: proporciona una vista de los gastos de cliente con implicaciones de conversión de moneda.
-   *Pagos previstos*: proporciona una vista anterior de posibles pagos.
-  *Identificadores de referencia de uso*: proporcionan una gran fidelidad de los datos entre el uso del cliente y los pedidos con pagos.
-   *Uso a intervalo diario*: proporciona mayor granularidad y una mejor información sobre el uso por parte del cliente.


### <a name="changes-to-data-structure-and-taxonomy"></a>Cambios en la estructura de datos y la taxonomía

En la tabla siguiente se enumeran las métricas que se han agregado o que han cambiado sustancialmente en esta versión. 

| **Nuevo término**                   |    **Definición**                                                             |
|--------------------------------|  ---------------------------------------------------------------------------- |
| Price (CC) [Precio (CC)]                     | Precio de una unidad de uso para una SKU determinada (en la moneda del cliente).       |
| Cargo total estimado (CC) | Cargo total estimado de la cantidad de unidades de uso de una SKU determinada (en la moneda del cliente). Este valor puede no ser exacto debido a errores de truncamiento o redondeo.   |
| Moneda del anunciante (PC)        | Moneda preferida del anunciante para el pago.                               |
| Precio estimado (PC)           | Precio estimado de una unidad de uso de una SKU determinada en función de la conversión de moneda el día en que se calcula el uso (en la moneda del anunciante). Este valor puede no ser exacto debido a errores de truncamiento o redondeo.   |
| Cargo total estimado (PC) | Cargo total estimado de la cantidad de unidades de uso de una SKU determinada en función de la conversión de moneda el día en que se calcula el uso (en la moneda del anunciante). Este valor puede no ser exacto debido a errores de truncamiento o redondeo. |
| Pago estimado (PC)          | Pago estimado de la cantidad de unidades de uso de una SKU determinada en función de la conversión de moneda el día en que se calcula el uso (en la moneda del anunciante). Este valor puede no ser exacto debido a errores de truncamiento o redondeo.   |
| Referencia de uso                | El identificador de uno o varios días de uso del cliente para una SKU determinada asociada con una entrada del informe de pagos. |
|  |  |
