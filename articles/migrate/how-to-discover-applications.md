---
title: Detección de aplicaciones, roles y características instaladas en servidores locales mediante Azure Migrate Server Assessment
description: Describe cómo detectar aplicaciones, roles y características en servidores locales mediante Azure Migrate Server Assessment.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: article
ms.date: 10/16/2019
ms.author: snehaa
ms.openlocfilehash: e09c430204dea490bace029b809f25304be53a9e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510249"
---
# <a name="discover-machine-apps-roles-and-features"></a>Detección de aplicaciones, roles y características de la máquina 

En este artículo se describe cómo detectar aplicaciones, roles y características en servidores locales mediante Azure Migrate: Server Assessment.

La detección del inventario de aplicaciones y roles o características que se ejecutan en las máquinas locales le ayuda a identificar y planear una ruta de migración a Azure que está adaptada a sus cargas de trabajo. 

Detección de aplicaciones mediante Azure Migrate: Server Assessment es una herramienta sin agente. No es necesario instalar nada en las máquinas ni las VM. Server Assessment usa el dispositivo de Azure Migrate para realizar la detección junto con las credenciales de invitado de la máquina. El dispositivo accede de forma remota a las máquinas mediante llamadas WMI y SSH. 

> [!NOTE]
> La detección de aplicaciones solo se admite actualmente para las VM de VMware y solo se limita a la detección. Todavía no se ofrece la evaluación basada en aplicaciones.  Actualmente, la herramienta Server Assessment evalúa VM de VMware locales, VM de Hyper-V y servidores físicos en el nivel de la máquina, para migraciones mediante lift-and-shift.


## <a name="before-you-start"></a>Antes de comenzar

1. Revise las [limitaciones de compatibilidad](migrate-support-matrix-vmware.md#application-discovery) para la detección de nivel de aplicación.
2. Asegúrese de que ha [creado](how-to-add-tool-first-time.md) un proyecto de Azure Migrate.
3. Si ya ha creado un proyecto, asegúrese de que ha [agregado](how-to-assess.md) la herramienta Azure Migrate: Server Assessment.
4. Compruebe los [requisitos de VMware](migrate-support-matrix-vmware.md#assessment-vcenter-server-requirements) para detectar y evaluar las VM de VMware con el dispositivo de Azure Migrate.
4. Compruebe los [requisitos ](migrate-support-matrix-vmware.md#assessment-appliance-requirements) para implementar el dispositivo de Azure Migrate.

## <a name="prepare-for-app-discovery"></a>Preparación para la detección de aplicaciones

1. [Prepárese para la implementación del dispositivo](https://docs.microsoft.com/azure/migrate/tutorial-prepare-vmware). La preparación incluye la comprobación de la configuración del dispositivo y la configuración de una cuenta que el dispositivo utilizará para acceder a vCenter Server.
2. Asegúrese de que tiene una cuenta de usuario (una para servidores Windows y otra para servidores Linux) con permisos de administrador para las máquinas en las que quiere detectar aplicaciones, roles y características.
3. [Implemente el dispositivo de VMware](how-to-set-up-appliance-vmware.md) para iniciar la detección. Para implementar el dispositivo, descargue e importe una plantilla de OVA en VMware para crear el dispositivo como una VM de VMware. Configure el dispositivo y, a continuación, regístrelo con Azure Migrate.
2. Mientras implementa el dispositivo, especifique lo siguiente para iniciar la detección continua:
    - El nombre de la instancia de vCenter Server que quiere conectar.
    - Las credenciales que ha creado para que el dispositivo se conecte a vCenter Server.
    - Las credenciales de la cuenta que ha creado para que el dispositivo se conecte a las VM de Windows o Linux.

Después de implementar el dispositivo y proporcionar las credenciales, el dispositivo inicia la detección continua de los metadatos de la VM y los datos de rendimiento, junto con la detección de aplicaciones, características y roles.  La duración de la detección de aplicaciones depende del número de VM que tenga. Normalmente, la detección de aplicaciones tarda una hora para 500 VM.

## <a name="review-and-export-the-inventory"></a>Revisión y exportación del inventario

Una vez finalizada la detección, si ha proporcionado las credenciales para la detección de aplicaciones, puede revisar y exportar el inventario de la aplicación en Azure Portal. 

1. En la página **Azure Migrate - Servidores** > **Azure Migrate: Server Assessment**, haga clic en el recuento que se muestra para abrir la página **Servidores detectados**.

    > [!NOTE]
    > En esta fase también tiene la opción de configurar la asignación de dependencias para las máquinas detectadas, a fin de poder visualizar las dependencias entre las máquinas que quiere evaluar. [Más información](how-to-create-group-machine-dependencies.md).

2. En **Aplicaciones detectadas**, haga clic en el recuento mostrado.
3. En **Inventario de aplicaciones**, puede revisar las aplicaciones, los roles y las características que se han detectado.
4. Para exportar el inventario, en **Servidores detectados**, haga clic en **Exportar el inventario de las aplicaciones**.

El inventario de la aplicación se exporta y descarga en formato de Excel. En la hoja **Inventario de aplicaciones** se muestran todas las aplicaciones detectadas en todas las máquinas.

## <a name="next-steps"></a>Pasos siguientes

- [Cree una evaluación](how-to-create-assessment.md) para la migración mediante lift-and-shift de los servidores detectados.
- Evalúe instancias de SQL Server Database con [Azure Migrate: Evaluación de la base de datos](https://docs.microsoft.com/sql/dma/dma-assess-sql-data-estate-to-sqldb?view=sql-server-2017).
