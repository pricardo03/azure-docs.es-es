---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 0c89ed34b21ca0c41d4f7765d99d8fe8bf7c647d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55888512"
---
## <a name="preparing-for-updates"></a>Preparación de las actualizaciones
Deberá realizar los siguientes pasos antes de buscar y aplicar la actualización:

1. Tome una instantánea de la nube de los datos del dispositivo.
2. Las direcciones IP fijas del controlador son enrutables y se pueden conectar a Internet. Estas direcciones IP fijas se usarán para atender las actualizaciones en el dispositivo. Para comprobarlo, ejecute el siguiente cmdlet en cada controlador de la interfaz de Windows PowerShell del dispositivo:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **Salida de ejemplo para la conexión de prueba cuando los IP fijos pueden conectarse a Internet**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Después de haber completado correctamente estas comprobaciones previas manuales, puede buscar e instalar las actualizaciones.

