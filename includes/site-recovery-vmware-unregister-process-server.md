---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 08/06/2018
ms.author: ramamill
ms.openlocfilehash: 81390d38b4c0c38b7ac6883ae2bf18c64542fa00
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39582790"
---
Los pasos necesarios para anular el registro de un servidor de procesos serán diferentes en función del estado de la conexión con el servidor de configuración.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Anulación del registro de un servidor de procesos que se encuentra en estado conectado

1. Conéctese de forma remota al servidor de procesos como administrador.
2. Inicie el **Panel de Control** y abra **Programas > Desinstalar un programa**
3. Desinstale el programa denominado **Servidor de configuración o de procesos de Microsoft Azure Site Recovery**
4. Cuando haya efectuado el paso 3, puede desinstalar las **dependencias del servidor de configuración o de procesos de Microsoft Azure Site Recovery**

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Anulación del registro de un servidor de procesos que se encuentra en estado desconectado

> [!WARNING]
> Utilice los pasos que se indican a continuación si no hay ninguna manera de reactivar la máquina virtual en la que se instaló el servidor de procesos.

1. Inicie sesión como administrador en el servidor de configuración.
2. Abra un símbolo del sistema administrativo y vaya al directorio `%ProgramData%\ASR\home\svsystems\bin`.
3. A continuación, ejecute el comando.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. El comando anterior mostrará la lista de servidores de procesos (puede ser más de uno, en caso de entradas duplicadas) con el número de serie (S.No), la dirección IP (IP), el nombre de la máquina virtual en la cual se implementa el servidor de proceso, el latido de la máquina virtual (latido) como se indica a continuación.
    ![Unregister-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)
5. Ahora, escriba el número de serie del servidor de proceso del que desea anular el registro.
6. Esto purgará los detalles del servidor de procesos del sistema y mostrará el mensaje: **Se anuló correctamente el registro de nombreDeServidor > (direcciónIPServidor)**

