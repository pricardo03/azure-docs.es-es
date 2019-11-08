---
title: Matriz de compatibilidad para Azure Migrate
description: Proporciona un resumen de limitaciones y configuraciones de compatibilidad para el servicio Azure Migrate.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: raynew
ms.openlocfilehash: 18032250bc5c321d638ad46204738f49f1a0c744
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73480132"
---
# <a name="azure-migrate-support-matrix"></a>Matriz de compatibilidad para Azure Migrate

Puede usar el [servicio Azure Migrate](migrate-overview.md) para evaluar y migrar máquinas a la nube de Microsoft Azure. En este artículo se resumen las configuraciones y limitaciones de compatibilidad generales para los escenarios e implementaciones de Azure Migrate.


## <a name="azure-migrate-versions"></a>Versiones de Azure Migrate

Existen dos versiones del servicio Azure Migrate:

- **Versión actual**: use esta versión para crear nuevos proyectos de Azure Migrate, detectar evaluaciones locales y organizar valoraciones y migraciones. [Más información](whats-new.md#release-version-july-2019).
- **Versión anterior**: para clientes que usaban la versión anterior de Azure Migrate (solo se admitía la evaluación de VM de VMware locales), ahora debería usar la versión actual. En la versión anterior, no puede crear nuevos proyectos de Azure Migrate ni realizar nuevas detecciones.

## <a name="supported-assessmentmigration-scenarios"></a>Escenarios de migración o evaluación admitidos

En la tabla se resumen los escenarios de detección, evaluación y migración admitidos.

**Implementación** | **Detalles** 
--- | --- 
**Detección específica de la aplicación** | Puede detectar aplicaciones, roles y características que se ejecutan en máquinas virtuales de VMware. Actualmente, esta característica solo se limita a la detección. La evaluación está actualmente en el nivel de máquina. Todavía no ofrecemos una evaluación específica de aplicaciones, roles o características. 
**Evaluación local** | Evalúe las cargas de trabajo y los datos locales que se ejecutan en máquinas virtuales de VMware y máquinas virtuales de Hyper-V. Realice la evaluación mediante la herramienta de evaluación de servidores de Azure Migrate y Microsoft Data Migration Assistant (DMA), así como con herramientas de terceros que incluyen Cloudamize, Corent Tech y Turbonomic Server.
**Migración del entorno local a Azure** | Migre las cargas de trabajo y los datos que se ejecutan en servidores físicos, máquinas virtuales de VMware, máquinas virtuales de Hyper-V, servidores físicos y máquinas virtuales basadas en la nube a Azure. Migre mediante la herramienta de evaluación de servidores de Azure Migrate y Azure Database Migration Service (DMS), así como con herramientas de terceros que incluyen Carbonite y CorentTech.

La compatibilidad con la herramienta específica se resume de la siguiente manera.

**Herramienta** | **Evaluación o migración** | **Detalles**
--- | --- | ---
Evaluación de servidores de Azure Migrate | Evaluación | Pruebe la evaluación de servidores [para Hyper-V](tutorial-prepare-hyper-v.md) y [VMware](tutorial-prepare-vmware.md).
Cloudamize | Evaluación | [Más información](https://www.cloudamize.com/platform#tab-0).
CorentTech | Evaluación | [Más información](https://www.corenttech.com/).
Turbonomic | Evaluación | [Más información](https://turbonomic.com/solutions/technologies/azure-cloud/).
Migración de servidores de Azure Migrate | Migración | Pruebe la migración de servidores para [Hyper-V](tutorial-migrate-hyper-v.md) y [VMware](tutorial-migrate-vmware.md).
Carbonite | Migración | [Más información](https://www.carbonite.com/data-protection-resources/resource/Datasheet/carbonite-migrate-for-microsoft-azure).
CorentTech | Migración | [Más información](https://www.corenttech.com/).


## <a name="azure-migrate-projects"></a>Proyectos de Azure Migrate

**Soporte técnico** | **Detalles**
--- | ---
Subscription | Puede tener varios proyectos de Azure Migrate en una suscripción.
Permisos de Azure | Necesita permisos de Colaborador o Propietario en la suscripción para crear un proyecto de Azure Migrate.
Máquinas virtuales de VMware  | Evalúe hasta 35 000 máquinas virtuales de VMware en un único proyecto.
Máquinas virtuales de Hyper-V | Evalúe hasta 35 000 máquinas virtuales de Hyper-V en un único proyecto.

Un proyecto puede incluir máquinas virtuales de VMware y máquinas virtuales de Hyper-V, hasta los límites de evaluación.

## <a name="supported-geographies"></a>Ubicaciones geográficas admitidas

Puede crear un proyecto de Azure Migrate en varias zonas geográficas. Aunque solo puede crear proyectos en estas zonas geográficas, puede evaluar o migrar las máquinas para otras ubicaciones de destino. La geografía del proyecto solo se usa para almacenar los metadatos detectados.

**Geografía** | **Ubicación de almacenamiento de metadatos**
--- | ---
Azure Government | Gobierno de EE. UU. - Virginia
Asia Pacífico | Asia Oriental o Sudeste Asiático
Australia | Este de Australia o Sudeste de Australia
Brasil | Sur de Brasil
Canadá | Centro de Canadá o Este de Canadá
Europa | Norte de Europa y Oeste de Europa
Francia | Centro de Francia
India | India central o India meridional
Japón |  Este de Japón u Oeste de Japón
Corea | Centro de Corea del Sur o Sur de Corea del Sur
Reino Unido | Sur de Reino Unido u Oeste de Reino Unido
Estados Unidos | Centro de EE. UU. u Oeste de EE. UU. 2


 > [!NOTE]
 > La compatibilidad con Azure Government solo está disponible actualmente para la [versión anterior](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) de Azure Migrate.



## <a name="vmware-assessment-and-migration"></a>Evaluación y migración de VMware

[Revise](migrate-support-matrix-vmware.md) la matriz de compatibilidad de la evaluación de servidores y migración de servidores de Azure Migrate para máquinas virtuales de VMware.

## <a name="hyper-v-assessment-and-migration"></a>Evaluación y migración de Hyper-V

[Revise](migrate-support-matrix-hyper-v.md) la matriz de compatibilidad de la evaluación de servidores y migración de servidores de Azure Migrate para máquinas virtuales de Hyper-V.


## <a name="next-steps"></a>Pasos siguientes

- [Evaluación de las máquinas virtuales de VMware](tutorial-assess-vmware.md) para la migración.
- [Evaluación de las máquinas virtuales de Hyper-V](tutorial-assess-hyper-v.md) para la migración.

