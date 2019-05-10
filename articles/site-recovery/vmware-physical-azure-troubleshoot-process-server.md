---
title: Solucionar problemas del servidor de proceso de Azure Site Recovery
description: En este artículo se describe cómo solucionar problemas relacionados con el servidor de procesos de Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 04/29/2019
ms.author: rayne
ms.openlocfilehash: 0383226853ed86943b73d2b8740825967f3124c9
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411527"
---
# <a name="troubleshoot-the-process-server"></a>Solucionar problemas del servidor de proceso

El [Site Recovery](site-recovery-overview.md) servidor de procesos se usa cuando configuró la recuperación ante desastres en Azure para máquinas virtuales de VMware locales y servidores físicos. En este artículo se describe cómo solucionar problemas con el servidor de procesos, incluidos los problemas de replicación y la conectividad.

[Obtenga más información](vmware-physical-azure-config-process-server-overview.md) sobre el servidor de procesos.

## <a name="before-you-start"></a>Antes de comenzar

Antes de empezar a solucionar problemas:

1. Asegúrese de que comprende cómo [supervisar servidores de procesos](vmware-physical-azure-monitor-process-server.md).
2. Revise las prácticas recomendadas siguientes.
3. Asegúrese de seguir [las consideraciones de capacidad](site-recovery-plan-capacity-vmware.md#capacity-considerations)y utilizar la Guía de ajuste de tamaño para el [servidor de configuración](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server-and-inbuilt-process-server) o [servidores de procesos independientes](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server).

## <a name="best-practices-for-process-server-deployment"></a>Procedimientos recomendados para la implementación de servidor de procesos

Para un rendimiento óptimo de los servidores de procesos, nos hemos resume una serie de prácticas recomendadas generales.

**Procedimiento recomendado** | **Detalles**
--- |---
**Uso** | Asegúrese de que el servidor de procesos independiente o servidor de configuración solo se usan para la finalidad prevista. No se ejecutan nada más en el equipo.
**Dirección IP** | Asegúrese de que el servidor de procesos tiene una dirección IPv4 estática y no tiene NAT configurado.
**Controlar el uso de memoria y CPU** |Mantenga un 70% de uso de CPU y memoria en.
**Garantizar espacio libre** | Espacio libre hace referencia al espacio de disco de caché en el servidor de procesos. Datos de replicación se almacenan en la memoria caché antes de cargarlos en Azure.<br/><br/> Mantener espacio libre por encima del 25%. Si deja por debajo del 20%, la replicación está limitada para las máquinas replicadas asociadas con el servidor de procesos.

## <a name="check-process-server-health"></a>Compruebe el estado del servidor de proceso

Es el primer paso para la solución de problemas comprobar el estado y el estado del servidor de proceso. Para ello, revise todas las alertas, compruebe que los servicios necesarios están ejecutando y comprobación que hay un latido del servidor de procesos. En el gráfico siguiente, se resumen estos pasos seguidos por los procedimientos que le ayudarán a realizar los pasos.

![Solución de problemas de estado del servidor de proceso](./media/vmware-physical-azure-troubleshoot-process-server/troubleshoot-process-server-health.png)

## <a name="step-1-troubleshoot-process-server-health-alerts"></a>Paso 1: Solución de problemas de alertas de estado del servidor de proceso

El servidor de procesos genera un número de alertas de estado. Estas alertas y las acciones recomendadas, se resumen en la tabla siguiente.

**Tipo de alerta** | **Error** | **Solución de problemas**
--- | --- | --- 
![Estado correcto][green] | None  | Servidor de procesos está conectado y en buen estado.
![Advertencia][yellow] | No se estén ejecutando los servicios especificados. | 1. Compruebe que se ejecutan los servicios.<br/> 2. Si los servicios se están ejecutando según lo previsto, siga las instrucciones siguientes para [solucionar problemas de conectividad y la replicación](#check-connectivity-and-replication).
![Advertencia][yellow]  | Uso de CPU > 80% durante los últimos 15 minutos. | 1. No agregue nuevos equipos.<br/>2. Compruebe que el número de máquinas virtuales con el servidor de procesos se alinea con [definido límites](site-recovery-plan-capacity-vmware.md#capacity-considerations)y considere la posibilidad de configurar un [servidor de procesos adicional](vmware-azure-set-up-process-server-scale.md).<br/>3. Siga las instrucciones siguientes para [solucionar problemas de conectividad y la replicación](#check-connectivity-and-replication).
![Fundamental][red] |  Uso de CPU > 95% durante los últimos 15 minutos. | 1. No agregue nuevos equipos.<br/>2. Compruebe que el número de máquinas virtuales con el servidor de procesos se alinea con [definido límites](site-recovery-plan-capacity-vmware.md#capacity-considerations)y considere la posibilidad de configurar un [servidor de procesos adicional](vmware-azure-set-up-process-server-scale.md).<br/>3. Siga las instrucciones siguientes para [solucionar problemas de conectividad y la replicación](#check-connectivity-and-replication).<br/> 4. Si el problema persiste, ejecute el [Deployment Planner](https://aka.ms/asr-v2a-deployment-planner) para la replicación de servidor de VMware y físicas.
![Advertencia][yellow] | Uso de memoria > 80% durante los últimos 15 minutos. |  1. No agregue nuevos equipos.<br/>2. Compruebe que el número de máquinas virtuales con el servidor de procesos se alinea con [definido límites](site-recovery-plan-capacity-vmware.md#capacity-considerations)y considere la posibilidad de configurar un [servidor de procesos adicional](vmware-azure-set-up-process-server-scale.md).<br/>3. Siga las instrucciones asociadas con la advertencia.<br/> 4. Si el problema persiste, siga las instrucciones siguientes para [solucionar problemas de conectividad y la replicación](#check-connectivity-and-replication).
![Fundamental][red] | Uso de memoria > 95% durante los últimos 15 minutos. | 1. No agregue nuevos equipos y teniendo en cuenta cómo configurar un [servidor de procesos adicional](vmware-azure-set-up-process-server-scale.md).<br/> 2. Siga las instrucciones asociadas con la advertencia.<br/> 3. 4. Si el problema persiste, siga las instrucciones siguientes para [solucionar problemas de conectividad y la replicación](#check-connectivity-and-replication).<br/> 4. Si el problema persiste, ejecute el [Deployment Planner](https://aka.ms/asr-v2a-deployment-planner) problemas de replicación de VMware/físico del servidor.
![Advertencia][yellow] | Carpeta de caché de espacio libre < 30% durante los últimos 15 minutos. | 1. No agregar nuevas máquinas y considere la posibilidad de configurar un [servidor de procesos adicional](vmware-azure-set-up-process-server-scale.md).<br/>2. Compruebe que el número de máquinas virtuales con el servidor de procesos se alinea con [directrices](site-recovery-plan-capacity-vmware.md#capacity-considerations).<br/> 3. Siga las instrucciones siguientes para [solucionar problemas de conectividad y la replicación](#check-connectivity-and-replication).
![Fundamental][red] |  < 25% de espacio libre para los últimos 15 minutos | 1. Siga las instrucciones asociadas con la advertencia para este problema.<br/> 2. 3. Siga las instrucciones siguientes para [solucionar problemas de conectividad y la replicación](#check-connectivity-and-replication).<br/> 3. Si el problema persiste, ejecute el [Deployment Planner](https://aka.ms/asr-v2a-deployment-planner) para la replicación de servidor de VMware y físicas.
![Fundamental][red] | Ningún latido desde el servidor de procesos durante 15 minutos o más. El servicio tmansvs no se comunica con el servidor de configuración. | (1) Compruebe que el servidor de procesos está en funcionamiento.<br/> 2. Compruebe que el tmassvc se ejecuta en el servidor de procesos.<br/> 3. Siga las instrucciones siguientes para [solucionar problemas de conectividad y la replicación](#check-connectivity-and-replication).


![Clave de la tabla](./media/vmware-physical-azure-troubleshoot-process-server/table-key.png)


## <a name="step-2-check-process-server-services"></a>Paso 2: Compruebe los servicios de servidor de proceso

En la tabla siguiente se resumen los servicios que deben ejecutarse en el servidor de procesos. Hay ligeras diferencias en los servicios, en función de cómo se implementa el servidor de procesos. 

Para todos los servicios excepto el agente de Microsoft Azure Recovery Services (obengine), compruebe que el StartType está establecido en **automática** o **automático (Inicio retrasado)**.
 
**Implementación** | **Servicios en ejecución**
--- | ---
**Servidor de procesos en el servidor de configuración** | ProcessServer; ProcessServerMonitor; cxprocessserver; InMage PushInstall; Servicio de carga de registro (LogUpload); InMage Scout Application Service; Agente de Microsoft Azure Recovery Services (obengine); InMage Scout VX Agent-Sentinel/Outpost (svagents); tmansvc; World Wide Web (W3SVC); servicio de publicación MySQL; Servicio de Microsoft Azure Site Recovery (dra)
**Servidor de procesos que se ejecuta como un servidor independiente** | ProcessServer; ProcessServerMonitor; cxprocessserver; InMage PushInstall; Servicio de carga de registro (LogUpload); InMage Scout Application Service; Agente de Microsoft Azure Recovery Services (obengine); InMage Scout VX Agent-Sentinel/Outpost (svagents); tmansvc.
**Servidor de procesos implementado en Azure para la conmutación por recuperación** | ProcessServer; ProcessServerMonitor; cxprocessserver; InMage PushInstall; Servicio de carga de registro (LogUpload)


## <a name="step-3-check-the-process-server-heartbeat"></a>Paso 3: Comprobar el latido del servidor de proceso

Si no hay ningún latido desde el servidor de procesos (código de error 806), realice lo siguiente:

1. Compruebe que el servidor de procesos de máquina virtual está en funcionamiento.
2. Compruebe estos registros de errores.

    C:\ProgramData\ASR\home\svsystems\eventmanager*C\ProgramData\ASR\home\svsystems\monitor_protection .log*.log

## <a name="check-connectivity-and-replication"></a>Compruebe la conectividad y la replicación

 Errores de replicación inicial y continua a menudo están provocados por problemas de conectividad entre máquinas de origen y el servidor de procesos o entre el servidor de procesos y Azure. En el gráfico siguiente, se resumen estos pasos seguidos por los procedimientos que le ayudarán a realizar los pasos.

![Solución de problemas de conectividad y la replicación](./media/vmware-physical-azure-troubleshoot-process-server/troubleshoot-connectivity-replication.png)


## <a name="step-4-verify-time-sync-on-source-machine"></a>Paso 4: Comprobar la sincronización de hora en la máquina de origen

Asegúrese de que la fecha y hora del sistema de la máquina replicada está sincronizada. [Más información](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)

## <a name="step-5-check-anti-virus-software-on-source-machine"></a>Paso 5: Compruebe el software antivirus en la máquina de origen

Compruebe que ningún software antivirus en la máquina replicada está bloqueando Site Recovery. Si tiene que excluir Site Recovery desde los programas antivirus, revise [en este artículo](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).

## <a name="step-6-check-connectivity-from-source-machine"></a>Paso 6: Compruebe la conectividad de máquina de origen


1. Instalar el [cliente Telnet](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx) en la máquina de origen si necesita. No use Ping.
2. En la máquina de origen, haga ping en el servidor de procesos en el puerto HTTPS con Telnet. De forma predeterminada 9443 es el puerto HTTPS para el tráfico de replicación.

    `telnet <process server IP address> <port>`

3. Compruebe si la conexión es correcta.


**Conectividad** | **Detalles** | **Acción**
--- | --- | ---
**Successful** | Telnet muestra una pantalla en blanco y el servidor de procesos sea accesible. | Ninguna otra acción necesaria.
**Incorrecto** | No puede conectarse | Asegúrese de que se permite el puerto de entrada 9443 en el servidor de procesos. Por ejemplo, si tiene una red perimetral o en una subred filtrada. Vuelva a comprobar la conectividad.
**Parcialmente correcta** | Puede conectarse, pero la máquina de origen notifica que no se puede alcanzar el servidor de procesos. | Continúe con el siguiente procedimiento para solucionar problemas.

## <a name="step-7-troubleshoot-an-unreachable-process-server"></a>Paso 7: Solución de problemas de un servidor de procesos inaccesible

Si el servidor de procesos no es accesible desde la máquina de origen, se mostrará el error 78186. Si no se trata este problema dará lugar a la aplicación coherente y puntos de recuperación coherente con el bloqueo no se generan según lo previsto.

Solución de problemas mediante la comprobación de si la máquina de origen puede llegar a la dirección IP del servidor de procesos y ejecute la herramienta de cxpsclient en la máquina de origen, para comprobar la conexión de extremo a otro.


### <a name="check-the-ip-connection-on-the-process-server"></a>Compruebe la conexión de IP en el servidor de procesos

Si telnet es correcta pero la máquina de origen notifica que no se puede alcanzar el servidor de procesos, compruebe si puede llegar a la dirección IP del servidor de procesos.

1. En un explorador web, intentar conectarse a IP dirección https://<PS_IP>:<PS_Data_Port>/.
2. Si esta comprobación muestra un error de certificado HTTPS, que es normal. Si omite el error, debería ver un 400 - Solicitud incorrecta. Esto significa que el servidor no puede atender la solicitud del explorador, y que la conexión HTTPS estándar está bien.
3. Si esta comprobación no funciona, tenga en cuenta el mensaje de error del explorador. Por ejemplo, un error 407 indicará un problema con la autenticación de proxy.

### <a name="check-the-connection-with-cxpsclient"></a>Compruebe la conexión con cxpsclient

Además, puede ejecutar la herramienta cxpsclient para comprobar la conexión de extremo a otro.

1. Ejecute la herramienta de la manera siguiente:

    ```
    <install folder>\cxpsclient.exe -i <PS_IP> -l <PS_Data_Port> -y <timeout_in_secs:recommended 300>
    ```

2. En el servidor de procesos, compruebe los registros generados en estas carpetas:

    C:\ProgramData\ASR\home\svsystems\transport\log\cxps.err  C:\ProgramData\ASR\home\svsystems\transport\log\cxps.xfer



### <a name="check-source-vm-logs-for-upload-failures-error-78028"></a>Compruebe los registros de la máquina virtual de origen para cargar los errores (error 78028)

Problema con cargas de datos bloqueada las máquinas de origen para el servicio de proceso puede dar lugar a ambos puntos de recuperación coherente con los bloqueos y coherentes con la aplicación no se generan. 

1. Para solucionar errores de carga de red, puede buscar errores en este registro:

    C:\Program archivos (x86) \Microsoft Azure Site Recovery\agent\svagents*.log 

2. Use el resto de los procedimientos descritos en este artículo puede ayudarle a resolver problemas de carga de datos.



## <a name="step-8-check-whether-the-process-server-is-pushing-data"></a>Paso 8: Compruebe si el servidor de procesos está insertando datos

Compruebe si el servidor de procesos está insertando activamente los datos en Azure.

  1. En el servidor de procesos, abra el Administrador de tareas (presione Ctrl+Mayús+Esc).
  2. Seleccione el **rendimiento** ficha > **abrir el Monitor de recursos**.
  3. En **Monitor de recursos** página, seleccione el **red** ficha. En **procesos con actividad de red**, compruebe si cbengine.exe está enviando activamente un vNotolume grande de datos.

       ![Volúmenes en los procesos con actividad de red](./media/vmware-physical-azure-troubleshoot-process-server/cbengine.png)

  Si cbengine.exe no está enviando un gran volumen de datos, realice los pasos de las secciones siguientes.

## <a name="step-9-check-the-process-server-connection-to-azure-blob-storage"></a>Paso 9: Compruebe la conexión de servidor de proceso en Azure blob storage

1. En el Monitor de recursos, seleccione **cbengine.exe**.
2. En **las conexiones TCP**, compruebe si hay conectividad entre el servidor de procesos para el almacenamiento de Azure.

  ![Conectividad entre cbengine.exe y la dirección URL de almacenamiento de blobs de Azure](./media/vmware-physical-azure-troubleshoot-process-server/rmonitor.png)

### <a name="check-services"></a>Comprobación de servicios

Si no hay ninguna conectividad desde el servidor de procesos a la dirección URL de almacenamiento de blobs de Azure, compruebe que se están ejecutando los servicios.

1. En el Panel de Control, seleccione **servicios**.
2. Compruebe que se están ejecutando los siguientes servicios:

    - cxprocessserver
    - InMage Scout VX Agent - Sentinel/Outpost
    - Agente de Microsoft Azure Recovery Services
    - Servicio Microsoft Azure Site Recovery
    - tmansvc

3. Inicie o reinicie cualquier servicio que no esté en ejecución.
4. Compruebe que el servidor de procesos está conectado y es accesible. 

## <a name="step-10-check-the-process-server-connection-to-azure-public-ip-address"></a>Paso 10: comprobar la conexión de servidor de proceso a la dirección IP pública de Azure

1. En el servidor de procesos en **%programfiles%\Microsoft Azure Recovery Services Agent\Temp**, abra el archivo del archivo CBEngineCurr.errlog más reciente.
2. En el archivo, busque **443**, o la cadena **intento de conexión no se pudo**.

  ![Los registros de errores en la carpeta Temp](./media/vmware-physical-azure-troubleshoot-process-server/logdetails1.png)

3. Si ve el problema, encuentra su dirección IP pública de Azure en el archivo CBEngineCurr.currLog mediante el puerto 443:

  `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

5. En la línea de comandos en el servidor de procesos, use Telnet para hacer ping a la dirección IP pública de Azure.
6. Si no se puede conectar, siga el procedimiento siguiente.

## <a name="step-11-check-process-server-firewall-settings"></a>Paso 11: Compruebe la configuración de firewall del servidor de proceso. 

Compruebe si el firewall basadas en direcciones IP en el servidor de procesos está bloqueando el acceso.

1. Para las reglas de firewall basadas en direcciones IP:

    (a) Descargar la lista completa de [intervalos IP de centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653).

    (b) agregue los intervalos de direcciones IP a la configuración de firewall para asegurarse de que el firewall permita la comunicación a Azure (y el puerto predeterminado HTTPS 443).

    Permitir (c) intervalos de direcciones IP para la región de Azure de su suscripción y para la región West US Azure (se usa para la administración de identidades y control de acceso).

2. Para los firewalls basados en direcciones URL, agregue las direcciones URL aparece en la tabla siguiente para la configuración del firewall.

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  


## <a name="step-12-verify-process-server-proxy-settings"></a>Paso 12: Compruebe la configuración de proxy del servidor de proceso 

1. Si usa un servidor proxy, asegúrese de que el servidor DNS resuelva el nombre de dicho servidor. Compruebe el valor que proporcionó al configurar el servidor de configuración en la clave del registro **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings**.
2. Asegúrese de que el agente de Azure Site Recovery se utiliza la misma configuración para enviar datos.

    (a) busque **copia de seguridad de Microsoft Azure**.

    (b) abierto **Microsoft Azure Backup**y seleccione **acción** > **cambiar las propiedades**.

    c) en el **configuración de Proxy** ficha, la dirección de proxy debe ser la misma que la dirección de proxy que se muestra en la configuración del registro. Si no es así, cámbiela por la misma dirección.

## <a name="step-13-check-bandwidth"></a>Paso 13: Comprobar el ancho de banda

Aumentar el ancho de banda entre el servidor de procesos y Azure y, a continuación, compruebe si el problema persiste.


## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda, puede publicar su pregunta en el [foro de Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). 

[green]: ./media/vmware-physical-azure-troubleshoot-process-server/green.png
[yellow]: ./media/vmware-physical-azure-troubleshoot-process-server/yellow.png
[red]: ./media/vmware-physical-azure-troubleshoot-process-server/red.png