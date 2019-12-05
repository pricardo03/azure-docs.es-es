---
title: Introducción a las etiquetas de servicio de Azure
titlesuffix: Azure Virtual Network
description: Obtenga información sobre las etiquetas de servicio. Las etiquetas de servicio ayudan a minimizar la complejidad de la creación de reglas de seguridad.
services: virtual-network
documentationcenter: na
author: jispar
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/22/2019
ms.author: jispar
ms.reviewer: kumud
ms.openlocfilehash: 33ee7351e547ee5ef57ef07f67ba6f5f4410b57f
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74384141"
---
# <a name="virtual-network-service-tags"></a>Etiquetas de servicio de red virtual 
<a name="network-service-tags"></a>

Una etiqueta de servicio representa un grupo de prefijos de direcciones IP de un servicio de Azure determinado. Ayuda a minimizar la complejidad de las actualizaciones frecuentes de las reglas de seguridad de red. Puede usar etiquetas de servicio para definir controles de acceso a la red en [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) o [Azure Firewall](https://docs.microsoft.com/azure/firewall/service-tags). Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, **ApiManagement**) en el campo de *origen* o *destino* apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian. 

## <a name="available-service-tags"></a>Etiquetas de servicio disponibles
En la tabla siguiente se incluyen todas las etiquetas de servicio disponibles que se pueden usar en reglas de [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules).

Las columnas indican si la etiqueta es:

- Adecuada para reglas que cubren el tráfico entrante o saliente
- Compatible con el ámbito [regional](https://azure.microsoft.com/regions) 
- Utilizable en las reglas de [Azure Firewall](https://docs.microsoft.com/azure/firewall/service-tags)

De forma predeterminada, las etiquetas de servicio reflejan los intervalos de toda la nube.  Algunas etiquetas de servicio también permiten un control más preciso restringiendo los intervalos de direcciones IP correspondientes a una región especificada.  Por ejemplo, mientras que la etiqueta de servicio **Storage** representa a Azure Storage para toda la nube, **Storage.WestUS** se limita solo a los intervalos de direcciones IP de almacenamiento de la región Oeste de EE. UU.  En las descripciones de cada etiqueta de servicio siguiente se indica si admiten este ámbito regional.  



| Etiqueta | Propósito | ¿Se puede usar para tráfico entrante o saliente? | ¿Puede ser regional? | ¿Se puede usar con Azure Firewall? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ApiManagement** | Tráfico de administración para implementaciones dedicadas de APIM. | Ambos | Sin | Sí |
| **AppService**    | Servicio App Service. Esta etiqueta se recomienda para reglas de seguridad de salida a front-ends de WebApp. | Salida | Sí | Sí |
| **AppServiceManagement** | Tráfico de administración para las implementaciones dedicadas de App Service Environment. | Ambos | Sin | Sí |
| **AzureActiveDirectory** | Servicio de Azure Active Directory. | Salida | Sin | Sí |
| **AzureActiveDirectoryDomainServices** | Tráfico de administración para las implementaciones dedicadas de Azure Active Directory Domain Services. | Ambos | Sin | Sí |
| **AzureBackup** |Servicio Azure Backup.<br/><br/>*Nota:* Esta etiqueta tiene dependencia de las etiquetas **Storage** y **AzureActiveDirectory**. | Salida | Sin | Sí |
| **AzureCloud** | Todos las [direcciones IP públicas del centro de recursos](https://www.microsoft.com/download/details.aspx?id=41653). | Salida | Sí | Sí |
| **AzureConnectors** | Conectores de Logic Apps para las conexiones de sondeo y back-end. | Entrada | Sí | Sí |
| **AzureContainerRegistry** | Servicio Azure Container Registry. | Salida | Sí | Sí |
| **AzureCosmosDB** | Servicio Azure Cosmos Database. | Salida | Sí | Sí |
| **AzureDataLake** | Servicio de Azure Data Lake. | Salida | Sin | Sí |
| **AzureIoTHub** | Servicio de Azure IoT Hub. | Salida | Sin | Sin |
| **AzureKeyVault** | Servicio de Azure Key Vault.<br/><br/>*Nota:* Esta etiqueta tiene dependencia en la etiqueta **AzureActiveDirectory**. | Salida | Sí | Sí |
| **AzureLoadBalancer** | Equilibrador de carga de la infraestructura de Azure. La etiqueta se traduce en la [dirección IP virtual del host](security-overview.md#azure-platform-considerations) (168.63.129.16) donde se originan los sondeos de mantenimiento de Azure. Si no usa el equilibrador de carga de Azure, puede reemplazar esta regla. | Ambos | Sin | Sin |
| **AzureMachineLearning** | Azure Machine Learning Service. | Salida | Sin | Sí |
| **AzureMonitor** | Log Analytics, App Insights, AzMon y métricas personalizadas (puntos de conexión GiG).<br/><br/>*Nota:* Para Log Analytics, esta etiqueta tiene dependencia de la etiqueta **Storage**. | Salida | Sin | Sí |
| **AzurePlatformDNS** | El servicio DNS de infraestructura básica (predeterminado).<br/><br>Puede usar esta etiqueta para deshabilitar el DNS predeterminado. Tenga cuidado al usar esta etiqueta. Se recomienda leer las [consideraciones sobre la plataforma Azure](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations). Se recomienda realizar pruebas antes de usar esta etiqueta. | Salida | Sin | Sin |
| **AzurePlatformIMDS** | IMDS, que es un servicio de infraestructura básico.<br/><br/>Puede usar esta etiqueta para deshabilitar el IMDS predeterminado.  Tenga cuidado al usar esta etiqueta. Se recomienda leer las [consideraciones sobre la plataforma Azure](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations). Se recomienda realizar pruebas antes de usar esta etiqueta. | Salida | Sin | Sin |
| **AzurePlatformLKM** | Servicio de administración de claves o licencias de Windows.<br/><br/>Puede usar esta etiqueta para deshabilitar los valores predeterminados para licencias. Tenga cuidado al usar esta etiqueta.  Se recomienda leer las [consideraciones sobre la plataforma Azure](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations). Se recomienda realizar pruebas antes de usar esta etiqueta. | Salida | Sin | Sin |
| **AzureTrafficManaged** | Direcciones IP de sondeo de Azure Traffic Manager.<br/><br/>En [Preguntas más frecuentes sobre Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs), puede encontrar más información acerca de las direcciones IP de sondeo de Azure Traffic Manager. | Entrada | Sin | Sí |  
| **BatchNodeManagement** | Tráfico de administración para implementaciones dedicadas de Azure Batch. | Ambos | Sin | Sí |
| **CognitiveServicesManagement** | Los intervalos de direcciones para el tráfico para Cognitive Services. | Salida | Sin | Sin |
| **Dynamics365ForMarketingEmail** | Los intervalos de direcciones del servicio de correo electrónico de marketing de Dynamics 365. | Salida | Sí | Sin |
| **EventHub** | Servicio Azure Event Hub. | Salida | Sí | Sí |
| **GatewayManager** | Tráfico de administración para implementaciones dedicadas de instancias de VPN y App Gateway. | Entrada | Sin | Sin |
| **Internet** | Espacio de direcciones IP que se encuentra fuera de la red virtual y es accesible a través de la red pública de Internet.<br/><br/>El intervalo de direcciones incluye el [espacio de direcciones IP públicas propiedad de Azure](https://www.microsoft.com/download/details.aspx?id=41653). | Ambos | Sin | Sin |
| **MicrosoftContainerRegistry** | Servicio Microsoft Container Registry. | Salida | Sí | Sí |
| **Service Bus** | Servicio Azure Service Bus que usa el nivel de servicio Premium. | Salida | Sí | Sí |
| **ServiceFabric** | Servicio de Service Fabric. | Salida | Sin | Sin |
| **Sql** | Servicios Azure SQL Database, Azure Database for MySQL, Azure Database for PostgreSQL y Azure SQL Data Warehouse.<br/><br/>*Nota:* Esta etiqueta representa el servicio, no instancias específicas del mismo. Por ejemplo, la etiqueta representa el servicio Azure SQL Database, pero no una cuenta de un servidor o base de datos SQL específicos. | Salida | Sí | Sí |
| **SqlManagement** | Tráfico de administración para implementaciones dedicadas de SQL. | Ambos | Sin | Sí |
| **Storage** | Servicio Azure Storage. <br/><br/>*Nota:* La etiqueta representa el servicio, no instancias específicas del mismo. Por ejemplo, la etiqueta representa el servicio Azure Storage, pero no una cuenta de específica de este. | Salida | Sí | Sí |
| **VirtualNetwork** | El espacio de direcciones de red virtual (todos los intervalos de direcciones IP definidos para la red virtual), todos los espacios de direcciones locales conectados, las redes virtuales [del mismo nivel](virtual-network-peering-overview.md) o redes virtuales conectadas a una [puerta de enlace de red virtual](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%3ftoc.json), la [dirección IP virtual del host](security-overview.md#azure-platform-considerations) y los prefijos de dirección usados en las [rutas definidas por el usuario](virtual-networks-udr-overview.md). Tenga en cuenta que esta etiqueta también puede contener rutas predeterminadas. | Ambos | Sin | Sin |

>[!NOTE]
>Cuando se trabaja en el entorno de *Clásico* (anterior a Azure Resource Manager), se admite un conjunto seleccionado de las etiquetas anteriores.  Usan una ortografía alternativa:

| Ortografía clásica | Etiqueta de Resource Manager equivalente |
|---|---|
| AZURE_LOADBALANCER | AzureLoadBalancer |
| INTERNET | Internet |
| VIRTUAL_NETWORK | VirtualNetwork |

> [!NOTE]
> Las etiquetas de servicios de los servicios de Azure indican los prefijos de dirección de la nube específica que se va a usar. Por ejemplo, los intervalos de direcciones IP subyacentes correspondientes al valor de la etiqueta **Sql** variarán entre la nube pública de Azure y la nube de China de Azure.

> [!NOTE]
> Si implementa un [punto de conexión de servicio de red virtual](virtual-network-service-endpoints-overview.md) para un servicio, como Azure Storage o Azure SQL Database, Azure agrega una [ruta](virtual-networks-udr-overview.md#optional-default-routes) a una subred de red virtual para el servicio. Los prefijos de dirección de la ruta son los mismos prefijos de dirección, o intervalos CIDR, que la etiqueta de servicio correspondiente.



## <a name="service-tags-in-on-premises"></a>Etiquetas de servicio en un entorno local  
Puede obtener información de intervalo y la etiqueta de servicio actual para incluirla como parte de las configuraciones del firewall local.  Esta información es la lista actual en un momento dado de los intervalos de direcciones IP correspondientes a cada etiqueta de servicio.  La información se puede obtener mediante programación o a través de la descarga de archivos JSON como se indica a continuación.

### <a name="service-tag-discovery-api-public-preview"></a>Service Tag Discovery API (versión preliminar pública)
Puede recuperar mediante programación la lista actual de etiquetas de servicio con los detalles del intervalo de direcciones IP:

- [REST](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
- [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag?view=azps-2.8.0&viewFallbackFrom=azps-2.3.2)
- [CLI de Azure](https://docs.microsoft.com/cli/azure/network?view=azure-cli-latest#az-network-list-service-tags)

> [!NOTE]
> En su versión preliminar pública, Discovery API puede devolver información que no es tan actual como las descargas JSON (a continuación).


### <a name="discover-service-tags-using-downloadable-json-files"></a>Detección de etiquetas de servicio mediante archivos JSON descargables 
Puede descargar archivos JSON que contengan la lista actual de etiquetas de servicio con los detalles del intervalo de direcciones IP. Se actualizan y publican semanalmente.  Las ubicaciones de cada nube son:

- [Azure público](https://www.microsoft.com/download/details.aspx?id=56519)
- [Azure Government](https://www.microsoft.com/download/details.aspx?id=57063)  
- [Azure en China](https://www.microsoft.com/download/details.aspx?id=57062) 
- [Azure Alemania](https://www.microsoft.com/download/details.aspx?id=57064)   

> [!NOTE]
>Un subconjunto de esta información se ha publicado previamente en archivos XML para [Azure público](https://www.microsoft.com/download/details.aspx?id=41653), [Azure China](https://www.microsoft.com/download/details.aspx?id=42064) y [Azure Alemania](https://www.microsoft.com/download/details.aspx?id=54770). Estas descargas XML dejarán de usarse en el 30 de junio de 2020 y ya no estarán disponibles después de esa fecha. Migre mediante Discovery API o las descargas de archivos JSON como se describió anteriormente.

### <a name="tips"></a>Sugerencias 
- Puede detectar actualizaciones de una publicación a la siguiente a través de valores *changeNumber* aumentados dentro del archivo JSON. Cada subsección (por ejemplo, **Storage.WestUS**) tiene su propio *changeNumber* que se incrementa a medida que se producen cambios.  El nivel superior de *changeNumber* del archivo se incrementa cuando alguna de las subsecciones ha cambiado.
- Para ejemplos de cómo analizar la información de la etiqueta de servicio (por ejemplo, obtener todos los intervalos de direcciones para Storage en Oeste de EE. UU.), consulte la documentación de [PowerShell de Service Tag Discovery API](https://aka.ms/discoveryapi_powershell).

## <a name="next-steps"></a>Pasos siguientes
- Aprenda a [crear un grupo de seguridad de red](tutorial-filter-network-traffic.md).

