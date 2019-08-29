---
title: Actualización del sistema operativo para SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Realizar la actualización del sistema operativo para SAP HANA en Azure (instancias grandes)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/04/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6341f58791c2fad71a65650e32cff02fb52d78c0
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70098678"
---
# <a name="operating-system-upgrade"></a>Actualización del sistema operativo
Este documento describe los detalles acerca de las actualizaciones del sistema operativo en las instancias grandes HANA.

>[!NOTE]
>La actualización del SO es responsabilidad de los clientes. El soporte técnico de las operaciones de Microsoft puede guiarle a las áreas clave que debe tener en cuenta durante la actualización. Debe consultar con el proveedor del sistema operativo antes de planear una actualización.

En el momento del aprovisionamiento de la unidad HLI, el equipo de operaciones de Microsoft instala el sistema operativo. Con el tiempo, se le pide realizar mantenimiento al sistema operativo (por ejemplo: aplicación de revisiones, optimizaciones, actualizaciones, etc.) en la unidad HLI.

Además, necesita abrir una incidencia de soporte técnico para ponerse en contacto con el equipo de operaciones de Microsoft y consultar con ellos antes de realizar cambios importantes en el sistema operativo (por ejemplo, la actualización de SP1 a SP2).

El vale incluye:

* El id. de suscripción de HLI.
* El nombre del servidor.
* El nivel de revisión que se va a aplicar.
* La fecha en la que está planeando este cambio. 

Recomendamos que abra esta incidencia al menos una semana antes de la fecha de actualización deseada para que el equipo de Operaciones pueda comprobar si es necesaria una actualización de firmware en la hoja del servidor.


Para la matriz de compatibilidad de las distintas versiones de SAP HANA con las diferentes versiones de Linux, consulte la [Nota de SAP 2235581](https://launchpad.support.sap.com/#/notes/2235581).


## <a name="known-issues"></a>Problemas conocidos

A continuación, se muestran algunos problemas habituales durante la actualización:
- En la clase SKU de tipo II, Software Foundation Server (SFS) se quita después de actualizar el SO. Necesita reinstalar la versión de SFS compatible después de actualizar el SO.
- Reversión de los controladores de la tarjeta Ethernet (ENIC y FNIC) a la versión anterior. Después de la actualización, necesita volver a instalar la versión compatible de los controladores.

## <a name="next-steps"></a>Pasos siguientes
- Consulte la sección de [copia de seguridad y restauración](hana-overview-high-availability-disaster-recovery.md) para obtener información sobre la copia de seguridad del SO de clase SKU de tipo I.
- Consulte [Copia de seguridad y restauración del sistema operativo para SKU de tipo II](os-backup-type-ii-skus.md) para la clase SKU de tipo II.
