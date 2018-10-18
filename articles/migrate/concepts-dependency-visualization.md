---
title: Visualización de dependencia en Azure Migrate | Microsoft Docs
description: En este artículo se proporciona una introducción a los cálculos de evaluación del servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: raynew
ms.openlocfilehash: 923a2a137bb4510e9490ce4077f744a43619a2c6
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165031"
---
# <a name="dependency-visualization"></a>Visualización de dependencia

El servicio [Azure Migrate](migrate-overview.md) evalúa los grupos en las máquinas locales para su migración a Azure. Puede usar la característica de visualización de dependencias en Azure Migrate para crear grupos. En este artículo se proporciona información sobre esta característica.


## <a name="overview"></a>Información general

Con la visualización de dependencias en Azure Migrate puede crear grupos para evaluar la migración de grupos con mayor confianza. Usar la visualización de dependencias permite ver las dependencias de red de máquinas e identificar máquinas relacionadas que es necesario migrar a Azure. Esta función es útil en situaciones donde no sabe con certeza qué máquinas constituyen la aplicación y deben migrarse juntas a Azure.

## <a name="how-does-it-work"></a>¿Cómo funciona?

Azure Migrate utiliza la solución [Service Map](../operations-management-suite/operations-management-suite-service-map.md) en [Log Analytics](../log-analytics/log-analytics-overview.md) para la visualización de dependencias.
- Si quiere aprovechar la visualización de dependencias, necesita asociar un área de trabajo de Log Analytics, actual o nueva, con un proyecto de Azure Migrate.
- Solo se puede crear o vincular un área de trabajo en la misma suscripción donde se crea el proyecto de migración.
- Para asociar un área de trabajo de Log Analytics a un proyecto, vaya a la sección **Essentials** de la página **Introducción** del proyecto y haga clic en **Requiere configuración**.

    ![Asociar un área de trabajo de Log Analytics](./media/concepts-dependency-visualization/associate-workspace.png)

- Cuando se crea una nueva área de trabajo, hay que especificar un nombre para el área de trabajo. Después, se crea el área de trabajo en una región en la misma [ubicación geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) que el proyecto de migración.
- El área de trabajo asociada se etiqueta con la clave **Proyecto de migración** y el valor **Nombre del proyecto**, que se puede usar para buscar en Azure Portal.
- Para navegar al área de trabajo asociada al proyecto, puede ir a la sección **Essentials** de la página **Introducción** del proyecto y acceder al área de trabajo.

    ![Navegar al área de trabajo de Log Analytics](./media/concepts-dependency-visualization/oms-workspace.png)

Para usar la visualización de dependencias, debe descargar e instalar agentes en cada máquina local que vaya a analizar.  

## <a name="do-i-need-to-pay-for-it"></a>¿Tengo qué pagar por ello?

Azure Migrate está disponible sin costo adicional. Para usar la característica de visualización de dependencias en Azure Migrate se necesita Service Map y asociar un área de trabajo de Log Analytics, ya sea nueva o existente, con el proyecto de Azure Migrate. La característica de visualización de dependencias en Azure Migrate es gratuita durante los primeros 180 días de Azure Migrate.

1. Si usa cualquier solución distinta de Service Map en esta área de trabajo de Log Analytics, se cobrarán los [cargos habituales de Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).
2. Para admitir escenarios de migración sin costo adicional, la solución Service Map no conllevará ningún gasto durante los primeros 180 días desde la asociación del área de trabajo de Log Analytics con el proyecto de Azure Migrate. Transcurridos los 180 días, se aplicarán las tarifas normales de Log Analytics.

Al registrar agentes en el área de trabajo, utilice el identificador y la clave proporcionados por el proyecto en la página de pasos de instalación del agente.

Al eliminar el proyecto de Azure Migrate, el área de trabajo no se elimina con él. Tras la eliminación del proyecto, el uso de Service Map dejará de ser gratuito y cada nodo se facturará conforme al nivel de pago del área de trabajo de Log Analytics.

> [!NOTE]
> La característica de visualización de dependencias usa Service Map mediante un área de trabajo de Log Analytics. Desde el 28 de febrero de 2018, con el anuncio de la disponibilidad general de Azure Migrate, la característica está ahora disponible sin costo adicional. Deberá crear un nuevo proyecto para poder usar el área de trabajo de uso gratuito. Las áreas de trabajo existentes antes de la disponibilidad general siguen siendo facturables, por lo que se recomienda que cambie a un nuevo proyecto.

[Aquí](https://azure.microsoft.com/pricing/details/azure-migrate/) puede encontrar más información sobre los precios de Azure Migrate.

## <a name="how-do-i-manage-the-workspace"></a>¿Cómo administro el área de trabajo?

Puede usar el área de trabajo de Log Analytics fuera de Azure Migrate. No se elimina si se elimina el proyecto de migración en el que se creó. Si ya no necesita el área de trabajo, [elimínelo](../log-analytics/log-analytics-manage-access.md) manualmente.

No elimine el área de trabajo creado por Azure Migrate, a menos que elimine el proyecto de migración. Si lo hace, la característica de visualización de dependencias no funcionará según lo previsto.

## <a name="next-steps"></a>Pasos siguientes
- [Agrupación de máquinas con dependencias de máquina](how-to-create-group-machine-dependencies.md)
- [Más información](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) sobre las preguntas más frecuentes sobre visualización de dependencias.
