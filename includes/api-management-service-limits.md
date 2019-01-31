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
ms.openlocfilehash: 646ebc79e3615b289b60e47c0a38595e7892d8cc
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2019
ms.locfileid: "55084789"
---
| Recurso | Límite |
| --- | --- |
| Número máximo de unidades de escalado | 10 por región<sup>1</sup> |
| Tamaño de memoria caché | 5 GB por unidad<sup>2</sup> |
| Conexiones simultáneas de back-end<sup>3</sup> por autoridad HTTP | 2048 por unidad<sup>4</sup> |
| Tamaño máximo de respuestas en caché | 2 MB |
| Tamaño máximo del documento de directiva | 256 KB<sup>5</sup> | 
| Dominios de puerta de enlace personalizados máximos por cada instancia de servicio<sup>6</sup> | 20 | 
| Número máximo de instancias de servicio por suscripción<sup>7</sup> | 20 | 
| Número máximo de suscripciones por instancia de servicio<sup>7</sup> | 500 |
| Número máximo de certificados de cliente por instancia de servicio<sup>7</sup> | 50 | 
| Número máximo de API por instancia de servicio<sup>7</sup> | 50 | 
| Número máximo de operaciones de API por instancia de servicio<sup>7</sup> | 1000 | 
| Duración total máxima de la solicitud<sup>7</sup> | 30 segundos | 
| Tamaño de carga útil máximo en búfer<sup>7</sup> | 2 MB | 


<sup>1</sup> Los límites de escalado dependen del plan de tarifa. Para ver los planes de tarifa y sus límites de escalado, vaya a [Precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup> El tamaño de la caché por unidad depende del plan de tarifa. Para ver los planes de tarifa y sus límites de escalado, vaya a [Precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup> Las conexiones se agrupan y se vuelven a utilizar, a menos que el back-end las cierre explícitamente.<br/>
<sup>4</sup> Por unidad de los planes Básico, Estándar y Premium. El plan de desarrollador está limitado a 1024. No se aplica al plan de consumo.<br/> 
<sup>5</sup> En los planes Básico, Estándar y Premium. En el plan de consumo, el tamaño del documento de directiva se limita a 4 KB.<br/>
<sup>6</sup> Disponible solo en el plan Premium.<br/>
<sup>7</sup> Aplicable solo al plan de consumo.<br/>



