---
title: Creación una instancia de Azure-SSIS Integration Runtime en Azure Data Factory | Microsoft Docs
description: Aprenda a crear una instancia de Azure-SSIS Integration Runtime en Azure Data Factory para que pueda implementar y ejecutar paquetes SSIS en Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 36cb4d306cd74dcde09fa55fc582a043bc324f65
ms.sourcegitcommit: a819209a7c293078ff5377dee266fa76fd20902c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71010029"
---
# <a name="create-azure-ssis-integration-runtime-in-azure-data-factory"></a>Creación de una instancia de Azure-SSIS Integration Runtime en Azure Data Factory

En este tutorial se describen los pasos para aprovisionar una instancia de Azure-SSIS IR en Azure Data Factory (ADF). Azure-SSIS IR admite la ejecución de paquetes implementados en el catálogo de SSIS (SSISDB) hospedados en el servidor de Azure SQL Database o Instancia administrada (modelo de implementación de proyectos) y los implementados en sistemas de archivos, recursos compartidos de archivos o Azure Files (modelo de implementación de paquetes). Una vez aprovisionado Azure-SSIS IR, puede usar herramientas familiares, como SQL Server Data Tools (SSDT)/ SQL Server Management Studio (SSMS) y utilidades de la línea de comandos, como `dtinstall`/`dtutil`/`dtexec`, para implementar y ejecutar los paquetes en Azure.

En el [Tutorial: Aprovisionamiento de Azure-SSIS IR](tutorial-create-azure-ssis-runtime-portal.md) se explica cómo crear una instancia de Azure-SSIS IR mediante Azure Portal o Azure Data Factory y cómo usar, opcionalmente, un servidor de Azure SQL Database o una instancia administrada para hospedar SSISDB. Este artículo permite ampliar el tutorial y muestra cómo hacer lo siguiente:

- Opcionalmente, utilice el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado para hospedar SSISDB. Como requisito previo, tiene que configurar los permisos y opciones de la red virtual para que la instancia de Azure-SSIS IR se una a una red virtual.

- De manera opcional, puede usar la autenticación de Azure Active Directory (AAD) con la identidad administrada de ADF para conectarse al servidor de Azure SQL Database o la instancia administrada. Como requisito previo, deberá agregar la identidad administrada de ADF como un usuario de base de datos capaz de crear instancias de SSISDB.

- Opcionalmente, una la instancia de Azure-SSIS IR a una red virtual o configure IR autohospedado como un proxy para que la instancia de Azure-SSIS IR tenga acceso a datos locales.

## <a name="overview"></a>Información general

