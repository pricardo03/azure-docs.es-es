---
title: Red virtual para servicios de Azure
titlesuffix: Azure Virtual Network
description: Obtenga información sobre las ventajas de la implementación de recursos en una red virtual. Los recursos de redes virtuales pueden comunicarse entre sí, y con recursos locales, sin tráfico que recorra Internet.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: malop;kumud
ms.openlocfilehash: c9b2f7244731be67628776b032e041457900353c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58886427"
---
# <a name="virtual-network-integration-for-azure-services"></a>Integración de red virtual para los servicios de Azure

La integración de servicios de Azure en una red virtual de Azure permite el acceso privado al servicio desde las máquinas virtuales o los recursos de proceso de la red virtual.
Puede integrar los servicios de Azure en la red virtual con las siguientes opciones:
- Implementando instancias dedicadas del servicio en una red virtual. Luego se puede acceder de forma privada a los servicios dentro de la red virtual y desde redes locales.
- Extensión de una red virtual al servicio, a través de puntos de conexión de servicio. Los puntos de conexión de servicio permite que los recursos de servicio individuales se protejan en la red virtual.

Para integrar varios servicios de Azure en una red virtual, puede combinar uno o varios de los patrones anteriores. Por ejemplo, puede implementar HDInsight en una red virtual y proteger una cuenta de almacenamiento en la subred de HDInsight mediante los punto de conexión de servicio.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Implementación de servicios de Azure en las redes virtuales

Al implementar servicios de Azure dedicados en una [red virtual](virtual-networks-overview.md), puede comunicarse con los recursos de los servicios de forma privada mediante direcciones IP privadas.

![Servicios implementados en una red virtual](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

La implementación de servicios dentro de una red virtual ofrece las siguientes funcionalidades:

- Los recursos dentro de la red virtual pueden comunicarse entre sí de forma privada a través de direcciones IP privadas. Ejemplo: transferencia directa de datos entre HDInsight y SQL Server que se ejecutan en una máquina virtual, en la red virtual.
- Los recursos locales pueden acceder a recursos de una red virtual mediante direcciones IP privadas a través de [VPN de sitio a sitio (VPN Gateway)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) o [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Las redes virtuales pueden estar [emparejadas](virtual-network-peering-overview.md) para habilitar que los recursos de las redes virtuales se comuniquen entre sí mediante direcciones IP privadas.
- Las instancias de servicio de una red virtual están totalmente administradas por el servicio de Azure, para supervisar el estado de dichas instancias y proporcionar la escala necesaria según la carga.
- Las instancias de los servicios se implementan en una subred de una red virtual. El acceso entrante y saliente de la red se debe abrir a través de [grupos de seguridad de red](security-overview.md#network-security-groups) para la subred, siguiendo las instrucciones proporcionadas por los servicios.
- Opcionalmente, los servicios pueden requerir una [subred delegada](virtual-network-manage-subnet.md#add-a-subnet) como un identificador explícito de que una subred puede hospedar un servicio determinado. La delegación de la subred proporciona al servicio permisos explícitos para crear recursos específicos del servicio en la subred.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Servicios que pueden implementarse en una red virtual

|Categoría|Servicio|
|-|-|
| Proceso | Máquinas virtuales: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) o [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Conjuntos de escalado de máquinas virtuales](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Servicio en la nube](https://msdn.microsoft.com/library/azure/jj156091): Solo Virtual Network (clásico)<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)  |
| Red | [Application Gateway - WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Firewall](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Aplicaciones virtuales de red](/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) 
|Datos|[RedisCache](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Instancia administrada de Azure SQL Database](../sql-database/sql-database-managed-instance-connectivity-architecture.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
Análisis | [HDInsight de Azure](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |
| Identidad | [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |
| Contenedores | [Azure Kubernetes Service (AKS)](../aks/concepts-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Instancia de Azure Container (ACI)](https://www.aka.ms/acivnet)<br/>[Motor de Azure Container Service](https://github.com/Azure/acs-engine) con el [complemento](https://github.com/Azure/acs-engine/tree/master/examples/vnet) CNI de Azure Virtual Network|
| Web | [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[entorno de App Service](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|
| Hospedada | [Azure Dedicated HSM](../dedicated-hsm/index.yml?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|
|||



## <a name="service-endpoints-for-azure-services"></a>Puntos de conexión de servicio para servicios de Azure

Algunos servicios de Azure no se pueden implementar en redes virtuales. Puede restringir el acceso a algunos de los recursos de servicio solo a subredes de red virtual específicas, si lo desea, habilitando un punto de conexión de servicio de red virtual.  Obtenga más información acerca de los [puntos de conexión de servicio de red virtual](virtual-network-service-endpoints-overview.md) y los servicios para los que se pueden habilitar los puntos de conexión.
