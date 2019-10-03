---
title: Visualización de dependencia en Azure Migrate | Microsoft Docs
description: En este artículo se brinda una introducción a los cálculos de evaluación del servicio Server Assessment en Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: hamusa
ms.openlocfilehash: 33594e09778b9a629645e12357e6bafe561ad35e
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71202902"
---
# <a name="dependency-visualization"></a>Visualización de dependencia

Azure Migrate: Server Assessment evalúa los grupos de las máquinas locales para su migración a Azure. Puede usar la funcionalidad de visualización de dependencias en Server Assessment para crear grupos. En este artículo se proporciona información sobre esta característica.

> [!NOTE]
> La funcionalidad de visualización de dependencias no está disponible en Azure Government.

## <a name="overview"></a>Información general

Con la visualización de dependencias en Server Assessment puede crear grupos para evaluar la migración de grupos con mayor confianza. Usar la visualización de dependencias permite ver las dependencias de red de máquinas e identificar máquinas relacionadas que es necesario migrar a Azure. Esta función es útil en situaciones donde no sabe con certeza qué máquinas constituyen la aplicación y deben migrarse juntas a Azure.

## <a name="before-you-start"></a>Antes de comenzar

- Asegúrese de que ha [creado](how-to-add-tool-first-time.md) un proyecto de Azure Migrate.
- Si ya ha creado un proyecto, asegúrese de que ha [agregado](how-to-assess.md) la herramienta Azure Migrate: Server Assessment.
- Compruebe que ha detectado sus máquinas en Azure Migrate; puede hacerlo, configure un dispositivo de Azure Migrate para [VMware](how-to-set-up-appliance-vmware.md) o [Hyper-V](how-to-set-up-appliance-hyper-v.md). El dispositivo detecta máquinas locales y envía metadatos y datos de rendimiento a Azure Migrate: Server Assessment. [Más información](migrate-appliance.md).

## <a name="how-does-it-work"></a>¿Cómo funciona?

Azure Migrate utiliza la solución [Service Map](../operations-management-suite/operations-management-suite-service-map.md) de los [registros de Azure Monitor](../log-analytics/log-analytics-overview.md) para la visualización de dependencias.
- Si quiere aprovechar la visualización de dependencias, necesita asociar un área de trabajo de Log Analytics, actual o nueva, con un proyecto de Azure Migrate.
- Solo se puede crear o vincular un área de trabajo en la misma suscripción donde se crea el proyecto de Azure Migrate.
- Para adjuntar un área de trabajo de Log Analytics a un proyecto:
    1. En la pestaña **Servidores**, en el icono **Azure Migrate: Server Assessment**, haga clic en **Información general**.
    2. En **Información general**, haga clic en la flecha abajo para expandir **Información esencial**.
    3. En **Área de trabajo de OMS**, haga clic en **Requiere configuración**.
    4. En **Configurar área de trabajo**, especifique si quiere crear un área de trabajo nueva o utilizar una existente:
    
    ![Incorporación de un área de trabajo](./media/how-to-create-group-machine-dependencies/workspace.png)

