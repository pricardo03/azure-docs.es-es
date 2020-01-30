---
title: Detección de aplicaciones, roles y características en servidores locales con Azure Migrate
description: Obtenga información sobre cómo detectar aplicaciones, roles y características en servidores locales con Azure Migrate Server Assessment.
ms.topic: article
ms.date: 11/20/2019
ms.openlocfilehash: 7c3a6360e051b8cb694393695e75578ce2c135ec
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846319"
---
# <a name="discover-machine-apps-roles-and-features"></a>Detección de aplicaciones, roles y características de la máquina

En este artículo se describe cómo detectar aplicaciones, roles y características en servidores locales mediante Azure Migrate: Server Assessment.

La detección del inventario de aplicaciones y roles o características que se ejecutan en las máquinas locales le ayuda a identificar y planear una ruta de migración a Azure que está adaptada a sus cargas de trabajo.

> [!NOTE]
> La detección de aplicaciones solo se admite actualmente para las VM de VMware y solo se limita a la detección. Todavía no se ofrece la evaluación basada en aplicaciones. Valoración basada en máquina de máquinas virtuales de Hyper-V locales, máquinas virtuales de VMware y servidores físicos.

Detección de aplicaciones mediante Azure Migrate: Server Assessment es una herramienta sin agente. No es necesario instalar nada en las máquinas ni las VM. Server Assessment usa el dispositivo de Azure Migrate para realizar la detección junto con las credenciales de invitado de la máquina. El dispositivo accede de forma remota a las máquinas de VMware mediante las API de VMware.


## <a name="before-you-start"></a>Antes de comenzar

1. Asegúrese de que ha [creado](how-to-add-tool-first-time.md) un proyecto de Azure Migrate.
2. Asegúrese de haber [agregado](how-to-assess.md) la herramienta Azure Migrate: Server Assessment a un proyecto.
4. Compruebe los [requisitos de VMware](migrate-support-matrix-vmware.md#vmware-requirements) para detectar y evaluar las VM de VMware con el dispositivo de Azure Migrate.
5. Compruebe los [requisitos ](migrate-appliance.md) para implementar el dispositivo de Azure Migrate.
6. [Compruebe la compatibilidad y los requisitos](migrate-support-matrix-vmware.md#application-discovery) para la detección de aplicaciones.

## <a name="prepare-for-app-discovery"></a>Preparación para la detección de aplicaciones

1. [Prepárese para la implementación del dispositivo](tutorial-prepare-vmware.md). La preparación incluye la comprobación de la configuración del dispositivo y la configuración de una cuenta que el dispositivo utilizará para acceder a vCenter Server.
2. Asegúrese de que tiene una cuenta de usuario (una para servidores Windows y otra para servidores Linux) con permisos de administrador para las máquinas en las que quiere detectar aplicaciones, roles y características.
3. [Implemente el dispositivo de Azure Migrate](how-to-set-up-appliance-vmware.md) para iniciar la detección. Para implementar el dispositivo, descargue e importe una plantilla de OVA en VMware para crear el dispositivo como una VM de VMware. Configure el dispositivo y, a continuación, regístrelo con Azure Migrate.
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
