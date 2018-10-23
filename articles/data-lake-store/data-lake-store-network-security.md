---
title: Seguridad de red de Azure Data Lake Storage Gen1 | Microsoft Docs
description: Funcionamiento del firewall para las direcciones IP y de la integración de redes virtuales en Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/09/2018
ms.author: elsung
ms.openlocfilehash: 0da5962bc0b48a387ee82a1db36099682e14bca3
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49168193"
---
# <a name="virtual-network-integration-for-azure-data-lake-storage-gen1---preview"></a>Integración de Virtual Network para Azure Data Lake Storage Gen1: versión preliminar

Presentación de la integración de Virtual Network para Azure Data Lake Storage Gen1 (versión preliminar). La integración de redes virtuales permite evitar el acceso no autorizado a las cuentas de Azure Data Lake Storage Gen1 mediante el bloqueo a las redes virtuales y las subredes distintas de las suyas. Ahora puede configurar su cuenta de ADLS Gen1 para que solo acepte tráfico desde las redes y subredes especificadas y bloquee el acceso desde las demás. Esto ayuda a proteger la cuenta de ADLS frente a amenazas externas.

Para la integración de redes virtuales para ADLS Gen1 se utiliza la seguridad del punto de conexión del servicio de redes virtuales entre su instancia de Virtual Network y el servicio Azure Active Directory para generar notificaciones de seguridad adicionales en el token de acceso. Estas notificaciones sirven para autenticar la red virtual en la cuenta de ADLS Gen1 y permitir el acceso.

