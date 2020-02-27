---
title: 'Preguntas comunes: detección, valoración y análisis de dependencias en Azure Migrate'
description: Obtenga respuestas a preguntas comunes sobre detección, valoración y análisis de dependencias en Azure Migrate.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 179be4f5b39806c717e94789c9fe52842d2a07d8
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589239"
---
# <a name="common-questions-about-discovery-assessment-and-dependency-analysis"></a>Preguntas comunes sobre detección, valoración y análisis de dependencias

En este artículo se responde a preguntas comunes sobre detección, valoración y análisis de dependencias en Azure Migrate. Si tiene otras preguntas, repase estos artículos:

- [Preguntas generales](resources-faq.md) sobre Azure Migrate.
- [Preguntas](common-questions-appliance.md) sobre el dispositivo de Azure Migrate.
- [Preguntas](common-questions-server-migration.md) sobre la migración de servidores.
- Publique preguntas en el [foro de Azure Migrate](https://aka.ms/AzureMigrateForum).



## <a name="how-many-vms-can-i-discover-with-an-appliance"></a>¿Cuántas máquinas virtuales puedo detectar con un dispositivo?

Puede detectar hasta 10 000 máquinas virtuales de VMware, 5000 de Hyper-V y 250 servidores con un único dispositivo. Si tiene más máquinas en el entorno local, obtenga información sobre el escalado de [Hyper-V](scale-hyper-v-assessment.md), [VMware](scale-vmware-assessment.md) y la valoración [física](scale-physical-assessment.md).



## <a name="vm-size-changed-can-i-run-an-assessment-again"></a>El tamaño de la máquina virtual ha cambiado. ¿Puedo volver a ejecutar una valoración?

El dispositivo de Azure Migrate recopila continuamente información sobre el entorno local. Pero una evaluación es una instantánea de un momento dado de las máquinas virtuales locales. Si cambia la configuración de una máquina virtual que quiere evaluar, use la opción de recalcular para actualizar la evaluación con los últimos cambios.

### <a name="how-do-i-discover-vms-in-a-multitenant-environment"></a>¿Cómo puedo detectar máquinas virtuales en un entorno de varios inquilinos?

- En el caso de VMware, si el entorno se comparte entre los inquilinos y no quiere detectar las máquinas virtuales de un inquilino en la suscripción de otro inquilino, cree credenciales de vCenter Server con acceso solo a las máquinas virtuales que quiere detectar. Después, use esas credenciales al iniciar la detección en el dispositivo de Azure Migrate.
- En el caso de Hyper-V, la detección usa credenciales de host de Hyper-V. Si las máquinas virtuales comparten el mismo host de Hyper-V, no hay actualmente ninguna manera de separar la detección.  


### <a name="do-i-need-vcenter-server-for-vmware-vm-discovery"></a>¿Necesito vCenter Server para la detección de máquinas virtuales de VMWare?

Sí, Azure Migrate necesita vCenter Server para detectar un entorno de VMware. No admite la detección de hosts de ESXi que no administra vCenter Server.


## <a name="whats-the-difference-sizing-options"></a>¿Cuáles son las diferentes opciones de tamaño?

Con el ajuste como tamaño local, Azure Migrate no tiene en cuenta los datos de rendimiento de la máquina virtual para la evaluación. Evalúa los tamaños de las máquinas virtuales en función de la configuración local. Con el ajuste de tamaño basado en el rendimiento, el ajuste de tamaño se basa en los datos de uso.

- Por ejemplo, si hay una máquina virtual local con 4 núcleos y 8 GB de memoria con un 50 % de uso de CPU y un 50 % de uso de memoria, ocurrirá lo siguiente:
    - El ajuste de tamaño como local recomienda una SKU de máquina virtual de Azure con 4 núcleos y 8 GB de memoria.
    - Pero el ajuste de tamaño basado en el rendimiento recomendará una SKU de máquina virtual de 2 núcleos y 4 GB de memoria, ya que se tiene en cuenta el porcentaje de uso.

- De forma similar, el ajuste de tamaño de disco depende de dos propiedades de evaluación: criterio de ajuste de tamaño y tipo de almacenamiento.
    - Si el criterio de ajuste de tamaño se basa en el rendimiento y el tipo de almacenamiento es automático, Azure Migrate tiene en cuenta los valores de rendimiento y de IOPS del disco al identificar el tipo de disco de destino (estándar o premium).
    - Si el criterio de ajuste de tamaño está basado en el rendimiento y el tipo de almacenamiento es premium, Azure Migrate recomienda una SKU de disco premium, basada en el tamaño del disco local. Se aplica la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es como local y el tipo de almacenamiento es estándar o premium.

## <a name="does-performance-historyutilization-impact-sizing"></a>¿Afectan el historial de rendimiento o el uso al ajuste de tamaño?

Estas propiedades solo son aplicables para el ajuste de tamaño basado en el rendimiento.

- Azure Migrate recopila el historial de rendimiento de las máquinas locales y lo usa para recomendar el tipo de disco y el tamaño de la máquina virtual en Azure.
- El dispositivo realiza un perfil del entorno local continuamente para recopilar datos de uso en tiempo real cada 20 segundos.
- El dispositivo acumula ejemplos de 20 segundos y crea un único punto de datos cada 15 minutos.
- Para crear el punto de datos, el dispositivo selecciona el valor máximo de todos los ejemplos de 20 segundos.
- Después, el dispositivo envía este punto de datos a Azure.

Cuando se crea una evaluación en Azure, en función de la duración del rendimiento y el valor del percentil del historial de rendimiento, Azure Migrate calcula el valor del uso efectivo y lo usa para ajustar el tamaño.

- Por ejemplo, si establece la duración del rendimiento en 1 día y el valor del percentil en el percentil 95, Azure Migrate ordena los puntos de ejemplo de 15 minutos que envía el recopilador para el último día de manera ascendente y elige el valor del percentil 95 como el uso efectivo.
- El uso del valor de percentil 95 garantiza que se están omitiendo todos los valores atípicos. Se podrían incluir valores atípicos si se usara el percentil 99. Si quiere elegir el uso máximo para el período y no quiere perderse ningún valor atípico, seleccione el percentil 99.

## <a name="what-is-dependency-visualization"></a>¿Qué es la visualización de dependencias?

Use la visualización de dependencias para evaluar grupos de máquinas virtuales para la migración con mayor confianza. La visualización de dependencias comprueba las dependencias de las máquinas antes de llevar a cabo una valoración. Este proceso ayuda a asegurar que nada se queda atrás y evita interrupciones inesperadas al migrar a Azure. Azure Migrate usa la solución Service Map en Azure Monitor para habilitar la visualización de dependencias. [Más información](concepts-dependency-visualization.md).

> [!NOTE]
> La visualización de dependencias no está disponible en Azure Government.

## <a name="do-i-pay-for-dependency-visualization"></a>¿Tengo que pagar por la visualización de dependencias?
No. [Más información](https://azure.microsoft.com/pricing/details/azure-migrate/) sobre los precios de Azure Migrate.

## <a name="what-do-i-install-for-dependency-visualization"></a>¿Qué tengo que instalar para la visualización de dependencias?

Para usar la visualización de dependencias, debe descargar e instalar agentes en cada máquina local que vaya a evaluar.

Debe instalar los siguientes agentes en cada máquina:
- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).
- [Dependency Agent](../azure-monitor/platform/agents-overview.md#dependency-agent).
- Si tiene máquinas sin conectividad a Internet, debe descargar e instalar en ellas la puerta de enlace de Log Analytics.

No necesita estos agentes a menos que esté usando la visualización de dependencias.

## <a name="can-i-use-an-existing-workspace"></a>¿Puedo usar un área de trabajo existente?

Sí, puede asociar un área de trabajo existente al proyecto de migración y usarla para la visualización de dependencias. 

## <a name="can-i-export-the-dependency-visualization-report"></a>¿Puedo exportar el informe de visualización de dependencias?

No, el informe de visualización de dependencias no se puede exportar. Sin embargo, Azure Migrate usa Service Map, y puede usar la [API REST de Service Map](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) para obtener las dependencias en formato JSON.

## <a name="can-i-automate--mmadependency-agent-installation"></a>¿Puedo automatizar la instalación de MMA o el agente de dependencias?

Use este script[ para instalar los agentes de dependencia ](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples). Siga estas [instrucciones para instalar MMA](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) mediante la línea de comandos o la automatización. Para MMA, use [este script](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Además de los scripts, también puede usar herramientas de desarrollo como Microsoft Endpoint Configuration Manager e [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) para implementar los agentes.


## <a name="what-operating-systems-does-mma-support"></a>¿Qué sistemas operativos admite MMA?

- Vea la lista de los [sistemas operativos Windows compatibles con MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
- Vea la lista de los [sistemas operativos Linux compatibles con MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

## <a name="can-i-visualize-dependencies-for-more-than-an-hour"></a>¿Puedo visualizar las dependencias durante más de una hora?
No. Puede visualizar las dependencias durante un máximo de una hora. Puede volver a una fecha determinada del historial, un mes como máximo, pero la duración máxima de la visualización es de una hora. Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora. Pero puede usar los registros de Azure Monitor para [consultar los datos de dependencia](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) durante un período más largo.

## <a name="can-visualize-dependencies-for-groups-of-more-than-10-vms"></a>¿Puedo visualizar dependencias para grupos de más de 10 máquinas virtuales?
Puede [visualizar las dependencias](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) de grupos que tengan un máximo de diez máquinas virtuales. Si tiene un grupo con más de diez máquinas virtuales, se recomienda dividirlo en grupos más pequeños y luego visualizar las dependencias.




## <a name="next-steps"></a>Pasos siguientes
Consulte la sección [Información general de Azure Migrate](migrate-services-overview.md).
