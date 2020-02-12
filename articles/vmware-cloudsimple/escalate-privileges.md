---
title: 'Azure VMware Solutions (AVS): escalado de privilegios de AVS'
description: Describe cómo escalar los permisos de AVS para realizar funciones administrativas en la instancia de vCenter de la nube privada de AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 90dd61fc9856978bab0b68de19d48493a8e0c5fd
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025340"
---
# <a name="escalate-avs-privileges-to-perform-administrative-functions-in-avs-private-cloud-vcenter"></a>Escalado de privilegios de AVS para realizar funciones administrativas en la instancia de vCenter de la nube privada de AVS

El enfoque de privilegios de AVS está diseñado para proporcionar a los usuarios de vCenter los privilegios necesarios para realizar operaciones normales. En algunos casos, un usuario puede requerir privilegios adicionales para realizar una tarea determinada. Puede escalar los privilegios de un usuario de inicio de sesión único de vCenter durante un período limitado.

Entre los motivos para escalar privilegios se pueden citar los siguientes:

* Configuración de orígenes de identidad
* Administración de usuarios
* Eliminación de un grupo de puertos distribuido
* Instalación de soluciones de vCenter (como aplicaciones de copia de seguridad)
* Creación de cuentas de servicio

> [!WARNING]
> Las acciones realizadas durante el estado con privilegios escalados pueden afectar negativamente a su sistema y pueden hacer que el sistema deje de estar disponible. Realice solo las acciones necesarias durante el período de escalado.

En el portal de AVS, [escale los privilegios](escalate-private-cloud-privileges.md) del usuario local CloudOwner en el inicio de sesión único de vCenter. Puede escalar los privilegios del usuario remoto solo si se configura un proveedor de identidades adicional en vCenter. El escalado de privilegios implica agregar el usuario seleccionado al grupo de administradores integrado de vSphere. Solo un usuario puede tener privilegios elevados. Si necesita escalar los privilegios de otro usuario, primero debe anular el escalado de los privilegios de los usuarios actuales.

Los usuarios de otros orígenes de identidades se deben agregar como miembros del grupo CloudOwner.

> [!CAUTION]
> Los nuevos usuarios solo se deben agregar a *Cloud-Owner-Group*, *Cloud-Global-Cluster-Admin-Group*, *Cloud-Global-Storage-Admin-Group*, *Cloud-Global-Network-Admin-Group* o *Cloud-Global-VM-Admin-Group*.  Los usuarios agregados al grupo *Administradores* se quitarán automáticamente.  Solo se deben agregar cuentas de servicio al grupo *Administradores* y estas cuentas no se deben usar para iniciar sesión en la interfaz de usuario web de vSphere.
Durante el período de escalado, AVS usa la supervisión automatizada con notificaciones de alerta asociadas para identificar los cambios involuntarios en el entorno.
