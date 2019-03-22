---
title: Solución de problemas de replicación para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se proporciona información para solucionar problemas comunes de replicación durante la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/14/2019
ms.author: mayg
ms.openlocfilehash: 1aaf13f01c7e7197001f3099fabd4b8be8545f0d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58094708"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Solución de problemas de replicación de máquinas virtuales de VMware y de servidores físicos

Puede recibir un mensaje de error específico al proteger sus máquinas virtuales de VMware o servidores físicos con Azure Site Recovery. En este artículo se describen algunos problemas comunes que pueden surgir al replicar máquinas virtuales de VMware locales y servidores físicos en Azure mediante [Site Recovery](site-recovery-overview.md).

## <a name="monitor-process-server-health-to-avoid-replication-issues"></a>Supervisión del estado del servidor de procesos para evitar problemas de replicación

Se recomienda supervisar el estado del servidor de procesos en el portal para asegurarse de que la replicación se lleva a cabo para las máquinas de origen asociadas. En el almacén, vaya a Administrar > Infraestructura de Site Recovery > Servidores de configuración. En la hoja Servidor de configuración, haga clic en el servidor de procesos bajo Servidores asociados. La hoja del servidor de procesos se abre con sus estadísticas de estado. Puede realizar un seguimiento del uso de la CPU, el uso de memoria, el estado de los servicios del servidor de procesos requeridos para la replicación, la fecha de expiración del certificado y el espacio libre disponible. El estado de todas las estadísticas debe ser verde. 

