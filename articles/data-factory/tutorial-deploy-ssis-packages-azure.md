---
title: Aprovisionamiento de un entorno de ejecución para la integración de SSIS en Azure | Microsoft Docs
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
ms.openlocfilehash: b3b180fe4b465f3e0c7de888043326fd1a43cf23
ms.sourcegitcommit: a819209a7c293078ff5377dee266fa76fd20902c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71009639"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Aprovisionamiento de Integration Runtime de Azure SSIS en Azure Data Factory

En este tutorial se describen los pasos necesarios para usar Azure Portal para aprovisionar Integration Runtime de Azure-SQL Server Integration Services (ISSIS) en Azure Data Factory (ADF). Azure-SSIS Integration Runtime admite la ejecución de paquetes implementados en el catálogo de SSIS (SSISDB) hospedados en el servidor de Azure SQL Database/Instancia administrada (modelo de implementación de proyectos) y los implementados en sistemas de archivos/recursos compartidos de archivos/Azure Files (modelo de implementación de paquetes). Una vez aprovisionado Azure-SSIS IR, puede usar herramientas familiares, como SQL Server Data Tools (SSDT)/ SQL Server Management Studio (SSMS) y utilidades de la línea de comandos, como `dtinstall`/`dtutil`/`dtexec`, para implementar y ejecutar los paquetes en Azure. Para obtener información conceptual acerca de Integration Runtime para la integración de SSIS en Azure, consulte [Introducción a Integration Runtime de SSIS de Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime).

