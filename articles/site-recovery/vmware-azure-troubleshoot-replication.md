---
title: Solución de problemas de replicación para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se proporciona información para solucionar problemas comunes de replicación durante la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: 1c37b764b47856d3a369228d3f224f2a464029bb
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790663"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Solución de problemas de replicación de máquinas virtuales de VMware y de servidores físicos

Puede recibir un mensaje de error específico al proteger sus máquinas virtuales de VMware o servidores físicos con Azure Site Recovery. En este artículo se describen algunos problemas comunes que pueden surgir al replicar máquinas virtuales de VMware locales y servidores físicos en Azure mediante [Azure Site Recovery](site-recovery-overview.md).


## <a name="initial-replication-issues"></a>Problemas de replicación inicial

En muchos casos, los errores de replicación inicial con los que se encuentra el soporte técnico se deben a problemas de conectividad entre el servidor de origen y el de proceso o entre el servidor de proceso y Azure. En la mayoría de los casos, puede solucionar estos problemas siguiendo los pasos indicados a continuación.

### <a name="verify-the-source-machine"></a>Verificación de la máquina de origen
* Desde la línea de comandos de la máquina del servidor de origen, utilice Telnet para hacer ping al servidor de proceso con el puerto https (el valor predeterminado es 9443) tal y como se muestra a continuación para ver si hay problemas de conectividad de red o problemas de bloqueo de puertos en el firewall.

    `telnet <PS IP address> <port>`
> [!NOTE]
    > Utilice Telnet, no use PING para probar la conectividad.  Si no está instalado Telnet, siga los pasos indicados [aquí](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Si no se puede conectar, habilite el puerto de entrada 9443 en el servidor de proceso y compruebe si el problema todavía persiste. Ha habido algunos casos en los que el servidor de proceso está detrás de la DMZ y que fue la causa del problema.

* Compruebe el estado del servicio `InMage Scout VX Agent – Sentinel/OutpostStart` si no está en ejecución y compruebe si el problema persiste.   

### <a name="verify-the-process-server"></a>Verificación del servidor de procesos

* **Compruebe si el servidor de proceso inserta datos en Azure de un modo activo**

Desde el equipo del servidor de proceso, abra el Administrador de tareas (presione CTRL-MAYÚS-ESC). Vaya a la pestaña Rendimiento y haga clic en el vínculo 'Abrir el Monitor de recursos'. En el Administrador de recursos, vaya a la pestaña Red. Compruebe en 'Procesos con actividad de red' si cbengine.exe está enviando activamente gran volumen (en MB) de datos.

![Habilitar replicación](./media/vmware-azure-troubleshoot-replication/cbengine.png)

En caso negativo, siga los pasos que se indican a continuación:

* **Compruebe si el servidor de proceso es capaz de conectarse a Azure Blob**: seleccione y marque cbengine.exe para ver las conexiones TCP y ver si hay conectividad entre el servidor de proceso y la dirección URL del blob de Azure Storage.

![Habilitar replicación](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

En caso negativo vaya al Panel de Control > Servicios y compruebe si los siguientes servicios están activados y en ejecución:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     *
(Re)inicie cualquier servicio que no se esté ejecutando y compruebe si el problema persiste.

* **Compruebe si el servidor de proceso es capaz de conectarse a través del puerto 443 a la dirección IP pública de Azure**

Abra el archivo CBEngineCurr.errlog más reciente desde `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` y busque: 443 e intento de conexión con error.

![Habilitar replicación](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

Si hay problemas, a continuación, desde la línea de comandos del servidor de proceso, use telnet para hacer ping a la dirección IP pública de Azure (aparece enmascarada en la imagen superior) encontrada en CBEngineCurr.currLog mediante el puerto 443.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Si no puede conectarse, compruebe si el problema de acceso es debido al firewall o Proxy, como se describe en el paso siguiente.


* **Compruebe si el firewall basado en direcciones IP en el servidor de proceso no está bloqueando el acceso**: si está usando reglas de firewall basadas en direcciones IP en el servidor, descargue la lista completa de los intervalos IP de Microsoft Azure Datacenter desde [aquí](https://www.microsoft.com/download/details.aspx?id=41653) y agréguelos a la configuración del firewall para asegurarse de que permiten la comunicación a Azure (y el puerto HTTPS [443]).  Permita los intervalos de direcciones IP correspondientes a la región de Azure de su suscripción y del oeste de EE. UU. (se usan para Access Control y para Identity Management).

* **Compruebe si el firewall basado en direcciones URL en el servidor de proceso no está bloqueando el acceso**:  si está usando reglas de firewall basadas en direcciones URL en el servidor, asegúrese de que las direcciones URL siguientes se agregan a la configuración del firewall.

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

* **Compruebe si la configuración del Proxy del servidor de proceso no está bloqueando el acceso**.  Si está utilizando un servidor Proxy, asegúrese de que el servidor DNS está resolviendo el nombre del servidor proxy.
Para comprobar lo proporcionado en el momento de la instalación del servidor de configuración. Vaya a la clave del registro

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Ahora asegúrese de que el agente de Azure Site Recovery utiliza la misma configuración para el envío de datos.
Busque Microsoft Azure Backup

Ábralo y haga clic en Acción > Cambiar propiedades. En la pestaña Configuración de Proxy, debería ver la dirección del proxy, que debe ser la misma que la que se muestra en la configuración del registro. En caso negativo, cámbiela por la misma dirección.


* **Compruebe si la limitación de ancho de banda no está restringida en el servidor de proceso**:  aumente el ancho de banda y compruebe si el problema persiste.

## <a name="source-machine-to-be-protected-through-site-recovery-is-not-listed-on-azure-portal"></a>La máquina de origen que se debe proteger mediante Site Recovery no aparece en Azure Portal

Al tratar de elegir la máquina de origen para permitir la replicación a través de Azure Site Recovery, si la máquina no está disponible para poder continuar puede ser debido a los siguientes motivos

* Si hay dos máquinas virtuales en vCenter con la misma UUID de instancia, en el portal se muestra la primera máquina virtual detectada por el servidor de configuración. Para solucionarlo, asegúrese de que no haya dos máquinas virtuales que tengan la misma UUID de instancia.
* Asegúrese de que ha agregado las credenciales correctas de vCenter durante la configuración a través de la plantilla de OVF o la configuración unificada. Para comprobar las credenciales agregadas, consulte las instrucciones compartidas [aquí](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* Si los permisos proporcionados para tener acceso a vCenter no tienen privilegios suficientes, podría provocar un error en la detección de máquinas virtuales. Asegúrese de que los permisos proporcionados [aquí](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) se agregan a la cuenta de usuario de vCenter.
* Si la máquina virtual ya está protegida mediante Site Recovery, no estará disponible para la protección. Asegúrese de que la máquina virtual que está buscando en el portal aún no esté protegida por ningún otro usuario o en otras suscripciones.

## <a name="protected-virtual-machines-are-greyed-out-in-the-portal"></a>Las máquinas virtuales protegidas aparecen atenuadas en el portal

Las máquinas virtuales que se replican en Site Recovery aparecen atenuadas si hay entradas duplicadas en el sistema. Consulte las directrices proporcionadas [aquí](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) para eliminar las entradas obsoletas y resolver el problema.

## <a name="next-steps"></a>Pasos siguientes
Si necesita más ayuda, puede enviar una consulta al [foro de Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Contamos con una comunidad activa y uno de nuestros ingenieros podrá ayudarle.
