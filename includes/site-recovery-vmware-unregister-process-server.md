---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 03/11/2019
ms.author: ramamill
ms.openlocfilehash: 0d090f43b69b42a07f1c8949d1662e8e720f3cf4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57908542"
---
Los pasos necesarios para anular el registro de un servidor de procesos serán diferentes en función del estado de la conexión con el servidor de configuración.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Anulación del registro de un servidor de procesos que se encuentra en estado conectado

1. Conéctese de forma remota al servidor de procesos como administrador.
2. Iniciar el **Panel de Control** y abra **programas > desinstalar un programa**.
3. Desinstalar un programa denominado **servidor de destino maestra del servicio de Microsoft Azure Site Recovery Mobility**.
4. Desinstalar un programa denominado **el servidor de configuración o procesos de Microsoft Azure Site Recovery**.
5. Cuando se desinstalan los programas en los pasos 3 y 4, puede desinstalar **dependencias del servidor de configuración o procesos de Microsoft Azure Site Recovery**.

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Anulación del registro de un servidor de procesos que se encuentra en estado desconectado

> [!WARNING]
> Use los siguientes pasos si no hay ninguna manera de reactivar la máquina virtual donde se instaló el servidor de procesos.

1. Inicie sesión como administrador en el servidor de configuración.
2. Abra un símbolo del sistema administrativo y vaya al directorio `%ProgramData%\ASR\home\svsystems\bin`.
3. A continuación, ejecute el comando.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. El comando anterior mostrará la lista de servidores de procesos (puede ser más de uno, en caso de entradas duplicadas) con el número de serie (S.No), la dirección IP (IP), el nombre de la máquina virtual en la cual se implementa el servidor de proceso, el latido de la máquina virtual (latido) como se indica a continuación.
    ![Unregister-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)
5. Ahora, escriba el número de serie del servidor de proceso del que desea anular el registro.
6. Esto purgará los detalles del servidor de procesos del sistema y mostrará el mensaje: **Anuló correctamente el registro de nombre de servidor > (dirección de IP de servidor)**

