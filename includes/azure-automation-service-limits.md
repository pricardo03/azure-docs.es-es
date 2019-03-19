---
title: archivo de inclusión
description: archivo de inclusión
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 12/13/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: 61f82771e53ac9bb594484b29bb109a03cee674b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554201"
---
#### <a name="process-automation"></a>Automatización de procesos

| Recurso | Límite máximo |Notas|
| --- | --- |---|
| Número máximo de trabajos nuevos que se pueden enviar cada 30 segundos por cuenta de Azure Automation (trabajos no programados) |100 |Cuando se alcanza este límite, las solicitudes posteriores para crear un trabajo producirá un error. El cliente recibe una respuesta de error.|
| Número máximo de trabajos simultáneos en ejecución en la misma instancia de tiempo por cuenta de Automation (trabajos no programados) |200 |Cuando se alcanza este límite, las solicitudes posteriores para crear un trabajo producirá un error. El cliente recibe una respuesta de error.|
| Tamaño máximo de almacenamiento de metadatos de trabajo para un período sucesivo de 30 días | 10 GB (aproximadamente 4 millones de trabajos)|Cuando se alcanza este límite, las solicitudes posteriores para crear un trabajo producirá un error. |
| Número máximo de módulos que se puede importar cada 30 segundos por cuenta de Automation |5 ||
| Tamaño máximo de un módulo |100 MB ||
| Tiempo de ejecución, nivel gratis de trabajo |500 minutos por suscripción por mes del calendario ||
| Cantidad máxima de espacio en disco permitida por el espacio aislado<sup>1</sup> |1 GB |Se aplica a sólo espacios aislados de Azure.|
| Cantidad máxima de memoria que se asigna a un espacio aislado<sup>1</sup> |400 MB |Se aplica a sólo espacios aislados de Azure.|
| Número máximo de sockets de red permitido por el espacio aislado<sup>1</sup> |1000 |Se aplica a sólo espacios aislados de Azure.|
| Tiempo de ejecución máximo permitido por runbook<sup>1</sup> |3 horas |Se aplica a sólo espacios aislados de Azure.|
| Número máximo de cuentas de automatización de una suscripción |Sin límite ||
|Número máximo de trabajos simultáneos que se pueden ejecutar en un único Hybrid Runbook Worker|50 ||
| Tamaño máximo del parámetro de trabajo   | 512 kilobits||
| Parámetros de runbook máximo   | 50|Si se alcanza el límite de 50 parámetros, puede pasar una cadena JSON o XML a un parámetro y analizarla con el runbook.|
| Tamaño de la carga máxima de webhook |  512 kilobits|
| Número máximo de días que se conservan los datos de trabajo|30 días|
| Tamaño máximo de estado de flujo de trabajo de PowerShell |5 MB| Se aplica a los runbooks de flujo de trabajo de PowerShell cuando el flujo de trabajo de los puntos de comprobación.|

<sup>1</sup>un espacio aislado es un entorno compartido que se puede utilizar varios trabajos. Los trabajos que usan el mismo espacio aislado están obligados por las limitaciones de recursos del espacio aislado.

#### <a name="change-tracking-and-inventory"></a>Change tracking e inventario

La siguiente tabla muestra los límites del elemento sometidas a seguimiento por máquina para el seguimiento de cambios.

| **Recurso** | **Límite**| **Notas** |
|---|---|---|
|Archivo|500||
|Registro|250||
|Software de Windows|250|No incluye las actualizaciones de software.|
|Paquetes Linux|1,250||
|Services|250||
|Daemon|250||
