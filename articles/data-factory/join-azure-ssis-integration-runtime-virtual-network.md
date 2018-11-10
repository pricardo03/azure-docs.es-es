---
title: Unión de una instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual | Microsoft Docs
description: Aprenda a unir el entorno de ejecución para la integración de SSIS en Azure a una red virtual de Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/22/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 38839379f584b40cdbefad3e4cbb3bc47881c9a7
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094602"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Unión de una instancia de Integration Runtime de SSIS de Azure a una red virtual
Una su instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual de Azure en los siguientes escenarios: 

- Quiere conectarse a almacenes de datos locales desde paquetes de SSIS que se ejecutan en una instancia de Integration Runtime de SSIS de Azure. 

- Va a hospedar la base de datos de catálogo de SQL Server Integration Services (SSIS) en Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada. 

 Azure Data Factory le permite unir su instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual creada mediante el modelo de implementación clásica o el modelo de implementación de Azure Resource Manager. 

> [!IMPORTANT]
> La red virtual clásica actualmente está en desuso, por tanto, use la red virtual de Azure Resource Manager en su lugar.  Si ya usa la red virtual clásica, cambie a la red virtual de Azure Resource Manager tan pronto como sea posible.

## <a name="access-to-on-premises-data-stores"></a>Acceso a los almacenes de datos locales
Si los paquetes SSIS acceden solo a almacenes de datos de la nube pública, no necesita unir la instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual. Si los paquetes SSIS acceden a almacenes de datos locales, debe unir la instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual que esté conectada a la red local. 

Estos son algunos puntos importantes a tener en cuenta: 

