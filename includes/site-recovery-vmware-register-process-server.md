---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: raynew
ms.openlocfilehash: cf39baf34096691144181332566cf567ebc02310
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66169981"
---
1. Establecer una conexión a Escritorio remoto a la máquina que ejecuta el servidor de procesos. 
2. Ejecute cspsconfigtool.exe para iniciar la herramienta de configuración del servidor de proceso de Azure Site Recovery.
    - La herramienta se inicia automáticamente la primera vez que inicie sesión en el servidor de procesos.
    - Si no se abre automáticamente, haga clic en su acceso directo del escritorio.

3. En **servidor de configuración IP o FQDN**, especifique el nombre o dirección IP del servidor de configuración con la que se va a registrar el servidor de procesos.
4. En **puerto del servidor de configuración**, asegúrese de que se especifica 443. Este es el puerto en el que el servidor de configuración escucha las solicitudes.
5. En **frase de contraseña de conexión**, especifique la frase de contraseña que especificó al configurar el servidor de configuración. Para buscar la frase de contraseña:
    -  En el servidor de configuración, navegue hasta la carpeta de instalación de Site Recovery **\home\svssystems\bin\**. 
    - Ejecute este comando: **genpassphrase.exe.n**. Esto muestra la ubicación de la frase de contraseña, a continuación, se puede tener en cuenta.

6. En **puerto de transferencia de datos**, deje el valor predeterminado a menos que haya especificado un puerto personalizado.

7. Haga clic en **guardar** guardar la configuración y registrar el servidor de procesos.

    
    ![Registrar el servidor de procesos](./media/site-recovery-vmware-register-process-server/register-ps.png)
