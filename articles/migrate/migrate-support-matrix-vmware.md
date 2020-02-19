---
title: Compatibilidad para la evaluación de VMware en Azure Migrate
description: Obtenga más información sobre la compatibilidad para la evaluación de VMware en Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 19ed506228bac425ad05edee1586740e6c33f69e
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77121323"
---
# <a name="support-matrix-for-vmware-assessment"></a>Matriz de compatibilidad para la evaluación de VMware 

En este artículo se resumen los valores de compatibilidad y las limitaciones para evaluar VM de VMware con [Azure Migrate: Server Assessment](migrate-services-overview.md#azure-migrate-server-migration-tool). Si quiere obtener información sobre cómo migrar VM de VMware a Azure, revise la [matriz de compatibilidad de migración](migrate-support-matrix-vmware-migration.md).

## <a name="overview"></a>Información general

A fin de evaluar las máquinas locales para la migración a Azure con este artículo, debe agregar la herramienta Azure Migrate: Server Assessment a un proyecto de Azure Migrate. Tendrá que implementar el [dispositivo de Azure Migrate](migrate-appliance.md). El dispositivo detecta las máquinas locales de forma continuada y envía datos de configuración y rendimiento a Azure. Después de la detección de la máquina, debe recopilar las máquinas detectadas en grupos y ejecutar una evaluación de un grupo.


## <a name="limitations"></a>Limitaciones

**Soporte técnico** | **Detalles**
--- | ---
**Límites de evaluación**| Evalúe hasta 35 000 VM de VMware en un único [proyecto](migrate-support-matrix.md#azure-migrate-projects).
**Límites del proyecto** | Puede crear varios proyectos en una suscripción a Azure. Un proyecto puede incluir servidores físicos y VM de VMware y de Hyper-V hasta los límites de evaluación.
**Detección** | Un dispositivo de Azure Migrate puede detectar hasta 10 000 VM de VMware en una instancia de vCenter Server.
**Valoración** | Puede agregar hasta 35 000 máquinas en un solo grupo.<br/><br/> Puede acceder hasta a 35 000 máquinas virtuales en una única evaluación.

[Más información](concepts-assessment-calculation.md) sobre las evaluaciones.


## <a name="application-discovery"></a>Detección de aplicaciones

Además de detectar máquinas, Azure Migrate: Server Assessment puede detectar aplicaciones, roles y características en ejecución en las máquinas. La detección del inventario de aplicaciones le permite identificar y planificar una ruta de migración adaptada a sus cargas de trabajo locales. 

**Soporte técnico** | **Detalles**
--- | ---
**Detección** | La detección se produce sin agentes con las credenciales de invitado de la máquina y obtiene acceso de forma remota a las máquinas mediante llamadas WMI y SSH.
**Máquinas compatibles** | VM de VMware locales.
**Sistema operativo de la máquina** | Todas las versiones de Windows y Linux.
**Credenciales de vCenter** | Cuenta de vCenter Server con acceso de solo lectura, así como privilegios habilitados para Máquinas virtuales > Operaciones de invitado.
**Credenciales de máquina virtual** | Actualmente admite el uso de una credencial para todos los servidores Windows y una credencial para todos los servidores Linux.<br/><br/> Se crea una cuenta de usuario invitado para máquinas virtuales Windows y una cuenta de usuario normal (acceso sin sudo) para todas las máquinas virtuales Linux.
**Herramientas de VMware** | Las herramientas de VMware se deben instalar y ejecutar en las máquinas virtuales que se quieren detectar. <br/> Si la versión de las herramientas de VMware está entre 9.10 y 10.2.0, asegúrese de actualizarla a una versión posterior a 10.2.0.
**Acceso a puertos** | En los hosts ESXi que ejecuten las máquinas virtuales que desea detectar, Azure Migrate debería ser capaz de conectarse al puerto TCP 443.
**Límites** | En el caso de la detección de aplicaciones, puede detectar hasta 10 000 por dispositivo. 

## <a name="vmware-requirements"></a>Requisitos de VMware

**VMware** | **Detalles**
--- | ---
**vCenter Server** | Las máquinas que quiera detectar y evaluar las debe administrar vCenter Server, versión 5.5, 6.0, 6.5 o 6.7.
**Permisos (evaluación)** | Cuenta de solo lectura de vCenter Server.
**Permisos (detección de aplicaciones)** | Cuenta de vCenter Server con acceso de solo lectura, así como privilegios habilitados para **Máquinas virtuales > Operaciones de invitado**.
**Permisos (visualización de dependencias)** | Cuenta de Center Server con acceso de solo lectura, así como privilegios habilitados **Máquinas virtuales** > **Operaciones de invitado**.


## <a name="azure-migrate-appliance-requirements"></a>Requisitos del dispositivo de Azure Migrate

Azure Migrate usa el [dispositivo de Azure Migrate](migrate-appliance.md) para la detección y la evaluación. El dispositivo para VMware se implementa mediante una plantilla OVA importada en vCenter Server. 

- Obtenga más información sobre los [requisitos del dispositivo](migrate-appliance.md#appliance---vmware) para VMware.
- Obtenga más información sobre las [direcciones URL](migrate-appliance.md#url-access) a las que tiene que acceder el dispositivo.

## <a name="port-access"></a>Acceso a puertos

**Dispositivo** | **Connection**
--- | ---
Dispositivo | Conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del Escritorio remoto al dispositivo.<br/><br/> Conexiones entrantes en el puerto 44368 para tener acceso de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: ```https://<appliance-ip-or-name>:44368``` <br/><br/>Conexiones salientes en los puertos 443 (HTTPS), 5671 y 5672 (AMQP) para enviar los metadatos de detección y rendimiento a Azure Migrate.
Servidor vCenter | Conexiones entrantes en el puerto TCP 443 para permitir que el dispositivo recopile los metadatos de configuración y rendimiento de las evaluaciones. <br/><br/> De forma predeterminada, el dispositivo se conecta a vCenter en el puerto 443. Si el servidor vCenter escucha en un puerto diferente, puede modificar el puerto al configurar la detección.
Hosts de ESXi | **Solo son necesarios para la [detección de aplicaciones](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#application-discovery) y la [visualización de dependencias sin agentes](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-dependency-visualization)** <br/><br/> El dispositivo se conecta a los hosts de ESXi en el puerto TCP 443 para detectar aplicaciones y ejecutar la visualización de dependencias sin agentes en las VM que se ejecutan en los hosts.

## <a name="agent-based-dependency-visualization"></a>Visualización de dependencias basada en agente

La [visualización de dependencias](concepts-dependency-visualization.md) le ayuda a observar las dependencias que hay entre las máquinas que desea evaluar y migrar. En el caso de la visualización basada en agente, los requisitos y las limitaciones se resumen en la siguiente tabla


**Requisito** | **Detalles**
--- | ---
**Implementación** | Antes de implementar la visualización de dependencias, debe tener un proyecto de Azure Migrate en su lugar, con la herramienta Azure Migrate: Server Assessment agregada al proyecto. La visualización de dependencias se implementa después de configurar un dispositivo Azure Migrate para detectar las máquinas locales.<br/><br/> La visualización de dependencias no está disponible en Azure Government.
**Mapa de servicio** | La visualización de la dependencia basada en agente usa la solución [Service Map](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) en los [registros de Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).<br/><br/> Para realizar la implementación, asocie un área de trabajo de Log Analytics nuevo o existente con un proyecto de Azure Migrate.
**Área de trabajo de Log Analytics** | El área de trabajo debe estar en la misma suscripción que el proyecto de Azure Migrate.<br/><br/> Azure Migrate admite áreas de trabajo que residen en las regiones Este de EE. UU., Sudeste de Asia y Oeste de Europa.<br/><br/>  El área de trabajo debe estar en una región en la que [se admita Service Map](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#prerequisites).<br/><br/> El área de trabajo de un proyecto de Azure Migrate no se puede modificar una vez que se ha agregado.
**Gastos** | La solución Service Map no supone ningún gasto durante los primeros 180 días (desde el mismo día que asoció el área de trabajo de Log Analytics con el proyecto de Azure Migrate).<br/><br/> Transcurridos los 180 días, se aplicarán las tarifas normales de Log Analytics.<br/><br/> Si se usa alguna solución que no sea Service Map en el área de trabajo de Log Analytics asociada generará los gastos estándar de Log Analytics.<br/><br/> Si elimina el proyecto de Azure Migrate, el área de trabajo no se elimina con él. Tras la eliminación del proyecto, Service Map deja de ser gratuito y cada nodo se cobrará conforme al nivel de pago del área de trabajo de Log Analytics.
**Agentes** | La visualización de la dependencia basada en agente requiere que se instalen dos agentes en los equipos que se quieran analizar.<br/><br/> - [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)<br/><br/> - [Dependency Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
**Conectividad de Internet** | Si las máquinas no están conectadas a Internet, necesita instalar la puerta de enlace de Log Analytics en ellas.


## <a name="agentless-dependency-visualization"></a>Visualización de dependencias sin agentes

Esta opción se encuentra actualmente en versión preliminar. [Más información](how-to-create-group-machine-dependencies-agentless.md). Los requisitos se resumen en la tabla siguiente.

**Requisito** | **Detalles**
--- | ---
**Implementación** | Antes de implementar la visualización de dependencias, debe tener un proyecto de Azure Migrate en su lugar, con la herramienta Azure Migrate: Server Assessment agregada al proyecto. La visualización de dependencias se implementa después de configurar un dispositivo Azure Migrate para detectar las máquinas locales.
**Soporte técnico de máquina virtual** | Actualmente solo se admite en máquinas virtuales VMware.
**Máquinas virtuales Windows** | Windows Server 2016<br/> Windows Server 2012 R2<br/> Windows Server 2012<br/> Windows Server 2008 R2 (64 bits)
**Máquinas virtuales Linux** | Red Hat Enterprise Linux 7, 6, 5<br/> Ubuntu Linux 14.04, 16.04<br/> Debian 7, 8<br/> Oracle Linux 6, 7<br/> CentOS 5, 6, 7
**Cuenta de Windows** |  Se necesita una cuenta de administrador local o de dominio para la visualización.
**Cuenta de Linux** | La visualización necesita una cuenta de usuario con privilegio de usuario raíz.<br/><br/> Como alternativa, la cuenta de usuario necesita estos permisos en los archivos/bin/netstat y /bin/ls: CAP_DAC_READ_SEARCH y CAP_SYS_PTRACE.
**Agentes de máquina virtual** | No se necesitan agentes en las máquinas virtuales.
**Herramientas de VMware** | Las herramientas de VMware se deben instalar y ejecutar en las máquinas virtuales que se quieren analizar. <br/> Si la versión de las herramientas de VMware está entre 9.10 y 10.2.0, asegúrese de actualizarla a una versión posterior a 10.2.0.
**Credenciales de vCenter** | Cuenta de vCenter Server con acceso de solo lectura, así como privilegios habilitados para Máquinas virtuales > Operaciones de invitado.
**Acceso a puertos** | En los hosts ESXi que ejecuten las máquinas virtuales que desea analizar, Azure Migrate debería ser capaz de conectarse al puerto TCP 443.



## <a name="next-steps"></a>Pasos siguientes

- [Revise](best-practices-assessment.md) los procedimientos recomendados para crear evaluaciones.
- [Prepárese para la evaluación de VMware](tutorial-prepare-vmware.md).
