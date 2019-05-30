---
title: archivo de inclusión
description: archivo de inclusión
services: log-analytics
author: MGoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 05/16/2018
ms.author: magoedte
ms.custom: include file
ms.openlocfilehash: 34f2ab8f7ccafb8b30e298cd71e09171ad8c87cb
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66238803"
---
Los límites siguientes se aplican a los recursos de Azure Log Analytics por suscripción.

| Recurso | Límite predeterminado | Comentarios
| --- | --- | --- |
| Número de áreas de trabajo gratuitas por suscripción | 10 | No se puede aumentar este límite. |
| Número de áreas de trabajo de pago por suscripción | N/D | Está limitado por el número de recursos dentro de un grupo de recursos y el número de grupos de recursos por suscripción. | 

>[!NOTE]
>A partir del 2 de abril de 2018, nuevas áreas de trabajo en una nueva suscripción utilizan automáticamente la *por GB* plan de precios. Para las suscripciones existentes creadas antes del 2 de abril, o una suscripción asociada a una inscripción al contrato Enterprise existente, puede elegir entre los tres planes de tarifa para nuevas áreas de trabajo. 
>

Los límites siguientes se aplican a cada área de trabajo de Log Analytics.

|  | Gratuito | Estándar | Premium | Independiente | OMS | Por GB |
| --- | --- | --- | --- | --- | --- |--- |
| Volumen de datos recopilado por día |500 MB<sup>1</sup> |None |None | None | None | None
| Período de retención de datos |7 días |1 mes |12 meses | 1 mes<sup>2</sup> | 1 mes<sup>2</sup>| 1 mes<sup>2</sup>|

<sup>1</sup>cuando los clientes alcanzan su límite de transferencia de datos diario de 500 MB, el análisis de datos se detiene y se reanuda al comienzo del día siguiente. Un día se basa en UTC.

<sup>2</sup>se puede aumentar el período de retención de datos para los planes de precios por GB, independiente y OMS a 730 días.

| Categoría | límites | Comentarios
| --- | --- | --- |
| API de recopilador de datos | Tamaño máximo de una sola publicación es 30 MB.<br>Tamaño máximo de los valores de campo es 32 KB. | Dividir volúmenes más grandes en varias publicaciones.<br>Los campos de más de 32 KB se truncan. |
| API de búsqueda | 5000 registros devuelven para los datos no agregados.<br>500.000 registros para los datos agregados. | Datos agregados están una búsqueda que incluye el `summarize` comando.
 
