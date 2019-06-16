---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 96cba4e077be8b7658c270b09b177a845e16c8b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66169973"
---
En este artículo se supone que

1. Ya se ha establecido una **VPN de sitio a sitio** o una conexión de **Express Route** entre su red local y Azure Virtual Network.
2. Su cuenta de usuario tiene permisos para crear una nueva máquina virtual en la suscripción de Azure en la que las máquinas virtuales se han conmutado por error.
3. La suscripción tiene un mínimo de 4 núcleos disponibles para poner en marcha una nueva máquina virtual de servidor de procesos.
4. Dispone de la **frase de contraseña del servidor de configuración**.

> [!TIP]
> Asegúrese de que se puede conectar al puerto 443 del servidor de configuración (que se ejecuta de forma local) desde la instancia de Azure Virtual Network en la que las máquinas virtuales se han conmutado por error.
