---
title: Aprovisionamiento de Azure-SSIS Integration Runtime
description: Aprenda a aprovisionar Integration Runtime de Azure SSIS en Azure Data Factory para que pueda implementar y ejecutar paquetes SSIS en Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: tutorial
ms.date: 09/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 14871aa790bd94dbd23dea30c8a229eac7945c75
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73683569"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Aprovisionamiento de Azure-SSIS Integration Runtime en Azure Data Factory

En este tutorial se describen los pasos necesarios para usar Azure Portal para aprovisionar Azure-SQL Server Integration Services (SSIS) Integration Runtime en Azure Data Factory. Azure-SSIS Integration Runtime admite:

- La ejecución de paquetes implementados en el catálogo de SSIS (SSISDB) hospedados en un servidor de Azure SQL Database o una instancia administrada (modelo de implementación de proyectos).
- La ejecución de paquetes implementados en sistemas de archivos, recursos compartidos de archivos o Azure Files (modelo de implementación de paquetes). 

Después de aprovisionar una instancia de Azure-SSIS IR, puede usar herramientas conocidas para implementar y ejecutar los paquetes en Azure. Algunas de estas herramientas son SQL Server Data Tools, SQL Server Management Studio y herramientas de línea de comandos como `dtinstall`, `dtutil` y `dtexec`.

Para obtener información conceptual acerca de Integration Runtime para la integración de SSIS en Azure, consulte [Introducción a Integration Runtime de SSIS de Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime).

