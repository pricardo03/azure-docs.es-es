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
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: ec3784a91f8aeb7f0fedd13c9ab86a844832a578
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48807182"
---
<a name="seller-insights-release-notes"></a>Notas de la versión de Seller Insights 
===============================
(Fecha de publicación: 28 de julio de 2018)

En este artículo se proporciona información sobre los cambios realizados en la característica Seller Insights de [Cloud Partner Portal](https://cloudpartner.azure.com/#insights).


<a name="release-highlights"></a>Cambios destacados
------------------

-   *Precios estimados*: proporciona una vista de los gastos de cliente con implicaciones de conversión de moneda.
-   *Pagos previstos*: proporciona una vista anterior de posibles pagos.
-  *Identificadores de referencia de uso*: proporcionan una gran fidelidad de los datos entre el uso del cliente y los pedidos con pagos.
-   *Uso a intervalo diario*: proporciona mayor granularidad y una mejor información sobre el uso por parte del cliente.


<a name="changes-to-data-structure-and-taxonomy"></a>Cambios en la estructura de datos y la taxonomía
--------------------------------------

En la tabla siguiente se enumeran las métricas que se han agregado o que han cambiado sustancialmente en esta versión. 

| **Nuevo término**                   |    **Definición**                                                             |
|--------------------------------|  ---------------------------------------------------------------------------- |
| Precio (CC)                     | Precio de una unidad de uso para una SKU determinada (en la moneda del cliente).       |
| Cargo total estimado (CC) | Cargo total estimado de la cantidad de unidades de uso de una SKU determinada (en la moneda del cliente). Este valor puede no ser exacto debido a errores de truncamiento o redondeo.   |
| Moneda del anunciante (PC)        | Moneda preferida del anunciante para el pago.                               |
| Precio estimado (PC)           | Precio estimado de una unidad de uso de una SKU determinada en función de la conversión de moneda el día en que se calcula el uso (en la moneda del anunciante). Este valor puede no ser exacto debido a errores de truncamiento o redondeo.   |
| Cargo total estimado (PC) | Cargo total estimado de la cantidad de unidades de uso de una SKU determinada en función de la conversión de moneda el día en que se calcula el uso (en la moneda del anunciante). Este valor puede no ser exacto debido a errores de truncamiento o redondeo. |
| Pago estimado (PC)          | Pago estimado de la cantidad de unidades de uso de una SKU determinada en función de la conversión de moneda el día en que se calcula el uso (en la moneda del anunciante). Este valor puede no ser exacto debido a errores de truncamiento o redondeo.   |
| Referencia de uso                | El identificador de uno o varios días de uso del cliente para una SKU determinada asociada con una entrada del informe de pagos. |
|  |  |
