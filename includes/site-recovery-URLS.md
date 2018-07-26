---
title: archivo de inclusión
description: archivo de inclusión
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/28/2018
ms.author: raynew
ms.openlocfilehash: f7d6c3f68618fec839ccff06b73ba44d106999d2
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38765545"
---
| NOMBRE | Dirección URL comercial | Dirección URL gubernamental | DESCRIPCIÓN |
|---|---|---|---|
| Azure AD | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Se usa para el control de acceso y la administración de identidades mediante AAD |
| Copia de seguridad | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Se usa para la transferencia de datos de replicación y coordinación |
| Replicación | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Se usa para las operaciones de administración de replicación y coordinación |
| Storage | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Se usa para tener acceso a la cuenta de almacenamiento que almacena los datos replicados |
| Datos de telemetría (opcional) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Se usa para telemetría |

``time.nist.gov`` y ``time.windows.com`` se usan para comprobar la sincronización de la hora entre el sistema y la hora global en todas las implementaciones.


