---
title: Uso de puntos de conexión privados con Azure Storage | Microsoft Docs
description: Información general de los puntos de conexión privados para el acceso seguro a las cuentas de almacenamiento de redes virtuales.
services: storage
author: santoshc
ms.service: storage
ms.topic: article
ms.date: 09/25/2019
ms.author: santoshc
ms.reviewer: santoshc
ms.subservice: common
ms.openlocfilehash: fb1f8a1d1f8e1ebbaf3e0e9fe96e3c1bf0ba9ba6
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74078750"
---
# <a name="using-private-endpoints-for-azure-storage-preview"></a>Uso de puntos de conexión privados para Azure Storage (versión preliminar)

Puede usar [puntos de conexión privados](../../private-link/private-endpoint-overview.md) en sus cuentas de Azure Storage para que los clientes de una red virtual (VNet) puedan acceder de forma segura a los datos a través de un [vínculo privado](../../private-link/private-link-overview.md). El punto de conexión privado usa una dirección IP del espacio de direcciones de la red virtual para el servicio de la cuenta de almacenamiento. El tráfico de red entre los clientes de la red virtual y la cuenta de almacenamiento atraviesa la red virtual y un vínculo privado de la red troncal de Microsoft, lo que elimina la exposición a la red pública de Internet.

El uso de puntos de conexión privados en una cuenta de almacenamiento permite:
- Proteger la cuenta de almacenamiento mediante la configuración del firewall de almacenamiento para que bloquee todas las conexiones del punto de conexión público del servicio de almacenamiento.
- Aumentar la seguridad de la red virtual (VNet), ya que permite bloquear la filtración de datos de la red virtual.
- Conectarse de forma segura a las cuentas de almacenamiento desde las redes locales que se conectan a la red virtual mediante [VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) o [ExpressRoute](../../expressroute/expressroute-locations.md) con emparejamiento privado.

## <a name="conceptual-overview"></a>Información general conceptual
![Puntos de conexión privados para Azure Storage](media/storage-private-endpoints/storage-private-endpoints-overview.jpg)

Un punto de conexión privado es una interfaz de red especial para un servicio de Azure de una [red virtual](../../virtual-network/virtual-networks-overview.md) (VNet). Cuando se crea un punto de conexión privado para una cuenta de almacenamiento, este proporciona conectividad segura entre los clientes de la red virtual y el almacenamiento. Al punto de conexión privado se le asigna una dirección IP del intervalo de direcciones IP de la red virtual. La conexión entre el punto de conexión privado y el servicio de almacenamiento usa un vínculo privado seguro.

Las aplicaciones de la red virtual se pueden conectar al servicio de almacenamiento a través del punto de conexión privado sin problemas, **ya que se usan las mismas cadenas de conexión y mecanismos de autorización que se usarían en cualquier otro caso**. Los puntos de conexión privados se pueden usar con todos los protocolos que admita la cuenta de almacenamiento, incluidos REST y SMB.

Cuando se crea un punto de conexión privado para un servicio de almacenamiento en la red virtual, se envía una solicitud de consentimiento para su aprobación al propietario de la cuenta de almacenamiento. Si el usuario que solicita la creación del punto de conexión privado también es propietario de la cuenta de almacenamiento, esta solicitud de consentimiento se aprueba automáticamente.

