---
title: Información general de las etiquetas del servicio Azure Firewall
description: Este artículo es una introducción a las etiquetas del servicio Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 2/21/2019
ms.author: victorh
ms.openlocfilehash: 9ecc5d3779ac6632f4a5c05914cbb0e756c79e91
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56458222"
---
# <a name="azure-firewall-service-tags"></a>Etiquetas de servicio de Azure Firewall

Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad. No puede crear su propia etiqueta de servicio, ni especificar qué direcciones IP se incluyen dentro de una etiqueta. Microsoft administra los prefijos de direcciones que incluye la etiqueta de servicio y actualiza automáticamente esta a medida que las direcciones cambian.

Las etiquetas de servicio de Azure Firewall se pueden usar en el campo de destino de las reglas de red. Puede usarlas en lugar de direcciones IP específicas.

> [!NOTE]
> Las etiquetas de servicio se agregan a las regiones de forma incremental y estarán disponibles en todas las regiones en un futuro próximo.

## <a name="supported-service-tags"></a>Etiquetas de servicio admitidas

Las siguientes etiquetas de servicio están disponibles para su uso en las reglas de red de Azure Firewall:

* **AzureCloud** (solo Resource Manager): esta etiqueta denota el espacio de direcciones IP de Azure e incluye todas las [direcciones IP públicas del centro de datos](https://www.microsoft.com/download/details.aspx?id=41653). Si especifica *AzureCloud* como valor, el tráfico a las direcciones IP públicas de Azure se permite o deniega. Si solo desea permitir el acceso a AzureCloud en una [región](https://azure.microsoft.com/regions) determinada, puede especificarla. Por ejemplo, si desea permitir el acceso a AzureCloud solo en la región este de EE. UU., puede especificar *AzureCloud.EastUS* como etiqueta de servicio. 
* **AzureTrafficManager** (solo Resource Manager): esta etiqueta denota el espacio de direcciones IP de las direcciones IP de sondeo de Azure Traffic Manager. En [Preguntas más frecuentes sobre Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs), puede encontrar más información acerca de las direcciones IP de sondeo de Azure Traffic Manager. 
* **Storage** (solo Resource Manager): esta etiqueta denota el espacio de direcciones IP del servicio Azure Storage. Si especifica *Storage* como valor, el tráfico al almacenamiento se permite o se deniega. Si solo desea permitir el acceso al almacenamiento de una determinada [región](https://azure.microsoft.com/regions), puede especificarla. Por ejemplo, si desea permitir el acceso a Azure Storage solo en la región este de EE. UU., puede especificar *Storage.EastUS* como etiqueta de servicio. La etiqueta representa el servicio, no instancias específicas del mismo. Por ejemplo, la etiqueta representa el servicio Azure Storage, pero no una cuenta de específica de este.
* **Sql** (solo Resource Manager): esta etiqueta denota los prefijos de direcciones de los servicios Azure SQL Database y Azure SQL Data Warehouse. Si especifica *Sql* como valor, el tráfico a Sql se permite o se deniega. Si solo desea permitir el acceso a SQL en una [región](https://azure.microsoft.com/regions) determinada, puede especificarla. Por ejemplo, si desea permitir el acceso a Azure SQL Database solo en la región este de EE. UU., puede especificar *Sql.EastUS* como etiqueta de servicio. La etiqueta representa el servicio, no instancias específicas del mismo. Por ejemplo, la etiqueta representa el servicio Azure SQL Database, pero no una cuenta de un servidor o base de datos SQL específicos.
* **AzureCosmosDB** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Cosmos Database. Si especifica *AzureCosmosDB* como valor, el tráfico a AzureCosmosDB se permite o deniega. Si solo desea permitir el acceso a AzureCosmosDB en una [región](https://azure.microsoft.com/regions), específica, puede especificar la región con el formato AzureCosmosDB.[nombre de región].
* **AzureKeyVault** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Key Vault. Si especifica *AzureKeyVault* como valor, el tráfico a AzureKeyVault se permite o deniega. Si solo desea permitir el acceso a AzureKeyVault en una [región](https://azure.microsoft.com/regions), específica, puede especificar la región con el formato AzureKeyVault.[nombre de región].
* **EventHub** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Event Hubs. Si especifica *EventHub* como valor, el tráfico a EventHub se permite o se deniega. Si solo desea permitir el acceso a EventHub en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región con el formato: EventHub.[nombre de región]. 
* **ServiceBus** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Service Bus. Si especifica *ServiceBus* como valor, el tráfico a ServiceBus se permite o se deniega. Si solo desea permitir el acceso a ServiceBus en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región con el formato: ServiceBus.[nombre de región].
* **MicrosoftContainerRegistry** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Microsoft Container Registry. Si especifica *MicrosoftContainerRegistry* como valor, el tráfico a MicrosoftContainerRegistry se permite o se deniega. Si solo desea permitir el acceso a MicrosoftContainerRegistry en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región con el formato: MicrosoftContainerRegistry.[nombre de región].
* **AzureContainerRegistry** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Container Registry. Si especifica *AzureContainerRegistry* como valor, el tráfico a AzureContainerRegistry se permite o deniega. Si solo desea permitir el acceso a AzureContainerRegistry en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región con el formato: AzureContainerRegistry.[nombre de región]. 
* **AppService** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure App Service. Si especifica *AppService* como valor, el tráfico a AppService se permite o se deniega. Si solo desea permitir el acceso a AppService en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región con el formato: AppService.[nombre de región]. 
* **AppServiceManagement** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure App Service Management. Si especifica *AppServiceManagement* como valor, el tráfico a AppServiceManagement se permite o se deniega. 
* **ApiManagement** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure API Management. Si especifica *ApiManagement* como valor, el tráfico a ApiManagement se permite o se deniega.  
* **AzureConnectors** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Connectors. Si especifica *AzureConnectors* como valor, el tráfico a AzureConnectors se permite o se deniega. Si solo desea permitir el acceso a AzureConnectors en una [región](https://azure.microsoft.com/regions) específica, puede especificar la región con el formato: AzureConnectors.[nombre de región].
* **AzureDataLake** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Data Lake. Si especifica *AzureDataLake* como valor, el tráfico a AzureDataLake se permite o se deniega.
* **AzureActiveDirectory** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio Azure Active Directory. Si especifica *AzureActiveDirectory* como valor, el tráfico a AzureActiveDirectory se permite o se deniega.
* **AzureMonitor** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio AzureMonitor. Si especifica *AzureMonitor* como valor, el tráfico a AzureMonitor se permite o se deniega.
* **ServiceFabric** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio ServiceFabric. Si especifica *ServiceFabric* como valor, el tráfico a ServiceFabric se permite o se deniega.
* **AzureMachineLearning** (solo Resource Manager): esta etiqueta denota los prefijos de dirección del servicio AzureMachineLearning. Si especifica *AzureMachineLearning* como valor, el tráfico a AzureMachineLearning se permite o se deniega.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reglas de Azure Firewall, consulte [Lógica de procesamiento de reglas de Azure Firewall](rule-processing.md).