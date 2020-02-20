---
title: ¿Qué es Azure Private Link?
description: Aprenda a usar Azure Private Link para acceder a los servicios PaaS de Azure (por ejemplo, Azure Storage y SQL Database) y a los servicios de asociados o clientes hospedados en Azure a través de un punto de conexión privado de la red virtual.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: overview
ms.date: 01/09/2020
ms.author: allensu
ms.custom: fasttrack-edit
ms.openlocfilehash: 452202555734a208a9f32d6f8899e1f679df4a68
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2020
ms.locfileid: "77443999"
---
# <a name="what-is-azure-private-link"></a>¿Qué es Azure Private Link? 
Azure Private Link le permite acceder a los servicios PaaS de Azure (por ejemplo, Azure Storage, Azure Cosmos DB y SQL Database) y a los servicios de asociados o clientes hospedados de cliente o asociado mediante un [punto de conexión privado](private-endpoint-overview.md) de la red virtual. El tráfico entre la red virtual y el servicio atraviesa la red troncal de Microsoft, eliminando la exposición a la red pública de Internet. También puede crear su propio [servicio Private Link](private-link-service-overview.md) en la red virtual y enviarlo de forma privada a los clientes. La experiencia de configuración y consumo con Azure Private Link es coherente en los servicios compartidos de PaaS de Azure, de propiedad del cliente y de asociados.

![Introducción al punto de conexión privado](media/private-link-overview/private-endpoint.png)

## <a name="key-benefits"></a>Ventajas principales
Azure Private Link proporciona las ventajas siguientes:  
- **Acceso privado a servicios en la plataforma Azure**: conecte la red virtual a los servicios que se ejecutan en Azure de forma privada sin necesidad de una dirección IP pública en el origen o el destino. Los proveedores de servicios pueden representar los servicios de forma privada en su propia red virtual y los consumidores pueden acceder a esos servicios de forma privada en su red virtual local. La plataforma Private Link administrará la conectividad entre el consumidor y los servicios a través de la red troncal de Azure. 
 
- **Redes locales y emparejadas**: acceda a los servicios que se ejecutan en Azure desde el entorno local a través del emparejamiento privado de ExpressRoute y los túneles VPN (desde el entorno local) y las redes virtuales emparejadas mediante puntos de conexión privados. No es necesario configurar el emparejamiento público ni atravesar Internet para alcanzar el servicio. Esta capacidad proporciona una manera segura de migrar cargas de trabajo a Azure.
 
- **Protección contra la filtración de datos**:  con Azure Private Link, el punto de conexión privado de la red virtual se asigna a una instancia específica del recurso de PaaS del cliente, en lugar de a todo el servicio. El uso de los consumidores del punto de conexión privado solo puede conectarse al recurso específico y no a ningún otro recurso del servicio. Este mecanismo integrado proporciona protección frente a los riesgos de la filtración de datos. 
 
- **Alcance global**: conéctese de forma privada a los servicios que se ejecutan en otras regiones. Esto significa que la red virtual del consumidor podría estar en la región A y puede conectarse a los servicios detrás de Private Link en la región B.  
 
- **Ampliación de los propios servicios**: aproveche la misma experiencia y funcionalidad para presentar su propio servicio de forma privada a los consumidores de Azure. Al colocar el servicio detrás de una instancia de Standard Load Balancer, puede habilitarlo para Private Link. Después, el consumidor puede conectarse directamente al servicio mediante un punto de conexión privado en su propia red virtual. Puede administrar estas solicitudes de conexión mediante un flujo de llamadas de aprobación simple. Azure Private Link funciona también para consumidores y servicios que pertenecen a distintos inquilinos de Active Directory. 

## <a name="availability"></a>Disponibilidad 
 En la tabla siguiente se enumeran los servicios de Private Link y las regiones en las que están disponibles. 

|Escenario  |Servicios admitidos  |Regiones disponibles | Status  |
|:---------|:-------------------|:-----------------|:--------|
|Private Link para servicios propiedad del cliente|Servicios Private Link detrás de Standard Load Balancer | Todas las regiones públicas  | Vista previa  |
|Private Link para los servicios PaaS de Azure   | Azure Storage        |  Todas las regiones públicas      | Vista previa <br/> [Más información](/azure/storage/common/storage-private-endpoints).  |
|  | Azure Data Lake Storage Gen2        |  Todas las regiones públicas      | Vista previa <br/> [Más información](/azure/storage/common/storage-private-endpoints).  |
|  |  Azure SQL Database         | Todas las regiones públicas      |   Vista previa      |
||Azure SQL Data Warehouse| Todas las regiones públicas |Vista previa|
||Azure Cosmos DB| Centro-oeste de EE. UU., Oeste de EE. UU., Centro-norte de EE. UU. |Vista previa|
|  |  Azure Database for PostgreSQL: servidor único         | Todas las regiones públicas      |   Vista previa      |
|  |  Azure Database for MySQL         | Todas las regiones públicas      |   Vista previa      |
|  |  Azure Database for MariaDB         | Todas las regiones públicas      |   Vista previa      |
|  |  Azure Key Vault         | Todas las regiones públicas      |   Vista previa      |

Para conocer las notificaciones más actualizadas, consulte la página [Actualizaciones de Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network).

## <a name="logging-and-monitoring"></a>Registro y supervisión

Azure Private Link se integra en Azure Monitor, lo que permite archivar registros en una cuenta de almacenamiento, transmitir eventos al centro de eventos o enviarlos a los registros de Azure Monitor. Puede tener acceso a la siguiente información sobre Azure Monitor: 
- **Punto de conexión privado**: datos procesados por el punto de conexión privado (IN/OUT)
 
- **Servicio Private Link**:
    - datos procesados por el servicio Private Link (IN/OUT)
    - Disponibilidad del puerto NAT  
 
## <a name="pricing"></a>Precios   
Para más información sobre los precios, consulte [Precios de Azure Private Link](https://azure.microsoft.com/pricing/details/private-link/).
 
## <a name="faqs"></a>Preguntas más frecuentes  
Para ver las preguntas más frecuentes, consulte [Preguntas más frecuentes sobre Azure Private Link](private-link-faq.md).
 
## <a name="limits"></a>límites  
Para conocer los límites, consulte [Límites de Azure Private Link](../azure-resource-manager/management/azure-subscription-service-limits.md#private-link-limits).

## <a name="next-steps"></a>Pasos siguientes
- [Creación de un punto de conexión privado para el servidor de SQL Database mediante el portal](create-private-endpoint-portal.md)
- [Creación de un punto de conexión privado para el servidor de SQL Database mediante PowerShell](create-private-endpoint-powershell.md)
- [Creación de un punto de conexión privado para el servidor de SQL Database mediante la CLI](create-private-endpoint-cli.md)
- [Creación de un punto de conexión privado para la cuenta de almacenamiento mediante el portal](create-private-endpoint-storage-portal.md)
- [Creación de un punto de conexión privado para la cuenta de Azure Cosmos mediante el portal](../cosmos-db/how-to-configure-private-endpoints.md)
- [Creación del propio servicio Private Link con Azure PowerShell](create-private-link-service-powershell.md)


 
