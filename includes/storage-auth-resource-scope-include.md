---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/21/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a50eb45291ada23f55057f3c440c5b8b23cc4bce
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58622644"
---
Antes de asignar un rol de RBAC a una entidad de seguridad, determinar el ámbito de acceso que debe tener la entidad de seguridad. Las recomendaciones dictan que siempre es mejor conceder únicamente el ámbito más restringido posible.

En la lista siguiente se describe los niveles en el que puede definir el ámbito acceso a recursos de blob y cola de Azure, empezando por el ámbito más restringido:

- **Un contenedor individual.** En este ámbito, una entidad de seguridad tiene acceso a todos los blobs en el contenedor, así como las propiedades del contenedor y los metadatos.
- **Una cola individual.** En este ámbito, una entidad de seguridad tiene acceso a los mensajes en la cola, así como propiedades de la cola y los metadatos.
- **La cuenta de almacenamiento.** En este ámbito, una entidad de seguridad tiene acceso a todos los contenedores y sus blobs, así como todas las colas y sus mensajes.
- **El grupo de recursos.** En este ámbito, una entidad de seguridad tiene acceso a todos los contenedores o las colas en todas las cuentas de almacenamiento en el grupo de recursos.
- **La suscripción.** En este ámbito, una entidad de seguridad tiene acceso a todos los contenedores o las colas en todas las cuentas de almacenamiento en todos los grupos de recursos en la suscripción.

> [!IMPORTANT]
> Si la suscripción incluye un espacio de nombres de Azure DataBricks, roles asignados en el ámbito de la suscripción no podrá conceder acceso a los datos de blob y cola.