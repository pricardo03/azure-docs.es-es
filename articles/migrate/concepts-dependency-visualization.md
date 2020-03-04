---
title: Visualización de dependencias en Azure Migrate
description: En este artículo se brinda una introducción a los cálculos de evaluación del servicio Server Assessment en Azure Migrate.
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: f24656d02e19f422ff26e6b06d1631a9128dff43
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77587114"
---
# <a name="dependency-visualization"></a>Visualización de dependencia

En este artículo se describe la visualización de dependencias en Azure Migrate: Server Assessment.

## <a name="what-is-dependency-visualization"></a>¿Qué es la visualización de dependencias?

La visualización de dependencias le ayuda a identificar las dependencias entre las máquinas locales que quiere evaluar y migrar a Azure. 

- En Azure Migrate: Server Assessment, puede recopilar las máquinas en un grupo y, después, evaluar el grupo. La visualización de dependencias le ayuda a agrupar las máquinas con más precisión y con una confianza alta para la evaluación.
- La visualización de dependencias le permite identificar las máquinas que se deben migrar juntas. Puede identificar si las máquinas están en uso o si se pueden retirar en lugar de migrarlas.
- La visualización de dependencias le ayuda a garantizar que no se olvida de nada y a evitar interrupciones inesperadas durante la migración.
- La visualización es especialmente útil si no está seguro de si las máquinas forman parte de la implementación de una aplicación que desea migrar a Azure.


> [!NOTE]
> La visualización de dependencias no está disponible en Azure Government.

## <a name="agent-basedagentless-visualization"></a>Visualización basada en agentes y visualización sin agentes

Hay dos opciones para implementar la visualización de dependencias:

- **Basada en agentes**: La visualización de dependencias basada en agente requiere que se instalen agentes en las máquinas locales que quiere analizar.
- **Sin agentes**: Con esta opción, no es necesario instalar agentes en las máquinas que desea comprobar. Esta opción se encuentra actualmente en versión preliminar y solo está disponible para máquinas virtuales de VMware.


## <a name="agent-based-visualization"></a>Visualización basada en agentes

