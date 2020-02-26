---
title: Preguntas sobre el dispositivo de Azure Migrate
description: Respuestas a preguntas comunes sobre el servicio Azure Migrate
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 3bb066b08447a951665e629da5ebcb75714b9f1e
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425362"
---
# <a name="azure-migrate-appliance-common-questions"></a>Dispositivo de Azure Migrate: Preguntas frecuentes

Este artículo contiene respuestas a algunas preguntas frecuentes sobre el dispositivo de Azure Migrate. Si tiene otras preguntas, repase estos artículos:

- [Preguntas generales](resources-faq.md) sobre Azure Migrate.
- [Preguntas](common-questions-discovery-assessment.md) sobre la detección, valoración y visualización de dependencias.
- [Preguntas](common-questions-server-migration.md) sobre la migración de servidores.
- Obtenga respuestas a sus preguntas en el [foro de Azure Migrate](https://aka.ms/AzureMigrateForum). 


## <a name="what-is-the-azure-migrate-appliance"></a>¿Qué es el dispositivo de Azure Migrate?

El dispositivo de Azure Migrate es un dispositivo ligero que usa la herramienta Azure Migrate: Server Assessment para descubrir y valorar servidores locales. El dispositivo también lo usa la herramienta Azure Migrate: Server Migration para la migración sin agente de VM de VMware locales. 

- El dispositivo se implementa de forma local como una máquina virtual o física.
- El dispositivo detecta máquinas locales y envía continuamente metadatos y datos de rendimiento de las máquinas a Azure Migrate.
- La detección del dispositivo es sin agente. No se instala nada en las máquinas detectadas.

[Obtenga más información](migrate-appliance.md) sobre el dispositivo.

## <a name="how-does-the-appliance-connect-to-azure"></a>¿Cómo se conecta el dispositivo a Azure?

El dispositivo puede conectarse a través de Internet, o mediante Azure ExpressRoute con emparejamiento público o de Microsoft.

## <a name="does-appliance-analysis-impact-performance"></a>¿Afecta el análisis del dispositivo al rendimiento?

El dispositivo de Azure Migrate genera perfiles de máquinas locales continuamente para medir los datos de rendimiento de la VM. Esta generación de perfiles apenas tiene impacto en el rendimiento de las máquinas.

### <a name="can-i-harden-the-appliance-vm"></a>¿Se puede proteger la máquina virtual del dispositivo?

Al crear la VM del dispositivo mediante la plantilla descargada, puede agregar componentes (por ejemplo, un antivirus) a la plantilla, siempre que las reglas de firewall y de comunicación necesarias para el dispositivo de Azure Migrate no se modifiquen.


## <a name="what-network-connectivity-is-needed"></a>¿Qué conectividad de red se necesita?

Revise la información acerca de la conectividad de red:
- Valoración de VMware con el dispositivo de Azure Migrate: Requisitos de acceso de [URL](migrate-appliance.md#url-access) y [puertos](migrate-support-matrix-vmware.md#port-access).
- Migración sin agentes de VMware con el dispositivo de Azure Migrate: Requisitos de acceso de [URL](migrate-appliance.md#url-access) y [puertos](migrate-support-matrix-vmware-migration.md#agentless-ports).
- Valoración de Hyper-V con el dispositivo de Azure Migrate: Requisitos de acceso de [URL](migrate-appliance.md#url-access) y [puertos](migrate-support-matrix-hyper-v.md#port-access).


## <a name="what-data-does-the-appliance-collect"></a>¿Qué datos recopila el dispositivo?

Revise los datos recopilados:

- [Datos de rendimiento](migrate-appliance.md#collected-performance-data-vmware) y [metadatos](migrate-appliance.md#collected-metadata-vmware) de máquinas virtuales de VMware.
- [Datos de rendimiento](migrate-appliance.md#collected-performance-data-hyper-v) y [metadatos](migrate-appliance.md#collected-metadata-hyper-v) de máquinas virtuales de Hyper-V.


## <a name="how-is-data-stored"></a>¿Cómo se almacenan los datos?

Los datos que recopila el dispositivo de Azure Migrate se almacenarán en la ubicación de Azure en la que creó el proyecto de Azure Migrate. 

- Los datos se almacenan de forma segura en una suscripción de Microsoft y se eliminan al eliminar el proyecto de Azure Migrate.
- Para la [visualización de dependencias](concepts-dependency-visualization.md), los datos recopilados se almacenan en los Estados Unidos, en un área de trabajo de Log Analytics creada en la suscripción de Azure. Estos datos se eliminan al eliminar el área de trabajo de Log Analytics en su suscripción.

## <a name="how-much-data-is-uploaded-in-continuous-profiling"></a>¿Cuántos datos se cargan en la generación de perfiles continua?

El volumen de datos enviados a Azure Migrate depende de una serie de parámetros. Para darle una idea, un proyecto de Azure Migrate con 10 máquinas (cada una con un disco y una NIC) envía aproximadamente 50 MB al día. El valor es aproximado, y cambia en función del número de puntos de datos de las NIC y los discos. El aumento de los datos enviados no es lineal si hay un aumento del número de máquinas, tarjetas NIC o discos.

## <a name="is-data-encrypted-at-restin-transit"></a>¿Los datos están cifrados en reposo o en tránsito?

Sí, en ambos.

- Los metadatos se envían de manera segura al servicio Azure Migrate por Internet a través de HTTPS.
- Los metadatos se almacenan en una base de datos de [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) y en [Azure Blob Storage](../storage/common/storage-service-encryption.md), en una suscripción de Microsoft. Los metadatos se cifran en reposo.
- Los datos para el análisis de dependencia también se cifran en tránsito (HTTPS seguro). Se almacenan en un área de trabajo de Log Analytics de la suscripción. También se cifran en reposo.

## <a name="how-does-the-appliance-connect-to-vcenter-server"></a>¿Cómo se conecta el dispositivo a vCenter Server?

1. El dispositivo se conecta a vCenter Server (puerto 443) mediante las credenciales que proporcionó al configurar el dispositivo.
2. El dispositivo usa VMware PowerCLI para realizar consultas en vCenter Server, para recopilar metadatos sobre las máquinas virtuales que administra vCenter Server.
3. El dispositivo recopila los datos de configuración sobre máquinas virtuales (núcleos, memoria, discos, NIC) y el historial de rendimiento de cada máquina virtual del último mes.
4. Los metadatos recopilados se envían a Azure Migrate: Server Assessment (mediante Internet a través de HTTPS) para la evaluación.

## <a name="can-i-connect-the-appliance-to-multiple-vcenter-servers"></a>¿Puedo conectar el mismo dispositivo en varios servidores de vCenter?

No. Hay una asignación unívoca entre un dispositivo y vCenter Server. Para detectar máquinas virtuales en varias instancias de vCenter Server, debe implementar varios dispositivos.

## <a name="how-many-vms-or-servers-can-i-discover-with-an-appliance"></a>¿Cuántas máquinas virtuales o servidores puedo detectar con un dispositivo?

Puede detectar hasta 10 000 VM de VMware, 5000 de Hyper-V y 250 servidores físicos con un único dispositivo. Si tiene más máquinas en el entorno local, obtenga información sobre el escalado de [Hyper-V](scale-hyper-v-assessment.md), [VMware](scale-vmware-assessment.md) y la valoración [física](scale-physical-assessment.md).

## <a name="can-i-delete-an-appliance"></a>¿Puedo eliminar un dispositivo?

Actualmente no se admite la eliminación de un dispositivo del proyecto.

- La única manera de eliminar el dispositivo es eliminar el grupo de recursos que contiene el proyecto de Azure Migrate asociado con el dispositivo.
- Si bien, al eliminar el grupo de recursos también se eliminarán otras aplicaciones registradas, el inventario detectado, las valoraciones y los demás componentes de Azure asociados al proyecto en el grupo de recursos.


## <a name="can-i-use-the-appliance-with-a-different-subscriptionproject"></a>¿Puedo usar el dispositivo con una suscripción o un proyecto distintos?

Después de usar el dispositivo para iniciar la detección, no puede volver a configurarlo con otra suscripción de Azure o usarlo en otro proyecto de Azure Migrate. Tampoco se pueden detectar máquinas virtuales en otro vCenter Server. Configure un dispositivo nuevo para estas tareas.

## <a name="can-i-set-up-the-appliance-on-an-azure-vm"></a>¿Se puede configurar el dispositivo en una máquina virtual de Azure?
No. No se admite actualmente. 

## <a name="can-i-discover-on-an-esxi-host"></a>¿Se puede detectar en un host ESXi?
No. Necesita vCenter Server para detectar VM de VMware.

## <a name="how-do-i-update-the-appliance"></a>¿Cómo se actualiza el dispositivo?

De forma predeterminada, el dispositivo y sus agentes instalados se actualizan automáticamente. El dispositivo busca actualizaciones una vez cada 24 horas. Si se produce un error en el proceso de actualización, se produce un nuevo intento. Las actualizaciones automáticas solo actualizan el dispositivo y los agentes del dispositivo. El sistema operativo no se actualiza. Use las actualizaciones de Microsoft Update para mantener actualizado el sistema operativo.

## <a name="can-i-check-agent-health"></a>¿Puedo comprobar el estado del agente?

En el portal, vaya a la página **Estado del agente** de la herramienta Server Assessment o Server Migration. Ahí puede comprobar el estado de la conexión de los agentes de detección y valoración del dispositivo con Azure.

## <a name="next-steps"></a>Pasos siguientes
Consulte la sección [Información general de Azure Migrate](migrate-services-overview.md).