En este artículo se muestran diferentes maneras de aprovisionar Azure-SSIS IR:

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Plantilla de Azure Resource Manager](#azure-resource-manager-template)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Suscripción de Azure**. Si no tiene una suscripción, puede crear una [cuenta de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
- **Servidor de Azure SQL Database/Instancia administrada de Azure SQL Database (opcional)** . Si aún no tiene un servidor de bases de datos, cree uno en Azure Portal antes de empezar. A su vez, ADF creará la instancia de SSISDB en este servidor de bases de datos. Le recomendamos que cree el servidor de bases de datos en la misma región de Azure que el entorno de ejecución de integración. Esta configuración permite que el entorno de ejecución de integración escriba registros de ejecución en SSISDB sin traspasar regiones de Azure. 
    - En función del servidor de bases de datos seleccionado, la SSISDB puede crearse en su nombre como base de datos única, como parte de un grupo elástico o en una instancia administrada y estar accesible desde la red pública o mediante la unión a una red virtual. Para directrices sobre cómo elegir el tipo de servidor de bases de datos para hospedar la instancia de SSISDB, consulte [Comparación entre base de datos únicas o grupos elásticos de SQL Database e Instancia administrada de SQL Database](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Si usa el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado para hospedar SSISDB o si necesita acceder a los datos locales sin configurar IR autohospedado, debe unir la instancia de Azure-SSIS IR a una red virtual. Para ello, consulte el artículo de [Unión de la instancia de Azure-SSIS IR a una red virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).
    - Confirme que el valor **Permitir el acceso a servicios de Azure** está habilitado para el servidor de bases de datos. Esto no es aplicable si se utiliza el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado para hospedar SSISDB. Para más información, consulte [Protección de Azure SQL Database](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Para habilitar este valor mediante PowerShell, consulte [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Agregue la dirección IP de la máquina cliente o un intervalo de direcciones IP que incluya la dirección IP de la máquina cliente en la lista de direcciones IP de cliente en la configuración del firewall del servidor de bases de datos. Para más información, consulte [Reglas de firewall de nivel de base de datos y de nivel de servidor de Azure SQL Database](../sql-database/sql-database-firewall-configure.md).
    - Puede conectarse al servidor de bases de datos mediante la autenticación de SQL con sus credenciales de administrador del servidor o por medio de la autenticación de Azure Active Directory (AAD) con la identidad administrada de ADF.  En el último de los casos, debe agregar la identidad administrada de su ADF a un grupo de AAD con permisos de acceso al servidor de base de datos; para ello, consulte [Habilitación de la autenticación de AAD para Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir).
    - Confirme que el servidor de bases de datos no tiene aún una instancia de SSISDB. El aprovisionamiento de Azure-SSIS IR no admite el uso de una instancia de SSISDB existente.
- **Red virtual de Azure Resource Manager (opcional)** . Debe tener una red virtual de Azure Resource Manager si se cumple al menos una de las siguientes condiciones:
    - Va a hospedar SSISDB en un servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado.
    - Desea conectarse a almacenes de datos locales desde paquetes SSIS que se ejecutan en una instancia de Azure-SSIS IR sin configurar IR autohospedado.
- **Azure PowerShell (opcional)** . Siga las instrucciones que se indican en [Instalación y configuración de Azure PowerShell](/powershell/azure/install-az-ps), si quiere ejecutar un script de PowerShell para aprovisionar Azure-SSIS IR.

### <a name="region-support"></a>Regiones admitidas

Para ver una lista de regiones de Azure en las que ADF y Azure-SSIS IR están actualmente disponibles, consulte [Disponibilidad de ADF + SSIS IR por región](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all).

### <a name="compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance"></a>Comparación entre base de datos únicas o grupos elásticos de SQL Database e Instancia administrada de SQL Database

En la tabla siguiente se comparan determinadas características del servidor y la instancia administrada de Azure SQL Database y su relación con Azure SSIR IR:

| Característica | base de datos única / grupo elástico| Instancia administrada |
|---------|--------------|------------------|
| **Programación** | El agente SQL Server no está disponible.<br/><br/>Consulte [Programar una ejecución de paquetes en una canalización de ADF](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages?view=sql-server-2017#activity).| El agente de Instancia administrada está disponible. |
| **Autenticación** | Puede crear SSISDB con un usuario de base de datos independiente que represente cualquier grupo de AAD con la identidad administrada de ADF como miembro del rol **db_owner**.<br/><br/>Consulte [Habilitación de la autenticación de Azure AD para crear SSISDB en el servidor de Azure SQL Database](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | Puede crear SSISDB con un usuario de base de datos independiente que represente la identidad administrada de ADF. <br/><br/>Consulte [Habilitación de la autenticación de Azure AD para crear SSISDB en Instancia administrada de Azure SQL Database](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Nivel de servicio** | Al crear una instancia de Azure-SSIS IR con el servidor de Azure SQL Database, puede seleccionar el nivel de servicio de SSISDB. Hay varios niveles de servicio. | Cuando crea una instancia de Azure-SSIS IR con su instancia administrada, no puede seleccionar el nivel de servicio de SSISDB. Todas las bases de datos de la instancia administrada comparten el mismo recurso asignado a esa instancia. |
| **Red virtual** | Solo admite la unión de Azure-SSIS IR a redes virtuales de Azure Resource Manager si usa el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o si necesita acceso a almacenes de datos locales sin configurar IR autohospedado. | Solo admite la unión de Azure-SSIS IR a redes virtuales de Azure Resource Manager. La red virtual es necesaria cuando no se habilita un punto de conexión público para la Instancia administrada.<br/><br/>Si une su instancia de Azure-SSIS IR a la misma red virtual que su instancia administrada, asegúrese de que ambas instancias se encuentran en subredes diferentes. Si une la instancia de Azure-SSIS IR a una red virtual diferente de aquella de la instancia administrada, se recomienda realizar un emparejamiento de redes virtuales o una conexión de red virtual a red virtual. Consulte [Conexión de la aplicación a Instancia administrada de Azure SQL Database](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Transacciones distribuidas** | Compatible mediante transacciones distribuidas. No se admiten las transacciones del Coordinador de transacciones distribuidas de Microsoft (MSDTC). Si los paquetes SSIS usan MSDTC para coordinar las transacciones distribuidas, considere la posibilidad de migrar a transacciones elásticas de Azure SQL Database. Para más información, consulte [Transacciones distribuidas en bases de datos en la nube](../sql-database/sql-database-elastic-transactions-overview.md). | No compatible. |
| | | |

## <a name="azure-portal"></a>Portal de Azure

En esta sección, usará Azure Portal, en concreto, la interfaz de usuario de ADF, para crear Azure-SSIS IR.

### <a name="create-a-data-factory"></a>Crear una factoría de datos

Para crear una instancia de Azure Data Factory mediante Azure Portal, siga las instrucciones paso a paso del artículo [Creación de una instancia de Azure Data Factory mediante la interfaz de usuario](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory) y seleccione **Anclar al panel** mientras lo hace para permitir el acceso rápido después de su creación. 

Una vez creada la instancia de Azure Data Factory, abra su página de información general en Azure Portal y haga clic en el icono de **creación y supervisión** para iniciar la página **Comencemos** en una pestaña independiente en la que pueda continuar creando la instancia de Azure-SSIS Integration Runtime.   

### <a name="provision-an-azure-ssis-integration-runtime"></a>Aprovisionamiento de Integration Runtime de SSIS de Azure

1. En la página **Comencemos**, haga clic en el icono **Configure SSIS Integration Runtime** (Configuración de Integration Runtime de SSIS).

   ![Icono Configure SSIS Integration Runtime (Configuración de Integration Runtime de SSIS)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

2. En la página **General Settings** (Configuración general) de **Integration Runtime Setup** (Configuración de Integration Runtime), realice los pasos siguientes:

   ![Configuración general](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. En **Name** (Nombre), escriba el nombre del entorno de ejecución de integración.

    b. En **Description** (Descripción), escriba la descripción del entorno de ejecución de integración.

    c. En **Location** (Ubicación), seleccione la ubicación del entorno de ejecución de integración. Se muestran solo las ubicaciones admitidas. Le recomendamos que seleccione la misma ubicación del servidor de bases de datos que va a hospedar SSISDB.

    d. En **Node Size** (Tamaño de nodo), seleccione el tamaño de nodo del clúster del entorno de ejecución de integración. Se muestran solo los tamaños de nodo admitidos. Seleccione un tamaño de nodo grande (escalado vertical) si desea ejecutar muchos paquetes de uso intensivo de proceso o de memoria.

    e. En **Node Number** (Número de nodos), seleccione el número de nodos del clúster del entorno de ejecución de integración. Se muestran solo los números de nodos admitidos. Seleccione un clúster de gran tamaño con muchos nodos (escalado horizontal), si desea ejecutar muchos paquetes en paralelo.

    f. En **Edition/License** (Edición o licencia), seleccione la edición o licencia de SQL Server para el entorno de ejecución de integración: Standard o Enterprise. Seleccione Enterprise, si desea utilizar las características avanzadas o premium en el entorno de ejecución de integración.

    g. En **Save Money** (Ahorrar dinero), seleccione la opción Azure Hybrid Benefit (AHB) (Ventaja híbrida de Azure [AHB]) para el entorno de ejecución de integración: Sí o No. Seleccione Sí si desea hacer que su propia licencia de SQL Server con Software Assurance se beneficie de ahorros con el uso híbrido.

    h. Haga clic en **Next**.

3. En la página **SQL Settings** (Configuración de SQL), realice los pasos siguientes:

   ![Configuración de SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. En la casilla **Create SSIS catalog...** (Crear catálogo de SSIS...), seleccione el modelo de implementación de paquetes para ejecutarlo en Azure-SSIS IR: el modelo de implementación de proyectos, donde los paquetes se implementan en la instancia de SSISDB que hospeda el servidor de bases de datos o el modelo de implementación de paquetes, donde los paquetes se implementan en los sistemas de archivos, recursos compartidos de archivos o Azure Files. Si la marca, deberá traer su propio servidor de bases de datos para hospedar la instancia de SSISDB que crearemos y administraremos por usted.
   
    b. En **Subscription** (Suscripción), seleccione la suscripción de Azure que tiene el servidor de bases de datos que va a hospedar SSISDB. 

    c. En **Location** (Ubicación), seleccione la ubicación del servidor de bases de datos que va a hospedar SSISDB. Es recomendable seleccionar la misma ubicación del entorno de ejecución de integración. 

    d. En **Catalog Database Server Endpoint** (Punto de conexión del servidor de bases de datos de catálogo), seleccione el punto de conexión de su servidor de bases de datos que va a hospedar SSISDB. En función del servidor de bases de datos seleccionado, la SSISDB puede crearse en su nombre como base de datos única, como parte de un grupo elástico o en una instancia administrada y estar accesible desde la red pública o mediante la unión a una red virtual. Para directrices sobre cómo elegir el tipo de servidor de bases de datos para hospedar la instancia de SSISDB, consulte [Comparación entre base de datos únicas o grupos elásticos de SQL Database e Instancia administrada de SQL Database](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Si selecciona el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado para hospedar SSISDB o si necesita acceder a los datos locales sin configurar IR autohospedado, debe unir la instancia de Azure-SSIS IR a una red virtual. Para ello, consulte el artículo de [Unión de la instancia de Azure-SSIS IR a una red virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

    e. En la casilla **Use AAD authentication...** (Usar autenticación de AAD...), seleccione el método de autenticación del servidor de bases de datos para hospedar SSISDB: autenticación de SQL o autenticación de AAD con la identidad administrada para ADF. Si la activa, debe agregar la identidad administrada de su ADF a un grupo de AAD con permisos de acceso al servidor de base de datos; para ello, consulte [Habilitación de la autenticación de AAD para Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

    f. En **Admin Username** (Nombre de usuario del administrador), escriba el nombre de usuario de la autenticación de SQL del servidor de bases de datos que va a hospedar SSISDB. 

    g. En **Admin Password** (Contraseña del administrador), escriba la contraseña de la autenticación de SQL del servidor de bases de datos que va a hospedar SSISDB. 

    h. En **Catalog Database Service Tier** (Nivel de servicio de bases de datos de catálogo), seleccione el nivel de servicio del servidor de bases de datos para hospedar SSISDB: Nivel Básico, Estándar o Prémium, o el nombre del grupo elástico. 

    i. Haga clic en **Test Connection** (Probar conexión) y si esta se realiza correctamente, haga clic en **Next** (Siguiente). 

4. En la página **Advanced Settings** (Configuración avanzada), realice los pasos siguientes:

    ![Configuración avanzada](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. En **Maximum Parallel Executions Per Node** (Número máximo de ejecuciones en paralelo por nodo), seleccione el número máximo de paquetes que se van a ejecutar simultáneamente por nodo en el clúster del entorno de ejecución de integración. Se muestran solo los números de paquetes admitidos. Seleccione un número bajo si desea usar más de un núcleo para ejecutar un único paquete grande o pesado con un uso intensivo de memoria o procesos. Seleccione un número alto si desea ejecutar uno o varios paquetes pequeños y ligeros en un único núcleo.

    b. En **Custom Setup Container SAS URI** (URI de SAS del contenedor de configuración personalizada), puede especificar el identificador uniforme de recursos (URI) de la firma de acceso compartido (SAS) del contenedor de Azure Storage Blob en el que se almacenan el script de configuración y sus archivos asociados. Para ello, consulte [Configuración personalizada de Integration Runtime de SSIS de Azure](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

5. En la casilla **Select a virtual network...**  (Seleccionar una red virtual...), seleccione si desea unir el entorno de ejecución de integración a una red virtual. Active la casilla si usa un servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado para hospedar SSISDB o si necesita acceder a los datos locales; es decir, tiene orígenes o destinos de datos locales en los paquetes de SSIS, sin configurar IR autohospedado. Puede consultar [Unión de una instancia de Azure-SSIS IR a una red virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Si activa la casilla, realice los pasos siguientes:

   ![Configuración avanzada con una red virtual](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. En **Subscription** (Suscripción), seleccione la suscripción de Azure que tiene la red virtual.

    b. En **Location** (Ubicación), está seleccionada la misma ubicación del entorno de ejecución de integración.

    c. En **Type** (Tipo), seleccione el tipo de la red virtual: clásica o Azure Resource Manager. Se recomienda que seleccione red virtual de Azure Resource Manager, puesto que la red virtual clásica dejará de utilizarse pronto.

    d. En **VNet Name** (Nombre de red virtual), seleccione el nombre de la red virtual. Esta red virtual debe ser la misma que se utiliza para el servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o instancia administrada en una red virtual para hospedar SSISDB o la que está conectada a la red del entorno local.

    e. En **Subnet Name** (Nombre de subred), seleccione el nombre de la subred en la red virtual. Esta red virtual debe ser una subred diferente a la utilizada por Instancia administrada en una red virtual para hospedar SSISDB.

6. En la casilla **Set-up Self-Hosted** (Configurar autohospedado), seleccione si desea configurar el entorno de ejecución de integración autohospedado como proxy para Azure-SSIS IR. Para ello, consulte [Configuración del entorno de ejecución de integración autohospedado como un proxy](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). Si activa la casilla, realice los pasos siguientes:

   ![Configuración avanzada con IR autohospedado](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

    a. En **Self-Hosted Integration Runtime** (Integration Runtime autohospedado), seleccione la instancia de IR autohospedado existente como proxy para Azure-SSIS IR.

    b. En **Staging Storage Linked Service** (Servicio vinculado de almacenamiento provisional), seleccione el servicio vinculado de Azure Blob Storage existente o cree uno nuevo para el almacenamiento provisional.

    c. En **Staging Path** (Ruta de acceso provisional), especifique un contenedor de blobs en la instancia seleccionada de Azure Blob Storage o deje este campo vacío para usar uno predeterminado para el almacenamiento provisional.

7. Haga clic en **VNet Validation** (Validación de red virtual) y en **Siguiente**. 

8. En la página **Resumen**, revise todos los valores de aprovisionamiento, marque los vínculos a la documentación recomendada y haga clic en **Finalizar** para empezar la creación del entorno de ejecución de integración.

    > [!NOTE]
    > Sin incluir el tiempo de configuración personalizada, este proceso debería completarse en 5 minutos, pero puede que tarde aproximadamente de 20 a 30 minutos en unir Azure-SSIS IR a una red virtual.
    >
    > Si usa SSISDB, el servicio ADF se conectará al servidor de bases de datos para prepararlo. También configura los permisos o los valores de la red virtual, si se especifica, y une la instancia de Azure-SSIS IR a la red virtual.
    > 
    > Al aprovisionar Azure-SSIS IR, también se instalan el acceso redistribuible y el paquete de característica de Azure para SSIS. Estos componentes proporcionan conectividad a los archivos de Excel y Access, y a diversos orígenes de datos de Azure, además de los ya admitidos por los componentes integrados. También puede instalar componentes adicionales, consulte [Configuración personalizada para Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

7. En la ventana **Connections**, cambie a **Integration Runtime** si es necesario. Haga clic en **Refresh** (Actualizar) para actualizar el estado.

   ![Estado de la creación](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

8. Use los vínculos de la columna **Acciones** para supervisar, detener, iniciar, editar o eliminar el entorno de ejecución de integración. Utilice el último vínculo para ver el código JSON de Integration Runtime. Los botones de edición y eliminación se habilitan únicamente cuando la instancia de IR se detiene.

   ![IR de SSIS de Azure: acciones](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Instancias de Integration Runtime de SSIS de Azure en el portal

1. En la interfaz de usuario de Azure Data Factory, cambie a la pestaña **Edit** (Editar), haga clic en **Connections** (Conexiones) y cambie a la pestaña **Integration Runtime** para ver las instancias de Integration Runtime existentes en la factoría de datos.

   ![Visualización de las instancias de IR existentes](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

2. Haga clic en **New** (Nuevo) para crear una instancia de IR de SSIS de Azure.

   ![Integration Runtime a través del menú](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

3. Para crear una instancia de Integration Runtime de SSIS de Azure, haga clic en **New** (Nuevo) tal como se muestra en la imagen.

4. En la ventana de configuración de Integration Runtime, seleccione **Lift-and-shift existing SSIS packages to execute in Azure** (Migrar los paquetes de SSIS existentes mediante lift-and-shift para la ejecución en Azure) y haga clic en **Next** (Siguiente).

   ![Especificación del tipo de instancia de Integration Runtime](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

5. Consulte la sección [Aprovisionamiento de Integration Runtime de SSIS de Azure](#provision-an-azure-ssis-integration-runtime) para el resto de pasos de configuración de IR de SSIS de Azure.

## <a name="azure-powershell"></a>Azure PowerShell

En esta sección, se usa Azure PowerShell para crear un entorno de ejecución de SSIS en Azure.

### <a name="create-variables"></a>Creación de variables

Copie y pegue el siguiente script. Especifique los valores de las variables. 

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance with a private endpoint/on-premises data without configuring Self-Hosted IR, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance with a private endpoint

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"
```

### <a name="sign-in-and-select-subscription"></a>Inicio de sesión y selección de la suscripción

Agregue el código siguiente al script para iniciar sesión y seleccione la suscripción de Azure:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database-server"></a>Validación de la conexión al servidor de bases de datos

Agregue el siguiente script para validar el servidor de Azure SQL Database o la instancia administrada.

```powershell
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}
```

### <a name="configure-virtual-network"></a>Configurar la red virtual

Agregue el siguiente script para configurar automáticamente los permisos o la configuración de la red virtual para unir la instancia de Integration Runtime de SSIS de Azure.

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un [grupo de recursos de Azure](../azure-resource-manager/resource-group-overview.md) con el comando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Un grupo de recursos es un contenedor lógico en el que se implementan y se administran recursos de Azure como un grupo.

Si ya existe el grupo de recursos, no copie este código en el script. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Crear una factoría de datos

Ejecute el comando siguiente para crear una factoría de datos.

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Creación de una instancia de Integration Runtime

Ejecute los siguientes comandos para crear una instancia de Integration Runtime de SSIS de Azure que ejecute paquetes de SSIS en Azure.

Si no usa SSISDB, puede omitir los parámetros CatalogServerEndpoint, CatalogPricingTier y CatalogAdminCredential.

Si no utiliza un servidor de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada con un punto de conexión privado para hospedar SSISDB ni requiere acceso a datos locales, puede omitir los parámetros VNetId y Subnet o pasar valores vacíos para ellos. También puede omitirlos si configura IR autohospedado como un proxy para que la instancia de Azure-SSIS IR tenga acceso a datos locales. En caso contrario, no puede omitirlos y debe pasar valores válidos de la configuración de red virtual; para ello, consulte [Unión de una instancia de Integration Runtime de SSIS de Azure a una red virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Si utiliza una Instancia administrada de Azure SQL Database para hospedar SSISDB, puede omitir el parámetro CatalogPricingTier o pasar un valor vacío. En caso contrario, no puede omitirlo y debe pasar un valor válido de la lista de planes de tarifa admitidos para Azure SQL Database; para ello, consulte [Límites de recursos de SQL Database](../sql-database/sql-database-resource-limits.md).

Si usa la autenticación de Azure Active Directory (AAD) con la identidad administrada de Azure Data Factory para conectarse al servidor de bases de datos, puede omitir el parámetro CatalogAdminCredential, pero debe agregar la identidad administrada de su ADF a un grupo de AAD que cuente con permisos de acceso al servidor de bases de datos; para ello, consulte [Habilitación de la autenticación de AAD para Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). En caso contrario, no puede omitirlo y debe pasar un objeto válido formado a partir de su nombre de usuario y contraseña de administrador del servidor para la autenticación de SQL.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

### <a name="start-integration-runtime"></a>Inicio de la instancia de Integration Runtime

Ejecute los siguientes comandos para iniciar la instancia de Azure-SSIS Integration Runtime.

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

> [!NOTE]
> Sin incluir el tiempo de configuración personalizada, este proceso debería completarse en 5 minutos, pero puede que tarde aproximadamente de 20 a 30 minutos en unir Azure-SSIS IR a una red virtual.
>
> Si usa SSISDB, el servicio ADF se conectará al servidor de bases de datos para prepararlo. También configura los permisos o los valores de la red virtual, si se especifica, y une la instancia de Azure-SSIS IR a la red virtual.
> 
> Al aprovisionar Azure-SSIS IR, también se instalan el acceso redistribuible y el paquete de característica de Azure para SSIS. Estos componentes proporcionan conectividad a los archivos de Excel y Access, y a diversos orígenes de datos de Azure, además de los ya admitidos por los componentes integrados. También puede instalar componentes adicionales, consulte [Configuración personalizada para Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="full-script"></a>Script completo

Este es el script completo que crea una instancia de Integration Runtime de SSIS de Azure.

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance with a private endpoint/on-premises data without configuring Self-Hosted IR, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance with a private endpoint

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"

### Sign in and select subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database server
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}

### Configure virtual network
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Plantilla del Administrador de recursos de Azure

En esta sección, se utiliza una plantilla de Azure Resource Manager para crear una instancia de Integration Runtime de SSIS de Azure. Aquí se muestra el tutorial de ejemplo:

1. Cree un archivo JSON con la siguiente plantilla de Azure Resource Manager. Reemplace los valores de los corchetes angulares (marcadores de posición) con sus propios valores.

    ```json
    {
      "contentVersion": "1.0.0.0",
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2018-06-01",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D8_v3",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 8
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL Database server name>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL Database server admin username>",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Database server admin password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```

2. Para implementar la plantilla de Azure Resource Manager, ejecute el comando New-AzResourceGroupDeployment como se muestra en el ejemplo siguiente, donde ADFTutorialResourceGroup es el nombre del grupo de recursos y ADFTutorialARM.json es el archivo que contiene la definición JSON de la factoría de datos y la instancia de Azure-SSIS IR.

    ```powershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Este comando crea la factoría de datos y una instancia de IR de SSIS de Azure, pero no inicia el entorno de ejecución de integración.

3. Para iniciar la instancia de Azure-SSIS IR, ejecute el comando Start-AzDataFactoryV2IntegrationRuntime:

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ```

> [!NOTE]
> Sin incluir el tiempo de configuración personalizada, este proceso debería completarse en 5 minutos, pero puede que tarde aproximadamente de 20 a 30 minutos en unir Azure-SSIS IR a una red virtual.
>
> Si usa SSISDB, el servicio ADF se conectará al servidor de bases de datos para prepararlo. También configura los permisos o los valores de la red virtual, si se especifica, y une la instancia de Azure-SSIS IR a la red virtual.
> 
> Al aprovisionar Azure-SSIS IR, también se instalan el acceso redistribuible y el paquete de característica de Azure para SSIS. Estos componentes proporcionan conectividad a los archivos de Excel y Access, y a diversos orígenes de datos de Azure, además de los ya admitidos por los componentes integrados. También puede instalar componentes adicionales, consulte [Configuración personalizada para Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="deploy-ssis-packages"></a>Implementación de paquetes de SSIS

Si usa SSISDB, puede implementar los paquetes ahí y ejecutarlos en Azure-SSIS IR con herramientas SSDT o SSMS que se conectan al servidor de bases de datos mediante su punto de conexión del servidor.  Para un servidor de Azure SQL Database o Instancia administrada con un punto de conexión privado o Instancia administrada con un punto de conexión público, el formato del punto de conexión del servidor es `<server name>.database.windows.net`/`<server name>.<dns prefix>.database.windows.net`/`<server name>.public.<dns prefix>.database.windows.net,3342`, respectivamente. Si no usa SSISDB, puede implementar los paquetes en sistemas de archivos, recursos compartidos de archivos o Azure Files y ejecutarlos en Azure-SSIS IR mediante las utilidades de la línea de comandos `dtinstall`/`dtutil`/`dtexec`. Para más información, consulte [Implementación de paquetes en SSIS](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). En ambos casos, también puede ejecutar los paquetes implementados en Azure-SSIS IR con actividad de ejecución de paquetes de SSIS en las canalizaciones de ADF, consulte [Invocación de la ejecución de paquetes en SSIS como actividad de ADF de primera clase](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="next-steps"></a>Pasos siguientes

Consulte también los restantes temas sobre instancias de Azure-SSIS IR en esta documentación:

- [Integration Runtime de SSIS de Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime). En este artículo se proporciona información sobre los entornos de ejecución de integración en general, incluido Azure-SSIS IR.
- [Supervisión de Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime) En este artículo se muestra cómo recuperar y comprender la información acerca de Azure-SSIS IR.
- [Administración de Azure-SSIS IR](manage-azure-ssis-integration-runtime.md). En este artículo se muestra cómo detener, iniciar o eliminar la instancia de Azure-SSIS IR. También se muestra cómo escalar horizontalmente esta instancia agregando más nodos.
- [Unión de la instancia de Azure-SSIS IR a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md). En este artículo se proporciona información sobre cómo unir la instancia de Azure-SSIS IR a una red virtual.