---
title: Compatibilidad para la evaluación de servidores físicos con Azure Migrate
description: Obtenga información sobre la compatibilidad para la evaluación de servidores físicos con Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 489f95bbbbeb261b56f1a3a86da44f5fcce0adf5
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846577"
---
# <a name="support-matrix-for-physical-server-assessment"></a>Matriz de compatibilidad para la evaluación de servidores físicos 

Puede usar el [servicio Azure Migrate](migrate-overview.md) para evaluar y migrar máquinas a la nube de Microsoft Azure. En este artículo se resumen los valores de compatibilidad y las limitaciones para evaluar y migrar servidores físicos locales.


## <a name="overview"></a>Información general

A fin de evaluar las máquinas locales para la migración a Azure con este artículo, debe agregar la herramienta Azure Migrate: Server Assessment a un proyecto de Azure Migrate. Tendrá que implementar el [dispositivo de Azure Migrate](migrate-appliance.md). El dispositivo detecta los equipos locales de forma continuada y envía metadatos y datos de configuración y rendimiento a Azure. Después de la detección, recopile en grupos los equipos detectados y ejecute una valoración del grupo.

## <a name="limitations"></a>Limitaciones

**Soporte técnico** | **Detalles**
--- | ---
**Límites de evaluación**| Descubra y evalúe hasta 35 000 servidores físicos en un solo [proyecto](migrate-support-matrix.md#azure-migrate-projects).
**Límites del proyecto** | Puede crear varios proyectos en una suscripción a Azure. Un proyecto puede incluir servidores físicos y VM de VMware y de Hyper-V hasta los límites de evaluación.
**Detección** | El dispositivo de Azure Migrate puede detectar hasta 250 servidores físicos.
**Valoración** | Puede agregar hasta 35 000 máquinas en un solo grupo.<br/><br/> Puede evaluar hasta 35 000 máquinas en una sola evaluación.

[Más información](concepts-assessment-calculation.md) sobre las evaluaciones.




## <a name="physical-server-requirements"></a>Requisitos del servidor físico

| **Soporte técnico**                | **Detalles**               
| :-------------------       | :------------------- |
| **Implementación de servidor físico**       | El servidor físico puede ser independiente o implementarse en un clúster. |
| **Permisos**           | **Windows:** Configure una cuenta de usuario local en todos los servidores de Windows que quiera incluir en la detección. Es necesario agregar la cuenta de usuario a estos grupos: Usuarios de escritorio remoto, Usuarios de Monitor de rendimiento y Usuarios del registro de rendimiento. <br/> **Linux:** Necesita una cuenta raíz en los servidores Linux que desee detectar. |
| **Sistema operativo** | Se admiten todos los sistemas operativos [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) y [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) excepto los siguientes:<br/> Windows Server 2003 <br/> SUSE Linux|


## <a name="azure-migrate-appliance-requirements"></a>Requisitos del dispositivo de Azure Migrate

Azure Migrate usa el [dispositivo de Azure Migrate](migrate-appliance.md) para la detección y la evaluación. El dispositivo para los servidores físicos se puede ejecutar en una máquina virtual o en una máquina física. Se configura mediante un script de PowerShell que se descarga desde Azure Portal.

- Obtenga información sobre los [requisitos del dispositivo](migrate-appliance.md#appliance---physical) para los servidores físicos.
- Obtenga información sobre las [direcciones URL](migrate-appliance.md#url-access) a las que tiene que acceder el dispositivo.

## <a name="port-access"></a>Acceso a puertos

En la tabla siguiente se resumen los requisitos de los puertos para la evaluación.

**Dispositivo** | **Connection**
--- | ---
**Dispositivo** | Conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del Escritorio remoto al dispositivo.<br/> Conexiones entrantes en el puerto 44368 para tener acceso de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: ``` https://<appliance-ip-or-name>:44368 ```<br/> Conexiones salientes en los puertos 443 (HTTPS), 5671 y 5672 (AMQP) para enviar los metadatos de detección y rendimiento a Azure Migrate.
**Servidores físicos** | **Windows:** Conexiones entrantes en los puertos WinRM 5985 (HTTP) y 5986 (HTTPS) para extraer los metadatos de configuración y rendimiento de los servidores de Windows. <br/> **Linux:**  Conexiones entrantes en el puerto 22 (UDP) para extraer los metadatos de configuración y rendimiento de los servidores Linux. |

## <a name="agent-based-dependency-visualization"></a>Visualización de dependencias basada en agente

La [visualización de dependencias](concepts-dependency-visualization.md) le ayuda a observar las dependencias que hay entre las máquinas que desea evaluar y migrar. En el caso de la visualización basada en agente, los requisitos y las limitaciones se resumen en la siguiente tabla.


**Requisito** | **Detalles**
--- | ---
**Implementación** | Antes de implementar la visualización de dependencias, debe tener un proyecto de Azure Migrate en su lugar, con la herramienta Azure Migrate: Server Assessment agregada al proyecto. La visualización de dependencias se implementa después de configurar un dispositivo Azure Migrate para detectar las máquinas locales.<br/><br/> La visualización de dependencias no está disponible en Azure Government.
**Mapa de servicio** | La visualización de la dependencia basada en agente usa la solución [Service Map](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) en los [registros de Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).<br/><br/> Para realizar la implementación, asocie un área de trabajo de Log Analytics nuevo o existente con un proyecto de Azure Migrate.
**Área de trabajo de Log Analytics** | El área de trabajo debe estar en la misma suscripción que el proyecto de Azure Migrate.<br/><br/> Azure Migrate admite áreas de trabajo que residen en las regiones Este de EE. UU., Sudeste Asiático y Oeste de Europa.<br/><br/>  El área de trabajo debe estar en una región en la que [se admita Service Map](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#prerequisites).<br/><br/> El área de trabajo de un proyecto de Azure Migrate no se puede modificar una vez que se ha agregado.
**Gastos** | La solución Service Map no supone ningún gasto durante los primeros 180 días (desde el mismo día que asoció el área de trabajo de Log Analytics con el proyecto de Azure Migrate).<br/><br/> Transcurridos los 180 días, se aplicarán las tarifas normales de Log Analytics.<br/><br/> Si se usa alguna solución que no sea Service Map en el área de trabajo de Log Analytics asociada generará los gastos estándar de Log Analytics.<br/><br/> Si elimina el proyecto de Azure Migrate, el área de trabajo no se elimina con él. Tras la eliminación del proyecto, Service Map deja de ser gratuito y cada nodo se cobrará conforme al nivel de pago del área de trabajo de Log Analytics.
**Agentes** | La visualización de la dependencia basada en agente requiere que se instalen dos agentes en los equipos que se quieran analizar.<br/><br/> - [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)<br/><br/> - [Dependency Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
**Conectividad de Internet** | Si las máquinas no están conectadas a Internet, necesita instalar la puerta de enlace de Log Analytics en ellas.

## <a name="next-steps"></a>Pasos siguientes

[Preparación de la valoración de servidores físicos](tutorial-prepare-physical.md).