En este tutorial, va a completar los siguientes pasos:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Aprovisionamiento de una instancia de Integration Runtime de SSIS en Azure.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Suscripción de Azure**. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- **Servidor de Azure SQL Database (opcional)** . Si aún no tiene un servidor de bases de datos, cree uno en Azure Portal antes de empezar. A su vez, Data Factory creará una instancia de SSISDB en este servidor de bases de datos. 

  Le recomendamos que cree el servidor de bases de datos en la misma región de Azure que el entorno de ejecución de integración. Esta configuración permite que el entorno de ejecución de integración escriba registros de ejecución en SSISDB sin traspasar regiones de Azure.

  Tenga en cuenta los siguientes puntos:

  - En función del servidor de bases de datos seleccionado, la instancia de SSISDB puede crearse en su nombre como una base de datos única, como parte de un grupo elástico o en una instancia administrada. Se puede acceder a ella en una red pública o mediante la unión a una red virtual. Para directrices sobre cómo elegir el tipo de servidor de bases de datos para hospedar la instancia de SSISDB, consulte [Comparación entre base de datos únicas o grupos elásticos de SQL Database e Instancia administrada de SQL Database](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance). 
  
    Si usa un servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o una instancia administrada con un punto de conexión privado para hospedar SSISDB, o si necesita acceder a los datos locales sin configurar IR autohospedado, debe unir la instancia de Azure-SSIS IR a una red virtual. Para más información, consulte [Creación de una instancia de Azure-SSIS IR en una red virtual](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
  - Confirme que el valor **Permitir el acceso a servicios de Azure** está habilitado para el servidor de bases de datos. Esta configuración no es aplicable si para hospedar SSISDB se usa el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o una instancia administrada con un punto de conexión privado. Para más información, consulte [Protección de Azure SQL Database](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Para habilitar este valor mediante PowerShell, consulte [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
  - Agregue la dirección IP de la máquina cliente, o un intervalo de direcciones IP que la incluya, a la lista de direcciones IP de cliente de la configuración del firewall del servidor de bases de datos. Para más información, consulte [Reglas de firewall de nivel de base de datos y de nivel de servidor de Azure SQL Database](../sql-database/sql-database-firewall-configure.md).
  - Puede conectarse al servidor de bases de datos mediante la autenticación de SQL con sus credenciales de administrador del servidor o por medio de la autenticación de Azure AD con la identidad administrada de la factoría de datos. En el último de los casos, debe agregar la identidad administrada de la factoría de datos a un grupo de Azure AD con permisos de acceso al servidor de base de datos. Para más información, consulte [Creación de una instancia de Azure-SSIS IR con autenticación de Azure AD](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
  - Confirme que el servidor de bases de datos no tiene aún una instancia de SSISDB. El aprovisionamiento de Azure-SSIS IR no admite el uso de una instancia de SSISDB existente.


> [!NOTE]
> Para ver una lista de las regiones de Azure en las que Data Factory y Azure-SSIS IR están disponibles actualmente, consulte [Disponibilidad de Data Factory y SSIS IR por región](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="create-a-data-factory"></a>Crear una factoría de datos

Para crear la factoría de datos mediante Azure Portal, siga las instrucciones paso a paso que se indican en [Creación de una factoría de datos mediante la interfaz de usuario](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory). Seleccione **Anclar al panel** mientras lo hace, para permitir el acceso rápido después de su creación. 

Después de crear la factoría de datos, abra su página de información general en Azure Portal. Seleccione el icono **Author & monitor**  (Creación y supervisión) para abrir su página de **inicio** en otra pestaña. Ahí puede continuar con la creación de la instancia de Azure-SSIS IR.

## <a name="create-an-azure-ssis-integration-runtime"></a>Creación de un entorno de ejecución de integración de SSIS para Azure

### <a name="from-the-data-factory-overview"></a>Desde la información general de Data Factory

1. En la página **Let's get started** (Comencemos), seleccione el icono **Configure SSIS Integration Runtime** (Configuración de Integration Runtime de SSIS). 

   ![Icono "Configure SSIS Integration Runtime" (Configuración de Integration Runtime de SSIS)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Para el resto de pasos de configuración de Integration Runtime para la integración de SSIS en Azure, consulte la sección [Aprovisionamiento de Integration Runtime de SSIS de Azure](#provision-an-azure-ssis-integration-runtime). 

### <a name="from-the-authoring-ui"></a>Desde la interfaz de usuario de creación

1. En la interfaz de usuario de Azure Data Factory, cambie a la pestaña **Editar** y seleccione **Conexiones**. A continuación, cambie a la pestaña **Integration Runtimes** (Entornos de ejecución de integración) para ver los entornos de ejecución de integración existentes en la factoría de datos. 

   ![Opciones para ver las instancias de Integration Runtime existentes](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Seleccione **New** (Nuevo) para crear una instancia de Integration Runtime para la integración de SSIS en Azure. 

   ![Integration Runtime a través del menú](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. En la ventana **Integration Runtime Setup** (Instalación de Integration Runtime), seleccione **Lift-and-shift existing SSIS packages to execute in Azure** (Migrar mediante lift-and-shift los paquetes de SSIS existentes para ejecutarlos en Azure) y, después, seleccione **Siguiente**. 

   ![Especificación del tipo de instancia de Integration Runtime](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Para el resto de pasos de configuración de Integration Runtime para la integración de SSIS en Azure, consulte la sección [Aprovisionamiento de Integration Runtime de SSIS de Azure](#provision-an-azure-ssis-integration-runtime). 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Aprovisionamiento de una instancia de Integration Runtime para la integración de SSIS en Azure

1. En la página **General Settings** (Configuración general) de **Integration Runtime Setup** (Configuración de Integration Runtime), realice los pasos siguientes. 

   ![Configuración general](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. En **Name** (Nombre), escriba el nombre del entorno de ejecución de integración. 

   b. En **Description** (Descripción), escriba la descripción del entorno de ejecución de integración. 

   c. En **Location** (Ubicación), seleccione la ubicación del entorno de ejecución de integración. Se muestran solo las ubicaciones admitidas. Le recomendamos que seleccione la misma ubicación del servidor de bases de datos que va a hospedar SSISDB. 

   d. En **Node Size** (Tamaño de nodo), seleccione el tamaño de nodo del clúster del entorno de ejecución de integración. Se muestran solo los tamaños de nodo admitidos. Seleccione un tamaño de nodo grande (escalado vertical) si quiere ejecutar muchos paquetes de uso intensivo de proceso o memoria. 

   e. En **Node Number** (Número de nodos), seleccione el número de nodos del clúster del entorno de ejecución de integración. Se muestran solo los números de nodos admitidos. Seleccione un clúster de grande con muchos nodos (escalado horizontal), si quiere ejecutar muchos paquetes en paralelo. 

   f. En **Edition/License** (Edición o licencia), seleccione la edición de SQL Server para el entorno de ejecución de integración: Standard o Enterprise. Seleccione Enterprise si quiere usar características avanzadas en el entorno de ejecución de integración. 

   g. En **Save Money** (Ahorrar dinero), seleccione la opción Azure Hybrid Benefit (Ventaja híbrida de Azure) para el entorno de ejecución de integración: **Yes** (Sí) o **No**. Seleccione **Yes** (Sí) si quiere que su propia licencia de SQL Server con Software Assurance se beneficie de los ahorros con el uso híbrido. 

   h. Seleccione **Next** (Siguiente). 

1. En la página **SQL Settings** (Configuración de SQL), haga lo siguiente. 

   ![Configuración de SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. Seleccione la casilla **Create SSIS catalog...** (Crear catálogo de SSIS...) para seleccionar el modelo de implementación de paquetes que se ejecutará en Azure-SSIS IR. Elegirá entre el modelo de implementación de proyectos, donde los paquetes se implementan en la instancia de SSISDB que hospeda el servidor de bases de datos, o el modelo de implementación de paquetes, donde los paquetes se implementan en sistemas de archivos, recursos compartidos de archivos o Azure Files.
   
   Si selecciona la casilla, deberá traer su propio servidor de bases de datos para hospedar la instancia de SSISDB que se creará y administrará en su nombre.
   
   b. En **Subscription** (Suscripción), seleccione la suscripción de Azure que tiene el servidor de bases de datos que va a hospedar SSISDB. 

   c. En **Location** (Ubicación), seleccione la ubicación del servidor de bases de datos que va a hospedar SSISDB. Es recomendable seleccionar la misma ubicación del entorno de ejecución de integración.

   d. En **Catalog Database Server Endpoint** (Punto de conexión del servidor de bases de datos de catálogo), seleccione el punto de conexión de su servidor de bases de datos que va a hospedar SSISDB. 
   
   En función del servidor de bases de datos seleccionado, la instancia de SSISDB puede crearse en su nombre como una base de datos única, como parte de un grupo elástico o en una instancia administrada. Se puede acceder a ella en una red pública o mediante la unión a una red virtual. Para directrices sobre cómo elegir el tipo de servidor de bases de datos para hospedar la instancia de SSISDB, consulte [Comparación entre base de datos únicas o grupos elásticos de SQL Database e Instancia administrada de SQL Database](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance).   

   Si selecciona un servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o una instancia administrada con un punto de conexión privado para hospedar SSISDB, o si necesita acceder a los datos locales sin configurar IR autohospedado, debe unir la instancia de Azure-SSIS IR a una red virtual. Para más información, consulte [Creación de una instancia de Azure-SSIS IR en una red virtual](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

   e. Seleccione la casilla **Use AAD authentication with the managed identity for your ADF** (Usar la autenticación de AAD con la identidad administrada en su ADF) para seleccionar el método de autenticación del servidor de bases de datos para hospedar SSISDB. Elegirá la autenticación de SQL o la autenticación de Azure AD con la identidad administrada para su factoría de datos.

   Si selecciona la casilla, deberá agregar la identidad administrada de la factoría de datos a un grupo de Azure AD con permisos de acceso al servidor de base de datos. Para más información, consulte [Creación de una instancia de Azure-SSIS IR con autenticación de Azure AD](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
   
   f. En **Admin Username** (Nombre de usuario del administrador), escriba el nombre de usuario de autenticación de SQL del servidor de bases de datos para hospedar SSISDB. 

   g. En **Admin Password** (Contraseña del administrador), escriba la contraseña de la autenticación de SQL del servidor de bases de datos para hospedar SSISDB. 

   h. En **Catalog Database Service Tier** (Nivel de servicio de bases de datos de catálogo), seleccione el nivel de servicio del servidor de bases de datos para hospedar SSISDB. Seleccione el nivel de servicio Basic, Standard o Premium, o seleccione un nombre de grupo elástico.

   i. Seleccione **Test Connection** (Probar conexión). Si la prueba se realiza correctamente, seleccione **Next** (Siguiente). 

1. En la página **Advanced Settings** (Configuración avanzada), haga lo siguiente. 

   ![Configuración avanzada](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. En **Maximum Parallel Executions Per Node** (Número máximo de ejecuciones en paralelo por nodo), seleccione el número máximo de paquetes que se van a ejecutar simultáneamente por nodo en el clúster del entorno de ejecución de integración. Se muestran solo los números de paquetes admitidos. Seleccione un número bajo si quiere usar más de un núcleo para ejecutar un único paquete grande o pesado con un uso intensivo de memoria o proceso. Seleccione un número alto si quiere ejecutar uno o varios paquetes pequeños en un único núcleo. 

   b. En **Custom Setup Container SAS URI** (URI de SAS del contenedor de configuración personalizada), especifique opcionalmente el identificador uniforme de recursos (URI) de la firma de acceso compartido (SAS) del contenedor de Azure Blob Storage en el que se almacenan el script de configuración y sus archivos asociados. Para más información, consulte [Instalación personalizada de una instancia de Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).
   
   c. Seleccione la casilla **Select a VNet for your Azure-SSIS Integration Runtime to join and allow ADF to create certain network resources** (Seleccionar una red virtual para Azure-SSIS Integration Runtime para unirse a ADF y permitir que cree determinados recursos de red) para elegir si desea unir el entorno de ejecución de integración a una red virtual.

   Seleccione esta casilla si usa un servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o una instancia administrada con un punto de conexión privado para hospedar SSISDB, o si necesita acceder a los datos locales sin configurar un entorno de ejecución de integración autohospedado. Para más información, consulte [Creación de una instancia de Azure-SSIS IR en una red virtual](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
   
   d. Seleccione la casilla **Set up Self-Hosted Integration Runtime as a proxy for your Azure-SSIS Integration Runtime** (Configurar el entorno de ejecución de integración autohospedado como proxy para su instancia de Azure-SSIS IR) para elegir si desea configurar un entorno de ejecución de integración autohospedado como proxy para la instancia de Azure-SSIS IR. Para más información, consulte [Configuración de un entorno de ejecución de integración autohospedado como proxy](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis).   

1. Seleccione **Next** (Siguiente). 

1. En la página **Resumen**, revise todos los valores de aprovisionamiento, marque los vínculos a la documentación recomendada y seleccione **Finalizar** para empezar la creación del entorno de ejecución de integración. 

   > [!NOTE]
   > Aparte del tiempo de configuración personalizada, este proceso debería realizarse en 5 minutos.
   >
   > Si usa SSISDB, el servicio Data Factory se conectará al servidor de bases de datos para prepararlo. 
   > 
   > Al aprovisionar Azure-SSIS IR, también se instalan el acceso redistribuible y el paquete de característica de Azure para SSIS. Estos componentes proporcionan conectividad a archivos de Excel y Access, y a diversos orígenes de datos de Azure, además de a aquellos que ya admiten los componentes integrados. Para información sobre otros componentes que puede instalar, consulte [Instalación personalizada de Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

1. En la pestaña **Connections** (Conexiones), cambie a **Integration Runtimes**, si es necesario. Seleccione **Refresh** (Actualizar) para actualizar el estado. 

   ![Estado de creación, con el botón "Refresh"](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Use los vínculos de la columna **Actions** (Acciones) para iniciar, detener, iniciar, editar o eliminar la instancia de Integration Runtime. Utilice el último vínculo para ver el código JSON de Integration Runtime. Los botones de edición y eliminación se habilitan únicamente cuando la instancia de IR se detiene. 

   ![Vínculo de la columna "Actions"](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png) 

## <a name="deploy-ssis-packages"></a>Implementación de paquetes de SSIS

Si usa SSISDB, puede implementar los paquetes en esta base de datos y ejecutarlos en Azure-SSIS IR mediante las herramientas SQL Server Data Tools o SQL Server Management Studio. Estas herramientas se conectan al servidor de bases de datos mediante su punto de conexión de servidor: 

- En el caso de un servidor de Azure SQL Database con un punto de conexión privado, el formato del punto de conexión de servidor es `<server name>.database.windows.net`.
- En el caso de una instancia administrada con un punto de conexión privado, el formato del punto de conexión de servidor es `<server name>.<dns prefix>.database.windows.net`.
- En el caso de una instancia administrada con un punto de conexión público, el formato del punto de conexión de servidor es `<server name>.public.<dns prefix>.database.windows.net,3342`. 

Si no usa SSISDB, puede implementar los paquetes en sistemas de archivos, recursos compartidos de archivos o Azure Files. Después, puede ejecutarlos en la instancia de Azure-SSIS IR mediante las herramientas de línea de comandos `dtinstall`, `dtutil` y `dtexec`. Para más información, consulte [Implementación de paquetes en SSIS](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). 

En ambos casos, también puede ejecutar los paquetes implementados en Azure-SSIS IR mediante la actividad de ejecución de paquetes SSIS de las canalizaciones de Data Factory. Para más información, consulte [Invocación de la ejecución de paquetes SSIS como una actividad de Data Factory de primera clase](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Consulte también la documentación de SSIS siguiente: 

- [Implementación, ejecución y supervisión de paquetes de SSIS en Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Conexión a SSISDB en Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Programación de la ejecución de paquetes en Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Conexión a orígenes de datos locales con autenticación de Windows) 

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de la personalización del entorno de ejecución de integración de SSIS de Azure, vaya al siguiente artículo: 

> [!div class="nextstepaction"]
> [Personalización de Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)