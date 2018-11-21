---
title: archivo de inclusión
description: archivo de inclusión
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 11/07/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: 70cdd5a9d0482c24dfeb2037ae56b86cd9339fcf
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285758"
---
| Recurso | Límite máximo |Notas|
| --- | --- |---|
| Cantidad máxima de trabajos nuevos que se puede enviar cada 30 segundos por cuenta de Automation (trabajos no programados) |100 |Si se alcanza este límite, se producirá un error en las siguientes solicitudes para crear un trabajo. El cliente recibe una respuesta de error.|
| Cantidad máxima de trabajos en ejecución simultáneos en la misma instancia de tiempo por cuenta de Automation (trabajos no programados) |200 |Si se alcanza este límite, se producirá un error en las siguientes solicitudes para crear un trabajo. El cliente recibe una respuesta de error.|
| Tamaño máximo de almacenamiento de metadatos de trabajo para un período sucesivo de 30 días. | 10 GB (aproximadamente, 4 millones de trabajos)|Si se alcanza este límite, se producirá un error en las siguientes solicitudes para crear un trabajo. |
| Cantidad máxima de módulos que se puede importar cada 30 segundos por cuenta de Automation |5 ||
| Tamaño máximo de un módulo |100 MB ||
| Tiempo de ejecución de trabajos: nivel Gratis |500 minutos por suscripción por mes del calendario ||
| Cantidad máxima de espacio en disco permitida por espacio aislado **<sup>1</sup>** |1 GB |Se aplica solo a los espacios aislados de Azure|
| Cantidad máxima de memoria que se asigna a un espacio aislado **<sup>1</sup>** |400 MB |Se aplica solo a los espacios aislados de Azure|
| Número máximo de sockets de red permitido por espacio aislado **<sup>1</sup>** |1000 |Se aplica solo a los espacios aislados de Azure|
| Tiempo de ejecución máximo permitido por runbook **<sup>1</sup>** |3 horas |Se aplica solo a los espacios aislados de Azure|
| Número máximo de cuentas de automatización en una suscripción |Ilimitado ||
|Número máximo de trabajos simultáneos que se puede ejecutar en un único Hybrid Runbook Worker|50 ||
| Tamaño máximo de los parámetros de un trabajo de runbook   | 512 kb||
| Parámetros máximos de runbook   | 50|Puede pasar una cadena JSON o XML a un parámetro y analizarlo con el runbook si alcanza el límite de 50 parámetros|
| Tamaño máximo de carga de webhook |  512 kb|
| Máximo de días que se conservan los datos de trabajo|30 días|

**<sup>1</sup>** Un espacio aislado es un entorno compartido que puede usarse en varios trabajos. Los trabajos que usan el mismo espacio aislado están limitados por los límites de los recursos del espacio aislado.
