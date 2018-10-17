---
title: ¿Qué es Azure DNS?
description: Información general del servicio de hospedaje de DNS en Microsoft Azure. Hospede el dominio en Microsoft Azure.
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: overview
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: f68281f71ed525798410fb80b0d0175d38050c40
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48829926"
---
# <a name="what-is-azure-dns"></a>¿Qué es Azure DNS?

DNS de Azure es un servicio de hospedaje para los dominios DNS, que permite resolver nombres mediante la infraestructura de Microsoft Azure. Al hospedar dominios en Azure, puede administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que con los demás servicios de Azure.

No se puede usar Azure DNS para comprar un nombre de dominio. Por una tarifa anual, puede comprar un nombre de dominio mediante [Azure Web Apps](https://docs.microsoft.com/azure/app-service/custom-dns-web-site-buydomains-web-app#buy-the-domain) o un registrador de nombres de dominio de terceros. Los dominios se pueden hospedar luego en Azure DNS para la administración de registros. Consulte [Delegación de un dominio en DNS de Azure](dns-domain-delegation.md) para más información.

Estas son las características disponibles en Azure DNS:

## <a name="reliability-and-performance"></a>Confiabilidad y rendimiento

Los dominios DNS de DNS de Azure se hospedan en la red global de servidores de nombres DNS de Azure. Azure DNS usa redes de difusión por proximidad para que el servidor DNS más próximo disponible responda a cada consulta de DNS. Esto proporciona un mejor rendimiento y una mayor disponibilidad para el dominio.

## <a name="security"></a>Seguridad

El servicio Azure DNS se basa en Azure Resource Manager, que proporciona características tales como:

* [Control de acceso basado en rol](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#access-control): para controlar quién tiene acceso a acciones específicas de su organización.

* [Registros de actividad](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#activity-logs): para supervisar cómo un usuario de su organización modificó un recurso o para encontrar errores al solucionar problemas.

* [Bloqueo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources): para bloquear una suscripción, un grupo de recursos o un recurso para impedir que otros usuarios de su organización eliminen o modifiquen por error recursos esenciales.

Para más información, consulte [Protección de registros y zonas DNS](dns-protect-zones-recordsets.md). 


## <a name="ease-of-use"></a>Facilidad de uso

El servicio Azure DNS puede administrar registros DNS para los servicios de Azure y también proporciona servicio de nombres de dominio para los recursos externos. Azure DNS está integrado en Azure Portal y usa las mismas credenciales, la misma facturación y el mismo contrato de soporte técnico que los demás servicios de Azure. 

La facturación de DNS se basa en el número de zonas DNS hospedadas en Azure y en el número de consultas de DNS recibidas. Para más información sobre precios, consulte [Precios de Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

Los dominios y registros pueden administrarse mediante Azure Portal, los cmdlets de Azure PowerShell y la CLI de Azure multiplataforma. Las aplicaciones que requieren la administración automática de DNS pueden integrarse con el servicio mediante las API REST y los SDK.

## <a name="customizable-virtual-networks-with-private-domains"></a>Redes virtuales personalizables con dominios privados

Azure DNS también admite dominios DNS privados, que están ahora en versión preliminar pública. Esto le permite usar sus propios nombres de dominio personalizados en sus redes virtuales privadas en lugar de los nombres proporcionados por Azure disponibles actualmente.

Para obtener más información, vea [Uso de Azure DNS para dominios privados](private-dns-overview.md).

## <a name="alias-records"></a>Registros de alias

Azure DNS admite conjuntos de registros de alias. Puede usar un registro de alias que se haga referencia a un recurso de Azure, como una dirección IP pública de Azure o un perfil de Traffic Manager. Si cambia la dirección IP del recurso subyacente, el conjunto de registros de alias se actualiza sin problemas durante la resolución DNS. El conjunto de registros de alias apunta a la instancia de servicio, y la instancia de servicio está asociada con una dirección IP. 

Además, ahora puede apuntar el vértice o dominio desnudo (por ejemplo, contoso.com) a un perfil de Traffic Manager mediante un registro de alias.

Para obtener más información, consulte [Overview of Azure DNS alias records](dns-alias.md) (Introducción a los registros de alias de Azure DNS).


## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre zonas y registros DNS, consulte la [introducción a las zonas y registros DNS](dns-zones-records.md).

* Para más información sobre cómo crear una zona en Azure DNS, consulte [Creación de una zona DNS](./dns-getstarted-create-dnszone-portal.md).

* Para ver las preguntas más frecuentes sobre DNS de Azure, vea [Azure DNS FAQ (P+F de DNS de Azure)](dns-faq.md).

