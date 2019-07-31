---
title: Matriz de compatibilidad para Azure Migrate
description: Proporciona un resumen de limitaciones y configuraciones de compatibilidad para el servicio Azure Migrate.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: raynew
ms.openlocfilehash: b2ca1b9118ecc3d112a49bb4c79b413c46fe67cb
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810147"
---
# <a name="azure-migrate-support-matrix"></a>Matriz de compatibilidad para Azure Migrate

Puede usar el [servicio Azure Migrate](migrate-overview.md) para evaluar y migrar máquinas a la nube de Microsoft Azure. En este artículo se resumen las configuraciones y limitaciones de compatibilidad generales para los escenarios e implementaciones de Azure Migrate.


## <a name="azure-migrate-versions"></a>Versiones de Azure Migrate

Existen dos versiones del servicio Azure Migrate:

- **Versión actual**: use esta versión para crear nuevos proyectos de Azure Migrate, detectar evaluaciones locales y organizar valoraciones y migraciones. [Más información](whats-new.md#azure-migrate-new-version).
- **Versión anterior**: para clientes que usaban la versión anterior de Azure Migrate (solo se admitía la evaluación de VM de VMware locales), ahora debería usar la versión actual. En la versión anterior, no puede crear nuevos proyectos de Azure Migrate ni realizar nuevas detecciones.

## <a name="supported-migration-scenarios"></a>Escenarios de migración admitidos

En la tabla se resumen los escenarios de migración admitidos.

**Implementación** | **Detalles*** 
--- | --- 
**Evaluación local** | Evalúe las cargas de trabajo y los datos locales que se ejecutan en máquinas virtuales de VMware y máquinas virtuales de Hyper-V. Realice la evaluación mediante la herramienta de evaluación de servidores de Azure Migrate y Microsoft Data Migration Assistant (DMA), así como con herramientas de terceros que incluyen Cloudamize, Corent Tech y Turbonomic Server.
**Migración del entorno local a Azure** | Migre las cargas de trabajo y los datos que se ejecutan en servidores físicos, máquinas virtuales de VMware, máquinas virtuales de Hyper-V y en instancias de AWS/GCP a Azure. Migre mediante la herramienta de evaluación de servidores de Azure Migrate y Azure Database Migration Service (DMS), así como con herramientas de terceros que incluyen Carbonite y CorentTech.

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
Subscription | Puede tener un único proyecto de Azure Migrate en una suscripción.
Permisos de Azure | Necesita permisos de Colaborador o Propietario en la suscripción para crear un proyecto de Azure Migrate.
Máquinas virtuales de VMware  | Evalúe hasta 35 000 máquinas virtuales de VMware en un único proyecto.
Máquinas virtuales de Hyper-V | Evalúe hasta 10 000 máquinas virtuales de Hyper-V en un único proyecto.

Un proyecto puede incluir máquinas virtuales de VMware y máquinas virtuales de Hyper-V, hasta los límites de evaluación.


## <a name="vmware-assessment-and-migration"></a>Evaluación y migración de VMware

[Revise](migrate-support-matrix-vmware.md) la matriz de compatibilidad de la evaluación de servidores y migración de servidores de Azure Migrate para máquinas virtuales de VMware.

## <a name="hyper-v-assessment-and-migration"></a>Evaluación y migración de Hyper-V

[Revise](migrate-support-matrix-hyper-v.md) la matriz de compatibilidad de la evaluación de servidores y migración de servidores de Azure Migrate para máquinas virtuales de Hyper-V.


## <a name="next-steps"></a>Pasos siguientes

- [Evaluación de las máquinas virtuales de VMware](tutorial-assess-vmware.md) para la migración.
- [Evaluación de las máquinas virtuales de Hyper-V](tutorial-assess-hyper-v.md) para la migración.

