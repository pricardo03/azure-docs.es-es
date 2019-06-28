---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: raynew
ms.openlocfilehash: cf39baf34096691144181332566cf567ebc02310
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186842"
---
1. Establezca una conexión a Escritorio remoto a la máquina que ejecuta el servidor de procesos. 
2. Ejecute cspsconfigtool.exe para iniciar la herramienta de configuración del servidor de procesos de Azure Site Recovery.
    - La herramienta se inicia automáticamente la primera vez que inicia sesión en el servidor de procesos.
    - Si no se abre automáticamente, haga clic en su acceso directo en el escritorio.

3. En **Configuration server FQDN or IP** (FQDN o IP del servidor de configuración), especifique el nombre o la dirección IP del servidor de configuración en el que se va a registrar el servidor de procesos.
4. En **Configuration Server Port** (Puerto del servidor de configuración), asegúrese de que se especifica 443. Este es el puerto en el que el servidor de configuración escucha las solicitudes.
5. En **Connection Passphrase** (Frase de contraseña de la conexión), especifique la frase de contraseña que definió al configurar el servidor de configuración. Para encontrar la frase de contraseña:
    -  En el servidor de configuración, vaya a la carpeta de instalación de Site Recovery * *\home\svssystems\bin\** . 
    - Ejecute este comando: **genpassphrase.exe.n**. Se muestra la ubicación de la frase de contraseña, que puede anotar.

6. En **Data Transfer Port** (Puerto de transferencia de datos), deje el valor predeterminado a menos que haya especificado un puerto personalizado.

7. Haga clic en **Save** (Guardar) para guardar la configuración y registrar el servidor de procesos.

    
    ![Registro del servidor de procesos](./media/site-recovery-vmware-register-process-server/register-ps.png)
