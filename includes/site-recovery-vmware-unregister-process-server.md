---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 00b0c1b1a40ad16db177916c57dba6e9d5a187a7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170024"
---
Siga los pasos para sus circunstancias específicas.

### <a name="unregister-a-connected-process-server"></a>Anular el registro de un servidor de procesos conectados

1. Establecer una conexión remota al servidor de procesos como administrador.
2. En el **Panel de Control**, abra **programas > desinstalar un programa**.
3. Desinstale el programa **servidor de destino maestra del servicio de Microsoft Azure Site Recovery Mobility**.
4. Desinstale el programa **el servidor de configuración o procesos de Microsoft Azure Site Recovery**.
5. Después de desinstalan los programas en los pasos 3 y 4, desinstale **dependencias del servidor de configuración o procesos de Microsoft Azure Site Recovery**.

### <a name="unregister-a-disconnected-process-server"></a>Anular el registro de un servidor de procesos desconectado

Sólo siga estos pasos si no hay ninguna manera de reactivar la máquina donde está instalado el servidor de procesos.

1. Inicie sesión en el servidor de configuración como administrador.
2. Abra un símbolo del sistema administrativo y vaya a `%ProgramData%\ASR\home\svsystems\bin`.
3. Ejecute este comando para obtener una lista de uno o más servidores de procesos.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
    - S. No: el número de serie del servidor de proceso.
    - Nombre o IP: La dirección IP y el nombre del equipo que ejecuta el servidor de procesos.
    - Latido: Último latido del equipo del servidor de proceso.
    ![Unregister-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)

4. Especifique el número de serie del servidor de proceso que desea anular el registro.
5. Anular el registro de un servidor de procesos quitar todos los detalles del sistema y muestra el mensaje: **Anuló correctamente el registro de nombre de servidor > (dirección de IP de servidor)**