> [!NOTE]
> Se trata de tecnología de versión preliminar y no se recomienda su uso en entornos de producción.
>
> No hay cargos adicionales asociados al uso de estas funcionalidades. Su cuenta se facturará según la tarifa estándar para ADLS Gen1 ([precios](https://azure.microsoft.com/pricing/details/data-lake-store/?cdn=disable)) y los servicios de Azure que use ([precios](https://azure.microsoft.com/pricing/#product-picker)).

## <a name="scenarios-for-vnet-integration-for-adls-gen1"></a>Escenarios de integración de redes virtuales para ADLS Gen1

La integración de redes virtuales en ADLS Gen1 permite restringir el acceso a su cuenta de ADLS Gen1 a las redes virtuales y las subredes designadas.  Otras redes o máquinas virtuales de Azure no podrán acceder a la cuenta una vez bloqueada la subred o la red virtual especificada.  Funcionalmente, la integración de redes virtuales en ADLS Gen1 permite el mismo escenario que los [puntos de conexión del servicio de redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview).  Hay algunas diferencias importantes que se detallan en las siguientes secciones. 

![Diagrama del escenario para la integración de redes virtuales en ADLS Gen1](media/data-lake-store-network-security/scenario-diagram.png)

> [!NOTE]
> Además de las reglas de red virtual, también se pueden usar reglas de firewall para las direcciones IP existentes para permitir el acceso desde redes locales. 

## <a name="optimal-routing-with-adls-gen1-vnet-integration"></a>Enrutamiento óptimo con la integración de redes virtuales en ADLS Gen1

Una ventaja clave de los puntos de conexión del servicio de redes virtuales es el [enrutamiento óptimo](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview#key-benefits) desde la red virtual.  Para llevar a cabo el mismo enrutamiento óptimo en las cuentas de ADLS Gen1, enrute la red virtual a la cuenta de ADLS Gen1 mediante las siguientes [rutas definidas por el usuario](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined):

- **Dirección IP pública de ADLS**: use la dirección IP pública para las cuentas de ADLS Gen1 de destino.  Puede identificar las direcciones IP de las cuentas de ADLS Gen1 al [resolver los nombres DNS](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-connectivity-from-vnets#enabling-connectivity-to-azure-data-lake-storage-gen1-from-vms-with-restricted-connectivity) de las cuentas.  Cree una entrada independiente para cada dirección.

```azurecli
# Create a Route table for your resource group
az network route-table create --resource-group $RgName --name $RouteTableName

# Create Route Table Rules for ADLS Public IP Addresses
# There will be one rule per ADLS Public IP Addresses 
az network route-table route create --name toADLSregion1 --resource-group $RgName --route-table-name $RouteTableName --address-prefix <ADLS Public IP Address> --next-hop-type Internet

# Update the VNet and apply the newly created Route Table to it
az network vnet subnet update --vnet-name $VnetName --name $SubnetName --resource-group $RgName --route-table $RouteTableName
```

## <a name="data-exfiltration-from-the-customer-vnet"></a>Extrusión de datos desde la red virtual del cliente

Además de proteger el acceso a las cuentas de ADLS desde Virtual Network, quizá le interese también asegurarse de que no se produce extrusión de datos a cuentas no autorizadas.

Nuestra recomendación es usar una solución de firewall de la red virtual para filtrar el tráfico saliente en función de la dirección URL de la cuenta de destino y permitir el acceso únicamente a las cuentas de ADLS Gen1 autorizadas.

Algunas de las opciones disponibles son las siguientes:
- [Firewall de Azure](https://docs.microsoft.com/azure/firewall/overview): puede [implementar y configurar una instancia de Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) para la red virtual y proteger el tráfico saliente de ADLS y restringirlo únicamente a la dirección URL de la cuenta conocida y autorizada.
- Firewall de [aplicación de red virtual](https://azure.microsoft.com/solutions/network-appliances/): si el administrador solo autoriza el uso de ciertos proveedores firewall comercial, puede usar una solución de firewall de aplicación de red virtual, que se encuentra disponible en Azure Marketplace para realizar la misma función.

> [!NOTE]
> El uso de firewalls en la ruta de acceso de datos supondrá un salto adicional en eta y podría afectar al rendimiento de la red en cuanto al intercambio de datos de un extremo a otro, incluida la disponibilidad de rendimiento y la latencia de la conexión. 

## <a name="limitations"></a>Limitaciones
1.  Los clústeres de HDInsight deben crearse de nuevo tras agregarse a la versión preliminar.  Los clústeres que se crearan antes de que la compatibilidad de la integración de redes virtuales en ADLS Gen1 estuviera disponible deberán volverse a crear para admitir esta nueva característica. 
2.  Al crear un nuevo clúster de HDInsight, la selección de una cuenta de ADLS Gen1 con la integración de redes virtuales habilitada provocará un error en el proceso. Primero debe deshabilitar la regla de red virtual o marcar **Allow access from all networks and services** (Permitir el acceso desde todas las redes y servicios) a través de la hoja **Firewall y redes virtuales** de la cuenta de ADLS.  Consulte la sección [Excepciones](##Exceptions) para más información.
3.  La versión preliminar de la integración de redes virtuales en ADLS Gen1 no funciona con [Managed Identities for Azure Resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).  
4.  Desde el portal no se puede acceder a los datos de archivo o carpeta de la cuenta de ADLS Gen1 con las redes virtuales habilitadas.  Esto incluye el acceso desde una máquina virtual que esté dentro de la red virtual y las actividades como el uso del Explorador de datos.  Las actividades de administración de cuentas seguirán funcionando.  A los datos de archivo o carpeta de la cuenta de ADLS con las redes virtuales habilitadas se accede desde cualquier recurso ajeno al portal: el acceso al SDK, los scripts de PowerShell, otros servicios de Azure (cuyo origen no sea el portal), etc. 

## <a name="configuration"></a>Configuración

### <a name="step1-configure-your-vnet-to-use-aad-service-endpoint"></a>Paso 1: configuración de la red virtual para usar el punto de conexión del servicio AAD
1.  Vaya a Azure Portal e inicie sesión en la cuenta. 
2.  [Cree una nueva red virtual](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) en la suscripción o vaya a una red virtual existente.  La red virtual debe encontrarse en la misma región que la cuenta de ADLS Gen 1. 
3.  En la hoja de la red virtual, elija **Puntos de conexión de servicio**. 
4.  Haga clic en **Agregar** para agregar un nuevo punto de conexión de servicio.
![Incorporación de un punto de conexión de servicio de red virtual](media/data-lake-store-network-security/config-vnet-1.png)
5.  Elija **Microsoft.AzureActiveDirectory** como servicio para el punto de conexión.
![Selección del punto de conexión del servicio Microsoft.azureactivedirectory](media/data-lake-store-network-security/config-vnet-2.png)
6.  Elija las subredes para las que se va a permitir la conectividad y haga clic en **Agregar**.
![Selección de la subred](media/data-lake-store-network-security/config-vnet-3.png)
7.  La incorporación del punto de conexión de servicio puede tardar hasta 15 minutos. Una vez agregado, se mostrará en la lista. Compruebe que se muestra y que todos los detalles son como se han configurado. 
![Incorporación correcta del punto de conexión de servicio](media/data-lake-store-network-security/config-vnet-4.png)

### <a name="step-2-set-up-the-allowed-vnetsubnet-for-your-adls-gen1-account"></a>Paso 2: configuración de la red virtual o la subred permitida para la cuenta de ADLS Gen1
1.  Después de configurar la red virtual, [cree una nueva cuenta de Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md#create-a-data-lake-storage-gen1-account) en la suscripción o vaya a una existente. La cuenta de ADLS Gen1 debe encontrarse en la misma región que la red virtual. 
2.  Elija **Firewall y redes virtuales**.

  > [!NOTE]
  > Si no ve **Firewall y redes virtuales** en la configuración, a continuación, cierre la sesión en el portal. Cierre el explorador. Limpie la caché del explorador. Reinicie la máquina y vuelva a intentarlo.

  ![Incorporación de una regla de red virtual a la cuenta de ADLS](media/data-lake-store-network-security/config-adls-1.png)
3.  Elija **Redes seleccionadas**. 
4.  Haga clic en **Agregar red virtual existente**.
  ![Incorporación de una red virtual existente](media/data-lake-store-network-security/config-adls-2.png)
5.  Elija las redes virtuales y las subredes para las cuales permitir la conectividad y haga clic en **Agregar**.
  ![Elección de la red virtual y las subredes](media/data-lake-store-network-security/config-adls-3.png)
6.  Asegúrese de que las redes virtuales y las subredes aparecen correctamente en la lista y haga clic en **Guardar**.
  ![Guardado de la nueva regla](media/data-lake-store-network-security/config-adls-4.png)

  > [!NOTE]
  > La configuración puede tardar hasta 5 minutos en entrar en vigor después de guardarla.

7.  [Opcional] Además de las redes virtuales y las subredes, si desea permitir la conectividad desde direcciones IP específicas, puede hacerlo en la sección **Firewall** de la misma página. 

## <a name="exceptions"></a>Excepciones
Hay dos casillas en el área Excepciones de la hoja **Firewall y redes virtuales** que permiten la conectividad desde un conjunto de servicios y máquinas virtuales de Azure.
![Excepciones de Firewall y redes virtuales](media/data-lake-store-network-security/firewall-exceptions.png)
- **Allow all Azure services to access this Data Lake Storage Gen1 account** (Permitir que todos los servicios accedan a esta cuenta de Data Lake Storage Gen1) permite que todos los servicios de Azure, como Azure Data Factory, Event Hubs, Azure Virtual Machines, etc. se comuniquen con la cuenta de ADLS.

- **Allow Azure Data Lake Analytics to access this Data Lake Storage Gen1 account** (Permitir que Azure Data Lake Analytics acceda a esta cuenta de Data Lake Storage Gen1) permite que el servicio Azure Data Lake Analytics se conecte con esta cuenta de ADLS. 

Se recomienda mantener estas excepciones desactivadas y solo activarlas cuando se necesite conectar con estos servicios desde fuera de la red virtual.
