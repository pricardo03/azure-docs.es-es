---
title: archivo de inclusión
description: archivo de inclusión
services: azure-digital-twins
author: adamgerard
ms.service: azure-digital-twins
ms.topic: include
ms.date: 09/27/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 075587df445b46c8b03c5448cebf8cd8b12f1179
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323911"
---
El control de acceso basado en roles es una estrategia de seguridad basada en la herencia para la administración de accesos, permisos y roles. Los roles descendientes heredan permisos de los roles primarios. También se pueden asignar permisos sin que se hereden de un rol principal. Además, se pueden asignar para personalizar un rol según sea necesario.

Por ejemplo, un **Administrador del espacio** puede necesitar acceso global para ejecutar todas las operaciones para un espacio especificado (incluidos todos los nodos bajo o dentro de él), mientras que un **instalador de dispositivos** solo necesitaría permisos de *lectura* y *actualización* para los dispositivos y sensores.

En todos los casos, se debe conceder a los roles **el acceso exacto y no más del necesario** para completar sus tareas por el **principio de privilegio mínimo**, que concede a una identidad *solo*:

* La cantidad de acceso necesario para completar su trabajo.
* Un rol adecuado y limitado para llevar a cabo su trabajo.

>[!IMPORTANT]
> **Siga siempre el principio de privilegio mínimo**.

Dos otros procedimientos importantes del control de acceso basado en roles que deben seguirse:

> [!div class="checklist"]
> * Audite periódicamente las asignaciones de roles para comprobar que cada rol tiene los permisos correctos.
> * Los roles y las asignaciones deben limpiarse a medida que las personas cambien los roles o las asignaciones.