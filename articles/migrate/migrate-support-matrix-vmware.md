---
title: Compatibilidad para la evaluación de VMware en Azure Migrate
description: Obtenga más información sobre la compatibilidad para la evaluación de VMware en Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 2a9c5504d99f439723a250b619b9f9b660ea9c59
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76029008"
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
Detección | La detección se produce sin agentes con las credenciales de invitado de la máquina y obtiene acceso de forma remota a las máquinas mediante llamadas WMI y SSH.
Equipos compatibles | VM de VMware locales.
Sistema operativo de la máquina | Todas las versiones de Windows y Linux
Credenciales | Actualmente admite el uso de una credencial para todos los servidores Windows y una credencial para todos los servidores Linux.<br/><br/> Se crea una cuenta de usuario invitado para máquinas virtuales Windows y una cuenta de usuario normal (acceso sin sudo) para todas las máquinas virtuales Linux.
límites | En el caso de la detección de aplicaciones, puede detectar hasta 10 000 por dispositivo. 

## <a name="vmware-requirements"></a>Requisitos de VMware

**VMware** | **Detalles**
--- | ---
**vCenter Server** | Las máquinas que quiera detectar y evaluar las debe administrar vCenter Server, versión 5.5, 6.0, 6.5 o 6.7.
**Permisos (evaluación)** | Cuenta de solo lectura de vCenter Server.
**Permisos (detección de aplicaciones)** | Cuenta de vCenter Server con acceso de solo lectura, así como privilegios habilitados para Máquinas virtuales > Operaciones de invitado.
**Permisos (visualización de dependencias)** | Cuenta de Center Server con acceso de solo lectura, así como privilegios habilitados **Máquinas virtuales** > **Operaciones de invitado**.


## <a name="azure-migrate-appliance-requirements"></a>Requisitos del dispositivo de Azure Migrate

Azure Migrate usa el [dispositivo de Azure Migrate](migrate-appliance.md) para la detección y la evaluación. El dispositivo para VMware se implementa mediante una plantilla OVA importada en vCenter Server. 

- Obtenga más información sobre los [requisitos del dispositivo](migrate-appliance.md#appliance---vmware) para VMware.
- Obtenga más información sobre las [direcciones URL](migrate-appliance.md#url-access) a las que tiene que acceder el dispositivo.

## <a name="port-access"></a>Acceso a puertos

**Dispositivo** | **Connection**
--- | ---
Dispositivo | Conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del Escritorio remoto al dispositivo.<br/><br/> Conexiones entrantes en el puerto 44368 para tener acceso de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: ```https://<appliance-ip-or-name>:44368``` <br/><br/>Conexiones salientes en el puerto 443, 5671 y 5672 para enviar metadatos de detección y rendimiento a Azure Migrate.
Servidor vCenter | Conexiones entrantes en el puerto TCP 443 para permitir que el dispositivo recopile los metadatos de configuración y rendimiento de las evaluaciones. <br/><br/> De forma predeterminada, el dispositivo se conecta a vCenter en el puerto 443. Si el servidor vCenter escucha en un puerto diferente, puede modificar el puerto al configurar la detección.

## <a name="dependency-visualization"></a>Visualización de dependencia

La visualización de dependencias le permite observar las dependencias en las máquinas que quiere evaluar y migrar. La asignación de dependencias suele usarse cuando se quiere evaluar máquinas con niveles de confianza más altos. En el caso de las máquinas virtuales de VMware, se admite la visualización de dependencias como se muestra a continuación:

- **Visualización de dependencias sin agentes**: Esta opción se encuentra actualmente en versión preliminar. No es necesario que instale ningún agente en las máquinas.
    - Funciona capturando los datos de conexión TCP de las máquinas para las que está habilitada. Una vez iniciada la detección de dependencias, el dispositivo recopila datos de las máquinas en un intervalo de sondeo de cinco minutos.
    - Se pueden recopilar los siguientes datos:
        - Conexiones TCP
        - Nombres de los procesos que tienen conexiones activas
        - Nombres de las aplicaciones instaladas que ejecutan los procesos anteriores
        - No. de conexiones detectadas en cada intervalo de sondeo
- **Visualización de dependencias basada en agente**: para usar la visualización de dependencias basada en agente, debe descargar e instalar los siguientes agentes en cada máquina local que vaya a analizar.
    - Instale Microsoft Monitoring Agent (MMA) en cada máquina. [Obtenga más información](how-to-create-group-machine-dependencies.md#install-the-mma) sobre cómo instalar el agente MMA.
    - Instale Dependency Agent en cada máquina. [Obtenga más información](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) sobre cómo instalar el agente de dependencia.
    - Además, si tiene máquinas sin conectividad a Internet, debe descargar e instalar en ellas la puerta de enlace de Log Analytics.


## <a name="next-steps"></a>Pasos siguientes

- [Revise](best-practices-assessment.md) los procedimientos recomendados para crear evaluaciones.
- [Prepárese para la evaluación de VMware](tutorial-prepare-vmware.md).