En este tutorial, va a completar los siguientes pasos:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Aprovisionamiento de una instancia de Integration Runtime de SSIS en Azure.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Suscripción de Azure**. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- **Servidor de Azure SQL Database (opcional)** . Si aún no tiene un servidor de bases de datos, cree uno en Azure Portal antes de empezar. A su vez, ADF creará la instancia de SSISDB en este servidor de bases de datos. Le recomendamos que cree el servidor de bases de datos en la misma región de Azure que el entorno de ejecución de integración. Esta configuración permite que el entorno de ejecución de integración escriba registros de ejecución en SSISDB sin traspasar regiones de Azure. 
    - En función del servidor de bases de datos seleccionado, la SSISDB puede crearse en su nombre como base de datos única, como parte de un grupo elástico o en una instancia administrada y estar accesible desde la red pública o mediante la unión a una red virtual. Para directrices sobre cómo elegir el tipo de servidor de bases de datos para hospedar la instancia de SSISDB, consulte [Comparación entre base de datos únicas o grupos elásticos de SQL Database e Instancia administrada de SQL Database](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Si usa el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado para hospedar SSISDB o si necesita acceder a los datos locales sin configurar IR autohospedado, debe unir la instancia de Azure-SSIS IR a una red virtual. Para ello, consulte el artículo de [Creación de una instancia de Azure-SSIS IR en una red virtual](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Confirme que el valor **Permitir el acceso a servicios de Azure** está habilitado para el servidor de bases de datos. Esto no es aplicable si se utiliza el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado para hospedar SSISDB. Para más información, consulte [Protección de Azure SQL Database](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Para habilitar este valor mediante PowerShell, consulte [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Agregue la dirección IP de la máquina cliente o un intervalo de direcciones IP que incluya la dirección IP de la máquina cliente en la lista de direcciones IP de cliente en la configuración del firewall del servidor de bases de datos. Para más información, consulte [Reglas de firewall de nivel de base de datos y de nivel de servidor de Azure SQL Database](../sql-database/sql-database-firewall-configure.md).
    - Puede conectarse al servidor de bases de datos mediante la autenticación de SQL con sus credenciales de administrador del servidor o por medio de la autenticación de Azure Active Directory (AAD) con la identidad administrada de ADF. En este último caso, debe agregar la entidad administrada para su ADF en un grupo de AAD con permisos de acceso al servidor de bases de datos. Para ello, consulte [Crear una instancia de Integration Runtime de Azure SSIS en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Confirme que el servidor de bases de datos no tiene aún una instancia de SSISDB. El aprovisionamiento de Azure-SSIS IR no admite el uso de una instancia de SSISDB existente.

> [!NOTE]
> - Para una lista de las regiones de Azure en las que ADF y Azure-SSIS IR están actualmente disponibles, consulte la [disponibilidad de ADF + SSIS IR por región](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="create-a-data-factory"></a>Crear una factoría de datos

Para crear una instancia de Azure Data Factory mediante Azure Portal, siga las instrucciones paso a paso del artículo [Creación de una instancia de Azure Data Factory mediante la interfaz de usuario](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory) y seleccione **Anclar al panel** mientras lo hace para permitir el acceso rápido después de su creación. 

Una vez creada la instancia de Azure Data Factory, abra su página de información general en Azure Portal y haga clic en el icono de **creación y supervisión** para iniciar la página **Comencemos** en una pestaña independiente en la que pueda continuar creando la instancia de Azure-SSIS Integration Runtime.   

## <a name="create-an-azure-ssis-integration-runtime"></a>Creación de un entorno de ejecución de integración de SSIS para Azure

### <a name="from-the-data-factory-overview"></a>Desde la información general de Data Factory

1. En la página **Comencemos**, haga clic en el icono **Configure SSIS Integration Runtime** (Configuración de Integration Runtime de SSIS). 

   ![Icono "Configure SSIS Integration Runtime" (Configuración de Integration Runtime de SSIS)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Para el resto de pasos de configuración de Integration Runtime para la integración de SSIS en Azure, consulte la sección [Aprovisionamiento de Integration Runtime de SSIS de Azure](#provision-an-azure-ssis-integration-runtime). 

### <a name="from-the-authoring-ui"></a>Desde la interfaz de usuario de creación

1. En la interfaz de usuario de Azure Data Factory, cambie a la pestaña **Edit** (Editar), seleccione **Connections** (Conexiones) y cambie a la pestaña **Integration Runtime** para ver las instancias de Integration Runtime existentes en la factoría de datos. 

   ![Opciones para ver las instancias de Integration Runtime existentes](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Seleccione **New** (Nuevo) para crear una instancia de Integration Runtime para la integración de SSIS en Azure. 

   ![Integration Runtime a través del menú](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. En la ventana **Integration Runtime Setup** (Instalación de Integration Runtime), seleccione **Lift-and-shift existing SSIS packages to execute in Azure** (Migrar mediante lift-and-shift los paquetes de SSIS existentes para ejecutarlos en Azure) y, después, seleccione **Siguiente**. 

   ![Especificación del tipo de instancia de Integration Runtime](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Para el resto de pasos de configuración de Integration Runtime para la integración de SSIS en Azure, consulte la sección [Aprovisionamiento de Integration Runtime de SSIS de Azure](#provision-an-azure-ssis-integration-runtime). 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Aprovisionamiento de una instancia de Integration Runtime para la integración de SSIS en Azure

1. En la página **General Settings** (Configuración general) de **Integration Runtime Setup** (Configuración de Integration Runtime), realice los pasos siguientes: 

   ![Configuración general](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. En **Name** (Nombre), escriba el nombre del entorno de ejecución de integración. 

   b. En **Description** (Descripción), escriba la descripción del entorno de ejecución de integración. 

   c. En **Location** (Ubicación), seleccione la ubicación del entorno de ejecución de integración. Se muestran solo las ubicaciones admitidas. Le recomendamos que seleccione la misma ubicación del servidor de bases de datos que va a hospedar SSISDB. 

   d. En **Node Size** (Tamaño de nodo), seleccione el tamaño de nodo del clúster del entorno de ejecución de integración. Se muestran solo los tamaños de nodo admitidos. Seleccione un tamaño de nodo grande (escalado vertical) si desea ejecutar muchos paquetes de uso intensivo de proceso o de memoria. 

   e. En **Node Number** (Número de nodos), seleccione el número de nodos del clúster del entorno de ejecución de integración. Se muestran solo los números de nodos admitidos. Seleccione un clúster de gran tamaño con muchos nodos (escalado horizontal), si desea ejecutar muchos paquetes en paralelo. 

   f. En **Edition/License** (Edición o licencia), seleccione la edición o licencia de SQL Server para el entorno de ejecución de integración: Standard o Enterprise. Seleccione Enterprise, si desea utilizar las características avanzadas o premium en el entorno de ejecución de integración. 

   g. En **Save Money** (Ahorrar dinero), seleccione la opción Azure Hybrid Benefit (AHB) (Ventaja híbrida de Azure [AHB]) para el entorno de ejecución de integración: Sí o No. Seleccione Sí si desea hacer que su propia licencia de SQL Server con Software Assurance se beneficie de ahorros con el uso híbrido. 

   h. Haga clic en **Next**. 

1. En la página **SQL Settings** (Configuración de SQL), realice los pasos siguientes: 

   ![Configuración de SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. En la casilla **Create SSIS catalog...** (Crear catálogo de SSIS...), seleccione el modelo de implementación de paquetes para ejecutarlo en Azure-SSIS IR: el modelo de implementación de proyectos, donde los paquetes se implementan en la instancia de SSISDB que hospeda el servidor de bases de datos o el modelo de implementación de paquetes, donde los paquetes se implementan en los sistemas de archivos, recursos compartidos de archivos o Azure Files. Si la marca, deberá traer su propio servidor de bases de datos para hospedar la instancia de SSISDB que crearemos y administraremos por usted.
   
   b. En **Subscription** (Suscripción), seleccione la suscripción de Azure que tiene el servidor de bases de datos que va a hospedar SSISDB. 

   c. En **Location** (Ubicación), seleccione la ubicación del servidor de bases de datos que va a hospedar SSISDB. Es recomendable seleccionar la misma ubicación del entorno de ejecución de integración.

   d. En **Catalog Database Server Endpoint** (Punto de conexión del servidor de bases de datos de catálogo), seleccione el punto de conexión de su servidor de bases de datos que va a hospedar SSISDB. En función del servidor de bases de datos seleccionado, la SSISDB puede crearse en su nombre como base de datos única, como parte de un grupo elástico o en una instancia administrada y estar accesible desde la red pública o mediante la unión a una red virtual. Para directrices sobre cómo elegir el tipo de servidor de bases de datos para hospedar la instancia de SSISDB, consulte [Comparación entre base de datos únicas o grupos elásticos de SQL Database e Instancia administrada de SQL Database](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Si selecciona el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado para hospedar SSISDB o si necesita acceder a los datos locales sin configurar IR autohospedado, debe unir la instancia de Azure-SSIS IR a una red virtual. Para ello, consulte el artículo de [Creación de una instancia de Azure-SSIS IR en una red virtual](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   e. En la casilla **Use AAD authentication...** (Usar autenticación de AAD...), seleccione el método de autenticación del servidor de bases de datos para hospedar SSISDB: autenticación de SQL o autenticación de AAD con la identidad administrada para ADF. Si la selecciona, debe agregar la identidad administrada para ADF en un grupo de AAD con permisos de acceso al servidor de bases de datos. Para ello, consulte [Crear una instancia de Azure-SSIS IR con autenticación de AAD](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   f. En **Admin Username** (Nombre de usuario del administrador), escriba el nombre de usuario de la autenticación de SQL del servidor de bases de datos que va a hospedar SSISDB. 

   g. En **Admin Password** (Contraseña del administrador), escriba la contraseña de la autenticación de SQL del servidor de bases de datos que va a hospedar SSISDB. 

   h. En **Catalog Database Service Tier** (Nivel de servicio de bases de datos de catálogo), seleccione el nivel de servicio del servidor de bases de datos para hospedar SSISDB: Nivel Básico, Estándar o Prémium, o el nombre del grupo elástico.

   i. Haga clic en **Test Connection** (Probar conexión) y si esta se realiza correctamente, haga clic en **Next** (Siguiente). 

1. En la página **Advanced Settings** (Configuración avanzada), realice los pasos siguientes: 

   ![Configuración avanzada](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. En **Maximum Parallel Executions Per Node** (Número máximo de ejecuciones en paralelo por nodo), seleccione el número máximo de paquetes que se van a ejecutar simultáneamente por nodo en el clúster del entorno de ejecución de integración. Se muestran solo los números de paquetes admitidos. Seleccione un número bajo si desea usar más de un núcleo para ejecutar un único paquete grande o pesado con un uso intensivo de memoria o procesos. Seleccione un número alto si desea ejecutar uno o varios paquetes pequeños y ligeros en un único núcleo. 

   b. En **Custom Setup Container SAS URI** (URI de SAS del contenedor de configuración personalizada), puede especificar el identificador uniforme de recursos (URI) de la firma de acceso compartido (SAS) del contenedor de Azure Storage Blob en el que se almacenan el script de configuración y sus archivos asociados. Para ello, consulte [Configuración personalizada de Integration Runtime de SSIS de Azure](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

   c. En la casilla **Select a VNet...**  (Seleccionar una red virtual...), seleccione si desea unir el entorno de ejecución de integración a una red virtual. Debe comprobar si usa el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado para hospedar SSISDB o si necesita acceder a los datos locales sin configurar IR autohospedado. Para ello, consulte [Creación de una instancia de Azure-SSIS IR en una red virtual](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   d. En la casilla **Set-up Self-Hosted** (Configurar autohospedado), seleccione si desea configurar el entorno de ejecución de integración autohospedado como proxy para Azure-SSIS IR. Para ello, consulte [Configuración del entorno de ejecución de integración autohospedado como un proxy](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). 

1. Haga clic en **Next**. 

1. En la página **Resumen**, revise todos los valores de aprovisionamiento, marque los vínculos a la documentación recomendada y haga clic en **Finalizar** para empezar la creación del entorno de ejecución de integración. 

   > [!NOTE]
   > Aparte del tiempo de configuración personalizada, este proceso debería completarse en 5 minutos.
   >
   > Si usa SSISDB, el servicio ADF se conectará al servidor de bases de datos para prepararlo. 
   > 
   > Al aprovisionar Azure-SSIS IR, también se instalan el acceso redistribuible y el paquete de característica de Azure para SSIS. Estos componentes proporcionan conectividad a los archivos de Excel y Access, y a diversos orígenes de datos de Azure, además de los ya admitidos por los componentes integrados. También puede instalar componentes adicionales, consulte [Configuración personalizada para Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

1. En la pestaña **Connections** (Conexiones), cambie a **Integration Runtimes**, si es necesario. Seleccione **Refresh** (Actualizar) para actualizar el estado. 

   ![Estado de creación, con el botón "Refresh"](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Use los vínculos de la columna **Actions** (Acciones) para iniciar, detener, iniciar, editar o eliminar la instancia de Integration Runtime. Utilice el último vínculo para ver el código JSON de Integration Runtime. Los botones de edición y eliminación se habilitan únicamente cuando la instancia de IR se detiene. 

   ![Vínculo de la columna "Actions"](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png) 

## <a name="deploy-ssis-packages"></a>Implementación de paquetes de SSIS

Si usa SSISDB, puede implementar los paquetes ahí y ejecutarlos en Azure-SSIS IR con herramientas SSDT o SSMS que se conectan al servidor de bases de datos mediante su punto de conexión del servidor. Para un servidor de Azure SQL Database o Instancia administrada con un punto de conexión público, el formato del punto de conexión del servidor es `<server name>.database.windows.net`/`<server name>.public.<dns prefix>.database.windows.net,3342`, respectivamente. Si no usa SSISDB, puede implementar los paquetes en sistemas de archivos, recursos compartidos de archivos o Azure Files y ejecutarlos en Azure-SSIS IR mediante las utilidades de la línea de comandos `dtinstall`/`dtutil`/`dtexec`. Para más información, consulte [Implementación de paquetes en SSIS](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). En ambos casos, también puede ejecutar los paquetes implementados en Azure-SSIS IR con actividad de ejecución de paquetes de SSIS en las canalizaciones de ADF, consulte [Invocación de la ejecución de paquetes en SSIS como actividad de ADF de primera clase](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity). 

Consulte también los siguientes artículos de la documentación de SSIS: 

- [Implementación, ejecución y supervisión de paquetes de SSIS en Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Conexión a SSISDB en Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Programación de la ejecución de paquetes en Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Conexión a orígenes de datos locales con autenticación de Windows) 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente: 

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Aprovisionamiento de una instancia de Integration Runtime de SSIS en Azure.
> * Implementación de paquetes de SSIS

Para obtener información acerca de la personalización del entorno de ejecución de integración de SSIS de Azure, vaya al siguiente artículo: 

> [!div class="nextstepaction"]
> [Personalización de Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)