---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 09/30/2019
ms.author: v-adgera
ms.custom: include file
ms.openlocfilehash: 1c8237ce4e8b758395567e939c1ed4bda1f297ff
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827669"
---
El control de acceso basado en rol es una estrategia de seguridad basada en la herencia para la administración de accesos, permisos y roles. Los roles descendientes heredan permisos de los roles primarios. También se pueden asignar permisos sin que se hereden de un rol primario. Además, se pueden asignar para personalizar un rol según sea necesario.

Por ejemplo, un Administrador del espacio podría necesitar acceso global para ejecutar todas las operaciones para un espacio especificado. El acceso incluye todos los nodos bajo o dentro del espacio. Un Instalador de dispositivos puede necesitar solamente permisos de *lectura* y *actualización* para los dispositivos y sensores.

En todos los casos, se concede a los roles *el acceso exacto y no más del necesario* para completar sus tareas, siguiendo el principio de privilegio mínimo. Según este principio, se concede a una identidad *solo*:

* La cantidad de acceso necesario para completar su trabajo.
* Un rol adecuado y limitado para llevar a cabo su trabajo.

>[!IMPORTANT]
> Siga siempre el principio de privilegio mínimo.

Otros dos procedimientos importantes del control de acceso basado en rol que deben seguirse:

> [!div class="checklist"]
> * Audite periódicamente las asignaciones de roles para comprobar que cada rol tiene los permisos correctos.
> * Limpie los roles y las asignaciones cuando las personas cambien de rol o de asignación.