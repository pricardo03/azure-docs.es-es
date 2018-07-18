---
title: archivo de inclusión
description: archivo de inclusión
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 04/05/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: 6b6e4afa7c8b18c8ce9af8c6abd371b4321e3343
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2018
ms.locfileid: "34852048"
---
| Recurso | Límite máximo |Notas|
| --- | --- |---|
| Cantidad máxima de trabajos nuevos que se puede enviar cada 30 segundos por cuenta de Automation (trabajos no programados) |100 |Si se alcanza este límite, se producirá un error en las siguientes solicitudes para crear un trabajo. El cliente recibe una respuesta de error.|
| Cantidad máxima de trabajos en ejecución simultáneos en la misma instancia de tiempo por cuenta de Automation (trabajos no programados) |200 |Si se alcanza este límite, se producirá un error en las siguientes solicitudes para crear un trabajo. El cliente recibe una respuesta de error.|
| Cantidad máxima de módulos que se puede importar cada 30 segundos por cuenta de Automation |5 ||
| Tamaño máximo de un módulo |100 MB ||
| Tiempo de ejecución de trabajos: nivel Gratis |500 minutos por suscripción por mes del calendario ||
| Cantidad máxima de espacio en disco permitida por espacio aislado**<sup>1</sup>** |1 GB |Se aplica solo a los espacios aislados de Azure|
| Cantidad máxima de memoria que se asigna a un espacio aislado**<sup>1</sup>** |400 MB |Se aplica solo a los espacios aislados de Azure|
| Número máximo de sockets de red permitido por espacio aislado**<sup>1</sup>** |1000 |Se aplica solo a los espacios aislados de Azure|
| Número máximo de cuentas de automatización en una suscripción |Ilimitado ||
|Número máximo de trabajos simultáneos que se puede ejecutar en un único Hybrid Runbook Worker|50 ||

**<sup>1</sup>** Un espacio aislado es un entorno compartido que puede usarse en varios trabajos. Los trabajos que usan el mismo espacio aislado están limitados por los límites de los recursos del espacio aislado.
