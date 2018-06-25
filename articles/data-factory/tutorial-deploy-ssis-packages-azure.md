---
title: Aprovisionamiento de un entorno de ejecución para la integración de SSIS en Azure | Microsoft Docs
description: Aprenda a aprovisionar Integration Runtime de Azure SSIS en Azure Data Factory para que pueda implementar y ejecutar paquetes SSIS en Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: hero-article
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 7633ebb4c05e4298574cad182406cead52a55edf
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36267666"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Aprovisionamiento de Integration Runtime de Azure SSIS en Azure Data Factory
En este tutorial se describen los pasos necesarios para usar Azure Portal para aprovisionar Integration Runtime de SSIS de Azure en Azure Data Factory. Posteriormente, podrá usar SQL Server Data Tools o SQL Server Management Studio para implementar y ejecutar paquetes de SQL Server Integration Services (SSIS) en este entorno de ejecución de Azure. Para obtener información conceptual acerca de Integration Runtime para la integración de SSIS en Azure, consulte [Introducción a Integration Runtime de SSIS de Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime).

En este tutorial, va a completar los siguientes pasos:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Aprovisionamiento de una instancia de Integration Runtime de SSIS en Azure.

> [!NOTE]
> Este artículo se aplica a la versión 2 de Data Factory, que actualmente se encuentra en versión preliminar. Si usa la versión 1 del servicio Data Factory, que está disponible con carácter general, consulte la [documentación de esta versión de Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>requisitos previos
- **Suscripción de Azure**. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar. 
- **Servidor de Azure SQL Database**. Si aún no tiene ningún servidor de base de datos, cree uno en Azure Portal antes de empezar a trabajar. Azure Data Factory crea la base de datos del catálogo de SSIS (SSISDB) en este servidor de bases de datos. Le recomendamos que cree el servidor de base de datos en la misma región de Azure que la instancia de Integration Runtime. Esta configuración permite que la instancia de Integration Runtime escriba registros de ejecución en la base de datos SSISDB sin traspasar las regiones de Azure. 
- Según el servidor de bases de datos seleccionado, SSISDB se puede crear en su nombre como base de datos independiente, como parte de un grupo elástico o en una instancia administrada (versión preliminar) y accesible desde la red pública, o mediante la unión a una red virtual. Si usa Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada (versión preliminar) para hospedar SSISDB o requiere acceso a datos locales, deberá unir el entorno de ejecución para la integración de SSIS en Azure a una red virtual. Para ello, consulte [Crear una instancia de Integration Runtime de Azure SSIS en Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).
- Confirme que el valor **Permitir el acceso a servicios de Azure** está habilitado para el servidor de bases de datos. Esto no es aplicable si se utiliza Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada (versión preliminar) para hospedar SSISDB. Para más información, consulte [Protección de Azure SQL Database](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Para habilitar este valor mediante PowerShell, consulte [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
- Agregue la dirección IP de la máquina cliente o un intervalo de direcciones IP que incluya la dirección IP de la máquina cliente en la lista de direcciones IP de cliente en la configuración del firewall del servidor de bases de datos. Para más información, consulte [Reglas de firewall de nivel de base de datos y de nivel de servidor de Azure SQL Database](../sql-database/sql-database-firewall-configure.md).
- Puede conectarse al servidor de bases de datos mediante la autenticación de SQL con sus credenciales de administrador del servidor, o la autenticación de Azure Active Directory (AAD) con Managed Service Identity (MSI) de Azure Data Factory (ADF).  En este último caso, debe agregar MSI de ADF en un grupo de AAD con permisos de acceso al servidor de bases de datos. Para ello, consulte [Crear una instancia de Integration Runtime de Azure SSIS en Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).
- Confirme que el servidor de Azure SQL Database no tiene un catálogo de SSIS (base de datos SSIDB). El aprovisionamiento de una instancia de Integration Runtime para la integración de SSIS en Azure no admite el uso de un catálogo de SSIS existente.

> [!NOTE]
> - Puede crear una factoría de datos de la versión 2 en las siguientes regiones: Este de EE. UU., Este de EE. UU. 2, Sudeste Asiático y Europa Occidental. 
> - Puede crear una instancia del entorno de ejecución de integración de Azure-SSIS en las siguientes regiones: Este de EE. UU., Este de EE. UU. 2, Centro de EE. UU., Oeste de EE. UU. 2, Europa del Norte, Europa Occidental, Sur de Reino Unido y Este de Australia. 

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. Inicie el explorador web **Microsoft Edge** o **Google Chrome**. Actualmente, la interfaz de usuario de Data Factory solo se admite en los exploradores web Microsoft Edge y Google Chrome.
2. Inicie sesión en el [Azure Portal](https://portal.azure.com/).    
3. Seleccione **Nuevo** en el menú de la izquierda, seleccione **Datos y análisis** y, después, seleccione **Data Factory**. 
   
   ![Selección de la factoría de datos en el panel Nuevo](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. En la página **Nueva factoría de datos**, escriba **MyAzureSsisDataFactory** en **Nombre**. 
      
   ![Página de la nueva factoría de datos](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   El nombre de Azure Data Factory debe ser *único de forma global*. Si recibe el siguiente error, cambie el nombre de la factoría de datos (por ejemplo, **&lt;sunombre&gt;MyAzureSsisDataFactory**) e intente crearla de nuevo. Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte el artículo [Azure Data Factory: reglas de nomenclatura](naming-rules.md).
  
   `Data factory name “MyAzureSsisDataFactory” is not available`
4. En **Suscripción**, seleccione la suscripción de Azure donde desea crear la factoría de datos. 
5. Para el **grupo de recursos**, realice uno de los siguientes pasos:
     
   - Seleccione en primer lugar **Usar existente** y después un grupo de recursos de la lista. 
   - Seleccione **Crear nuevo**y escriba el nombre de un grupo de recursos.   
         
   Para obtener más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/resource-group-overview.md).  
6. En **Versión**, seleccione **V2 (versión preliminar)**.
7. En **Ubicación**, seleccione la ubicación de la factoría de datos. En la lista solo se muestran las ubicaciones que se admiten para la creación de factorías de datos.
8. Seleccione **Anclar al panel**.     
9. Seleccione **Crear**.
10. En el panel, verá el icono siguiente con el estado **Deploying data factory** (Implementando factoría de datos): 

   ![Icono "Deploying Data Factory" (Implementando Data Factory)](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
11. Una vez completada la creación, verá la página **Data Factory**.
   
   ![Página principal de la factoría de datos](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
12. Haga clic en **Author & Monitor** (Creación y supervisión) para abrir la interfaz de usuario de Data Factory en una pestaña independiente. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Aprovisionamiento de una instancia de Integration Runtime para la integración de SSIS en Azure

1. En la página **Let's get started** (Comencemos), seleccione el icono **Configure SSIS Integration Runtime** (Configuración de Integration Runtime de SSIS). 

   ![Icono "Configure SSIS Integration Runtime" (Configuración de Integration Runtime de SSIS)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. En la página **General Settings** (Configuración general) de **Integration Runtime Setup** (Configuración de Integration Runtime), realice los pasos siguientes: 

   ![Configuración general](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. En **Nombre**, escriba el nombre de su entorno de ejecución de integración.
   
   b. En **Descripción**, escriba la descripción del entorno de ejecución de integración.

   c. En **Ubicación**, seleccione la ubicación para el entorno de ejecución de integración. Se muestran solo las ubicaciones admitidas. Le recomendamos que seleccione la misma ubicación de su servidor de bases de datos para hospedar SSISDB.

   d. En **Tamaño del nodo**, seleccione el tamaño del nodo en el clúster del entorno de ejecución de integración. Se muestran solo los tamaños de nodo admitidos. Seleccione un tamaño de nodo grande (escalado vertical) si desea ejecutar muchos paquetes de proceso o de uso intensivo de memoria.

   e. En **Número de nodos**, seleccione el número de nodos en el clúster del entorno de ejecución de integración. Se muestran solo los números de nodos admitidos. Seleccione un clúster de gran tamaño con muchos nodos (escalado horizontal), si desea ejecutar muchos paquetes en paralelo.
   
   f. En **Edición o licencia**, seleccione la edición o licencia de SQL Server para el entorno de ejecución de integración: Estándar o Enterprise. Seleccione Enterprise, si desea utilizar las características avanzadas o premium en el entorno de ejecución de integración.
   
   g. En **Ahorrar dinero**, seleccione el valor de la opción Ventaja híbrida de Azure (AHB) para el entorno de ejecución de integración: Sí o No. Seleccione Sí si desea hacer que su propia licencia de SQL Server con Software Assurance se beneficie de ahorros con el uso híbrido.
   
   h. Haga clic en **Next**. 
3. En la página **SQL Settings** (Configuración de SQL), realice los pasos siguientes: 

   ![Configuración de SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. En **Suscripción**, seleccione la suscripción de Azure que tiene el servidor de bases de datos para hospedar SSISDB.

   b. En **Ubicación**, seleccione la ubicación de su servidor de bases de datos para hospedar SSISDB. Es recomendable seleccionar la misma ubicación del entorno de ejecución de integración.
   
   c. En **Catalog Database Server Endpoint** (Punto de conexión del servidor de bases de datos de catálogo), seleccione el punto de conexión de su servidor de bases de datos para hospedar SSISDB. Según el servidor de bases de datos seleccionado, SSISDB se puede crear en su nombre como base de datos independiente, como parte de un grupo elástico o en una instancia administrada (versión preliminar) y accesible desde la red pública, o mediante la unión a una red virtual. Si selecciona Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada (versión preliminar) para hospedar SSISDB o requiere acceso a datos locales, deberá unir el entorno de ejecución para la integración de SSIS en Azure a una red virtual. Para ello, consulte [Crear una instancia de Integration Runtime de Azure SSIS en Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).

   d. En la casilla **Use AAD authentication...** (Usar autenticación de AAD...), seleccione el método de autenticación del servidor de bases de datos para hospedar SSISDB: SQL o Azure Active Directory (AAD) con Managed Service Identity (MSI) de Azure Data Factory (ADF). Si lo hace, deberá agregar MSI de ADF en un grupo de AAD con permisos de acceso al servidor de bases de datos. Para ello, consulte [Crear una instancia de Integration Runtime de Azure SSIS en Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).
   
   e. En **Nombre de usuario del administrador**, escriba el nombre de usuario de la autenticación de SQL del servidor de bases de datos para hospedar SSISDB.

   f. En **Contraseña del administrador**, escriba la contraseña de la autenticación de SQL del servidor de bases de datos para hospedar SSISDB.

   g. En **Catalog Database Service Tier** (Nivel de servicio de base de datos de catálogo), seleccione el nivel de servicio del servidor de bases de datos para hospedar SSISDB: Básico, Estándar o Premium o el nombre del grupo elástico.

   h. Haga clic en **Probar conexión** y si esta se realiza correctamente, haga clic en **Siguiente**. 
4. En la página **Configuración avanzada**, realice los pasos siguientes: 

   ![Configuración avanzada](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
   
   a. En **Maximum Parallel Executions Per Node** (Número máximo de ejecuciones en paralelo por nodo), seleccione el número máximo de paquetes que se van a ejecutar simultáneamente por nodo en el clúster del entorno de ejecución de integración. Se muestran solo los números de paquetes admitidos. Seleccione un número bajo si desea usar más de un núcleo para ejecutar un único paquete grande o pesado con un uso intensivo de memoria o procesos. Seleccione un número alto si desea ejecutar uno o varios paquetes pequeños y ligeros en un único núcleo.

   b. En **Custom Setup Container SAS URI** (URI de SAS del contenedor de configuración personalizada), puede especificar el identificador uniforme de recursos (URI) de la firma de acceso compartido (SAS) del contenedor de Azure Storage Blob en el que se almacenan el script de configuración y sus archivos asociados. Para ello, consulte [Configuración personalizada del entorno de ejecución de integración de Azure-SSIS](https://docs.microsoft.com/en-us/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).
 
   c. En la casilla **Select a VNet...**  (Seleccionar una red virtual...), seleccione si desea unir el entorno de ejecución de integración a una red virtual. Debe marcar esta casilla si usa Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada (versión preliminar) para hospedar SSISDB o requiere acceso a datos locales. Para ello, consulte [Crear una instancia de Integration Runtime de Azure SSIS en Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).
5. Haga clic en **Finalizar** para iniciar la creación del entorno de ejecución de integración. 

   > [!IMPORTANT]
   > Este proceso tarda aproximadamente veinte minutos.
   >
   > El servicio Data Factory se conecta al servidor de bases de datos de Azure SQL para preparar el catálogo de SSIS (base de datos SSISDB). 
   > 
   > Al aprovisionar una instancia de Integration Runtime para la integración de SSIS en Azure, también se instala el paquete de características de Azure para SSIS y el acceso redistribuible. Estos componentes proporcionan conectividad a los archivos de Excel y Access, y a diversos orígenes de datos de Azure, además de los admitidos por los componentes integrados. También puede instalar componentes adicionales. Para más información, consulte [Custom setup for the Azure-SSIS integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md) (Instalación personalizada de la instancia de Integration Runtime para la integración de SSIS en Azure).

6. En la pestaña **Connections** (Conexiones), cambie a **Integration Runtimes**, si es necesario. Seleccione **Refresh** (Actualizar) para actualizar el estado. 

   ![Estado de creación, con el botón "Refresh"](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
7. Use los vínculos de la columna **Actions** (Acciones) para iniciar, detener, iniciar, editar o eliminar la instancia de Integration Runtime. Utilice el último vínculo para ver el código JSON de Integration Runtime. Los botones de edición y eliminación se habilitan únicamente cuando la instancia de IR se detiene. 

   ![Vínculo de la columna "Actions"](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        

## <a name="create-an-azure-ssis-integration-runtime"></a>Creación de una instancia de Integration Runtime de SSIS de Azure

1. En la interfaz de usuario de Azure Data Factory, cambie a la pestaña **Edit** (Editar), seleccione **Connections** (Conexiones) y cambie a la pestaña **Integration Runtime** para ver las instancias de Integration Runtime existentes en la factoría de datos. 
   ![Opciones para ver las instancias de Integration Runtime existentes](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
2. Seleccione **New** (Nuevo) para crear una instancia de Integration Runtime para la integración de SSIS en Azure. 

   ![Integration Runtime a través del menú](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
3. En la ventana **Integration Runtime Setup** (Instalación de Integration Runtime), seleccione **Lift-and-shift existing SSIS packages to execute in Azure** (Migrar mediante lift-and-shift los paquetes de SSIS existentes para ejecutarlos en Azure) y, después, seleccione **Siguiente**.

   ![Especificación del tipo de instancia de Integration Runtime](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Para el resto de pasos de configuración de Integration Runtime para la integración de SSIS en Azure, consulte la sección [Aprovisionamiento de Integration Runtime de SSIS de Azure](#provision-an-azure-ssis-integration-runtime). 

 
## <a name="deploy-ssis-packages"></a>Implementación de paquetes SSIS
Luego, use SQL Server Data Tools (SSDT) o SQL Server Management Studio (SSMS) para implementar los paquetes de SSIS en Azure. Conéctese al servidor de Azure SQL Database que hospeda el catálogo de SSIS (base de datos SSISDB). El nombre del servidor de Azure SQL Database tiene el formato `<servername>.database.windows.net`. 

Consulte los siguientes artículos de la documentación de SSIS: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (Implementación, ejecución y supervisión de un paquete SSIS en Azure)   
- [Connect to SSIS catalog on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Conexión al catálogo de SSIS en Azure)
- [Schedule package execution on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Programación de la ejecución de un paquete en Azure)
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Conexión a orígenes de datos locales con autenticación de Windows) 

## <a name="next-steps"></a>Pasos siguientes
En este tutorial aprendió lo siguiente: 

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Aprovisionamiento de una instancia de Integration Runtime de SSIS en Azure.

Para obtener información sobre cómo copiar datos desde el entorno local a la nube, continúe con el tutorial siguiente: 

> [!div class="nextstepaction"]
> [Copia de datos en la nube](tutorial-copy-data-portal.md)
