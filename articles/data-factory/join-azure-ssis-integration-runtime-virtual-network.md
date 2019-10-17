---
title: Conexión de Azure-SSIS Integration Runtime a una red virtual | Microsoft Docs
description: Aprenda a conectar Azure-SSIS Integration Runtime a una red virtual de Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 065f69cc98f05fcb19648f190a7dba4b43da1a9a
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72326616"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Unión de una instancia de Integration Runtime de SSIS de Azure a una red virtual
Si usa SQL Server Integration Services (SSIS) en Azure Data Factory, debe conectar el entorno de Azure-SSIS Integration Runtime (IR) a una red virtual de Azure en los escenarios siguientes: 

- Quiere conectarse a almacenes de datos locales desde paquetes SSIS que se ejecutan en Azure-SSIS Integration Runtime sin configurar o administrar un entorno de ejecución de integración autohospedado como proxy. 

- Quiere conectarse a los recursos del servicio de Azure compatibles con los puntos de conexión de servicio de red virtual desde los paquetes SSIS que se ejecutan en Azure-SSIS Integration Runtime.

- Hospeda una base de datos de catálogos de SSIS (SSISDB) en Azure SQL Database con puntos de conexión de servicio de red virtual o una instancia administrada en una red virtual. 

Azure Data Factory le permite conectar Azure-SSIS IR a una red virtual creada mediante el modelo de implementación clásica o el modelo de implementación con Azure Resource Manager. 

> [!IMPORTANT]
> La red virtual clásica está en desuso, así que debe usar la red virtual de Azure Resource Manager.  Si ya usa la red virtual clásica, cambie a la red virtual de Azure Resource Manager en cuanto sea posible.

## <a name="access-to-on-premises-data-stores"></a>Acceso a los almacenes de datos locales
Si los paquetes SSIS acceden a almacenes de datos locales, puede conectar Azure-SSIS Integration Runtime a una red virtual que esté conectada a la red local. O bien, puede configurar o administrar un entorno de ejecución de integración autohospedado como si fuera un proxy para Azure-SSIS Integration Runtime. Para más información, consulte [Configuración del entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). 

Al conectar Azure-SSIS Integration Runtime a una red virtual, recuerde estos puntos, ya que son importantes: 

- Si no hay ninguna red virtual existente conectada a su red local, primero cree una [red virtual de Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) a la que pueda conectarse Azure-SSIS Integration Runtime. Luego, configure una [conexión de puerta de enlace de VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) de sitio a sitio o una conexión [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) entre esa red virtual su la red local. 

- Si hay una red de virtual de Azure Resource Manager que ya esté conectada a su red local en la misma ubicación que Azure-SSIS Integration Runtime, puede conectar el entorno de ejecución de integración a esa red virtual. 

- Si hay una red virtual clásica conectada a la red local en una ubicación diferente a la de Azure-SSIS Integration Runtime, puede crear una [red virtual de Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) para que se conecte Azure-SSIS Integration Runtime. A continuación, configure una conexión de [red virtual clásica a Azure Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md). 
 
