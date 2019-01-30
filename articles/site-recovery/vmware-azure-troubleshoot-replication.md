---
title: Solución de problemas de replicación para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se proporciona información para solucionar problemas comunes de replicación durante la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 01/18/2019
ms.author: ramamill
ms.openlocfilehash: 5c2d33b39614ded95ac38e07c844b0a8cafa7cd2
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411482"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Solución de problemas de replicación de máquinas virtuales de VMware y de servidores físicos

Puede recibir un mensaje de error específico al proteger sus máquinas virtuales de VMware o servidores físicos con Azure Site Recovery. En este artículo se describen algunos problemas comunes que pueden surgir al replicar máquinas virtuales de VMware locales y servidores físicos en Azure mediante [Site Recovery](site-recovery-overview.md).

## <a name="initial-replication-issues"></a>Problemas de replicación inicial

Los errores de replicación iniciales se deben a menudo a problemas de conectividad entre el servidor de origen y el servidor de procesos o entre el servidor de procesos y Azure. En la mayoría de los casos, puede seguir los pasos descritos en las secciones siguientes para resolver estos problemas.

### <a name="check-the-source-machine"></a>Comprobación de la máquina de origen

En la siguiente lista se muestran formas de comprobar la máquina de origen:

*  En la línea de comandos en el servidor de origen, use Telnet para hacer ping al servidor de procesos mediante el puerto HTTPS (el puerto HTTPS predeterminado es 9443) con la ejecución del comando siguiente. El comando comprueba si hay problemas de conectividad de red y problemas que bloqueen el puerto de firewall.


   `telnet <process server IP address> <port>`


   > [!NOTE]
   > Use Telnet para probar la conectividad. No use `ping`. Si no está instalado Telnet, realice los pasos indicados en [Install Telnet Client](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx) (Instalación del cliente de Telnet).

   Si no se puede conectar al servidor de procesos, permita el puerto de entrada 9443 en el servidor de procesos. Por ejemplo, puede que deba permitir el puerto de entrada 9443 en el servidor de procesos si la red tiene una red perimetral o una subred filtrada. A continuación, compruebe si el problema persiste.

*  Compruebe el estado del servicio **InMage Scout VX Agent - Sentinel/OutpostStart**. Si el servicio no se está ejecutando, inícielo y, luego, compruebe si el problema continúa.   

### <a name="check-the-process-server"></a>Comprobación del servidor de procesos

En la lista siguiente se muestran formas de comprobar el servidor de procesos:

*  **Compruebe si el servidor de procesos está insertando datos activamente en Azure**.

   1. En el servidor de procesos, abra el Administrador de tareas (presione Ctrl+Mayús+Esc).
   2. Seleccione la pestaña **Rendimiento** y, luego, el vínculo **Abrir el Monitor de recursos**. 
   3. En la página **Monitor de recursos**, seleccione la pestaña **Red**. En **Procesos con actividad de red**, compruebe si **cbengine.exe** está enviando activamente un gran número de datos.

        ![Captura de pantalla que muestra los volúmenes en Procesos con Actividad de red](./media/vmware-azure-troubleshoot-replication/cbengine.png)

   Si cbengine.exe no está enviando un gran volumen de datos, realice los pasos de las secciones siguientes.

