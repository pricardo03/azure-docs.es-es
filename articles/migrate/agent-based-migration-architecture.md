---
title: Migración basada en agente en Azure Migrate Server
description: Proporciona información general sobre la migración de VM de VMware basada en agente con Azure Migrate Server Migration.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: a8477b4c10ccbc76f36eed4d64ac12e8bb648a28
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74186097"
---
# <a name="agent-based-migration-architecture"></a>Arquitectura de migración basada en agente

En este artículo se proporciona información general sobre la arquitectura y los procesos que se usan para la replicación basada en agente con la herramienta Azure Migrate Server Migration.

[Azure Migrate](migrate-services-overview.md) proporciona un centro de conectividad para realizar el seguimiento de la detección, evaluación y migración a Azure de las aplicaciones y cargas de trabajo locales, así como de las instancias de AWS o VM de GCP. El centro proporciona herramientas de Azure Migrate para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV).

## <a name="agent-based-replication"></a>Replicación basada en agente

La replicación basada en agente en la herramienta Azure Migrate Server Replication se usa para migrar VM de VMware locales y servidores físicos a Azure. También se puede usar para migrar otros servidores virtualizados locales, así como VM de nube privada y pública, incluidas instancias de AWS y VM de GCP.

Para la migración de VMware, la herramienta Azure Migrate Server Migration ofrece un par de opciones:

- Migración mediante la replicación basada en agente, como se describe en este artículo.
- Replicación sin agente, para migrar VM sin necesidad de instalar nada en ellas.

Obtenga más información sobre [cómo seleccionar un método de migración para VMware](server-migrate-overview.md).

## <a name="server-migration-and-azure-site-recovery"></a>Server Migration y Azure Site Recovery

Azure Migrate Server Migration es una herramienta para migrar a Azure cargas de trabajo locales y de la nube pública. Está optimizada para la migración. Site Recovery es una herramienta de recuperación ante desastres. Azure Server Migration y Site Recovery comparten algunos componentes tecnológicos comunes que se usan para la replicación de datos, pero sirven para propósitos diferentes.

## <a name="architectural-components"></a>Componentes de la arquitectura

![Arquitectura](./media/agent-based-replication-architecture/architecture.png)

En la tabla se resumen los componentes usados para la migración basada en agente.

**Componente** | **Detalles** | **Instalación**
--- | --- | ---
**Dispositivo de replicación** | El dispositivo de replicación (servidor de configuración) es un equipo local que sirve de puente entre el entorno local y la herramienta Azure Migrate Server Migration. El dispositivo detecta el inventario de VM local, de modo que Azure Server Migration pueda orquestar la replicación y migración. El dispositivo tiene dos componentes:<br/><br/> **Servidor de configuración**: Se conecta a Azure Migrate Server Migration y coordina la replicación.<br/> **Servidor de proceso**: Controla la replicación de datos. Recibe datos de las VM, los comprime y los cifra y, luego, los envía a la suscripción de Azure. Allí, Server Migration escribe los datos en discos administrados. | De manera predeterminada, el servidor de procesos se instala junto con el servidor de configuración en el dispositivo de replicación.
**Servicio de movilidad** | Mobility Service es un agente que se instala en cada máquina que quiera replicar y migrar. Envía los datos de replicación desde la máquina al servidor de procesos. Hay diversos agentes de Mobility Service disponibles. | Los archivos de instalación de Mobility Service se encuentran en el dispositivo de replicación. Descargue e instale el agente que necesita, de acuerdo con el sistema operativo y la versión de la máquina que quiere replicar.

### <a name="mobility-service-installation"></a>Instalación de Mobility Service

Puede implementar Mobility Service con los métodos siguientes:

- **Instalación de inserción**: El servidor de procesos instala Mobility Service cuando usted habilita la protección para una máquina. 
- **Instalación manual**: Puede instalar Mobility Service manualmente en cada máquina a través de la interfaz de usuario o del símbolo del sistema.

Mobility Service se comunica con el dispositivo de replicación y con las máquinas replicadas. Si tiene software antivirus que se ejecuta en el dispositivo de replicación, en los servidores de procesos o en las máquinas que se están replicando, se deben excluir del análisis las siguientes carpetas:


- C:\Archivos de programa\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- C:\Program Files (x86)\Microsoft Azure Site Recovery
- C:\ProgramData\ASR\agent (en máquinas Windows con Mobility Service instalado)

## <a name="replication-process"></a>Proceso de replicación

1. Cuando habilita la replicación para una VM, comienza la replicación inicial en Azure.
2. Durante la replicación inicial, Mobility Service lee los datos de los discos de las máquinas y los envía al servidor de procesos.
3. Estos datos se usan para inicializar una copia del disco en la suscripción de Azure. 
4. Una vez terminada la replicación inicial, comienza la de los cambios incrementales en Azure. La replicación es a nivel de bloque y casi continua.
4. Mobility Service intercepta las escrituras en la memoria de disco de las VM, al integrarse en el subsistema de almacenamiento del sistema operativo. Este método evita las operaciones de E/S de disco en la máquina de replicación para una replicación incremental. 
5. Los cambios marcados para una máquina se envían al servidor de procesos en el puerto HTTPS 9443 entrante. Este puerto se puede modificar. El servidor de procesos los comprime y cifra, y los envía a Azure. 

