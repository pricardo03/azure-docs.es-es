---
title: Arquitectura de Azure Migrate | Microsoft Docs
description: Proporciona información general acerca del servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: raynew
ms.openlocfilehash: 16fbf8d3523ac2ab36447aa51a93a0bb5a9fad54
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810107"
---
# <a name="azure-migrate-architecture-and-processes"></a>Arquitectura y procesos de Azure Migrate

[Azure Migrate](migrate-overview.md) proporciona un centro de herramientas que le ayuda a detectar las aplicaciones, la infraestructura y las cargas de trabajo, a evaluarlas y a migrarlas a Microsoft Azure. Este centro incluye herramientas de Azure Migrate y ofertas de fabricantes de software independientes (ISV) de terceros. 

 En este artículo se resumen la arquitectura y los procesos de evaluación y migración de Azure Migrate Server Assessment y Azure Migrate Server Migration.

## <a name="assessment-with-azure-migrate-server-assessment"></a>Evaluación con Azure Migrate Server Assessment

Azure Migrate Server Assessment puede evaluar VM de VMware y VM de Hyper-V para la migración a Azure. La evaluación funciona como sigue:

1. **Preparación para la evaluación**: En el entorno local, configure un dispositivo de Azure Migrate ligero local como VM de VMware o VM de Hyper-V, y registre el dispositivo en Azure Migrate.
2. **Detección de las VM**: Se ejecuta el dispositivo de Azure Migrate para detectar VM locales. 
    - No es necesario instalar nada en las VM detectadas.
    - Los metadatos de las VM incluyen información acerca de los núcleos, la memoria, los discos, los tamaños de disco y los adaptadores de red.
    - Los datos de rendimiento incluyen información sobre el uso de la CPU y de la memoria, el IOPS de disco, el rendimiento del disco (Mbps) y la salida de red (Mbps).
- **Recopilación y evaluación de las máquinas**: Una vez finalizada la detección, en Azure Portal, agrupará las VM detectadas, que normalmente se componen de VM que quiere migrar juntas. Ejecuta una evaluación en un grupo.


## <a name="vmware-assessment"></a>Evaluación en VMware 

En el diagrama se resume cómo funciona la evaluación de VM de VMware con Azure Migrate Server Assessment.

![Arquitectura de evaluación de VMware](./media/migrate-architecture/sas-architecture-vmware.png)

El proceso es el siguiente:

1. **Implementación del dispositivo de Azure Migrate**:

    a. Descargue la plantilla (OVA) de Azure Portal.

    b. Impórtela a la máquina de vCenter Server para crear la VM.

    c. Conéctese a esta VM, configure las opciones básicas y regístrela en Azure Migrate.

2. **Inicio de la detección**:

    a. Conéctese a la aplicación de recopilador que se ejecuta en el dispositivo para iniciar la detección.

    b. El dispositivo envía continuamente los metadatos y los datos de rendimiento de las VM a Azure.

    - El dispositivo está siempre conectado al servicio Azure Migrate.
    - Se capturan los cambios de entorno durante la detección continua. Por ejemplo, la adición de VM en el ámbito de detección o la adición de NIC o discos de VM.

3. **Evaluación de las máquinas**:

    a. Una vez finalizada la detección, en Azure Portal, agrupará las VM detectadas.  Normalmente, un grupo consta de las VM que quiere migrar juntas.

    b. Ejecute una evaluación para cada grupo.

4. **Revisión de la evaluación**: 

    a. Una vez finalizada la evaluación, véala en el portal.

    b. Para un análisis más exhaustivo, descargue la evaluación en formato de Excel.



### <a name="vmware-ports"></a>Puertos de VMware
Azure Migrate usa los siguientes puertos de conexión para VMware:

**Origen** | **Destino** | **Puerto** | **Detalles**
--- | --- | --- | ---
Dispositivo con Azure Migrate | Servicio Azure Migrate | Destino: TCP 443 | Envíe metadatos y datos de rendimiento a Azure Migrate.
Dispositivo con Azure Migrate | vCenter Server | Destino: TCP 443 | Conéctese a vCenter Server para metadatos y datos de rendimiento. 443 es el valor predeterminado, pero se puede modificar para que vCenter escuche en otro puerto. 
Cliente RDP | Dispositivo con Azure Migrate | Destino: TCP 3389 | Conexión RDP para desencadenar la detección desde el dispositivo.

### <a name="collected-vmware-metadata"></a>Metadatos de VMware recopilados

El dispositivo envía los metadatos y los datos relacionados con el rendimiento de las VM a Azure.