*  **Compruebe si el servidor de procesos puede conectarse a Azure Blob Storage**.

   Seleccione **cbengine.exe**. En **Conexiones TCP**, compruebe si hay conectividad entre el servidor de procesos y la dirección URL de Azure Blob Storage.

   ![Captura de pantalla que muestra la conectividad entre cbengine.exe y la dirección URL de Azure Blob Storage](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

   Si no hay conectividad entre el servidor de procesos y la dirección URL de Azure Blob Storage, en el Panel de Control, seleccione **Servicios**. Compruebe si se están ejecutando los siguientes servicios:

   *  cxprocessserver
   *  InMage Scout VX Agent - Sentinel/Outpost
   *  Agente de Microsoft Azure Recovery Services
   *  Servicio Microsoft Azure Site Recovery
   *  tmansvc

   Inicie o reinicie cualquier servicio que no esté en ejecución. Compruebe si el problema persiste.

*  **Compruebe si el servidor de procesos puede conectarse a la dirección IP pública de Azure mediante el puerto 443**.

   En %programfiles%\Microsoft Azure Recovery Services Agent\Temp, abra el archivo CBEngineCurr.errlog más reciente. En el archivo, busque **443** o la cadena **error en el intento de conexión**.

   ![Captura de pantalla que muestra los registros de errores en la carpeta Temp](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

   Si se muestran problemas, en la línea de comandos del servidor de procesos, use Telnet para hacer ping a la dirección IP pública Azure (la dirección IP se enmascara en la imagen anterior). Puede encontrar la dirección IP pública de Azure en el archivo CBEngineCurr.currLog mediante el puerto 443:

   `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

   Si no se puede conectar, compruebe si el problema de acceso se debe a la configuración del firewall o del proxy como se describe en el paso siguiente.

*  **Compruebe si el firewall basado en la dirección IP en el servidor de procesos bloquea el acceso**.

   Si usa reglas de firewall basadas en la dirección IP en el servidor, descargue la lista completa de [intervalos IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). Agregue los intervalos de direcciones IP a la configuración del firewall para asegurarse de que el firewall permita la comunicación con Azure (y con el puerto HTTPS predeterminado, 443). Permita intervalos de direcciones IP correspondientes a la región de Azure de la suscripción y a la región Oeste de EE. UU. de Azure (se usan para controlar el acceso y administrar las identidades).

*  **Compruebe si un firewall basado en la dirección URL en el servidor de procesos bloquea el acceso**.

   Si usa una regla de firewall basada en la dirección URL en el servidor, agregue las direcciones URL enumeradas en la tabla siguiente a la configuración del firewall:

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

*  **Compruebe si la configuración del proxy en el servidor de procesos bloquea el acceso**.

   Si usa un servidor proxy, asegúrese de que el servidor DNS resuelva el nombre de dicho servidor. Para comprobar el valor que proporcionó al configurar el servidor de configuración, vaya a la clave del Registro **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings**.

   Luego, asegúrese de que el agente de Azure Site Recovery usa la misma configuración para enviar datos: 
      
   1. Busque **Microsoft Azure Backup**. 
   2. Abra **Microsoft Azure Backup** y seleccione **Action** > **Change Properties** (Acción > Cambiar propiedades). 
   3. En la pestaña **Proxy Configuration** (Configuración de proxy), verá la dirección del proxy. La dirección del proxy debe ser igual que la que se muestra en la configuración del Registro. Si no es así, cámbiela por la misma dirección.

*  **Compruebe si el umbral de ancho de banda está restringido en el servidor de procesos**.

   Aumente el ancho de banda y, luego, compruebe si el problema persiste.

## <a name="source-machine-isnt-listed-in-the-azure-portal"></a>La máquina de origen no aparece en Azure Portal

Cuando intenta seleccionar la máquina de origen para habilitar la replicación con Site Recovery, puede que no esté disponible por uno de los siguientes motivos:

* **Dos máquinas virtuales con el mismo UUID de instancia**: Si dos máquinas virtuales en vCenter tienen el mismo UUID de instancia, en Azure Portal se muestra la primera máquina virtual detectada por el servidor de configuración. Para solucionar este problema, asegúrese de que no haya dos máquinas virtuales con el mismo UUID de instancia. Este escenario se observa habitualmente en instancias donde una máquina virtual de copia de seguridad se activa y se registra en nuestros registros de detección. Consulte [Azure Site Recovery VMware-to-Azure: How to clean up duplicate or stale entries](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) (De VMware a Azure en Azure Site Recovery: cómo limpiar entradas duplicadas u obsoletas) para solucionarlo.
* **Credenciales de usuario de vCenter incorrectas**: Asegúrese de que ha agregado las credenciales correctas de vCenter al configurar el servidor de configuración mediante la plantilla OVF o la instalación unificada. Para comprobar las credenciales que ha agregado durante la instalación, consulte [Modificación de las credenciales para la detección automática](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* **Privilegios de vCenter insuficientes**: Si los permisos proporcionados para acceder a vCenter no son los necesarios, es posible que no se puedan detectar las máquinas virtuales. Asegúrese de que los permisos descritos en [Preparación de una cuenta de detección automática](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) se agregan a la cuenta de usuario de vCenter.
* **Servidores de administración de Azure Site Recovery**: Si la máquina virtual se usa como servidor de administración con al menos uno de los roles de servidor de configuración, servidor de procesos de escalado horizontal o servidor de destino principal, no podrá seleccionar la máquina virtual en el portal. Los servidores de administración no se pueden replicar.
* **Máquina virtual ya protegida o conmutada por error mediante servicios de Azure Site Recovery**: Si la máquina virtual ya se ha conmutado por error o ya está protegida mediante Site Recovery, no estará disponible para seleccionarla para la protección en el portal. Asegúrese de que la máquina virtual que busca en el portal no esté aún protegida por ningún otro usuario o en otras suscripciones.
* **vCenter no conectado**: Compruebe si vCenter tiene el estado conectado. Para ello, vaya al almacén de Recovery Services > Infraestructura de Site Recovery > Servidores de configuración y haga clic en el servidor de configuración correspondiente; se abrirá una hoja a la derecha con detalles acerca de los servidores asociados. Compruebe si vCenter está conectado. Si se muestra el estado "No conectado", resuelva el problema y, a continuación, [actualice el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server) en el portal. Una vez hecho esto, la máquina virtual se mostrará en el portal.
* **ESXi apagado**: Si el host ESXi en el que reside la máquina virtual está apagado, la máquina virtual no se mostrará ni se podrá seleccionar en Azure Portal. Encienda el host ESXi y [actualice el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server) en el portal. Una vez hecho esto, la máquina virtual se mostrará en el portal.
* **Reinicio pendiente**: Si la máquina virtual está pendiente de reiniciarse, no se podrá seleccionar en Azure Portal. Asegúrese de completar el reinicio pendiente y [actualice el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Una vez hecho esto, la máquina virtual se mostrará en el portal.
* **Dirección IP no encontrada**: Si la máquina virtual no tiene una dirección IP válida asociada, no se podrá seleccionar en Azure Portal. Asegúrese de asignar una dirección IP válida a la máquina virtual y [actualice el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Una vez hecho esto, la máquina virtual se mostrará en el portal.

## <a name="protected-virtual-machines-are-greyed-out-in-the-portal"></a>Las máquinas virtuales protegidas aparecen atenuadas en el portal

Las máquinas virtuales que se replican en Site Recovery no están disponibles en Azure Portal si hay entradas duplicadas en el sistema. Para aprender a eliminar entradas obsoletas y resolver el problema, consulte [Azure Site Recovery VMware-to-Azure: How to clean up duplicate or stale entries](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) (De VMware a Azure en Azure Site Recovery: cómo limpiar entradas duplicadas u obsoletas).

## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda, puede publicar su pregunta en el [foro de Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Contamos con una comunidad activa y uno de nuestros ingenieros puede ayudarle.
