---
title: Arquitectura del dispositivo de replicación de Azure Migrate | Microsoft Docs
description: Proporciona información general sobre el dispositivo de replicación de Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: 4f4dc307bee4190a0e94ace493053e0cfd01150e
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810111"
---
# <a name="replication-appliance"></a>Dispositivo de replicación

En este artículo se describe el dispositivo de replicación que usa Azure Migrate: Server Assessment al migrar máquinas virtuales de VMware, máquinas físicas y máquinas virtuales de nube privada o pública a Azure mediante una migración basada en agente. 

La herramienta está disponible en el centro de [Azure Migrate](migrate-overview.md). El centro proporciona herramientas nativas para la evaluación y la migración, así como herramientas de otros servicios de Azure y de proveedores de software independientes (ISV) de terceros.


## <a name="appliance-overview"></a>Información general del dispositivo

El dispositivo de replicación se implementa como una máquina local individual, ya sea como una máquina virtual de VMware o como un servidor físico. Ejecuta lo siguiente:
- **Dispositivo de replicación**: el dispositivo de replicación coordina las comunicaciones y administra la replicación de datos para las máquinas virtuales de VMware locales y los servidores físicos que se replican en Azure.
- **Servidor de proceso**: el servidor de proceso, que se instala de forma predeterminada en el dispositivo de replicación, hace lo siguiente:
    - **Puerta de enlace de replicación**: actúa como puerta de enlace de replicación. Recibe datos de replicación de las máquinas habilitadas para la replicación. Optimiza los datos de replicación con el almacenamiento en caché, la compresión y el cifrado, y los envía a Azure.
    - **Instalador de agente**: realiza una instalación de inserción de Mobility Service. Este servicio debe instalarse y ejecutarse en cada máquina local que quiera replicar para la migración.

## <a name="appliance-deployment"></a>Implementación del dispositivo

**Implementación como** | **Se usa para** | **Detalles**
--- | --- |  ---
Máquina virtual de VMware | Normalmente se usa al migrar máquinas virtuales de VMware mediante la herramienta de migración de Azure Migrate con la migración basada en agente. | Descargue la plantilla OVA del centro de Azure Migrate e impórtela en vCenter Server para crear la máquina virtual del dispositivo.
Máquina física | Se usa al migrar servidores físicos locales si no se tiene una infraestructura de VMware o si no se puede crear una máquina virtual de VMware mediante una plantilla OVA. | Descargue un instalador de software del centro de Azure Migrate y ejecútelo para configurar el equipo de la aplicación.

## <a name="appliance-deployment-requirements"></a>Requisitos de implementación del dispositivo

[Revise](migrate-support-matrix-vmware.md#agent-based-migration-replication-appliance-requirements) los requisitos de implementación.



## <a name="appliance-license"></a>Licencia del dispositivo
El dispositivo viene con una licencia de evaluación de Windows Server 2016 que es válida durante 180 días. Si el período de evaluación está a punto de expirar, se recomienda descargar e implementar un nuevo dispositivo, o bien activar la licencia del sistema operativo de la máquina virtual del dispositivo.

## <a name="replication-process"></a>Proceso de replicación

1. Al habilitar la replicación para una VM, comienza la replicación inicial en el almacenamiento de Azure mediante la directiva de replicación especificada. 
2. El tráfico se replica en los puntos de conexión públicos de Azure Storage a través de Internet. No se admite la replicación del tráfico entre un sitio local y Azure a través de una red privada virtual (VPN) de sitio a sitio.
3. Cuando finaliza esta replicación inicial, comienza la replicación diferencial. Las marcas de revisión de una máquina se registran.
4. Comunicación se realiza como se indica a continuación:
    - Las máquinas virtuales se comunican con el dispositivo de replicación en el puerto HTTPS 443 entrante para la administración de la replicación.
    - El dispositivo de replicación organiza la replicación con Azure a través del puerto HTTPS 443 saliente.
    - Las máquinas virtuales envían los datos de replicación al servidor de procesos (que se ejecuta en el dispositivo de replicación) en el puerto HTTPS 9443 entrante. Este puerto se puede modificar.
    - El servidor de procesos recibe datos de replicación, los optimiza y los cifra para enviarlos después a Azure Storage a través del puerto 443 de salida.
5. Los datos de replicación registran el primer aterrizaje en una cuenta de almacenamiento en caché de Azure. Estos registros se procesan y los datos se almacenan en un disco administrado de Azure.

![Arquitectura](./media/migrate-replication-appliance/architecture.png)

## <a name="appliance-upgrades"></a>Actualizaciones del dispositivo

El dispositivo se actualiza manualmente desde el centro de Azure Migrate. Se recomienda que ejecute siempre la versión más reciente.

1. En Azure Migrate > Servidores > Azure Migrate: Server Assessment, Servidores de infraestructura, haga clic en **Servidores de configuración**.
2. En **Servidores de configuración**, aparece un vínculo en **Versión del agente** cuando hay disponible una nueva versión del dispositivo de replicación. 
3. Descargue el instalador en el equipo del dispositivo de replicación e instale la actualización. El instalador detecta la versión que se ejecuta actualmente en el dispositivo.
 
## <a name="next-steps"></a>Pasos siguientes

[Obtenga información](tutorial-assess-vmware.md#set-up-the-appliance-vm) sobre cómo configurar el dispositivo para VMware.
[Obtenga información](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) sobre cómo configurar el dispositivo para Hyper-V.

