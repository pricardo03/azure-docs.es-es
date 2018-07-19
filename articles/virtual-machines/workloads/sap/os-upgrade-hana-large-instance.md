---
title: Actualización del sistema operativo para SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Realizar la actualización del sistema operativo para SAP HANA en Azure (instancias grandes)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cff9be3b074dde4a0335675663133a8df81ae62d
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114599"
---
# <a name="operating-system-upgrade"></a>Actualización del sistema operativo
Este documento describe los detalles acerca de las actualizaciones del sistema operativo en las instancias grandes HANA.

>[!NOTE]
>La actualización del SO es responsabilidad de los clientes. El soporte técnico de las operaciones de Microsoft puede guiarle a las áreas clave que debe tener en cuenta durante la actualización. Debe consultar con el proveedor del sistema operativo antes de planear una actualización.

En el momento del aprovisionamiento de la unidad HLI, el equipo de operaciones de Microsoft instala el sistema operativo. Con el tiempo, se le pide realizar mantenimiento al sistema operativo (por ejemplo: aplicación de revisiones, optimizaciones, actualizaciones, etc.) en la unidad HLI.

Además, debe abrir una incidencia de soporte técnico para ponerse en contacto con el equipo de operaciones de Microsoft y consultar con ellos antes de realizar cambios importantes en el sistema operativo (por ejemplo, la actualización de SP1 a SP2).


Para la matriz de compatibilidad de las distintas versiones de SAP HANA con las diferentes versiones de Linux, consulte la [Nota de SAP 2235581](https://launchpad.support.sap.com/#/notes/2235581).


## <a name="known-issues"></a>Problemas conocidos

A continuación, se muestran algunos problemas habituales durante la actualización:
- En la clase SKU de tipo II, Software Foundation Server (SFS) se quita después de actualizar el SO. Debe reinstalar la versión de SFS compatible después de actualizar el SO.
- Reversión de los controladores de la tarjeta Ethernet (ENIC y FNIC) a la versión anterior. Después de la actualización, debe volver a instalar la versión compatible de los controladores.

## <a name="next-steps"></a>Pasos siguientes
- Consulte la sección de [copia de seguridad y restauración](hana-overview-high-availability-disaster-recovery.md) para obtener información sobre la copia de seguridad del SO de clase SKU de tipo I.
- Consulte [Copia de seguridad del SO de SKU tipo II](os-backup-type-ii-skus.md) para la clase SKU de tipo II.