**Requisito** | **Detalles** | **Más información**
--- | --- | ---
**Antes de la implementación** | Debe tener un proyecto de Azure Migrate activo, con la herramienta Azure Migrate: Server Assessment agregada al proyecto.<br/><br/>  La visualización de dependencias se implementa después de configurar un dispositivo Azure Migrate para detectar las máquinas locales. | [Aprenda](create-manage-projects.md) a crear un proyecto por primera vez.<br/><br/> [Aprenda](how-to-assess.md) a agregar una herramienta de evaluación a un proyecto existente.<br/><br/> Aprenda a configurar un dispositivo de Azure Migrate para la evaluación de [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md) o de servidores físicos.
**Agentes necesarios** | En cada máquina que desee analizar, instale los siguientes agentes:<br/><br/> [Microsoft Monitoring agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).<br/><br/> [Dependency Agent](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Si las máquinas locales no están conectadas a Internet, tiene que descargar e instalar la puerta de enlace de Log Analytics en ellas. | Conozca más información sobre la instalación de [Dependency Agent](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) y [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**Log Analytics** | Azure Migrate utiliza la solución [Service Map](../operations-management-suite/operations-management-suite-service-map.md) de los [registros de Azure Monitor](../log-analytics/log-analytics-overview.md) para la visualización de dependencias.<br/><br/> Puede asociar un área de trabajo de Log Analytics nueva o existente con un proyecto de Azure Migrate. El área de trabajo de un proyecto de Azure Migrate no se puede modificar una vez que se ha agregado. <br/><br/> El área de trabajo debe estar en la misma suscripción que el proyecto de Azure Migrate.<br/><br/> El área de trabajo debe residir en las regiones Este de EE. UU., Sudeste Asiático u Oeste de Europa. Las áreas de trabajo de otras regiones no se pueden asociar a un proyecto.<br/><br/> El área de trabajo debe estar en una región en la que [se admita Service Map](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites).<br/><br/> En Log Analytics, el área de trabajo asociada con Azure Migrate se etiqueta con la clave del proyecto de migración y el nombre del proyecto.
**Costos** | La solución Service Map no supone ningún gasto durante los primeros 180 días (a partir del día que asocie el área de trabajo de Log Analytics con el proyecto de Azure Migrate).<br/><br/> Transcurridos los 180 días, se aplicarán las tarifas normales de Log Analytics.<br/><br/> Si se usa una solución que no sea Service Map en el área de trabajo de Log Analytics asociada generará los [gastos estándar](https://azure.microsoft.com/pricing/details/log-analytics/) de Log Analytics.<br/><br/> Al eliminar el proyecto de Azure Migrate, el área de trabajo no se elimina con él. Tras la eliminación del proyecto, el uso de Service Map no será gratuito y cada nodo se facturará conforme al nivel de pago del área de trabajo de Log Analytics.<br/><br/>Si tiene proyectos creados antes de que Azure Migrate estuviera disponible con carácter general (el 28 de febrero de 2018), puede que haya incurrido en cargos de Service Map adicionales. Para garantizar el pago solo después de 180 días, es recomendable que cree un nuevo proyecto, ya que las áreas de trabajo existentes antes de la disponibilidad general siguen siendo facturables.
**Administración** | Al registrar agentes en el área de trabajo, se usa el identificador y la clave proporcionados por el proyecto de Azure Migrate.<br/><br/> Puede usar el área de trabajo de Log Analytics fuera de Azure Migrate.<br/><br/> Si elimina el proyecto de Azure Migrate asociado, el área de trabajo no se elimina automáticamente. [Elimínela manualmente](../azure-monitor/platform/manage-access.md).<br/><br/> No elimine el área de trabajo creado por Azure Migrate, a menos que elimine el proyecto de Azure Migrate. Si lo hace, la característica de visualización de dependencias no funcionará según lo previsto.

## <a name="agentless-visualization"></a>Visualización sin agentes


**Requisito** | **Detalles** | **Más información**
--- | --- | ---
**Antes de la implementación** | Debe tener un proyecto de Azure Migrate activo, con la herramienta Azure Migrate: Server Assessment agregada al proyecto.<br/><br/>  La visualización de dependencias se implementa después de configurar un dispositivo de Azure Migrate para detectar las máquinas de VMWare locales. | [Aprenda](create-manage-projects.md) a crear un proyecto por primera vez.<br/><br/> [Aprenda](how-to-assess.md) a agregar una herramienta de evaluación a un proyecto existente.<br/><br/> Aprenda a configurar un dispositivo de Azure Migrate para la evaluación de máquinas virtuales de [VMware](how-to-set-up-appliance-vmware.md).
**Agentes necesarios** | No se requiere ningún agente en las máquinas que desea analizar.
**Sistemas operativos compatibles** | Repase los [sistemas operativos](migrate-support-matrix-vmware.md#agentless-dependency-visualization) que se admiten en la visualización sin agentes.
**VM** | **Herramientas de VMware**: Las herramientas de VMware se deben instalar y ejecutar en las máquinas virtuales que se quieren analizar.<br/><br/> **Cuenta**: En el dispositivo de Azure Migrate, debe agregar una cuenta de usuario que se pueda usar para acceder a las máquinas virtuales que desea analizar.<br/><br/> **Máquinas virtuales Windows**: La cuenta de usuario debe tener permisos de administrador local o de dominio sobre la máquina.<br/><br/> **Máquinas virtuales Linux**: se requiere el privilegio raíz en la cuenta. Como alternativa, la cuenta de usuario requiere estas dos funcionalidades en los archivos/bin/netstat y /bin/ls: CAP_DAC_READ_SEARCH y CAP_SYS_PTRACE. | [Más información](migrate-appliance.md) sobre el dispositivo de Azure Migrate.
**VMware** | **vCenter**: El dispositivo necesita una cuenta de vCenter Server con acceso de solo lectura, así como privilegios habilitados para Máquinas virtuales > Operaciones de invitado.<br/><br/> **Hosts de ESXi**: En los hosts ESXi que ejecuten las máquinas virtuales que desea analizar, Azure Migrate debería ser capaz de conectarse al puerto TCP 443.
**Datos recopilados** |  La visualización de dependencias sin agentes funciona capturando los datos de conexión TCP de las máquinas para las que está habilitada. Una vez iniciada la detección de dependencias, el dispositivo recopila estos datos de las máquinas mediante la realización de sondeos cada cinco minutos:<br/> - Conexiones TCP.<br/> - Nombres de los procesos que tienen conexiones activas.<br/> - Nombres de las aplicaciones instaladas que ejecutan los procesos con conexiones activas.<br/> - El número de conexiones detectadas en cada intervalo de sondeo.


## <a name="next-steps"></a>Pasos siguientes
- [Configuración de la visualización de dependencias](how-to-create-group-machine-dependencies.md)
- [Pruebe la visualización de dependencias sin agentes](how-to-create-group-machine-dependencies-agentless.md) para las máquinas virtuales de VMware.
- Revise las [preguntas habituales](common-questions-discovery-assessment.md#what-is-dependency-visualization) sobre la visualización de dependencias.