**Acción** | **Detalles**
--- | ---
**Metadatos recopilados** | Nombre de la VM de vCenter<br/> Ruta de acceso de la VM de vCenter (carpeta de host/clúster)<br/> Direcciones IP y MAC<br/> Sistema operativo<br/> Número de núcleos, discos, NIC<br/> Memoria y tamaño de disco.
**Datos de rendimiento recopilados** | Uso de CPU/memoria<br/> Datos por disco (rendimiento de lectura/escritura de disco; lecturas/escrituras de disco por segundo)<br/> Datos de NIC (entrada de red, salida de red).<br/><br/> Los datos de rendimiento se recopilan continuamente una vez que el dispositivo se conecta a vCenter Server. Los datos históricos no se recopilan.

## <a name="hyper-v-assessment"></a>Evaluación de Hyper-V

En el diagrama se resume cómo funciona la evaluación de VM de Hyper-V con Azure Migrate Server Assessment.

![Arquitectura de evaluación de Hyper-V](./media/migrate-architecture/sas-architecture-hyper-v.png)

El proceso es el siguiente:

1. **Creación del dispositivo de Azure Migrate**:

    a. Descargue la VM en formato comprimido desde Azure Portal.

    b. Impórtela en un host de Hyper-V.

    c. Conéctese a la VM del dispositivo. Configure las opciones básicas y regístrela en Azure Migrate.

2. **Inicio de la detección**:

    a. Conéctese al dispositivo Azure Migrate para iniciar la detección. No es necesario instalar nada en las VM detectadas.

    b. El dispositivo envía continuamente los metadatos y los datos de rendimiento a Azure Migrate.

    - El dispositivo está siempre conectado al servicio Azure Migrate (con sesiones de CIM).
    - Se capturan los cambios de entorno durante la detección continua. Por ejemplo, la adición de VM en el ámbito de detección o la adición de NIC o discos de VM.


3. **Evaluación de las máquinas**:

    a. Una vez finalizada la detección, en Azure Portal, agrupará las VM detectadas.  Normalmente, un grupo consta de las VM que quiere migrar juntas.

    b. Ejecute una evaluación para cada grupo.

4. **Revisión de la evaluación**:

    a. Una vez finalizada la evaluación, véala en el portal.

    b. Para un análisis más exhaustivo, descargue la evaluación en formato de Excel.

### <a name="hyper-v-ports"></a>Puertos de Hyper-V

El servicio Azure Migrate usa los siguientes puertos de conexión para Hyper-V:

**Origen** | **Destino** | **Puerto** | **Detalles**
--- | --- | --- | ---
Dispositivo con Azure Migrate | Servicio Azure Migrate | Destino: TCP 443 | Envíe metadatos y datos de rendimiento a Azure Migrate.
Dispositivo con Azure Migrate | Host o clúster de Hyper-V | Destino: puertos de WinRM predeterminados, HTTP: 5985; HTTPS: 5986 | Conéctese al host para metadatos y datos de rendimiento. Sesión de CIM usada para la conexión
Cliente RDP | Dispositivo con Azure Migrate | Destino: TCP 3389 | Conexión RDP para desencadenar la detección desde el dispositivo.

## <a name="collected-hyper-v-vm-metadata"></a>Metadatos recopilados de VM de Hyper-V

El dispositivo envía los metadatos y los datos relacionados con el rendimiento de las VM a Azure.


**Acción** | **Detalles**
--- | ---
**Metadatos recopilados** | Nombre de la máquina virtual<br/> Ruta de acceso de la VM (carpeta de host/clúster)<br/> Direcciones IP y MAC<br/> Sistema operativo<br/> Número de núcleos, discos, NIC<br/> Memoria y tamaño de disco<br/> IOPS de disco, rendimiento de disco (Mbps), salida de red (Mbps)
**Datos de rendimiento recopilados** |  Uso de CPU/memoria<br/> Datos por disco (rendimiento de lectura/escritura de disco; lecturas/escrituras de disco por segundo)<br/> Datos de NIC (entrada de red, salida de red).<br/><br/> Los datos de rendimiento se recopilan continuamente una vez que el dispositivo se conecta al host de Hyper-V. Los datos históricos no se recopilan.




## <a name="migration-with-azure-migrate-server-migration"></a>Migración con Azure Migrate Server Migration

Con Azure Migrate Server Migration, puede migrar lo siguiente a Azure:

- Máquinas virtuales de VMware locales
- VM de Hyper-V locales
- Servidores físicos locales
- Instancias de VM Windows de AWS
- Instancias de VM Windows de GCP

El proceso de migración difiere ligeramente, en función de lo que vaya a migrar.


## <a name="migrate-vmware-vms"></a>Migración de máquinas virtuales VMware

Azure Migrate Server Migration ofrece dos métodos para migrar VM de VMware locales:

- **Replicación sin agente**: Las VM se migran sin necesidad de instalar nada en ellas.
- **Replicación basada en agente**. Instale un agente en la VM para su replicación.

