---
title: Visualización de dependencias en Azure Migrate
description: En este artículo se brinda una introducción a los cálculos de evaluación del servicio Server Assessment en Azure Migrate.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 65a99e230262ae05d34dc8c04e87252c15133fda
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425687"
---
# <a name="dependency-visualization"></a>Visualización de dependencia

En este artículo se describe la característica de visualización de dependencias en Azure Migrate: Server Assessment.

La visualización de dependencias le permite comprender las dependencias en las máquinas que quiere evaluar y migrar. La asignación de dependencias suele usarse cuando se quiere evaluar máquinas con niveles de confianza más altos.

- En Azure Migrate: Server Assessment, puede reunir las máquinas en grupos para evaluarlas. Normalmente, los grupos se componen de las máquinas que desea migrar y la visualización de dependencias lo ayuda a comprobar las dependencias de las máquinas para que pueda agrupar estas con precisión.
- Con la visualización, puede detectar los sistemas interdependientes que necesitan migrarse juntos. Puede identificar si los sistemas que se están ejecutando todavía están en uso o si los sistemas se pueden retirar en lugar de migrarse.
- La visualización de dependencias le ayuda a garantizar que no se olvida de nada y a evitar interrupciones inesperadas durante la migración.
- Esta característica es especialmente útil si no conoce por completo las máquinas que forman parte de las aplicaciones y, por tanto, se deben migrar a Azure.


> [!NOTE]
> La funcionalidad de visualización de dependencias no está disponible en Azure Government.

## <a name="agent-based-and-agentless"></a>Basada en agente y sin agente

Hay dos opciones para implementar la visualización de dependencias:

- **Visualización de dependencias sin agentes**: Esta opción está actualmente en versión preliminar y solo está disponible para máquinas virtuales de VMware. No es necesario que instale ningún agente en las máquinas. 
    - Funciona capturando los datos de conexión TCP de las máquinas para las que está habilitada. [Más información](how-to-create-group-machine-dependencies-agentless.md).
Una vez iniciada la detección de dependencias, el dispositivo recopila datos de las máquinas en un intervalo de sondeo de cinco minutos.
    - Se pueden recopilar los siguientes datos:
        - Conexiones TCP
        - Nombres de los procesos que tienen conexiones activas
        - Nombres de las aplicaciones instaladas que ejecutan los procesos anteriores
        - No. de conexiones detectadas en cada intervalo de sondeo
- **Visualización de dependencias basada en agente**: para usar la visualización de dependencias basada en agente, debe descargar e instalar los siguientes agentes en cada máquina local que vaya a analizar.  
    - Es necesario tener instalado [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) en cada máquina. [Obtenga más información](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies#install-the-mma) sobre cómo instalar el agente MMA.
    - Es necesario tener instalado [Dependency Agent](../azure-monitor/platform/agents-overview.md#dependency-agent) en cada máquina. [Obtenga más información](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies#install-the-dependency-agent) sobre cómo instalar el agente de dependencia.
    - Además, si tiene máquinas sin conectividad a Internet, debe descargar e instalar en ellas la puerta de enlace de Log Analytics.

## <a name="agent-based-requirements"></a>Requisitos basados en agente

### <a name="what-do-i-need-to-deploy-dependency-visualization"></a>¿Qué necesito para implementar la visualización de dependencias?

Antes de implementar la visualización de dependencias, debe tener un proyecto de Azure Migrate en su lugar, con la herramienta Azure Migrate: Server Assessment agregada al proyecto. La visualización de dependencias se implementa después de configurar un dispositivo Azure Migrate para detectar las máquinas locales.

[Obtenga más información](how-to-assess.md) sobre cómo agregar la herramienta e implementar un dispositivo para [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md) o servidores físicos.


### <a name="how-does-it-work"></a>¿Cómo funciona?

Azure Migrate utiliza la solución [Service Map](../operations-management-suite/operations-management-suite-service-map.md) de los [registros de Azure Monitor](../log-analytics/log-analytics-overview.md) para la visualización de dependencias.

- Si quiere aprovechar la visualización de dependencias, necesita asociar un área de trabajo de Log Analytics (nueva o existente) con un proyecto de Azure Migrate.
- El área de trabajo debe estar en la misma suscripción que en la que se crea el proyecto de Azure Migrate.
- Azure Migrate admite áreas de trabajo que residen en las regiones Este de EE. UU., Sudeste de Asia y Oeste de Europa. Las áreas de trabajo de otras regiones no se pueden asociar a un proyecto. Además, tenga en cuenta que el área de trabajo debe estar en una región en la que [se admita Service Map](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites).
- El área de trabajo de un proyecto de Azure Migrate no se puede modificar una vez que se ha agregado.
- En Log Analytics, el área de trabajo asociada con Azure Migrate se etiqueta con la clave del proyecto de migración y el nombre del proyecto.

    ![Navegar al área de trabajo de Log Analytics](./media/concepts-dependency-visualization/oms-workspace.png)



### <a name="do-i-need-to-pay-for-it"></a>¿Tengo qué pagar por ello?

La visualización de dependencias requiere Service Map y un área de trabajo de Log Analytics asociado. 

- La solución de Service Map no incurre en ningún cargo durante los primeros 180 días. El plazo comienza a partir del día en el que asoció el área de trabajo de Log Analytics con el proyecto de Azure Migrate.
- Transcurridos los 180 días, se aplicarán las tarifas normales de Log Analytics.
- Si usa cualquier solución distinta de Service Map en esta área de trabajo de Log Analytics asociada, se cobrarán los cargos [habituales de Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).
- Al eliminar el proyecto de Azure Migrate, el área de trabajo no se elimina con él. Tras la eliminación del proyecto, el uso de Service Map no será gratuito y cada nodo se facturará conforme al nivel de pago del área de trabajo de Log Analytics.

[Aquí](https://azure.microsoft.com/pricing/details/azure-migrate/) puede encontrar más información sobre los precios de Azure Migrate.

> [!NOTE]
> Si tiene proyectos creados antes de que Azure Migrate estuviera disponible con carácter general el 28 de febrero de 2018, puede que haya incurrido en cargos de Service Map adicionales. Para asegurarse de que solo paga después de 180 días, se recomienda crear un nuevo proyecto, ya que las áreas de trabajo existentes antes de la disponibilidad general siguen siendo facturables.



### <a name="how-do-i-manage-the-workspace"></a>¿Cómo administro el área de trabajo?

- Al registrar agentes en el área de trabajo, se usa el identificador y la clave proporcionados por el proyecto de Azure Migrate.
- Puede usar el área de trabajo de Log Analytics fuera de Azure Migrate.
- Si elimina el proyecto de Azure Migrate asociado, el área de trabajo no se elimina automáticamente. Debe [eliminarla manualmente](../azure-monitor/platform/manage-access.md).
- No elimine el área de trabajo creado por Azure Migrate, a menos que elimine el proyecto de Azure Migrate. Si lo hace, la característica de visualización de dependencias no funcionará según lo previsto.

## <a name="next-steps"></a>Pasos siguientes
- [Agrupación de máquinas con dependencias de máquina](how-to-create-group-machine-dependencies.md)
- [Más información](common-questions-discovery-assessment.md#what-is-dependency-visualization) sobre las preguntas más frecuentes sobre visualización de dependencias.


