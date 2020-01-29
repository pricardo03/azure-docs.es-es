---
title: Compatibilidad con la evaluación de Hyper-V en Azure Migrate
description: Obtenga información sobre la compatibilidad de la evaluación de Hyper-V con Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 1a036e2f22bb1fd9dac65a3cc643224ecbea3c69
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76154812"
---
# <a name="support-matrix-for-hyper-v-assessment"></a>Matriz de compatibilidad para la evaluación de Hyper-V

En este artículo se resumen los valores de compatibilidad y las limitaciones para evaluar máquinas virtuales de Hyper-V con [Azure Migrate: Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool). Si quiere obtener información sobre cómo migrar máquinas virtuales de Hyper-V a Azure, revise la [matriz de compatibilidad de migración](migrate-support-matrix-hyper-v-migration.md).

## <a name="overview"></a>Información general

A fin de evaluar las máquinas locales para la migración a Azure con este artículo, debe agregar la herramienta Azure Migrate: Server Assessment a un proyecto de Azure Migrate. Tendrá que implementar el [dispositivo de Azure Migrate](migrate-appliance.md). El dispositivo detecta las máquinas locales de forma continuada y envía datos de configuración y rendimiento a Azure. Después de la detección de la máquina, debe recopilar las máquinas detectadas en grupos y ejecutar una evaluación de un grupo.


## <a name="limitations"></a>Limitaciones