Si bien la replicación sin agente es más fácil desde una perspectiva de implementación, actualmente tiene una serie de limitaciones. [Aprenda más](server-migrate-overview.md) acerca de estas limitaciones.
 

### <a name="agent-based-migration"></a>Migración basada en agente

La migración basada en agente de VM de VMware requiere la implementación de una serie de componentes, como se resume en la tabla.

**Componente** | **Requisito** | **Detalles**
--- | --- | ---
**Equipo del servidor de configuración** | Una única VM de VMware local que se implementa a partir de una plantilla de OVF descargada.<br/><br/> La máquina ejecuta todos los componentes locales de Site Recovery, incluido el servidor de configuración y el servidor de procesos. | **Servidor de configuración**: coordina las comunicaciones entre el entorno local y Azure, además de administrar la replicación de datos.<br/><br/> **Servidor de proceso**: Se instala de forma predeterminada en el servidor de configuración. Recibe datos de replicación, los optimiza con almacenamiento en caché, compresión y cifrado, y los envía a Azure. El servidor de procesos también instala Azure Site Recovery Mobility Service en las VM y se encarga de la detección automática de las máquinas locales.
**Servicio de movilidad** | Mobility Service debe estar instalado en cada VM de VMware que quiera replicar. | Se recomienda permitir la instalación automática desde el servidor de procesos. Si lo desea, también puede instalar manualmente el servicio o usar un método de implementación automatizada, como System Center Configuration Manager.





### <a name="agent-based-replication-process"></a>Proceso de replicación basada en agente

![Proceso de replicación](./media/migrate-architecture/v2a-architecture-henry.png)

1. Cuando habilita la replicación para una VM, comienza la replicación inicial en Azure. 
    - La replicación es a nivel de bloque, casi continua, mediante el agente de Mobility Service que se ejecuta en la VM.
    - Se aplica la configuración de las directivas de replicación:
        - **Umbral de RPO**. Esta configuración no afecta a la replicación. Ayuda con la supervisión. Se genera un evento y, opcionalmente, se envía un correo electrónico, si el RPO actual supera el límite del umbral que especifique.
        - **Retención de punto de recuperación**. Esta configuración especifica cuánto tiempo atrás quiere ir cuando se produce una interrupción. La retención máxima en almacenamiento premium es de 24 horas. En el almacenamiento estándar es de 72 horas. 
        - **Instantáneas coherentes con la aplicación**. Las instantáneas coherentes con la aplicación pueden tomarse cada 1 a 12 horas, según las necesidades de su aplicación. Son instantáneas de blob de Azure estándar. El agente de movilidad que se ejecuta en una VM solicita una instantánea de VSS de acuerdo con esta configuración, y marca ese momento como punto coherente con la aplicación en el flujo de replicación.

2. El tráfico se replica en los puntos de conexión públicos de Azure Storage a través de Internet.

    - Como alternativa, puede usar Azure ExpressRoute con [emparejamiento de Microsoft](../expressroute/expressroute-circuit-peerings.md#microsoftpeering).
    - No se admite la replicación del tráfico entre un sitio local y Azure a través de una red privada virtual (VPN) de sitio a sitio.
3. Una vez terminada la replicación inicial, comienza la de los cambios incrementales en Azure. Los cambios marcados para una máquina se envían al servidor de procesos.
2. Comunicación se realiza como se indica a continuación:

    - Las máquinas virtuales se comunican con el servidor de configuración local en el puerto HTTPS 443 entrante para la administración de la replicación.
    - El servidor de configuración organiza la replicación con Azure a través del puerto HTTPS 443 saliente.
    - Las máquinas virtuales envían datos de replicación al servidor de procesos (que se ejecuta en la máquina del servidor de configuración) en el puerto HTTPS 9443 entrante. Este puerto se puede modificar.
    - El servidor de procesos recibe datos de replicación, los optimiza y los cifra para enviarlos después a Azure Storage a través del puerto 443 de salida.
5. Los datos de replicación registran el primer aterrizaje en una cuenta de almacenamiento en caché de Azure. Estos registros se procesan y los datos se almacenan en un disco administrado de Azure (el disco de inicialización de Azure Site Recovery). Los puntos de recuperación se crean en ese disco.







## <a name="next-steps"></a>Pasos siguientes

- [Repase las preguntas más frecuentes](resources-faq.md) sobre Azure Migrate.
- Para ayuda de la comunidad, visite el [foro de MSDN de Azure Migrate](https://social.msdn.microsoft.com/Forums/home?forum=AzureMigrate&filter=alltypes&sort=lastpostdesc) o [Stack Overflow](https://stackoverflow.com/search?q=azure+migrate).

