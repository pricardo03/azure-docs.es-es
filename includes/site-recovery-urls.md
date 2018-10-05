---
title: archivo de inclusión
description: archivo de inclusión
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/12/2018
ms.author: raynew
ms.openlocfilehash: ae8eebf9667f2bc03fdc1082fb38c19c5c645c10
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060646"
---
**Nombre** | **Dirección URL comercial**  | **Dirección URL gubernamental** | **Descripción** |
--- | --- | --- | ---
Azure AD | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Se usa para el control de acceso y la administración de identidades mediante AAD 
Copia de seguridad | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Se usa para la transferencia de datos de replicación y coordinación 
Replicación | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Se usa para las operaciones de administración de replicación y coordinación 
Storage | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Se usa para tener acceso a la cuenta de almacenamiento que almacena los datos replicados 
Datos de telemetría (opcional) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Se usa para telemetría 
Sincronización de la hora | ``time.windows.com`` | ``time.nist.gov`` | Se usan para comprobar la sincronización de la hora entre el sistema y la hora global en todas las implementaciones.


