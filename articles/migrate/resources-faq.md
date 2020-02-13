---
title: Preguntas habituales sobre Azure Migrate
description: Obtenga respuestas a preguntas comunes sobre el servicio Azure Migrate.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: snehaa
ms.openlocfilehash: baf01c0a0d5c6154305f7137c24deb0365b5e812
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77062105"
---
# <a name="azure-migrate-common-questions"></a>Azure Migrate: Preguntas frecuentes

En este artículo se responden a preguntas habituales sobre Azure Migrate. Si tiene más preguntas después de leer este artículo, publíquelas en el [Foro de Azure Migrate](https://aka.ms/AzureMigrateForum). Si tiene otras preguntas, repase estos artículos:

- [Preguntas](common-questions-appliance.md) sobre el dispositivo de Azure Migrate.
- [Preguntas](common-questions-discovery-assessment.md) sobre la detección, valoración y visualización de dependencias.


## <a name="what-is-azure-migrate"></a>¿Qué es Azure Migrate?

Azure Migrate proporciona un centro de conectividad para realizar el seguimiento de la detección, la valoración y la migración a Azure de aplicaciones y cargas de trabajo locales, así como de máquinas virtuales en la nube privadas o públicas. El centro proporciona herramientas de Azure Migrate para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV). [Más información](migrate-services-overview.md).


## <a name="what-can-i-do-with-azure-migrate"></a>¿Qué puedo hacer con Azure Migrate?

Use Azure Migrate para descubrir, valorar y migrar la infraestructura, las aplicaciones y los datos locales a Azure. Azure Migrate admite la valoración y migración de máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V, servidores físicos, otras máquinas virtuales virtualizadas, bases de datos, aplicaciones web y escritorios virtuales. 

## <a name="whats-the-difference-between-azure-migrate-and-site-recovery"></a>¿Cuál es la diferencia entre Azure Migrate y Site Recovery?

Azure Migrate ofrece un centro principal para la valoración y la migración a Azure. [Azure Site Recovery](../site-recovery/site-recovery-overview.md) es una herramienta de recuperación ante desastres. Azure Migrate: La herramienta Server Migration usa cierta funcionalidad de back-end de Site Recovery para la migración mediante lift-and-shift de algunas máquinas locales.

## <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-map-toolkit"></a>¿Cuál es la diferencia entre usar Azure Migrate Server Assessment y MAP Toolkit?

Server Assessment proporciona una valoración para ayudar con la preparación de la migración y la evaluación de las cargas de trabajo para la migración a Azure. [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) ayuda con otras tareas, como el planeamiento de la migración para versiones más recientes de los sistemas operativos cliente y servidor de Windows y el seguimiento del uso de software. Para esos escenarios, siga usando MAP Toolkit.

## <a name="whats-the-difference-between-server-assessment-and-the-site-recovery-deployment-planner"></a>¿Cuál es la diferencia entre Server Assessment y Site Recovery Deployment Planner?

Server Assessment es una herramienta de planeamiento de la migración. Site Recovery Deployment Planner es una herramienta de planeamiento de recuperación ante desastres.

- **Planeamiento de la migración del entorno local a Azure**: Si planea migrar los servidores locales a Azure, use Server Assessment para planear la migración. Esta solución evalúa las cargas de trabajo locales y proporciona orientación y herramientas que le ayudarán a migrar. Una vez que esté implementado el plan de migración, puede usar herramientas como Azure Migrate Server Migration para migrar las máquinas a Azure.
- **Planeamiento de recuperación ante desastres en Azure**: Si planea configurar la recuperación ante desastres de una ubicación local a Azure con Site Recovery, use Site Recovery Deployment Planner. Deployment Planner realiza una evaluación profunda y específica de Site Recovery de su entorno local con el fin de la recuperación ante desastres. Proporciona recomendaciones sobre la recuperación ante desastres, como la replicación y la conmutación por error.

## <a name="how-does-server-migration-work-with-site-recovery"></a>¿Cómo funciona Server Migration con Site Recovery?

- Si usa Azure Migrate: Server Migration para realizar una migración sin agente de máquinas virtuales de VMware locales y, a continuación, la migración es nativa de Azure Migrate y no se utiliza Site Recovery.
- Si usa Azure Migrate: Server Migration para realizar una migración basada en agentes de máquinas virtuales de VMware o migrar máquinas virtuales de Hyper-V o servidores físicos y, a continuación, Azure Migrate Server Migration utiliza el motor de replicación de Azure Site Recovery.


## <a name="which-geographies-are-supported"></a>¿Qué zonas geográficas se admiten?

Revise las geografías compatibles de Azure Migrate para [VM de VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware) y para [VM de Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v).

## <a name="how-do-i-get-started"></a>¿Cómo empiezo?

Identifique la herramienta que necesita y agréguela a un proyecto de Azure Migrate. Si va a agregar una herramienta de ISV o a Movere:
- Comience mediante la obtención de una licencia o regístrese para obtener una evaluación gratuita de acuerdo con la directiva de la herramienta. La licencia para las herramientas se conceden de conformidad con el modelo de licencias de la herramienta o de ISV.
- En cada herramienta, existe una opción para conectarse a Azure Migrate. Siga las instrucciones y la documentación de la herramienta para conectar la herramienta con Azure Migrate.
Realice un seguimiento centralizado del recorrido de la migración desde el proyecto de Azure Migrate, en Azure y en otras herramientas.

## <a name="how-do-i-delete-a-project"></a>¿Cómo puedo eliminar un proyecto?

[Aprenda a](how-to-delete-project.md) eliminar un proyecto. 




## <a name="next-steps"></a>Pasos siguientes
Consulte la sección [Información general de Azure Migrate](migrate-services-overview.md).
