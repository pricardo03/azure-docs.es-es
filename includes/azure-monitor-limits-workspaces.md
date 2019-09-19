---
title: archivo de inclusión
description: archivo de inclusión
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 5d0c43fbcc1c59c3281f412aad96a3942a5c79b1
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "70392892"
---
**Volumen de colección de datos y retención** 

| Nivel | Límite por día | Retención de datos | Comentario |
|:---|:---|:---|:---|
| Plan de tarifa actual por GB<br>(Introducido en abril de 2018) | Sin límite | Entre 30 y 730 días | La retención de datos más allá de 31 días está disponible con cargos adicionales. Obtenga información sobre los precios de Azure Monitor. |
| Niveles gratis heredados<br>(Introducidos en abril de 2016) | 500 MB | 7 días | Cuando el área de trabajo alcance el límite de 500 MB por día, la ingesta de datos se detiene y se reanuda al comienzo del día siguiente. Un día se basa en UTC. Tenga en cuenta que los datos que recopila Azure Security Center no se incluyen en este límite de 500 MB por día y se seguirán recopilando más allá de este límite.  |
| Nivel heredado independiente por GB<br>(Introducidos en abril de 2016) | Sin límite | De 30 a 730 días | La retención de datos más allá de 31 días está disponible con cargos adicionales. Obtenga información sobre los precios de Azure Monitor. |
| Heredado por nodo (OMS)<br>(Introducidos en abril de 2016) | Sin límite | De 30 a 730 días | La retención de datos más allá de 31 días está disponible con cargos adicionales. Obtenga información sobre los precios de Azure Monitor. |
| Nivel estándar heredado | Sin límite | 30 días  | No se puede ajustar la retención |
| Nivel Premium heredado | Sin límite | 365 días  | No se puede ajustar la retención |

**Número de áreas de trabajo por suscripción.**

| Plan de tarifa    | Límite del área de trabajo | Comentarios
|:---|:---|:---|
| Nivel Gratis  | 10 | Este límite no se puede aumentar. |
| Todos los demás niveles | Sin límite | Está limitado por el número de recursos de un grupo de recursos y el número de grupos de recursos por suscripción. |

**Azure Portal**

| Categoría | límites | Comentarios |
|:---|:---|:---|
| Número máximo de registros devueltos por una consulta de registro | 10 000 | Para reducir los resultados, use un ámbito de consulta, intervalo de tiempo y filtros en la consulta. |


**API de recopilador de datos**

| Categoría | límites | Comentarios |
|:---|:---|:---|
| Tamaño máximo de una sola publicación | 30 MB | Dividir volúmenes más grandes en varias publicaciones. |
| Tamaño máximo de los valores de campo  | 32 KB | Los campos de más de 32 KB se truncan. |

**API de búsqueda**

| Categoría | límites | Comentarios |
|:---|:---|:---|
| Número máximo de registros devueltos por una única consulta | 500.000 | |
| Tamaño máximo de los datos devueltos | 64 000 000 bytes (~61 MiB)| |
| Tiempo máximo de ejecución de la consulta | 10 minutos | Consulte [Tiempos de espera](https://dev.loganalytics.io/documentation/Using-the-API/Timeouts) para obtener más detalles.  |
| Velocidad máxima de solicitud | 200 solicitudes cada 30 segundos por usuario AAD o dirección IP de cliente | Consulte [Límites de velocidad](https://dev.loganalytics.io/documentation/Using-the-API/Limits) para obtener más detalles. |

**Límites generales del área de trabajo**

| Categoría | límites | Comentarios |
|:---|:---|:---|
| Número máximo de columnas en una tabla         | 500 | |
| Número máximo de caracteres para el nombre de columna | 500 | |
| Regiones a capacidad | Centro occidental de EE.UU. | Actualmente, no se puede crear una nueva área de trabajo en esta región, ya que está al límite de la capacidad temporal. Se espera que se solucione este límite a finales de octubre de 2019. |
| Exportación de datos | No está disponible actualmente | Use Azure Function o Logic App para agregar y exportar datos. | 

>[!NOTE]
>Dependiendo del tiempo que lleve utilizando Log Analytics, es posible que tenga acceso a planes de tarifa heredados. Obtenga más información sobre los [planes de tarifa heredados de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers). 