- Si hay una red virtual de Azure Resource Manager conectada a la red local en una ubicación diferente a la de Azure-SSIS Integration Runtime, puede crear primero una [red virtual de Azure Resource Manager](../virtual-network/quick-create-portal.md##create-a-virtual-network) para que se conecte a Azure-SSIS Integration Runtime. Luego, configure una conexión de red virtual de Azure Resource Manager a Azure Resource Manager. 

## <a name="access-to-azure-services"></a>Acceso a servicios de Azure
Si los paquetes SSIS acceden a los recursos del servicio de Azure compatibles con los [puntos de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md) y quiere proteger esos recursos en Azure-SSIS Integration Runtime, puede unir Azure-SSIS Integration Runtime a la subred de la red virtual configurada con los puntos de conexión de servicio de red virtual. De forma simultánea, puede agregar una regla de red virtual al recurso del servicio de Azure para permitir el acceso desde la misma subred.

## <a name="hosting-the-ssis-catalog-in-sql-database"></a>Hospedaje del catálogo de SSIS en SQL Database
Si hospeda el catálogo de SSIS en Azure SQL Database con puntos de conexión de servicio de red virtual, asegúrese de unir la instancia de Azure-SSIS Integration Runtime a la misma red virtual y subred.

Para conectar Azure-SSIS Integration Runtime a la misma red virtual que la instancia administrada, asegúrese de que ambas se encuentran en subredes diferentes. Para conectar Azure-SSIS Integration Runtime a una red virtual en la que no se encuentra la instancia administrada, es aconsejable realizar un emparejamiento de redes virtuales (que se limite a la misma región) o una conexión de red virtual a red virtual. Para más información, consulte [Conexión de una aplicación a Instancia administrada de Azure SQL Database](../sql-database/sql-database-managed-instance-connect-app.md).

En todos los casos, la red virtual solo puede implementarse mediante el modelo de implementación de Azure Resource Manager.

En las siguientes secciones se proporciona más información. 

## <a name="virtual-network-configuration"></a>Configuración de redes virtuales

Configure la red virtual para que cumpla estos requisitos: 

-   Asegúrese de que `Microsoft.Batch` es un proveedor registrado en la suscripción de la subred de la red virtual que hospeda Azure-SSIS Integration Runtime. Si usa una red virtual clásica, también debe conectar `MicrosoftAzureBatch` al rol Colaborador de la máquina virtual clásica de esa red virtual. 

-   Asegúrese de que tiene los permisos necesarios. Para más información, consulte [Configuración de permisos](#perms).

-   Seleccione la subred adecuada para hospedar la instancia de Integration Runtime para la integración de SSIS en Azure. Para más información, consulte el apartado sobre la [selección de la subred](#subnet). 

-   Si usa su propio servidor del sistema de nombres de dominio (DNS) en la red virtual, consulte [Configuración del servidor DNS](#dns_server). 

-   Si usa un grupo de seguridad de red (NSG) en la subred, consulte la sección acerca de la [configuración de un grupo de seguridad de red](#nsg). 

-   Si usa Azure ExpressRoute o una ruta definida por el usuario (UDR), consulte [Uso de Azure ExpressRoute o una ruta definida por el usuario](#route). 

-   Asegúrese de que el grupo de recursos de la red virtual puede crear y eliminar determinados recursos de Azure Network. Para más información, consulte [Configuración del grupo de recursos](#resource-group). 

-   Si personaliza Azure-SSIS Integration Runtime como se describe en [Instalación personalizada de Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup), los nodos de Azure-SSIS IR obtendrán direcciones IP privadas de un intervalo predefinido de 172.16.0.0 a 172.31.255.255. Por tanto, asegúrese de que los intervalos de direcciones IP privadas de las redes virtuales o locales no entran en conflicto con este intervalo.

Este diagrama muestra las conexiones necesarias para Azure-SSIS Integration Runtime:

![Integration Runtime de SSIS de Azure](media/join-azure-ssis-integration-runtime-virtual-network/azure-ssis-ir.png)

### <a name="perms"></a> Configuración de permisos

El usuario que cree Azure-SSIS Integration Runtime debe tener los permisos siguientes:

- Si va a unir SSIS IR a una red virtual de Azure Resource Manager, tiene dos opciones:

  - Use el rol Colaborador de red integrado. Este rol requiere el permiso _Microsoft.Network /\*_ , que tiene un ámbito mucho mayor del necesario.

  - Crear un rol personalizado que incluya solo el permiso _Microsoft.Network/virtualNetworks/\*/join/action_ necesario. 

- Si va a conectar SSIS IR a una red virtual clásica, se recomienda usar el rol Colaborador de máquina virtual clásica integrado. En caso contrario, tendrá que definir un rol personalizado que incluya el permiso para unirse a la red virtual.

### <a name="subnet"></a> Seleccionar la subred

Cuando elija una subred: 

-   No seleccione GatewaySubnet para implementar Azure-SSIS Integration Runtime. Está dedicado a las puertas de enlace de red virtual. 

-   Asegúrese de que la subred que seleccione tenga suficiente espacio de direcciones disponible para usar Azure-SSIS IR. Deje direcciones IP disponibles para al menos dos veces el número de nodos del entorno de ejecución de integración. Azure reserva algunas direcciones IP dentro de cada subred. Estas direcciones no se pueden usar. La primera y la última direcciones IP de las subredes están reservadas para la conformidad con el protocolo y las otras tres se usan para los servicios de Azure. Para más información, consulte [¿Hay alguna restricción en el uso de direcciones IP dentro de estas subredes?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets) 

-   No use una subred que esté ocupada exclusivamente por otros servicios de Azure (por ejemplo, Instancia administrada de SQL Database, App Service, etc.) 

### <a name="dns_server"></a> Configuración del servidor DNS 
Si tiene que usar su propio servidor DNS en una red virtual a la que se ha conectado Azure-SSIS Integration Runtime, asegúrese de que puede resolver nombres de host globales de Azure (por ejemplo, un blob de Azure Storage llamado `<your storage account>.blob.core.windows.net`). 

Se recomienda seguir estos pasos: 

-   Configure el DNS personalizado para que reenvié las solicitudes a Azure DNS. Los registros DNS no resueltos se pueden reenviar a la dirección IP de resoluciones recursivas de Azure (168.63.129.16) de su propio servidor DNS. 

-   Configure el DNS personalizado como servidor DNS principal de la red virtual. Configure Azure DNS como servidor DNS secundario. Si su propio servidor DNS no está disponible, registre la dirección IP de las resoluciones recursivas de Azure (168.63.129.16) como servidor DNS secundario. 

Para más información, consulte [Resolución de nombres con su propio servidor DNS](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). 

### <a name="nsg"></a> Configuración de un grupo de seguridad de red
Si tiene que implementar grupos de seguridad de red para la subred que usa Azure-SSIS Integration Runtime, permita que el tráfico entrante y saliente atraviese los siguientes puertos: 

| Dirección | Protocolo de transporte | Source | Intervalo de puertos de origen | Destination | Destination port range | Comentarios |
|---|---|---|---|---|---|---|
| Entrada | TCP | BatchNodeManagement | * | VirtualNetwork | 29876, 29877 (si conecta el entorno de ejecución de integración a una red virtual de Resource Manager) <br/><br/>10100, 20100, 30100 (si une la instancia de Integration Runtime a una red virtual clásica)| El servicio Data Factory usa estos puertos para comunicarse con los nodos de Azure-SSIS Integration Runtime en la red virtual. <br/><br/> Tanto si crea un grupo de seguridad de red en el nivel de subred como si no lo hace, Data Factory siempre configura uno en el nivel de las tarjetas de interfaz de red (NIC) conectadas a las máquinas virtuales que hospedan Azure-SSIS Integration Runtime. Se permite solo el tráfico entrante desde direcciones IP de Data Factory en los puertos especificados por ese grupo de seguridad de red a nivel de NIC. Aunque si se abren estos puertos al tráfico de Internet en el nivel de la subred, el tráfico de las direcciones IP que no sean de Data Factory se bloquea en el nivel de las tarjetas de interfaz de red. |
| Salida | TCP | VirtualNetwork | * | AzureCloud | 443 | Los nodos de Azure-SSIS Integration Runtime en la red virtual usan este puerto para acceder a servicios de Azure, como Azure Storage y Azure Event Hubs. |
| Salida | TCP | VirtualNetwork | * | Internet | 80 | Los nodos de Azure-SSIS Integration Runtime en la red virtual usan este puerto para descargar una lista de revocación de certificados de Internet. |
| Salida | TCP | VirtualNetwork | * | Sql | 1433, 11000-11999 | Los nodos de Azure-SSIS Integration Runtime en la red virtual usan estos puertos para acceder a una base de datos de catálogos de SSIS que hospeda el servidor de SQL Database. Si la directiva de conexión del servidor de SQL Database está establecida en **Proxy**, en lugar de **Redirigir**, solo se necesita el puerto 1433. Esta regla de seguridad de salida no es aplicable a las bases de datos de catálogos de SSIS hospedadas por una instancia administrada en la red virtual. |
||||||||

### <a name="route"></a> Use Azure ExpressRoute o una ruta definida por el usuario
Cuando conecte un circuito de [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) a la infraestructura de su red virtual para extender su red local a Azure, una configuración común usa la tunelización forzada (lo que anuncia una ruta BGP, 0.0.0.0/0, a la red virtual). Esta tunelización fuerza el tráfico de Internet saliente desde el flujo de red virtual a un dispositivo de red local cuando se realizan operaciones de inspección y registro. 
 
También puede definir varias [rutas definidas por el usuario](../virtual-network/virtual-networks-udr-overview.md) para forzar el tráfico de Internet saliente desde la subred que hospeda Azure-SSIS Integration Runtime a otra subred que hospeda un dispositivo de red virtual (NVA), como un firewall o Azure Firewall, para realizar inspección y registro. 

En ambos casos, la ruta del tráfico interrumpirá la conectividad de entrada necesaria de los servicios dependientes de Azure Data Factory (en concreto, los servicios de administración de Azure Batch) a Azure-SSIS Integration Runtime en la red virtual. Para evitarlo, defina una o varias rutas definidas por el usuario en la subred que contiene Azure-SSIS Integration Runtime. 

Puede aplicar una ruta 0.0.0.0/0 con el tipo de próximo salto como **Internet** en la subred que hospeda Azure-SSIS Integration Runtime en un escenario de Azure ExpressRoute. O bien, puede modificar la ruta 0.0.0.0/0 existente desde el tipo de próximo salto como **Dispositivo virtual** a **Internet** en el escenario de una NVA.

![Agregar una ruta](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

Si le preocupa perder la capacidad de inspeccionar el tráfico de Internet saliente desde esa subred, puede definir las rutas definidas por el usuario específicas para enrutar el tráfico únicamente entre los servicios de administración de Azure Batch y Azure-SSIS Integration Runtime con un tipo de próximo salto **Internet**.

Por ejemplo, si Azure-SSIS Integration Runtime se encuentra en `UK South`, obtendría una lista de intervalos IP de la etiqueta de servicio `BatchNodeManagement.UKSouth` del [vínculo de descarga de intervalos IP de las etiquetas de servicio](https://www.microsoft.com/en-us/download/details.aspx?id=56519) o mediante [Service Tag Discovery API](https://aka.ms/discoveryapi). Luego, aplique las siguientes rutas definidas por el usuario de las rutas de los intervalos IP relacionadas con el tipo de próximo salto como **Internet**.

![Configuración de UDR de Azure Batch](media/join-azure-ssis-integration-runtime-virtual-network/azurebatch-udr-settings.png)

> [!NOTE]
> Este enfoque conlleva un costo de mantenimiento adicional. Compruebe periódicamente el intervalo de direcciones IP y agregue nuevos intervalos de direcciones IP a la ruta definida por el usuario UDR para evitar romper Azure-SSIS Integration Runtime. Se recomienda comprobar el intervalo de direcciones IP mensualmente porque cuando la nueva dirección IP aparece en la etiqueta de servicio, la dirección IP tardará otro mes en surtir efecto. 

### <a name="resource-group"></a> Configuración del grupo de recursos
La instancia de Integration Runtime para la integración de SSIS en Azure necesita crear determinados recursos de red en el mismo grupo de recursos de la red virtual. Estos recursos incluyen:
   -   Una instancia de Azure Load Balancer, con el nombre *\<Guid>-azurebatch-cloudserviceloadbalancer*.
   -   Una dirección IP pública de Azure, con el nombre *\<Guid>-azurebatch-cloudservicepublicip*.
   -   Un grupo de seguridad de red, con el nombre *\<Guid>-azurebatch-cloudservicenetworksecuritygroup*. 

Estos recursos se crearán cuando se inicie el entorno de ejecución de integración y se eliminarán cuando se detenga el entorno de ejecución de integración. Para evitar bloquear la detención del entorno de ejecución de integración, no vuelva a usar estos recursos de red en los restantes recursos. 

Asegúrese de que no tiene ningún bloqueo de recursos en el grupo de recursos ni en la suscripción a la que pertenece la red virtual. Si configura un bloqueo de solo lectura o un bloqueo de eliminación, se puede producir un error al iniciar o detener el entorno de ejecución de integración, o bien puede dejar de responder. 

Asegúrese de que no tiene ninguna directiva de Azure que impida que se creen los siguientes recursos en el grupo de recursos o en la suscripción a la que pertenece la red virtual: 
   -   Microsoft.Network/LoadBalancers 
   -   Microsoft.Network/NetworkSecurityGroups 
   -   Microsoft.Network/PublicIPAddresses 

### <a name="faq"></a> Preguntas más frecuentes

- ¿Cómo se puede proteger la dirección IP pública expuesta en Azure-SSIS Integration Runtime para la conexión entrante? ¿Es posible quitar la dirección IP pública?
 
    En este momento, se creará automáticamente una dirección IP pública cuando Azure-SSIS Integration Runtime se conecte a la red virtual. Tenemos un grupo de seguridad de red de nivel de NIC para permitir que solo los servicios de administración de Azure Batch se conecten de forma entrante a Azure-SSIS Integration Runtime. También puede especificar un grupo de seguridad de red de nivel de subred para la protección de la entrada.

    Si no desea que se exponga la dirección IP pública, considere la posibilidad de [configurar el entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis), en lugar de la red virtual, si se puede hacer en su escenario.
 
- ¿Puedo agregar la dirección IP estática de Azure-SSIS Integration Runtime a la lista de permitidos del firewall para el origen de datos?
 
    - Si el origen de datos es local, después de conectar la red virtual a la red local y su red local y conectar Azure-SSIS Integration Runtime la subred de esa red virtual, puede especificar el intervalo de direcciones IP de esa subred a la lista de permitidos.
    - Si el origen de datos es un servicio de Azure compatible con el punto de conexión de servicio de red virtual, puede configurar el punto de servicio de red virtual en la red virtual y conectar Azure-SSIS Integration Runtime a la subred de esa red virtual. Después, puede permitir el acceso mediante la regla de red virtual de los servicios de Azure, en lugar del intervalo de direcciones IP.
    - Si el origen de datos es un tipo diferente de origen de datos en la nube, puede usar UDR para enrutar el tráfico saliente de Azure-SSIS Integration Runtime a la NVA o a Azure Firewall mediante una dirección IP pública estática. Puede agregar la dirección IP pública de la NVA o Azure Firewall a la lista de permitidos.
    - Si las respuestas anteriores no satisfacen sus necesidades, considere la posibilidad de proporcionar acceso a un origen de datos mediante la [configuración de un entorno de ejecución de integración autohospedado](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) como proxy para Azure-SSIS Integration Runtime. A continuación, puede agregar la dirección IP de la máquina que hospeda el entorno de ejecución de integración autohospedado a la lista de permitidos, en lugar de conectar el Azure-SSIS Integration Runtime a la red virtual.

## <a name="azure-portal-data-factory-ui"></a>Azure Portal (interfaz de usuario de Data Factory)
En esta sección se muestra cómo conectar una instancia de Azure-SSIS Integration Runtime existente a una red virtual (clásica o de Azure Resource Manager) mediante Azure Portal y la interfaz de usuario de Data Factory. 

Antes de conectar Azure-SSIS Integration Runtime a la red virtual debe configurar correctamente esta última. Siga los pasos de la sección que se aplica a su tipo de red virtual (clásica o de Azure Resource Manager). Después, siga los pasos de la tercera sección para conectar Azure-SSIS Integration Runtime a la red virtual. 

### <a name="configure-an-azure-resource-manager-virtual-network"></a>Configuración de una red virtual creada con el método de Azure Resource Manager

Use el portal para configurar una red virtual de Azure Resource Manager antes conectar Azure-SSIS Integration Runtime a ella.

1. Inicie Microsoft Edge o Google Chrome. Actualmente, estos exploradores web son los únicos que admiten la interfaz de usuario de Data Factory. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 

1. Seleccione **Más servicios**. Filtre y seleccione **Redes virtuales**. 

1. Filtre y seleccione su red virtual en la lista. 

1. En la página **Red virtual**, seleccione **Propiedades**. 

1. Seleccione el botón de copia en **ID. DE RECURSO** para copiar el identificador de recurso de la red virtual en el Portapapeles. Guarde el identificador del Portapapeles en OneNote o en un archivo. 

1. En el menú de la izquierda, seleccione **Subredes**. Asegúrese de que el número de direcciones disponibles es mayor que el número de nodos de Azure-SSIS Integration Runtime. 

1. Compruebe que el proveedor de Azure Batch está registrado en la suscripción de Azure que tiene la red virtual. O bien, registre el proveedor de Azure Batch. Si ya tiene una cuenta de Azure Batch en su suscripción, significa que la suscripción está registrada para Azure Batch. Si crea la instancia de Integration Runtime para la integración de SSIS en Azure en el portal de Data Factory, el proveedor de Azure Batch se registrará automáticamente. 

   a. En Azure Portal, en el menú de la izquierda, seleccione **Suscripciones**. 

   b. Seleccione su suscripción. 

   c. A la izquierda, seleccione **Proveedores de recursos** y confirme que **Microsoft.Batch** es un proveedor registrado. 

   ![Confirmación del estado "Registrado"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Si no ve **Microsoft.Batch** en la lista, regístrelo. Para ello, [cree una cuenta de Azure Batch vacía](../batch/batch-account-create-portal.md) en su suscripción. Puede eliminarlo más tarde. 

### <a name="configure-a-classic-virtual-network"></a>Configuración de una red virtual a través del método clásico
Use el portal para configurar una red virtual clásica antes conectar Azure-SSIS Integration Runtime a ella. 

1. Inicie Microsoft Edge o Google Chrome. Actualmente, estos exploradores web son los únicos que admiten la interfaz de usuario de Data Factory. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 

1. Seleccione **Más servicios**. Filtre y seleccione **Redes virtuales (clásicas)** . 

1. Filtre y seleccione su red virtual en la lista. 

1. En la página **Red virtual (clásica)** , seleccione **Propiedades**. 

   ![Identificador de recurso de red virtual clásica](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

1. Seleccione el botón de copia en **ID. DE RECURSO** para copiar el identificador de recurso de la red clásica en el Portapapeles. Guarde el identificador del Portapapeles en OneNote o en un archivo. 

1. En el menú de la izquierda, seleccione **Subredes**. Asegúrese de que el número de direcciones disponibles es mayor que el número de nodos de Azure-SSIS Integration Runtime. 

   ![Número de direcciones disponibles en la red virtual](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

1. Una **MicrosoftAzureBatch** al rol **Colaborador de la máquina virtual clásica** de la red virtual. 

    a. En el menú de la izquierda, seleccione **Control de acceso (IAM)** y, después, la pestaña **Asignaciones de roles**. 

    ![Botones "Control de acceso" y "Agregar"](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Seleccione **Agregar asignación de roles**.

    c. En la página **Agregar asignación de roles**, en **Rol**, seleccione **Colaborador de la máquina virtual clásica**. En el cuadro **Seleccionar**, pegue **ddbf3205-c6bd-46ae-8127-60eb93363864** y seleccione **Microsoft Azure Batch** en la lista de resultados de la búsqueda. 

    ![Resultados de la búsqueda en la página "Agregar asignación de roles"](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    d. Seleccione **Guardar** para guardar la configuración y cerrar la página. 

    ![Guardado de la configuración de acceso](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    e. Confirme que ve **Microsoft Azure Batch** en la lista de colaboradores. 

    ![Confirmación del acceso a Azure Batch](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

1. Compruebe que el proveedor de Azure Batch está registrado en la suscripción de Azure que tiene la red virtual. O bien, registre el proveedor de Azure Batch. Si ya tiene una cuenta de Azure Batch en su suscripción, significa que la suscripción está registrada para Azure Batch. Si crea la instancia de Integration Runtime para la integración de SSIS en Azure en el portal de Data Factory, el proveedor de Azure Batch se registrará automáticamente. 

   a. En Azure Portal, en el menú de la izquierda, seleccione **Suscripciones**. 

   b. Seleccione su suscripción. 

   c. A la izquierda, seleccione **Proveedores de recursos** y confirme que **Microsoft.Batch** es un proveedor registrado. 

   ![Confirmación del estado "Registrado"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Si no ve **Microsoft.Batch** en la lista, regístrelo. Para ello, [cree una cuenta de Azure Batch vacía](../batch/batch-account-create-portal.md) en su suscripción. Puede eliminarlo más tarde. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Unión de la instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual

Después de configurar la red virtual de Azure Resource Manager o la red virtual clásica, puede conectar Azure-SSIS Integration Runtime a la red virtual:

1. Inicie Microsoft Edge o Google Chrome. Actualmente, estos exploradores web son los únicos que admiten la interfaz de usuario de Data Factory. 

1. En [Azure Portal](https://portal.azure.com), en el menú izquierdo, seleccione **Factorías de datos**. Si no ve **Factorías de datos** en el menú, seleccione **Más servicios** y luego, en la sección **INTELIGENCIA Y ANÁLISIS**, seleccione **Factorías de datos**. 

   ![Lista de factorías de datos](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Seleccione su factoría de datos con Azure-SSIS Integration Runtime en la lista. Verá la página principal de la factoría de datos. Seleccione el icono **Crear e implementar**. Verá la interfaz de usuario de Data Factory en una pestaña independiente. 

   ![Página principal Factoría de datos](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. En la interfaz de usuario de Data Factory, vaya a la pestaña **Editar**, seleccione **Conexiones** y vaya a la pestaña **Integration Runtimes** (Entornos de ejecución de integración). 

   ![Pestañas "Integration runtimes" (Entornos de ejecución de integración)](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Si Azure-SSIS Integration Runtime está en funcionamiento, en la lista **Integration Runtimes** (Entornos de ejecución de integración), en la columna **Acciones**, seleccione el botón **Detener** de Azure-SSIS Integration Runtime. No puede editar ningún entorno de ejecución de integración hasta que lo detenga. 

   ![Detención de Integration Runtime](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. En la lista **Integration Runtimes** (Entornos de ejecución de integración), en la columna **Actions** (Acciones), seleccione el botón **Edición** de Azure-SSIS Integration Runtime. 

   ![Edición de Integration Runtime](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. En el panel **Integration Runtime Setup** (Configuración de Integration Runtime), avance por las páginas **Configuración general** y **Configuración de SQL** seleccionando el botón **Siguiente**. 

1. En la página **Configuración avanzada**: 

   a. Seleccione la casilla situada junto a **Select a VNet** (Seleccionar una red virtual). 

   b. En **Suscripción**, seleccione su suscripción de Azure. En la suscripción, puede seleccionar una red virtual existente. 
  
   c. En **Nombre de red virtual**, seleccione la red virtual. 

   d. En **Nombre de subred**, seleccione la subred de la red virtual. 

   e. Si también desea configurar o administrar un entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS Integration Runtime, active la casilla **Set-up Self-Hosted** (Configurar autohospedado). Para más información, consulte [Configuración del entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis).

   f. Seleccione el botón **VNet Validation** (Validación de red virtual). Si la validación es correcta, seleccione el botón **Siguiente**. 

   ![Configuración avanzada de Integration Runtime](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)

1. En la página **Resumen**, revise toda la configuración de Azure-SSIS Integration Runtime. Después, seleccione el botón **Actualizar**.

1. Inicie Azure-SSIS Integration Runtime mediante la selección del botón **Iniciar** de la columna **Acciones** de Azure-SSIS IR. Se tarda aproximadamente entre 20 y 30 minutos en iniciar la instancia de Azure-SSIS IR que se conecta a una red virtual. 

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="configure-a-virtual-network"></a>Configuración de una red virtual
Para poder conectar Azure-SSIS Integration Runtime a una red virtual, debe configurar dicha red. Para configurar automáticamente los permisos y valores de la red virtual para que Azure-SSIS Integration Runtime se conecte a la red virtual, agregue el siguiente script:

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
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

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Creación de una instancia de Integration Runtime para la integración de SSIS en Azure y su unión a una red virtual
Puede crear una instancia de Integration Runtime para la integración de SSIS en Azure y unirla a una red virtual al mismo tiempo. Para ver el script completo y las instrucciones, consulte [Creación de una instancia de Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md#use-azure-powershell-to-create-an-integration-runtime).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Unión de una instancia de Integration Runtime para la integración de SSIS en Azure ya existente a una red virtual
El artículo [Creación de una instancia de Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md) muestra cómo crear una instancia de Azure-SSIS Integration Runtime y conectarla a una red virtual en el mismo script. Si ya tiene una instancia de Azure-SSIS IR, siga estos pasos para conectarla a la red virtual: 
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
Debe detener Azure-SSIS Integration Runtime para poder conectarlo a una red virtual. Este comando libera todos sus nodos y detiene la facturación:

```powershell
Stop-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Configuración de la red virtual para que se una la instancia de Integration Runtime para la integración de SSIS en Azure

Para configurar las opciones de la red virtual a la que se conectará Azure-SSIS, use este script: 

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
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
        # Assign VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Configurar la instancia de Integration Runtime de SSIS de Azure
Para configurar Azure-SSIS Integration Runtime para que se conecte a la red virtual, ejecute el comando `Set-AzDataFactoryV2IntegrationRuntime`: 

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Iniciar la instancia de Integration Runtime de SSIS de Azure
Para iniciar Azure-SSIS Integration Runtime, ejecute el siguiente comando: 

```powershell
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Este comando tarda entre 20 y 30 minutos en finalizar.

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de Azure-SSIS IR, consulte los siguientes artículos: 
- [Azure-SSIS IR](concepts-integration-runtime.md#azure-ssis-integration-runtime). En este artículo se proporciona información conceptual general acerca de los entornos de ejecución de integración, incluido Azure-SSIS IR. 
- [Tutorial: Implementación de paquetes SSIS en Azure](tutorial-create-azure-ssis-runtime-portal.md). En este tutorial se proporcionan instrucciones paso a paso para crear Azure-SSIS IR. Usa Azure SQL Database para hospedar el catálogo de SSIS. 
- [Creación de una instancia de Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md). En este artículo se amplía el tutorial. Proporciona instrucciones acerca del uso de Azure SQL Database con puntos de conexión de servicio de red virtual o una instancia administrada en una red virtual para hospedar el catálogo SSIS. Muestra cómo conectar Azure-SSIS IR a una red virtual. 
- [Monitor an Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Supervisión de una instancia de Integration Runtime de SSIS de Azure). En este artículo se muestra cómo obtener información acerca de Azure-SSIS IR. Proporciona descripciones del estado de la información devuelta. 
- [Administración de Integration Runtime de SSIS de Azure](manage-azure-ssis-integration-runtime.md). En este artículo se muestra cómo iniciar, detener o eliminar Azure-SSIS IR. También se muestra cómo escalar horizontalmente la instancia de Integration Runtime para la integración de SSIS en Azure mediante la adición de nodos.
