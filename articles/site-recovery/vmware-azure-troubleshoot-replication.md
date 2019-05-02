---
title: Solución de problemas de replicación para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se proporciona información para solucionar problemas comunes de replicación durante la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/14/2019
ms.author: mayg
ms.openlocfilehash: 3221b2bd18b8b0756f280d88fffc6016d0498b8f
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924819"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Solución de problemas de replicación de máquinas virtuales de VMware y de servidores físicos

En este artículo se describe algunos problemas comunes y errores específicos que pueden surgir al replicar máquinas virtuales de VMware locales y servidores físicos en Azure mediante [Site Recovery](site-recovery-overview.md).

## <a name="step-1-monitor-process-server-health"></a>Paso 1: Monitor estado del servidor de proceso

Usos de la recuperación del sitio el [servidor de procesos](vmware-physical-azure-config-process-server-overview.md#process-server) para recibir y optimizar los datos replicados y enviarla a Azure.

Recomendamos que supervise el estado de los servidores de procesos en el portal, para asegurarse de que están conectados y funcionan correctamente y que la replicación se lleva a cabo para las máquinas de origen asociadas con el servidor de procesos.

- [Obtenga información sobre](vmware-physical-azure-monitor-process-server.md) supervisar los servidores de proceso.
- [Revisión de los procedimientos recomendados](vmware-physical-azure-troubleshoot-process-server.md#best-practices-for-process-server-deployment)
- [Solución de problemas de](vmware-physical-azure-troubleshoot-process-server.md#check-process-server-health) procesar el estado del servidor.

## <a name="step-2-troubleshoot-connectivity-and-replication-issues"></a>Paso 2: Solución de problemas de conectividad y la replicación

A menudo, los errores de replicación inicial y continua están provocados por problemas de conectividad entre el servidor de origen y el servidor de procesos o entre el servidor de procesos y Azure. 

Para solucionar estos problemas, [solucionar problemas de conectividad y la replicación](vmware-physical-azure-troubleshoot-process-server.md#check-connectivity-and-replication).




## <a name="step-3-troubleshoot-source-machines-that-arent-available-for-replication"></a>Paso 3: Solucionar problemas de máquinas de origen que no están disponibles para la replicación

Cuando intenta seleccionar la máquina de origen para habilitar la replicación con Site Recovery, puede que no esté disponible por uno de los siguientes motivos:

* **Dos máquinas virtuales con el mismo UUID de instancia**: Si dos máquinas virtuales en vCenter tienen el mismo UUID de instancia, en Azure Portal se muestra la primera máquina virtual detectada por el servidor de configuración. Para solucionar este problema, asegúrese de que no haya dos máquinas virtuales con el mismo UUID de instancia. Este escenario se suele producir en instancias donde una máquina virtual de copia de seguridad se activa y se registra en nuestros registros de detección. Consulte [Azure Site Recovery VMware-to-Azure: How to clean up duplicate or stale entries](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) (De VMware a Azure en Azure Site Recovery: cómo limpiar entradas duplicadas u obsoletas) para solucionarlo.
* **Credenciales de usuario de vCenter incorrectas**: Asegúrese de que ha agregado las credenciales correctas de vCenter al configurar el servidor de configuración mediante la plantilla OVF o la instalación unificada. Para comprobar las credenciales que ha agregado durante la instalación, consulte [Modificación de las credenciales para la detección automática](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* **Privilegios de vCenter insuficientes**: Si los permisos proporcionados para acceder a vCenter no son los necesarios, es posible que no se puedan detectar las máquinas virtuales. Asegúrese de que los permisos descritos en [Preparación de una cuenta de detección automática](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) se agregan a la cuenta de usuario de vCenter.
* **Servidores de administración de Azure Site Recovery**: Si la máquina virtual se usa como servidor de administración con al menos uno de los roles de servidor de configuración, servidor de procesos de escalado horizontal o servidor de destino principal, no podrá seleccionar la máquina virtual en el portal. Los servidores de administración no se pueden replicar.
* **Máquina virtual ya protegida o conmutada por error mediante servicios de Azure Site Recovery**: Si la máquina virtual ya se ha conmutado por error o ya está protegida mediante Site Recovery, no estará disponible para seleccionarla para la protección en el portal. Asegúrese de que la máquina virtual que busca en el portal no esté aún protegida por ningún otro usuario o en otras suscripciones.
* **vCenter no conectado**: Compruebe si vCenter tiene el estado conectado. Para ello, vaya al almacén de Recovery Services > Infraestructura de Site Recovery > Servidores de configuración y haga clic en el servidor de configuración correspondiente; se abrirá una hoja a la derecha con detalles acerca de los servidores asociados. Compruebe si vCenter está conectado. Si está en un estado "No conectado", resuelva el problema y, a continuación, [actualizar el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server) en el portal. Una vez hecho esto, la máquina virtual se mostrará en el portal.
* **ESXi apagado**: Si el host ESXi en el que reside la máquina virtual está apagado, la máquina virtual no se mostrará ni se podrá seleccionar en Azure Portal. Encienda el host ESXi y [actualice el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server) en el portal. Una vez hecho esto, la máquina virtual se mostrará en el portal.
* **Reinicio pendiente**: Si la máquina virtual está pendiente de reiniciarse, no se podrá seleccionar en Azure Portal. Asegúrese de completar el reinicio pendiente y [actualice el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Una vez hecho esto, la máquina virtual se mostrará en el portal.
* **Dirección IP no encontrada**: Si la máquina virtual no tiene una dirección IP válida asociada, no se podrá seleccionar en Azure Portal. Asegúrese de asignar una dirección IP válida a la máquina virtual y [actualice el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Una vez hecho esto, la máquina virtual se mostrará en el portal.

### <a name="troubleshoot-protected-virtual-machines-greyed-out-in-the-portal"></a>Solución de problemas de las máquinas virtuales protegidas atenuadas en el portal

Las máquinas virtuales que se replican en Site Recovery no están disponibles en Azure Portal si hay entradas duplicadas en el sistema. Para aprender a eliminar entradas obsoletas y resolver el problema, consulte [Azure Site Recovery VMware-to-Azure: How to clean up duplicate or stale entries](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) (De VMware a Azure en Azure Site Recovery: cómo limpiar entradas duplicadas u obsoletas).

## <a name="common-errors-and-solutions"></a>Errores comunes y soluciones

### <a name="initial-replication-issues-error-78169"></a>Problemas de replicación inicial [error 78169]

A través de un anterior lo que garantiza que no hay ninguna conectividad, ancho de banda o tiempo sincronizar problemas relacionados con, asegúrese de que:

- Ningún software antivirus está bloqueando Azure Site Recovery. Obtenga información sobre [más](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) en exclusiones de carpetas necesarias para Azure Site Recovery.

### <a name="missing-app-consistent-recovery-points-error-78144"></a>Faltan puntos de recuperación coherente con la aplicación [error 78144]

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

### <a name="source-machines-with-high-churn-error-78188"></a>Máquinas de origen con la renovación elevada [error 78188]

Causas posibles:
- La tasa de cambio de datos (bytes de escritura/s) en los discos enumerados de la máquina virtual es más que [límites admitidos de Azure Site Recovery](site-recovery-vmware-deployment-planner-analyze-report.md#azure-site-recovery-limits) para el tipo de cuenta de almacenamiento de destino de replicación.
- Hay un pico repentino de la tasa de renovación debido a que gran cantidad de datos está pendiente para la carga.

Para resolver el problema:
- Asegúrese de que el tipo de cuenta de almacenamiento de destino (Standard o Premium) se haya aprovisionado según el requisito de tasa de renovación en el origen.
- Si la actividad observada es temporal, espere unas pocas horas para la carga de datos pendientes para ponerse al día y a crear puntos de recuperación.
- Si el problema persiste, use la recuperación del sitio [planner implementación](site-recovery-deployment-planner.md#overview) para ayudar a planear la replicación.

### <a name="source-machines-with-no-heartbeat-error-78174"></a>Máquinas de origen con ningún latido [error 78174]

Esto sucede cuando el agente de Azure Site Recovery Mobility en la máquina de origen no se comunica con el servidor de configuración (CS).

Para resolver el problema, use los pasos siguientes para comprobar la conectividad de red de la máquina virtual de origen al servidor de configuración:

1. Compruebe que se está ejecutando la máquina de origen.
2. Inicie sesión en la máquina de origen con una cuenta que tenga privilegios de administrador.
3. Compruebe que los siguientes servicios se están ejecutando y si no reiniciar los servicios:
   - Svagents (agente VX de InMage Scout)
   - Servicio de aplicación de InMage Scout
4. En la máquina de origen, examine los registros en la ubicación de los detalles del error:

       C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log
    
### <a name="process-server-with-no-heartbeat-error-806"></a>Servidor de procesos con ningún latido [error 806]
En caso de que no hay ningún latido desde el servidor de proceso (PS), compruebe que:
1. PS Máquina virtual está en funcionamiento
2. Compruebe los registros siguiente en el servidor de procesos para obtener información detallada del error:

       C:\ProgramData\ASR\home\svsystems\eventmanager*.log
       and
       C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

### <a name="master-target-server-with-no-heartbeat-error-78022"></a>Servidor de destino maestro con ningún latido [error 78022]

Esto sucede cuando el agente de Azure Site Recovery Mobility en el destino maestro no se comunica con el servidor de configuración.

Para resolver el problema, use los pasos siguientes para comprobar el estado del servicio:

1. Compruebe que se está ejecutando la máquina virtual de destino maestro.
2. Inicie sesión en la VM de destino maestro con una cuenta que tenga privilegios de administrador.
    - Compruebe que se está ejecutando el servicio svagents. Si se está ejecutando, reinicie el servicio
    - Compruebe los registros en la ubicación de los detalles del error:
        
          C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log



## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda, puede publicar su pregunta en el [foro de Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Contamos con una comunidad activa y uno de nuestros ingenieros puede ayudarle.