**Soporte técnico** | **Detalles**
--- | ---
**Límites de evaluación**| Detecte y evalúe hasta 35 000 máquinas virtuales de Hyper-V en un único [proyecto](migrate-support-matrix.md#azure-migrate-projects).
**Límites del proyecto** | Puede crear varios proyectos en una suscripción a Azure. Un proyecto puede incluir servidores físicos y VM de VMware y de Hyper-V hasta los límites de evaluación.
**Detección** | El dispositivo de Azure Migrate puede detectar hasta 5000 máquinas virtuales de Hyper-V.<br/><br/> Un dispositivo puede conectarse a un total de 300 hosts de Hyper-V.
**Valoración** | Puede agregar hasta 35 000 máquinas en un solo grupo.<br/><br/> Puede acceder hasta a 35 000 máquinas virtuales en una única evaluación.

[Más información](concepts-assessment-calculation.md) sobre las evaluaciones.



## <a name="hyper-v-host-requirements"></a>Requisitos del host de Hyper-V:

| **Soporte técnico**                | **Detalles**               
| :-------------------       | :------------------- |
| **Implementación del host**       | El host de Hyper-V puede ser independiente o implementarse en un clúster. |
| **Permisos**           | Necesita permisos de administrador en el host de Hyper-V. <br/> Como alternativa, si no quiere asignar permisos de administrador, cree una cuenta de usuario local o de dominio y agregue el usuario a estos grupos: Usuarios de administración remota, Administradores de Hyper-V y Usuarios del monitor de sistema. |
| **Sistema operativo host** | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2.<br/> No puede evaluar VM que se encuentran en hosts de Hyper-V que ejecutan Windows Server 2012. |
| **Comunicación remota de PowerShell**   | Debe estar habilitada en cada host. |
| **Réplica de Hyper-V**       | Si usa la réplica de Hyper-V (o tiene varias VM con los mismos identificadores de VM) y detecta las VM originales y las replicadas mediante Azure Migrate, es posible que la evaluación generada por Azure Migrate no sea precisa. |


## <a name="hyper-v-vm-requirements"></a>Requisitos de la máquina virtual de Hyper-V

| **Soporte técnico**                  | **Detalles**               
| :----------------------------- | :------------------- |
| **Sistema operativo** | Azure admite todos los sistemas operativos [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) y [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros). |
| **Servicio de integración**       | Los [servicio de integración de Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) deben ejecutarse en las VM que evalúe, con el fin de capturar la información del sistema operativo. |


## <a name="azure-migrate-appliance-requirements"></a>Requisitos del dispositivo de Azure Migrate

Azure Migrate usa el [dispositivo de Azure Migrate](migrate-appliance.md) para la detección y la evaluación. El dispositivo para Hyper-V se ejecuta en una máquina virtual de Hyper-V, y se implementa mediante un disco duro virtual de Hyper-V comprimido que se descarga de Azure Portal. 

- Obtenga información sobre los [requisitos del dispositivo](migrate-appliance.md#appliance---hyper-v) para Hyper-V.
- Obtenga información sobre las [direcciones URL](migrate-appliance.md#url-access) a las que tiene que acceder el dispositivo.

## <a name="port-access"></a>Acceso a puertos

En la tabla siguiente se resumen los requisitos de los puertos para la evaluación.

**Dispositivo** | **Connection**
--- | ---
**Dispositivo** | Conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del Escritorio remoto al dispositivo.<br/> Conexiones entrantes en el puerto 44368 para tener acceso de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: ``` https://<appliance-ip-or-name>:44368 ```<br/> Conexiones salientes en los puertos 443, 5671 y 5672 para enviar los metadatos de detección y rendimiento a Azure Migrate.
**Host o clúster de Hyper-V** | Conexiones entrantes en los puertos de WinRM 5985 (HTTP) y 5986 (HTTPS) para extraer los metadatos de configuración y rendimiento de las VM de Hyper-V mediante una sesión de Modelo de información común (CIM).

## <a name="agent-based-dependency-visualization"></a>Visualización de dependencias basada en agente

La [visualización de dependencias](concepts-dependency-visualization.md) le ayuda a observar las dependencias que hay entre las máquinas que desea evaluar y migrar. En el caso de la visualización basada en agente, los requisitos y las limitaciones se resumen en la siguiente tabla


**Requisito** | **Detalles**
--- | ---
**Implementación** | Antes de implementar la visualización de dependencias, debe tener un proyecto de Azure Migrate en su lugar, con la herramienta Azure Migrate: Server Assessment agregada al proyecto. La visualización de dependencias se implementa después de configurar un dispositivo Azure Migrate para detectar las máquinas locales.<br/><br/> La visualización de dependencias no está disponible en Azure Government.
**Mapa de servicio** | La visualización de la dependencia basada en agente usa la solución [Service Map](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) en los [registros de Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).<br/><br/> Para realizar la implementación, asocie un área de trabajo de Log Analytics nuevo o existente con un proyecto de Azure Migrate.
**Área de trabajo de Log Analytics** | El área de trabajo debe estar en la misma suscripción que el proyecto de Azure Migrate.<br/><br/> Azure Migrate admite áreas de trabajo que residen en las regiones Este de EE. UU., Sudeste Asiático y Oeste de Europa.<br/><br/>  El área de trabajo debe estar en una región en la que [se admita Service Map](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#prerequisites).<br/><br/> El área de trabajo de un proyecto de Azure Migrate no se puede modificar una vez que se ha agregado.
**Gastos** | La solución Service Map no supone ningún gasto durante los primeros 180 días (desde el mismo día que asoció el área de trabajo de Log Analytics con el proyecto de Azure Migrate).<br/><br/> Transcurridos los 180 días, se aplicarán las tarifas normales de Log Analytics.<br/><br/> Si se usa alguna solución que no sea Service Map en el área de trabajo de Log Analytics asociada generará los gastos estándar de Log Analytics.<br/><br/> Si elimina el proyecto de Azure Migrate, el área de trabajo no se elimina con él. Tras la eliminación del proyecto, Service Map deja de ser gratuito y cada nodo se cobrará conforme al nivel de pago del área de trabajo de Log Analytics.
**Agentes** | La visualización de la dependencia basada en agente requiere que se instalen dos agentes en los equipos que se quieran analizar.<br/><br/> - [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)<br/><br/> - [Dependency Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
**Conectividad de Internet** | Si las máquinas no están conectadas a Internet, necesita instalar la puerta de enlace de Log Analytics en ellas.


## <a name="next-steps"></a>Pasos siguientes

[Preparación de la evaluación de máquinas virtuales de Hyper-V](tutorial-prepare-hyper-v.md)