- Si no hay ninguna red virtual existente conectada a su red local, primero cree una [red virtual de Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) o una [red virtual clásica](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para que se una su instancia de Integration Runtime para la integración de SSIS en Azure. A continuación, configure una [conexión de puerta de enlace VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) de sitio a sitio o de [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) entre esa red virtual y la red local. 

- Si hay una red de virtual clásica o de Azure Resource Manager existente conectada a la red local en la misma ubicación que su instancia de Integration Runtime para la integración de SSIS en Azure, puede unir Integration Runtime a esa red virtual. 

- Si hay una red virtual clásica existente conectada a la red local en una ubicación diferente a la de su instancia de Integration Runtime para la integración de SSIS en Azure, puede crear primero una [red virtual clásica](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para que se una su instancia. A continuación, configure una [red virtual clásica a clásica](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md). O también, puede crear una [red virtual de Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) para que se una su instancia de Integration Runtime para la integración de SSIS en Azure. A continuación, configure una conexión de [red virtual clásica a Azure Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md). 
 
- Si hay una red virtual de Azure Resource Manager existente conectada a la red local en una ubicación diferente a la de la instancia de Integration Runtime para la integración de SSIS en Azure, puede crear primero una [red virtual de Azure Resource Manager](../virtual-network/quick-create-portal.md##create-a-virtual-network) para que se una su instancia. A continuación, configure una conexión de red virtual de Azure Resource Manager a Azure Resource Manager. O bien, puede crear una [red virtual clásica](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para que se una dicha instancia. A continuación, configure una conexión de [red virtual clásica a Azure Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md). 

## <a name="host-the-ssis-catalog-database-in-azure-sql-database-with-virtual-network-service-endpointsmanaged-instance"></a>Hospedaje de la base de datos de catálogo de SSIS en Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada
Si el catálogo de SSIS se hospeda en Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada, puede unir una instancia de Integration Runtime para la integración de SSIS en Azure a: 

- La misma red virtual. 
- Una red virtual diferente que tenga una conexión de red a red con la que se usa para la Instancia administrada. 

Si hospeda el catálogo de SSIS en Azure SQL Database con puntos de conexión de servicio de red virtual, asegúrese de unir la instancia de Azure-SSIS Integration Runtime a la misma red virtual y subred.

Si une la instancia de Azure-SSIS Integration Runtime a la misma red virtual que una instancia administrada, asegúrese de que ambas instancias se encuentran en subredes diferentes. Si une la instancia de Azure-SSIS Integration Runtime a una red virtual diferente de aquella de la instancia administrada, le recomendamos realizar un emparejamiento de redes virtuales (que se limite a la misma región) o una conexión de red virtual a red virtual. Consulte [Conexión de la aplicación a Instancia administrada de Azure SQL Database](../sql-database/sql-database-managed-instance-connect-app.md).

En todos los casos, la red virtual solo puede implementarse mediante el modelo de implementación de Azure Resource Manager.

En las siguientes secciones se proporciona más información. 

## <a name="requirements-for-virtual-network-configuration"></a>Requisitos para la configuración de una red virtual
-   Asegúrese de que `Microsoft.Batch` es un proveedor registrado en la suscripción de la subred de la red virtual que hospeda la instancia de Integration Runtime para la integración de SSIS en Azure. Si usa una red virtual clásica, también debe unir `MicrosoftAzureBatch` al rol Colaborador de la máquina virtual clásica de esa red virtual. 

-   Asegúrese de que tiene los permisos necesarios. Consulte [Permisos necesarios](#perms).

-   Seleccione la subred adecuada para hospedar la instancia de Integration Runtime para la integración de SSIS en Azure. Vea [Seleccionar la subred](#subnet). 

-   Si usa su propio servidor de Servicios de nombres de dominio (DNS) en la red virtual, vea [ Servidor de Servicio de nombres de dominio](#dns_server). 

-   Si usa un grupo de seguridad de red (NSG) en la subred, vea [Grupo de seguridad de red](#nsg). 

-   Si usa Azure ExpressRoute o un ruta definida por el usuario (UDR), consulte [Usar Azure ExpressRoute o una ruta definida por el usuario](#route). 

-   Asegúrese de que el grupo de recursos de la red virtual pueda crear y eliminar determinados recursos de Azure Network. Vea [Requisitos para el grupo de recursos](#resource-group). 

### <a name="perms"></a> Permisos necesarios

El usuario que crea la instancia de Azure-SSIS Integration Runtime debe tener los permisos siguientes:

- Si va a unir SSIS IR a una red virtual de Azure Resource Manager, tiene dos opciones:

  - Usar el rol integrado *Colaborador de red*. Este rol requiere el permiso *Microsoft.Network /\**, que tiene un ámbito mucho mayor del necesario.

  - Crear un rol personalizado que incluya solo el permiso *Microsoft.Network/virtualNetworks/\*/join/action* necesario. 

- Si va a unir la instancia de SSIS IR a una red virtual clásica, se recomienda usar el rol integrado *Colaborador de máquina virtual clásica*. En caso contrario, tendrá que definir un rol personalizado que incluya el permiso para unirse a la red virtual.

### <a name="subnet"></a> Seleccionar la subred
-   No seleccione GatewaySubnet para implementar una instancia de Azure-SSIS Integration Runtime, porque está dedicada para las puertas de enlace de red virtual. 

-   Asegúrese de que la subred que selecciona tenga suficiente espacio de direcciones disponible para usar Azure-SSIS IR. Deje al menos 2 * número de nodo de IR en direcciones IP disponibles. Azure reserva algunas direcciones IP dentro de cada subred y estas direcciones no se pueden usar. La primera y la última dirección IP de las subredes están reservadas para la conformidad con el protocolo, junto con otras tres direcciones usadas para los servicios de Azure. Para más información, consulte [¿Hay alguna restricción en el uso de direcciones IP dentro de estas subredes?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets) 

-   No use una subred que esté ocupada exclusivamente por otros servicios de Azure (por ejemplo, Instancia administrada de SQL Database, App Service, etc.). 

### <a name="dns_server"></a> Servidor de Servicio de nombres de dominio 
Si tiene que usar su propio servidor de Servicios de nombres de dominio (DNS) en una red virtual unida por su instancia de Integration Runtime para la integración de SSIS en Azure, asegúrese de que puede resolver nombres de host públicos de Azure (por ejemplo, un nombre de blob de Azure Storage, `<your storage account>.blob.core.windows.net`). 

Se recomienda seguir estos pasos: 

-   Configure un DNS personalizado para reenviar solicitudes a Azure DNS. Puede reenviar los registros DNS no resueltos a la dirección IP de resoluciones recursivas de Azure (168.63.129.16) de su propio servidor DNS. 

-   Configure el DNS personalizado como principal y Azure DNS como secundario para la red virtual. Registre la dirección IP de resoluciones recursivas de Azure (168.63.129.16) como servidor DNS secundario en el caso de que su propio servidor DNS no esté disponible. 

Para más información, vea [Resolución de nombres con su propio servidor DNS](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). 

### <a name="nsg"></a> Grupo de seguridad de red
Si tiene que implementar grupos de seguridad de red (NSG) par la subred que usa su instancia de Integration Runtime para la integración de SSIS en Azure, permita el tráfico entrante y saliente mediante los siguientes puertos: 

| Dirección | Protocolo de transporte | Origen | Intervalo de puertos de origen | Destino | Intervalo de puertos de destino | Comentarios |
|---|---|---|---|---|---|---|
| Entrada | TCP | AzureCloud<br/>(o un ámbito mayor, como Internet) | * | VirtualNetwork | 29876, 29877 (si une la instancia de Integration Runtime a una red virtual de Azure Resource Manager) <br/><br/>10100, 20100, 30100 (si une la instancia de Integration Runtime a una red virtual clásica)| El servicio Data Factory usa estos puertos para comunicarse con los nodos de su instancia de Integration Runtime para la integración de SSIS en Azure de la red virtual. <br/><br/> Tanto si crea un grupo de seguridad de red en el nivel de subred como si no, Data Factory siempre configura uno en el nivel de las tarjetas de interfaz de red (NIC) conectadas a las máquinas virtuales que hospedan la instancia de Integration Runtime para la integración de SSIS en Azure. Se permite solo el tráfico entrante desde direcciones IP de Data Factory en los puertos especificados por ese grupo de seguridad de red a nivel de NIC. Aunque si se abren estos puertos al tráfico de Internet en el nivel de la subred, el tráfico de direcciones IP que no sean de Data Factory se bloquea en el nivel de NIC. |
| Salida | TCP | VirtualNetwork | * | AzureCloud<br/>(o un ámbito mayor, como Internet) | 443 | Los nodos de su instancia de Integration Runtime para la integración de SSIS en Azure de la red virtual usan este puerto para acceder a servicios de Azure, como Azure Storage y Azure Event Hubs. |
| Salida | TCP | VirtualNetwork | * | Internet | 80 | Los nodos de su instancia de Azure-SSIS Integration Runtime en la red virtual usan este puerto para descargar la lista de revocación de certificados de Internet. |
| Salida | TCP | VirtualNetwork | * | Sql<br/>(o un ámbito mayor, como Internet) | 1433, 11000-11999, 14000-14999 | Los nodos de su instancia de Integration Runtime de Azure SSIS en la red virtual usan estos puertos para acceder a la SSISDB que hospeda el servidor de Azure SQL Database. Esta finalidad no se aplica a la SSISDB hospedada por Instancia administrada de Azure SQL Database. |
||||||||

### <a name="route"></a> Usar Azure ExpressRoute o una ruta definida por el usuario
Puede conectar un circuito de [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) a su infraestructura de red virtual para ampliar la red local a Azure. 

Una configuración común consiste en usar la tunelización forzada (se recomienda una ruta BGP, 0.0.0.0/0 a la red virtual) que fuerza el tráfico saliente de Internet desde el flujo de la red virtual al dispositivo de red local para la inspección y el registro. Este flujo de tráfico interrumpe la conectividad entre la instancia de Integration Runtime para la integración de SSIS en Azure en la red virtual con los servicios dependientes de Azure Data Factory. La solución es establecer una, o varias, [rutas definidas por el usuario (UDR)](../virtual-network/virtual-networks-udr-overview.md) en la subred que contiene Azure-SSIS IR. Una ruta definida por el usuario define las rutas de subred específica que se respetan en lugar de la ruta BGP. 

También puede definir rutas definidas por el usuario (UDR) para forzar el tráfico saliente de Internet desde la subred que hospeda la instancia de Integration Runtime para la integración de SSIS en Azure a otra subred, que hospeda un dispositivo de red virtual como firewall o un host de red perimetral para fines de inspección y de registro. 

En ambos casos debe aplicar una ruta 0.0.0.0/0 con el tipo de próximo salto **Internet** en la subred que hospeda la instancia de Integration Runtime para la integración de SSIS en Azure para que pueda establecerse correctamente la comunicación entre el servicio de Data Factory y la instancia de Integration Runtime para la integración de SSIS en Azure. 

![Agregar una ruta](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

Si le preocupa perder la capacidad de inspeccionar el tráfico saliente de Internet de esa subred, también puede agregar una regla de NSG en la subred para restringir los destinos de salida para las [direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653). 

Vea [este script de PowerShell](https://gallery.technet.microsoft.com/scriptcenter/Adds-Azure-Datacenter-IP-dbeebe0c) para un ejemplo. Tendrá que ejecutar el script cada semana para mantener actualizada la lista de direcciones IP de centro de datos de Azure. 

### <a name="resource-group"></a> Requisitos para el grupo de recursos
-   La instancia de Integration Runtime para la integración de SSIS en Azure necesita crear determinados recursos de red en el mismo grupo de recursos de la red virtual. Entre estos recursos se incluyen los siguientes:
    -   Una instancia de Azure Load Balancer, con el nombre *<Guid>-azurebatch-cloudserviceloadbalancer*.
    -   Una dirección IP pública de Azure llamada *<Guid>-azurebatch-cloudservicepublicip*.
    -   Un grupo de seguridad de red llamado *<Guid>-azurebatch-cloudservicenetworksecuritygroup*. 

-   Asegúrese de que no tiene ningún bloqueo de recursos en el grupo de recursos ni en la suscripción a la que pertenece la red virtual. Si configura un bloqueo de solo lectura o un bloqueo de eliminación, se puede producir un error o un bloqueo al iniciar o detener la instancia de IR. 

-   Asegúrese de que no tiene ninguna directiva de Azure que impida que se creen los siguientes recursos en el grupo de recursos o en la suscripción a la que pertenece la red virtual: 
    -   Microsoft.Network/LoadBalancers 
    -   Microsoft.Network/NetworkSecurityGroups 
    -   Microsoft.Network/PublicIPAddresses 

## <a name="azure-portal-data-factory-ui"></a>Azure Portal (interfaz de usuario de Data Factory)
En esta sección se muestra cómo unir un entorno de ejecución existente para la integración de SSIS en Azure a una red virtual (clásica o de Azure Resource Manager) mediante Azure Portal y la interfaz de usuario de Data Factory. En primer lugar, debe configurar la red virtual de forma adecuada antes de unir a ella su instancia de Integration Runtime para la integración de SSIS en Azure. Repase una de las dos secciones siguientes según el tipo de red virtual (clásica o de Azure Resource Manager). Después, continúe con la tercera sección para unir su instancia de Integration Runtime para la integración de SSIS en Azure a la red virtual. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>Uso del portal para configurar una red virtual de Azure Resource Manager
Antes de poder unir una instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual, debe configurar dicha red. 

1. Inicie Microsoft Edge o Google Chrome. Actualmente, la interfaz de usuario de Data Factory solo se admite en esos exploradores web. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 

1. Seleccione **Más servicios**. Filtre y seleccione **Redes virtuales**. 

1. Filtre y seleccione su red virtual en la lista. 

1. En la página **Red virtual**, seleccione **Propiedades**. 

1. Seleccione el botón de copia en **ID. DE RECURSO** para copiar el identificador de recurso de la red virtual en el Portapapeles. Guarde el identificador del Portapapeles en OneNote o en un archivo. 

1. En el menú de la izquierda, seleccione **Subredes**. Asegúrese de que el número de **direcciones disponibles** sea mayor que los nodos de su instancia de Integration Runtime para la integración de SSIS en Azure. 

1. Compruebe que el proveedor de Azure Batch está registrado en la suscripción de Azure que tiene la red virtual. O bien, registre el proveedor de Azure Batch. Si ya tiene una cuenta de Azure Batch en su suscripción, entonces su suscripción está registrada para Azure Batch. Si crea la instancia de Integration Runtime para la integración de SSIS en Azure en el portal de Data Factory, el proveedor de Azure Batch se registrará automáticamente. 

   a. En Azure Portal, seleccione **Suscripciones** en el menú izquierdo. 

   b. Seleccione su suscripción. 

   c. Haga clic en **Proveedores de recursos** a la izquierda y confirme que **Microsoft.Batch** es un proveedor registrado. 

   ![Confirmación del estado "Registrado"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Si no ve **Microsoft.Batch** en la lista, regístrelo. Para ello, [cree una cuenta de Azure Batch vacía](../batch/batch-account-create-portal.md) en su suscripción. Puede eliminarlo más tarde. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>Uso del portal para configurar una red virtual clásica
Antes de poder unir una instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual, debe configurar dicha red. 

1. Inicie Microsoft Edge o Google Chrome. Actualmente, la interfaz de usuario de Data Factory solo se admite en estos exploradores web. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 

1. Seleccione **Más servicios**. Filtre y seleccione **Redes virtuales (clásicas)**. 

1. Filtre y seleccione su red virtual en la lista. 

1. En la página **Red virtual (clásica)**, seleccione **Propiedades**. 

   ![Identificador de recurso de red virtual clásica](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

1. Seleccione el botón de copia en **ID. DE RECURSO** para copiar el identificador de recurso de la red clásica en el Portapapeles. Guarde el identificador del Portapapeles en OneNote o en un archivo. 

1. En el menú de la izquierda, seleccione **Subredes**. Asegúrese de que el número de **direcciones disponibles** sea mayor que los nodos de su instancia de Integration Runtime para la integración de SSIS en Azure. 

   ![Número de direcciones disponibles en la red virtual](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

1. Una **MicrosoftAzureBatch** al rol **Colaborador de la máquina virtual clásica** de la red virtual. 

    a. Seleccione **Control de acceso (IAM)** en el menú izquierdo y seleccione **Agregar** en la barra de herramientas. 

    ![Botones "Control de acceso" y "Agregar"](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. En la página **Agregar permisos**, seleccione **Colaborador de la máquina virtual clásica** en **Rol**. Pegue **ddbf3205-c6bd-46ae-8127-60eb93363864** en el cuadro de texto **Seleccionar** y, después, seleccione **Microsoft Azure Batch** en la lista de resultados de la búsqueda. 

    ![Resultados de búsqueda de la página "Agregar permisos"](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    c. Seleccione **Guardar** para guardar la configuración y cerrar la página. 

    ![Guardado de la configuración de acceso](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    d. Confirme que ve **Microsoft Azure Batch** en la lista de colaboradores. 

    ![Confirmación del acceso a Azure Batch](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

1. Compruebe que el proveedor de Azure Batch está registrado en la suscripción de Azure que tiene la red virtual. O bien, registre el proveedor de Azure Batch. Si ya tiene una cuenta de Azure Batch en su suscripción, entonces su suscripción está registrada para Azure Batch. Si crea la instancia de Integration Runtime para la integración de SSIS en Azure en el portal de Data Factory, el proveedor de Azure Batch se registrará automáticamente. 

   a. En Azure Portal, seleccione **Suscripciones** en el menú izquierdo. 

   b. Seleccione su suscripción. 

   c. Haga clic en **Proveedores de recursos** a la izquierda y confirme que **Microsoft.Batch** es un proveedor registrado. 

   ![Confirmación del estado "Registrado"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Si no ve **Microsoft.Batch** en la lista, regístrelo. Para ello, [cree una cuenta de Azure Batch vacía](../batch/batch-account-create-portal.md) en su suscripción. Puede eliminarlo más tarde. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Unión de la instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual
1. Inicie Microsoft Edge o Google Chrome. Actualmente, la interfaz de usuario de Data Factory solo se admite en esos exploradores web. 

1. En [Azure Portal](https://portal.azure.com), seleccione **Factorías de datos** en el menú de la izquierda. Si no ve **Factorías de datos** en el menú, seleccione **Más servicios** y seleccione **Factorías de datos** en la sección **INTELIGENCIA Y ANÁLISIS**. 

   ![Lista de factorías de datos](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Seleccione su factoría de datos con la instancia de Integration Runtime para la integración de SSIS en Azure en la lista. Verá la página principal de la factoría de datos. Seleccione el icono **Crear e implementar**. Verá la interfaz de usuario de Data Factory en una pestaña independiente. 

   ![Página principal Factoría de datos](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. En la interfaz de usuario de Data Factory, vaya a la pestaña **Editar**, seleccione **Conexiones** y vaya a la pestaña **Integration Runtimes** (Entornos de ejecución de integración). 

   ![Pestañas "Integration runtimes" (Entornos de ejecución de integración)](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Si su instancia de Integration Runtime para la integración de SSIS en Azure está en funcionamiento, en la lista de entornos de ejecución de integración, seleccione el botón **Detener** de la columna **Acciones** de dicha instancia. No podrá editar el entorno de ejecución de integración hasta que lo detenga. 

   ![Detención de Integration Runtime](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. En la lista de entornos de ejecución de integración, seleccione el botón **Editar** de la columna **Acciones** de su instancia de Integration Runtime para la integración de SSIS en Azure. 

   ![Edición de Integration Runtime](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. En la página **General Settings** (Configuración general) de la ventana **Integration Runtime Setup** (Configuración de Integration Runtime), seleccione **Next** (Siguiente). 

   ![Configuración general de Integration Runtime](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)

1. En la página **Configuración de SQL**, escriba la contraseña del administrador y seleccione **Siguiente**. 

   ![Configuración de SQL Server para Integration Runtime](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)

1. Haga lo siguiente en la página **Configuración avanzada**: 

   a. Active la casilla **Select a VNet for your Azure-SSIS Integration Runtime to join and allow Azure services to configure VNet permissions/settings** (Seleccionar una red virtual a la que unir su instancia de Integration Runtime para la integración de SSIS en Azure y permitir que los servicios de Azure configuren los permisos y la configuración de la red virtual). 

   b. En **Type** (Tipo), especifique si la red virtual es una red virtual clásica o una red virtual de Azure Resource Manager. 

   c. En **Nombre de red virtual**, seleccione la red virtual. 

   d. En **Nombre de subred**, seleccione la subred de la red virtual. 

   e. Haga clic en **VNet Validation** (Validación de red virtual) y, si se realiza correctamente, haga clic en **Actualizar**. 

   ![Configuración avanzada de Integration Runtime](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)

1. Ahora, puede iniciar Integration Runtime mediante el botón **Iniciar** de la columna **Acciones** de su instancia de Integration Runtime para la integración de SSIS en Azure. Se tardan de 20 a 30 minutos aproximadamente en iniciar una instancia de Integration Runtime para la integración de SSIS en Azure. 

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="configure-a-virtual-network"></a>Configuración de una red virtual
Antes de poder unir una instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual, debe configurar dicha red. Para configurar automáticamente los permisos o los valores de la red virtual para que su instancia de Integration Runtime para la integración de SSIS en Azure se una a la red virtual, agregue el siguiente script.

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Creación de una instancia de Integration Runtime para la integración de SSIS en Azure y su unión a una red virtual
Puede crear una instancia de Integration Runtime para la integración de SSIS en Azure y unirla a una red virtual al mismo tiempo. Para conocer el script y las instrucciones en su totalidad, consulte [Creación de una instancia de Integration Runtime para la integración de SSIS en Azure](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Unión de una instancia de Integration Runtime para la integración de SSIS en Azure ya existente a una red virtual
El script del artículo [Creación de una instancia de Integration Runtime para la integración de SSIS en Azure](create-azure-ssis-integration-runtime.md) muestra cómo crear una instancia de Integration Runtime para la integración de SSIS en Azure y unirla a una red virtual en el mismo script. Si ya tiene una instancia de Integration Runtime para la integración de SSIS en Azure, realice los pasos siguientes para unirla a la red virtual: 
1. Detenga la instancia de Integration Runtime de SSIS de Azure. 
1. Configure la instancia de Integration Runtime para la integración de SSIS en Azure para unirla a la red virtual. 
1. Inicie la instancia de Integration Runtime de SSIS de Azure. 

### <a name="define-the-variables"></a>Definir las variables
```powershell
$ResourceGroupName = "<your Azure resource group name>"
$DataFactoryName = "<your Data Factory name>" 
$AzureSSISName = "<your Azure-SSIS IR name>"
# Specify the information about your classic or Azure Resource Manager virtual network.
$VnetId = "<your Azure virtual network resource ID>"
$SubnetName = "<the name of subnet in your virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>Detener la instancia de Integration Runtime de SSIS de Azure
Detenga la instancia de Integration Runtime para la integración de SSIS en Azure antes de unirla a una red virtual. Este comando libera todos sus nodos y detiene la facturación:

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Configuración de la red virtual para que se una la instancia de Integration Runtime para la integración de SSIS en Azure
```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Configurar la instancia de Integration Runtime de SSIS de Azure
Para configurar la instancia de Integration Runtime para la integración de SSIS en Azure para que se una a la red virtual, ejecute el comando `Set-AzureRmDataFactoryV2IntegrationRuntime`: 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Iniciar la instancia de Integration Runtime de SSIS de Azure
Para iniciar la instancia de Integration Runtime para la integración de SSIS en Azure, ejecute el siguiente comando: 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Este comando tarda entre 20 y 30 minutos en finalizar.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre el tiempo de ejecución de integración de SSIS en Azure, consulte los temas siguientes: 
- [Integration Runtime de SSIS de Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime). En este artículo se proporciona información conceptual sobre los entornos de ejecución de integración en general, incluida la instancia de Integration Runtime para la integración de SSIS en Azure. 
- [Tutorial: Implementación de paquetes SSIS en Azure](tutorial-create-azure-ssis-runtime-portal.md). En este artículo se proporcionan instrucciones paso a paso para crear una instancia de Integration Runtime para la integración de SSIS en Azure. Usa Azure SQL Database para hospedar el catálogo de SSIS. 
- [Creación de una instancia de Integration Runtime de SSIS de Azure](create-azure-ssis-integration-runtime.md). En este artículo se amplía el tutorial y se proporcionan instrucciones sobre el uso de Azure SQL Database con puntos de conexión de servicio de red virtual o Instancia administrada para hospedar el catálogo de SSIS y cómo unir Integration Runtime a una red virtual. 
- [Monitor an Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Supervisión de una instancia de Integration Runtime de SSIS de Azure). En este artículo se muestra cómo recuperar información sobre una instancia de IR de SSIS de Azure, junto con descripciones de los estados en la información devuelta. 
- [Administración de Integration Runtime de SSIS de Azure](manage-azure-ssis-integration-runtime.md). En este artículo se muestra cómo detener, iniciar o quitar una instancia de IR de SSIS de Azure. También se muestra cómo escalar horizontalmente la instancia de Integration Runtime para la integración de SSIS en Azure mediante la adición de nodos. 