**Se recomienda tener el uso de memoria y CPU en un valor inferior al 70 % y liberar espacio por encima del 25 %**. El espacio libre hace referencia al espacio en disco de la memoria caché en el servidor de procesos, que se usa para almacenar los datos de replicación de las máquinas de origen antes de cargarlos en Azure. Si reduce a menos del 20 %, se limitará la replicación para todas las máquinas de origen asociadas. Siga las [orientaciones de capacidad](./site-recovery-plan-capacity-vmware.md#capacity-considerations) para comprender la configuración necesaria para replicar las máquinas de origen.

Asegúrese de que los siguientes servicios están ejecutándose en la máquina del servidor de procesos. Inicie o reinicie cualquier servicio que no esté en ejecución.

**Servidor de procesos integrado**

* ProcessServer
* ProcessServerMonitor
* cxprocessserver
* InMage PushInstall
* Servicio de carga de registro (LogUpload)
* Servicio de aplicación de InMage Scout
* Agente de Microsoft Azure Recovery Services (obengine)
* InMage Scout VX Agent - Sentinel/Outpost (svagents)
* tmansvc
* Servicio de publicación World Wide Web (W3SVC)
* MySQL
* Servicio Microsoft Azure Site Recovery (dra)

**Servidor de procesos de escalado horizontal**

* ProcessServer
* ProcessServerMonitor
* cxprocessserver
* InMage PushInstall
* Servicio de carga de registro (LogUpload)
* Servicio de aplicación de InMage Scout
* Agente de Microsoft Azure Recovery Services (obengine)
* InMage Scout VX Agent - Sentinel/Outpost (svagents)
* tmansvc

**Servidor de procesos en Azure para la conmutación por recuperación**

* ProcessServer
* ProcessServerMonitor
* cxprocessserver
* InMage PushInstall
* Servicio de carga de registro (LogUpload)

Asegúrese de que el tipo de inicio de todos los servicios está establecido en **Automático o Automático (Inicio retrasado)**. El servicio del agente de Microsoft Azure Recovery Services (obengine) no necesita tener su StartType establecido como antes.

## <a name="replication-issues"></a>Problemas de replicación

A menudo, los errores de replicación inicial y continua están provocados por problemas de conectividad entre el servidor de origen y el servidor de procesos o entre el servidor de procesos y Azure. En la mayoría de los casos, puede seguir los pasos descritos en las secciones siguientes para resolver estos problemas.

>[!Note]
>Asegúrese de que:
>1. El sistema de tiempo de fecha para el elemento protegido está sincronizado.
>2. Ningún software antivirus está bloqueando Azure Site Recovery. Obtenga información sobre [más](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) en exclusiones de carpetas necesarias para Azure Site Recovery.

### <a name="check-the-source-machine-for-connectivity-issues"></a>Comprobar los problemas de conectividad de la máquina de origen

La siguiente lista muestra formas puede comprobar la máquina de origen.

*  En la línea de comandos del servidor de origen, use Telnet para hacer ping al servidor de procesos mediante el puerto HTTPS con la ejecución del comando siguiente. El puerto 9443 HTTPS es el valor predeterminado utilizado por el servidor de procesos para enviar y recibir tráfico de replicación. Puede modificar este puerto en el momento del registro. El comando siguiente comprueba si hay problemas de conectividad de red y problemas que bloqueen el puerto de firewall.


   `telnet <process server IP address> <port>`


   > [!NOTE]
   > Use Telnet para probar la conectividad. No use `ping`. Si no está instalado Telnet, realice los pasos indicados en [Install Telnet Client](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx) (Instalación del cliente de Telnet).

   Si Telnet es capaz de conectarse correctamente al puerto del servidor de procesos, se verá una pantalla en blanco.

   Si no se puede conectar al servidor de procesos, permita el puerto de entrada 9443 en el servidor de procesos. Por ejemplo, puede que deba permitir el puerto de entrada 9443 en el servidor de procesos si la red tiene una red perimetral o una subred filtrada. A continuación, compruebe si el problema persiste.

*  Si Telnet no presenta problemas y aun así la máquina de origen notifica que el servidor de procesos no es accesible, abra el explorador web en la máquina de origen y compruebe si la dirección https://<PS_IP>:<PS_Data_Port>/ es accesible.

    Se espera un error de certificado HTTPS al alcanzar esta dirección. Si se omite el error del certificado y se continúa, debería aparecer 400 – Solicitud incorrecta, que significa que el servidor no puede atender la solicitud del explorador y que la conexión HTTPS estándar al servidor funciona correctamente.

    Si no se realiza correctamente, los detalles del mensaje de error en el explorador le sirven de pista. Por ejemplo, si la autenticación de proxy es incorrecta, el servidor proxy devuelve 407 – Se requiere autenticación del proxy junto con las acciones necesarias en el mensaje de error. 

*  Compruebe los siguientes registros sobre la máquina virtual de origen para ver si hay errores relacionados con la carga de la red:

       C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\svagents*.log 

### <a name="check-the-process-server-for-connectivity-issues"></a>Compruebe el servidor de procesos para problemas de conectividad

En la lista siguiente se muestran formas de comprobar el servidor de procesos:

> [!NOTE]
> El servidor de procesos debe tener una dirección IPv4 estática y no tener ninguna IP de NAT configurada.

* **Comprobación de la conectividad entre las máquinas de origen y el servidor de procesos**
* Si Telnet funciona desde la máquina de origen y aun así el servidor de procesos no es accesible desde el origen, compruebe la conexión de un extremo a otro con cxprocessserver desde la máquina virtual de origen ejecutando la herramienta cxpsclient en la máquina virtual de origen:

      <install folder>\cxpsclient.exe -i <PS_IP> -l <PS_Data_Port> -y <timeout_in_secs:recommended 300>

   Compruebe los registros generados en el servidor de procesos en los directorios siguientes para obtener más información sobre los errores correspondientes:

      C:\ProgramData\ASR\home\svsystems\transport\log\cxps.err
      and
      C:\ProgramData\ASR\home\svsystems\transport\log\cxps.xfer
* Compruebe los registros siguientes en el servidor de procesos en caso de que no hay ningún latido de PS. Esto se identifica mediante **código de Error 806** en el portal.

      C:\ProgramData\ASR\home\svsystems\eventmanager*.log
      and
      C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

* **Compruebe si el servidor de procesos está insertando datos activamente en Azure**.

  1. En el servidor de procesos, abra el Administrador de tareas (presione Ctrl+Mayús+Esc).
  2. Seleccione la pestaña **Rendimiento** y, luego, el vínculo **Abrir el Monitor de recursos**. 
  3. En la página **Monitor de recursos**, seleccione la pestaña **Red**. En **Procesos con actividad de red**, compruebe si **cbengine.exe** está enviando activamente un gran número de datos.

       ![Captura de pantalla que muestra los volúmenes en Procesos con Actividad de red](./media/vmware-azure-troubleshoot-replication/cbengine.png)

  Si cbengine.exe no está enviando un gran volumen de datos, realice los pasos de las secciones siguientes.

* **Compruebe si el servidor de procesos puede conectarse a Azure Blob Storage**.

  Seleccione **cbengine.exe**. En **Conexiones TCP**, compruebe si hay conectividad entre el servidor de procesos y la dirección URL de Azure Blob Storage.

  ![Captura de pantalla que muestra la conectividad entre cbengine.exe y la dirección URL de Azure Blob Storage](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

  Si no hay conectividad entre el servidor de procesos y la dirección URL de Azure Blob Storage, en el Panel de Control, seleccione **Servicios**. Compruebe si se están ejecutando los siguientes servicios:

  *  cxprocessserver
  *  InMage Scout VX Agent - Sentinel/Outpost
  *  Agente de Microsoft Azure Recovery Services
  *  Servicio Microsoft Azure Site Recovery
  *  tmansvc

  Inicie o reinicie cualquier servicio que no esté en ejecución. Compruebe si el problema persiste.

* **Compruebe si el servidor de procesos puede conectarse a la dirección IP pública de Azure mediante el puerto 443**.

  En %programfiles%\Microsoft Azure Recovery Services Agent\Temp, abra el archivo CBEngineCurr.errlog más reciente. En el archivo, busque **443** o la cadena **error en el intento de conexión**.

  ![Captura de pantalla que muestra los registros de errores en la carpeta Temp](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

  Si se muestran problemas, en la línea de comandos del servidor de procesos, use Telnet para hacer ping a la dirección IP pública Azure (la dirección IP se enmascara en la imagen anterior). Puede encontrar la dirección IP pública de Azure en el archivo CBEngineCurr.currLog mediante el puerto 443:

  `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

  Si no se puede conectar, compruebe si el problema de acceso se debe a la configuración del firewall o del proxy como se describe en el paso siguiente.

* **Compruebe si el firewall basado en la dirección IP en el servidor de procesos bloquea el acceso**.

  Si usa reglas de firewall basadas en la dirección IP en el servidor, descargue la lista completa de [intervalos IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). Agregue los intervalos de direcciones IP a la configuración del firewall para asegurarse de que el firewall permita la comunicación con Azure (y con el puerto HTTPS predeterminado, 443). Permita intervalos de direcciones IP correspondientes a la región de Azure de la suscripción y a la región Oeste de EE. UU. de Azure (se usan para controlar el acceso y administrar las identidades).

* **Compruebe si un firewall basado en la dirección URL en el servidor de procesos bloquea el acceso**.

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

## <a name="common-errors-and-recommended-steps-for-resolution"></a>Los pasos para la resolución de errores comunes y recomendado

### <a name="initial-replication-issues-error-78169"></a>[Error 78169] de problemas de replicación inicial

A través de un anterior lo que garantiza que no hay ninguna conectividad, ancho de banda o tiempo sincronizar problemas relacionados con, asegúrese de que:

- Ningún software antivirus está bloqueando Azure Site Recovery. Obtenga información sobre [más](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) en exclusiones de carpetas necesarias para Azure Site Recovery.

### <a name="application-consistency-recovery-point-missing-error-78144"></a>[Error 78144]: falta el punto de recuperación de coherencia de aplicación

 Esto sucede debido a problemas con Volume Shadow copy Service (VSS). Para resolver este problema: 
 
- Compruebe que la versión instalada del agente de Azure Site Recovery es al menos 9.22.2. 
- Compruebe que el proveedor de VSS se instala como un servicio de servicios de Windows y compruebe también el servicio de componentes de MMC para comprobar que aparece el proveedor de VSS de Azure Site Recovery.
- Si no está instalado el proveedor de VSS, consulte el [artículo de solución de problemas de errores de instalación](vmware-azure-troubleshoot-push-install.md#vss-installation-failures).

- Si VSS está deshabilitado,
    - Compruebe que el tipo de inicio del servicio de proveedor de VSS está establecido en **automática**.
    - Reinicie los servicios siguientes:
        - Servicio VSS
        - Proveedor VSS de Azure Site Recovery
        - Servicio VDS

### <a name="high-churn-on-source-machine-error-78188"></a>Alto grado de renovación en la máquina de origen [Error 78188]

Causas posibles:
- La tasa de cambio de datos (bytes de escritura/s) en los discos enumerados de la máquina virtual es más que [límites admitidos de Azure Site Recovery](site-recovery-vmware-deployment-planner-analyze-report.md#azure-site-recovery-limits) para el tipo de cuenta de almacenamiento de destino de replicación.
- Hay un pico repentino de la tasa de renovación debido a que gran cantidad de datos está pendiente para la carga.

Para resolver el problema:
- Asegúrese de que el tipo de cuenta de almacenamiento de destino (Standard o Premium) se haya aprovisionado según el requisito de tasa de renovación en el origen.
- Si la actividad observada es temporal, espere unas pocas horas para la carga de datos pendientes para ponerse al día y a crear puntos de recuperación.
- Si el problema persiste, utilice ASR [planner implementación](site-recovery-deployment-planner.md#overview) para ayudar a planear la replicación.

### <a name="no-heartbeat-from-source-machine-error-78174"></a>No hay latido de la máquina de origen [Error 78174]

Esto sucede cuando el agente de Azure Site Recovery Mobility en la máquina de origen no se comunica con el servidor de configuración (CS).

Para resolver el problema, use los pasos siguientes para comprobar la conectividad de red de la máquina virtual de origen al servidor de configuración:

1. Compruebe que se está ejecutando la máquina de origen.
2. Inicie sesión en la máquina de origen con una cuenta que tenga privilegios de administrador.
3. Compruebe que los siguientes servicios se están ejecutando y si no reiniciar los servicios:
   - Svagents (agente VX de InMage Scout)
   - Servicio de aplicación de InMage Scout
4. En la máquina de origen, examine los registros en la ubicación de los detalles del error:

       C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log
    
### <a name="no-heartbeat-from-process-server-error-806"></a>Ningún latido desde el servidor de procesos [Error 806]
En caso de que no hay ningún latido desde el servidor de proceso (PS), compruebe que:
1. PS Máquina virtual está en funcionamiento
2. Compruebe los registros siguiente en el servidor de procesos para obtener información detallada del error:

       C:\ProgramData\ASR\home\svsystems\eventmanager*.log
       and
       C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

### <a name="no-heartbeat-from-master-target-error-78022"></a>Ningún latido desde el destino maestro [Error 78022]

Esto sucede cuando el agente de Azure Site Recovery Mobility en el destino maestro no se comunica con el servidor de configuración.

Para resolver el problema, use los pasos siguientes para comprobar el estado del servicio:

1. Compruebe que se está ejecutando la máquina virtual de destino maestro.
2. Inicie sesión en la VM de destino maestro con una cuenta que tenga privilegios de administrador.
    - Compruebe que se está ejecutando el servicio svagents. Si se está ejecutando, reinicie el servicio
    - Compruebe los registros en la ubicación de los detalles del error:
        
          C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log

### <a name="process-server-is-not-reachable-from-the-source-machine-error-78186"></a>Servidor de procesos no es accesible desde la máquina de origen [Error 78186]

Este error se lleva a los puntos coherentes de aplicación y bloqueos no se generan si no se trata. Para resolver el problema, siga los vínculos de la solución de problemas siguiente:
1. Asegúrese de que [PS servicios se están ejecutando](vmware-azure-troubleshoot-replication.md#monitor-process-server-health-to-avoid-replication-issues)
2. [Comprobar los problemas de conectividad de la máquina de origen](vmware-azure-troubleshoot-replication.md#check-the-source-machine-for-connectivity-issues)
3. [Comprobar los problemas de conectividad del servidor de proceso](vmware-azure-troubleshoot-replication.md#check-the-process-server-for-connectivity-issues) y siga las instrucciones proporcionadas para:
    - Comprobando la conectividad con origen
    - Problemas de proxy y Firewall

### <a name="data-upload-blocked-from-source-machine-to-process-server-error-78028"></a>Carga de datos bloqueada desde la máquina de origen al servidor de procesos [Error 78028]

Este error se lleva a los puntos coherentes de aplicación y bloqueos no se generan si no se trata. Para resolver el problema, siga los vínculos de la solución de problemas siguiente:

1. Asegúrese de que [PS servicios se están ejecutando](vmware-azure-troubleshoot-replication.md#monitor-process-server-health-to-avoid-replication-issues)
2. [Comprobar los problemas de conectividad de la máquina de origen](vmware-azure-troubleshoot-replication.md#check-the-source-machine-for-connectivity-issues)
3. [Comprobar los problemas de conectividad del servidor de proceso](vmware-azure-troubleshoot-replication.md#check-the-process-server-for-connectivity-issues) y siga las instrucciones proporcionadas para:
    - Comprobando la conectividad con origen
    - Problemas de proxy y Firewall

## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda, puede publicar su pregunta en el [foro de Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Contamos con una comunidad activa y uno de nuestros ingenieros puede ayudarle.
