---
title: archivo de inclusión
description: archivo de inclusión
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 03/22/2018
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: fc945a7e9389c8aec48a6a1dba969fbf92002d3a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553881"
---
| Recurso | Límite |
| --- | --- |
| Número máximo de unidades de escalado | 10 por región<sup>1</sup> |
| Tamaño de memoria caché | 5 GB por unidad<sup>2</sup> |
| Conexiones simultáneas de back-end<sup>3</sup> por autoridad HTTP | 2048 por unidad<sup>4</sup> |
| Tamaño máximo de respuestas en caché | 2 MB |
| Tamaño máximo del documento de directiva | 256 KB<sup>5</sup> | 
| Dominios de puerta de enlace personalizados máximos por cada instancia de servicio<sup>6</sup> | 20 |
| Número máximo de certificados de CA por instancia de servicio | 10 | 
| Número máximo de instancias de servicio por suscripción<sup>7</sup> | 20 | 
| Número máximo de suscripciones por instancia de servicio<sup>7</sup> | 500 |
| Número máximo de certificados de cliente por instancia de servicio<sup>7</sup> | 50 | 
| Número máximo de API por instancia de servicio<sup>7</sup> | 50 | 
| Número máximo de operaciones de API por instancia de servicio<sup>7</sup> | 1000 | 
| Duración máxima de solicitud total<sup>7</sup> | 30 segundos | 
| Tamaño de carga útil máximo en búfer<sup>7</sup> | 2 MB | 


<sup>1</sup>dependen de los límites de escala en el plan de tarifa. Para ver los planes de tarifa y sus límites de escalado, consulte [precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup>por la memoria caché de la unidad tamaño depende del plan de tarifa. Para ver los planes de tarifa y sus límites de escalado, consulte [precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup>las conexiones se agrupan y volver a usar a menos que el back-end las cierre explícitamente.<br/>
<sup>4</sup>este límite es por unidad de los niveles básico, estándar y Premium. El nivel desarrollador está limitado a 1024. Este límite no se aplica a la capa de consumo.<br/> 
<sup>5</sup>este límite se aplica a los niveles básico, estándar y Premium. En el nivel de consumo, el tamaño del documento de directiva está limitado a 4 KB.<br/>
<sup>6</sup>este recurso está disponible en el nivel Premium.<br/>
<sup>7</sup>este recurso se aplica a solo el plan de consumo.<br/>