- Al asociar un área de trabajo, obtendrá la opción de crear una o de conectar una existente:
  - Cuando se crea una nueva área de trabajo, hay que especificar un nombre para el área de trabajo. Puede elegir la [región](https://azure.microsoft.com/global-infrastructure/regions/) en que se creará el área de trabajo.
  - Al asociar un área de trabajo existente, puede elegir entre las disponibles en la misma suscripción del proyecto de migración. Tenga en cuenta que solo se enumeran las áreas de trabajo que se crearon en una región donde [se admita Service Map](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites). Para poder asociar un área de trabajo, asegúrese de que tiene acceso de lectura a ella.

  > [!NOTE]
  > Una vez asociada el área de trabajo a un proyecto, no podrá cambiarla más adelante.

  > [!NOTE]
  > Actualmente, Azure Migrate admite la creación de un área de trabajo de OMS en las regiones Este de EE. UU., Sudeste Asiático y Oeste de Europa. Si el área de trabajo se crea fuera de Azure Migrate en cualquier otra región, actualmente no se puede asociar a un proyecto de Azure Migrate. 

- El área de trabajo asociada se etiqueta con la clave **Proyecto de migración** y el valor **Nombre del proyecto**, que se puede usar para buscar en Azure Portal.
- Para navegar al área de trabajo asociada al proyecto, puede ir a la sección **Essentials** de la página **Introducción** del proyecto y acceder al área de trabajo.

    ![Navegar al área de trabajo de Log Analytics](./media/concepts-dependency-visualization/oms-workspace.png)

Para usar la visualización de dependencias, debe descargar e instalar agentes en cada máquina local que vaya a analizar.  

- Es necesario tener instalado [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) en cada máquina. [Obtenga más información](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies#install-the-mma) sobre cómo instalar el agente MMA.
- Es necesario tener instalado [Dependency Agent](../azure-monitor/platform/agents-overview.md#dependency-agent) en cada máquina. [Obtenga más información](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies#install-the-dependency-agent) sobre cómo instalar el agente de dependencia.
- Además, si tiene máquinas sin conectividad a Internet, debe descargar e instalar en ellas la puerta de enlace de Log Analytics.

No necesita estos agentes en las máquinas que quiera evaluar a menos que esté usando la visualización de dependencias.

## <a name="do-i-need-to-pay-for-it"></a>¿Tengo qué pagar por ello?

La característica de visualización de dependencias está disponible sin cargo adicional. Para usar la característica de visualización de dependencias en Server Assessment se necesita Service Map y asociar un área de trabajo de Log Analytics, ya sea nueva o existente, con el proyecto de Azure Migrate. La funcionalidad de visualización de dependencias en Server Assessment es gratis durante los primeros 180 días.

1. Si usa cualquier solución distinta de Service Map en esta área de trabajo de Log Analytics, se cobrarán los [cargos habituales de Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).
2. Para admitir escenarios de migración sin costo adicional, la solución Service Map no conllevará ningún gasto durante los primeros 180 días desde la asociación del área de trabajo de Log Analytics con el proyecto de Azure Migrate. Transcurridos los 180 días, se aplicarán las tarifas normales de Log Analytics.

Al registrar agentes en el área de trabajo, utilice el identificador y la clave proporcionados por el proyecto en la página de pasos de instalación del agente.

Al eliminar el proyecto de Azure Migrate, el área de trabajo no se elimina con él. Tras la eliminación del proyecto, el uso de Service Map dejará de ser gratuito y cada nodo se facturará conforme al nivel de pago del área de trabajo de Log Analytics.

> [!NOTE]
> La característica de visualización de dependencias usa Service Map mediante un área de trabajo de Log Analytics. Desde el 28 de febrero de 2018, con el anuncio de la disponibilidad general de Azure Migrate, la característica está ahora disponible sin costo adicional. Deberá crear un nuevo proyecto para poder usar el área de trabajo de uso gratuito. Las áreas de trabajo existentes antes de la disponibilidad general siguen siendo facturables, por lo que se recomienda que cambie a un nuevo proyecto.

[Aquí](https://azure.microsoft.com/pricing/details/azure-migrate/) puede encontrar más información sobre los precios de Azure Migrate.

## <a name="how-do-i-manage-the-workspace"></a>¿Cómo administro el área de trabajo?

Puede usar el área de trabajo de Log Analytics fuera de Azure Migrate. No se elimina si se elimina el proyecto de Azure Migrate en el que se creó. Si ya no necesita el área de trabajo, [elimínelo](../azure-monitor/platform/manage-access.md) manualmente.

No elimine el área de trabajo creado por Azure Migrate, a menos que elimine el proyecto de Azure Migrate. Si lo hace, la característica de visualización de dependencias no funcionará según lo previsto.

## <a name="next-steps"></a>Pasos siguientes
- [Agrupación de máquinas con dependencias de máquina](how-to-create-group-machine-dependencies.md)
- [Más información](https://docs.microsoft.com/azure/migrate/resources-faq#what-is-dependency-visualization) sobre las preguntas más frecuentes sobre visualización de dependencias.