## <a name="ports"></a>Puertos

**Dispositivo** | **Connection**
--- | --- 
Máquinas virtuales | La instancia de Mobility Service que se ejecuta en las VM se comunica con el dispositivo de replicación local en el puerto HTTPS 443 entrante, para la administración de la replicación.<br/><br/> Las VM envían los datos de replicación al servidor de procesos (que se ejecuta en el dispositivo de replicación de manera predeterminada) en el puerto HTTPS 9443 entrante. Este puerto se puede modificar.
Dispositivo de replicación | El dispositivo de replicación organiza la replicación con Azure a través del puerto HTTPS 443 saliente.
Servidor de proceso | El servidor de procesos recibe datos de replicación, los optimiza y los cifra para enviarlos después a Azure Storage a través del puerto 443 de salida.


## <a name="performance-and-scaling"></a>Rendimiento y escalado

De forma predeterminada, se implementa un único dispositivo de replicación que ejecuta tanto el servidor de configuración como el servidor de procesos. Si solo va a replicar algunas máquinas, esta implementación es suficiente. Sin embargo, si va a replicar y migrar cientos de máquinas, es posible que un único servidor de procesos no pueda controlar todo el tráfico de replicación. En este caso, puede implementar servidores de procesos de escalabilidad horizontal adicionales.

### <a name="site-recovery-deployment-planner-for-vmware"></a>Site Recovery Deployment Planner para VMware

Si va a replicar VM de VMware, puede usar [Site Recovery Deployment Planner](../site-recovery/site-recovery-deployment-planner.md) para VMware para ayudar a determinar los requisitos de rendimiento, incluida la frecuencia diaria de cambio de datos y los servidores de procesos que necesita.

### <a name="replication-appliance-capacity"></a>Capacidad del dispositivo de replicación

Los valores de esta tabla se pueden usar para averiguar si necesita un servidor de procesos adicional en la implementación.

- Si la tasa de cambio diaria (tasa de renovación) es superior a 2 TB, implemente un servidor de procesos adicional.
- Si va a replicar más de 200 máquinas, implemente un dispositivo de replicación adicional.

**CPU** | **Memoria** | **Espacio libre para el almacenamiento de datos en caché** | **Tasa de renovación** | **Límites de replicación**
--- | --- | --- | --- | ---
8 vCPU (2 sockets * 4 núcleos \@ 2,5 GHz) | 16 GB | < 300 GB | 500 GB o menos | < 100 máquinas 
12 vCPU (2 sockets * 6 núcleos \@ 2,5 GHz) | 18 GB | 600 GB | De 501 GB a 1 TB | 100-150 máquinas.
16 vCPU (2 sockets * 8 núcleos \@ 2,5 GHz) | 32 G1 |  1 TB | 1 TB a 2 TB | 151-200 máquinas.

### <a name="scale-out-process-server-sizing"></a>Tamaño del servidor de procesos de escalado horizontal

Si necesita implementar un servidor de procesos de escalado horizontal, esta tabla puede ayudarle a averiguar el tamaño del servidor.

**Servidor de proceso** | **Espacio libre para el almacenamiento de datos en caché** | **Tasa de renovación** | **Límites de replicación**
--- | --- | --- | --- 
4 vCPU (2 sockets * 2 núcleos \@ 2,5 GHz), 8 GB de memoria | < 300 GB | 250 GB o menos | Hasta 85 máquinas 
8 vCPU (2 sockets * 4 núcleos \@ 2,5 GHz), 12 GB de memoria | 600 GB | De 251 GB a 1 TB    | 86-150 máquinas.
12 vCPU (2 sockets * 6 núcleos \@ 2,5 GHz), 24 GB de memoria | 1 TB | 1 - 2 TB | 151-225 máquinas.

## <a name="control-upload-throughput"></a>Control del rendimiento de carga


 el tráfico de VMware que se replica en Azure pasa por un servidor de procesos específico. Puede limitar el rendimiento de la carga limitando el ancho de banda en las máquinas que se ejecutan como servidores de procesos. Puede influir en el ancho de banda con esta clave del Registro:

- El valor del Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM especifica el número de subprocesos que se utilizan para la transferencia de datos (replicación inicial o diferencial) de un disco. Un valor mayor aumenta el ancho de banda de red utilizado para la replicación. El valor predeterminado es cuatro. El valor máximo es 32. Supervise el tráfico para optimizar el valor.
- Además, puede limitar el ancho de banda en la máquina del servidor de procesos de la siguiente manera:

    1. En la máquina del servidor de procesos, abra el complemento MMC de Azure Backup. Hay un acceso directo en el escritorio o en la carpeta C:\Archivos de Programa\Microsoft Azure Recovery Services Agent\bin. 
    2. En el complemento, seleccione **Cambiar propiedades**.
    3. En **Limitación**, seleccione **Habilitar límite de uso del ancho de banda de Internet para las operaciones de copia de seguridad**. Establezca los límites para las horas laborables y no laborables. Los intervalos válidos van de 512 Kbps a 1023 Mbps.


## <a name="next-steps"></a>Pasos siguientes

Pruebe la [migración de VM de VMware](tutorial-migrate-vmware-agent.md) basada en agente con Azure Migrate Server Migration.