Los propietarios de cuentas de almacenamiento pueden administrar las solicitudes de consentimiento y los puntos de conexión privados desde la pestaña "*Puntos de conexión privados*" de la cuenta de almacenamiento en [Azure Portal](https://portal.azure.com).

> [!TIP]
> Si desea restringir el acceso a una cuenta de almacenamiento, de modo que solo se pueda realizar a través del punto de conexión privado, configure el firewall de almacenamiento para denegar todo acceso que se realice a través del punto de conexión público.

Puede proteger la cuenta de almacenamiento para que acepte solo las conexiones que provengan de la red virtual. Para ello, debe [configurar el firewall de almacenamiento](storage-network-security.md#change-the-default-network-access-rule) para que, de forma predeterminada, deniegue el acceso a través de su punto de conexión. Para permitir el tráfico de una red virtual que tenga un punto de conexión privado no se necesita ninguna regla de firewall, ya que el firewall de almacenamiento solo controla el acceso a través del punto de conexión privado. En su lugar, los puntos de conexión privados usan el flujo de consentimiento para conceder el acceso de las subredes al servicio de almacenamiento.

### <a name="private-endpoints-for-storage-service"></a>Punto de conexión privado de un servicio de almacenamiento

Al crear el punto de conexión privado, debe especificar la cuenta de almacenamiento y el servicio de almacenamiento al que se conecta. Necesita un punto de conexión privado independiente para cada servicio de almacenamiento de la cuenta de almacenamiento a la que necesite acceder, a saber, [Blobs](../blobs/storage-blobs-overview.md), [Data Lake Storage Gen2](../blobs/data-lake-storage-introduction.md), [Files](../files/storage-files-introduction.md), [Queues](../queues/storage-queues-introduction.md), [Tables](../tables/table-storage-overview.md) o [Static Websites](../blobs/storage-blob-static-website.md).

> [!TIP]
> Cree un punto de conexión privado independiente para la instancia secundaria del servicio de almacenamiento para mejorar el rendimiento de lectura en las cuentas de almacenamiento con redundancia geográfica con acceso de lectura.

Para obtener disponibilidad de lectura en una [cuenta de almacenamiento con redundancia geográfica con acceso de lectura](storage-redundancy-grs.md#read-access-geo-redundant-storage), se necesitan puntos de conexión privados para las instancias principal y secundaria del servicio. No es preciso crear un punto de conexión privado para la instancia secundaria para la **conmutación por error**. El punto de conexión privado se conectará automáticamente a la nueva instancia principal después de la conmutación por error.

#### <a name="resources"></a>Recursos

Para obtener información más detallada sobre la creación de un punto de conexión privado para la cuenta de almacenamiento, consulte los siguientes artículos:

- [Conexión privada a una cuenta de almacenamiento desde la experiencia de Cuentas de almacenamiento en Azure Portal](../../private-link/create-private-endpoint-storage-portal.md)
- [Creación de un punto de conexión privado mediante Private Link Center en Azure Portal](../../private-link/create-private-endpoint-portal.md)
- [Creación de un punto de conexión privado mediante la CLI de Azure](../../private-link/create-private-endpoint-cli.md)
- [Creación de un punto de conexión privado mediante Azure PowerShell](../../private-link/create-private-endpoint-powershell.md)

### <a name="dns-changes-for-private-endpoints"></a>Cambios de DNS en puntos de conexión privados

Los clientes de una red virtual deben usar la misma cadena de conexión para la cuenta de almacenamiento, incluso cuando usen un punto de conexión privado.

Cuando crea un punto de conexión privado, actualizamos el registro de recurso CNAME de DNS para ese punto de conexión de almacenamiento a un alias de un subdominio con el prefijo "*privatelink*". De forma predeterminada, también creamos una [zona DNS privada](../../dns/private-dns-overview.md) conectada a la red virtual. Esta zona DNS privada corresponde al subdominio con el prefijo "*privatelink*" y contiene los registros de recursos D de DNS para los puntos de conexión privados.

Cuando se resuelve la dirección URL del punto de conexión de almacenamiento desde fuera de la red virtual con el punto de conexión privado, se resuelve en el punto de conexión público del servicio de almacenamiento. Cuando se resuelve desde la red virtual que hospeda el punto de conexión privado, la dirección URL del punto de conexión de almacenamiento se resuelve en la dirección IP del punto de conexión privado.

En el ejemplo anterior, los registros de recursos DNS de la cuenta de almacenamiento "StorageAccountA", cuando se resuelven desde fuera de la red virtual que hospeda el punto de conexión privado, serán:

| NOMBRE                                                  | type  | Valor                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``StorageAccountA.blob.core.windows.net``             | CNAME | ``StorageAccountA.privatelink.blob.core.windows.net`` |
| ``StorageAccountA.privatelink.blob.core.windows.net`` | CNAME | \<Punto de conexión público del servicio Storage\>                   |
| \<Punto de conexión público del servicio Storage\>                   | Una     | \<Dirección IP pública del servicio Storage\>                 |

Como ya se ha mencionado, puede denegar todo el acceso a través del punto de conexión público mediante el firewall de almacenamiento.

Los registros de recursos DNS de StorageAccountA, cuando los resuelve un cliente en la red virtual que hospeda el punto de conexión privado, serán:

| NOMBRE                                                  | type  | Valor                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``StorageAccountA.blob.core.windows.net``             | CNAME | ``StorageAccountA.privatelink.blob.core.windows.net`` |
| ``StorageAccountA.privatelink.blob.core.windows.net`` | Una     | 10.1.1.5                                              |

Este enfoque permite el acceso a la cuenta de almacenamiento **mediante la misma cadena de conexión** desde la red virtual que hospeda los puntos de conexión privados, así como a los clientes que están fuera de la red virtual. Puede usar el firewall de almacenamiento para denegar el acceso a todos los clientes que están fuera de la red virtual.

> [!IMPORTANT]
> Use la misma cadena de conexión para conectarse a la cuenta de almacenamiento a través de puntos de conexión privados, tal como la usaría en otras ocasiones. No se conecte a la cuenta de almacenamiento mediante su dirección URL de subdominio "*privatelink*".

> [!TIP]
> Cuando use un servidor DNS personalizado o local, debe configurar los registros de recursos DNS para los puntos de conexión privados de una zona DNS correspondiente al subdominio "privatelink" del servicio de almacenamiento.

Los nombres de zona DNS recomendados para los puntos de conexión privados de los servicios de almacenamiento son:

| Servicio de Storage        | Nombre de zona                            |
| :--------------------- | :----------------------------------- |
| Blob service           | `privatelink.blob.core.windows.net`  |
| Data Lake Storage Gen2 | `privatelink.dfs.core.windows.net`   |
| File service           | `privatelink.file.core.windows.net`  |
| Queue service          | `privatelink.queue.core.windows.net` |
| Table service          | `privatelink.table.core.windows.net` |
| Static Websites        | `privatelink.web.core.windows.net`   |

## <a name="pricing"></a>Precios

Para más información sobre los precios, consulte [Precios de Azure Private Link](https://azure.microsoft.com/pricing/details/private-link).

## <a name="known-issues"></a>Problemas conocidos

### <a name="copy-blob-support"></a>Compatibilidad de Copy Blob

Durante la versión preliminar, no se admiten los comandos [Copy Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) emitidos para las cuentas de almacenamiento a las que se accede a través de puntos de conexión privados cuando la cuenta de almacenamiento de origen está protegida mediante un firewall.

### <a name="subnets-with-service-endpoints"></a>Subredes con puntos de conexión de servicio
Actualmente, no se puede crear un punto de conexión privado en una subred que tenga puntos de conexión de servicio. Como solución alternativa, se pueden crear subredes independientes en la misma red virtual para los puntos de conexión de servicio y los puntos de conexión privados.

### <a name="storage-access-constraints-for-clients-in-vnets-with-private-endpoints"></a>Restricciones de acceso al almacenamiento para los clientes de redes virtuales con puntos de conexión privados

Los clientes de redes virtuales con puntos de conexión privados existentes se enfrentan a ciertas restricciones al acceder a otras cuentas de almacenamiento que tienen puntos de conexión privados. Por ejemplo, imagine que la red virtual N1 tiene un punto de conexión privado para una cuenta de almacenamiento A1 para, por ejemplo, Blob service. Si la cuenta de almacenamiento A2 tiene un punto de conexión privado en una red virtual N2 para Blob service, los clientes de la red virtual N1 también deben acceder a la instancia de Blob service de la cuenta A2 mediante un punto de conexión privado. Si la cuenta de almacenamiento A2 no tiene puntos de conexión privados para Blob service, los clientes de la red virtual N1 pueden acceder a su instancia de Blob service sin ningún punto de conexión privado.

Esta restricción es el resultado de los cambios de DNS realizados cuando la cuenta A2 crea un punto de conexión privado.

### <a name="network-security-group-rules-for-subnets-with-private-endpoints"></a>Reglas de grupo de seguridad de red para subredes con puntos de conexión privados

Actualmente, no se pueden configurar reglas de [grupo de seguridad de red](../../virtual-network/security-overview.md) para subredes con puntos de conexión privados. Una solución alternativa limitada para este problema es implementar las reglas de acceso para los puntos de conexión privados en las subredes de origen, aunque este enfoque puede requerir mayor sobrecarga de administración.
