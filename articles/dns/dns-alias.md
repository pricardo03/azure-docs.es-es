---
title: Introducción a los registros de alias de Azure DNS
description: Introducción a la compatibilidad de los registros de alias en Microsoft Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 52b42e964e7abe207064aff49f7f8f27f8476ef4
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092849"
---
# <a name="azure-dns-alias-records-overview"></a>Introducción a los registros de alias de Azure DNS

Los registros de alias de Azure DNS son calificaciones en un conjunto de registros de DNS. Pueden hacer referencia a otros recursos de Azure desde dentro de la zona DNS. Por ejemplo, puede crear un conjunto de registros de alias que haga referencia a una dirección IP pública de Azure en lugar de a un registro D. El conjunto de registros de alias señala dinámicamente a una instancia de servicio de direcciones IP públicas de Azure. Como resultado, el conjunto de registros de alias se actualiza sin problemas durante la resolución DNS.

Un conjunto de registros de alias se admite para los siguientes tipos de registros en una zona de Azure DNS: 

- Una  
- AAAA 
- CNAME 

> [!NOTE]
> Los registros de alias de los tipos de registro D o AAAA de Azure Traffic Manager solo se admiten para los tipos de punto de conexión externo. Debe proporcionar la dirección IPv4 o IPv6, según corresponda, para los puntos de conexión externos en Traffic Manager. Lo ideal es usar direcciones IP estáticas para la dirección.

## <a name="capabilities"></a>Capacidades

- **Apuntar a un recurso de dirección IP pública desde un conjunto de registros D/AAAA de DNS.** Puede crear un conjunto de registros D/AAAA y hacer que sea un conjunto de registros de alias que apunte a un recurso de dirección IP pública.

- **Apuntar a un perfil de Traffic Manager desde un conjunto de registros D/AAAA/CNAME de DNS.** Puede apuntar al CNAME de un perfil de Traffic Manager desde un conjunto de registros CNAME de DNS. Un ejemplo es contoso.trafficmanager.net. Ahora, también puede apuntar a un perfil de Traffic Manager que tenga puntos de conexión externos desde un registro D o AAAA establecido en su zona DNS.

   > [!NOTE]
   > Los registros de alias de los tipos de registro D o AAAA de Traffic Manager solo se admiten para los tipos de punto de conexión externo. Debe proporcionar la dirección IPv4 o IPv6, según corresponda, para los puntos de conexión externos en Traffic Manager. Lo ideal es usar direcciones IP estáticas para la dirección.
   
- **Apuntar a otro conjunto de registros DNS dentro de la misma zona.** Los registros de alias pueden hacer referencia a otros conjuntos de registros del mismo tipo. Por ejemplo, un conjunto de registros CNAME de DNS puede ser un alias de otro conjunto de registros CNAME del mismo tipo. Esta organización resulta útil si desea que algunos conjuntos de registros sean alias y otros no-alias.

## <a name="scenarios"></a>Escenarios
Hay algunos escenarios comunes para los registros de alias.

### <a name="prevent-dangling-dns-records"></a>Impedir que los registros DNS queden pendientes
 Dentro de las zonas de Azure DNS, los registros de alias se pueden usar para realizar un seguimiento estrecho del ciclo de vida de los recursos de Azure. Los recursos incluyen una dirección IP pública o un perfil de Traffic Manager. Un problema común de los registros DNS tradicionales son los registros pendientes. Este problema se produce especialmente con los tipos de registros D/AAAA o CNAME. 

En el caso de un registro de zona DNS tradicional, si la dirección IP de destino o CNAME ya no existen, el registro de la zona DNS no tiene conocimiento de este hecho. Como resultado, el registro debe actualizarse manualmente. En algunas organizaciones, esta actualización manual podría no ocurrir a tiempo. También puede resultar problemático debido a la separación de roles y niveles de permisos asociados.

Por ejemplo, un rol podría tener autoridad para eliminar una dirección IP o CNAME que pertenezca a una aplicación. Pero no tiene autoridad suficiente para actualizar el registro DNS que apunta a esos destinos. Un retardo de tiempo se produce entre el momento en que se elimina la dirección IP o CNAME, y cuando se quita el registro DNS que apunta a ella. Este retardo de tiempo puede provocar una interrupción del servicio para los usuarios.

Los registros de alias eliminan la complejidad asociada a este escenario. Ayudan a evitar referencias pendientes. Tomemos, por ejemplo, un registro DNS que se califica como un registro de alias que apunte a una dirección IP pública o a un perfil de Traffic Manager. Si se eliminan los recursos subyacentes, se quita el registro de alias DNS al mismo tiempo. Este proceso garantiza que los usuarios nunca sufran una interrupción del servicio.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>Actualización automática de zonas DNS cuando cambian las direcciones IP de la aplicación

Este escenario es similar al anterior. Puede que una aplicación se mueva o la máquina virtual subyacente se reinicie. Un registro de alias se actualiza entonces automáticamente cuando la dirección IP cambia para el recurso de direcciones IP públicas subyacente. Para evitar posibles riesgos de seguridad, dirija a los usuarios a otra aplicación que tenga la dirección IP antigua.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Hospedaje de aplicaciones con equilibrio de carga en el vértice de la zona

El protocolo DNS evita la asignación de cualquier elemento que no sea un registro D o AAAA en el vértice de la zona. Un ejemplo es contoso.com. Esta restricción presenta un problema para los propietarios de aplicaciones que tienen aplicaciones con equilibrio de carga detrás de Traffic Manager. No es posible señalar al perfil de Traffic Manager desde el registro de vértice de zona. Como consecuencia, los propietarios de aplicaciones deben usar una solución alternativa. Un redireccionamiento en la capa de la aplicación debe redirigirse desde el vértice de zona a otro dominio. Un ejemplo es el redireccionamiento de contoso.com a www.contoso.com. Esa organización presenta un único punto de error en cuanto a la función de redireccionamiento.

Con los registros de alias, este problema ya no existe. Ahora los propietarios de aplicaciones pueden apuntar su registro de vértice de zona a un perfil de Traffic Manager que tenga puntos de conexión externos. Los propietarios de aplicaciones pueden apuntar al mismo perfil de Traffic Manager que se utilice para cualquier otro dominio dentro de su zona DNS. Por ejemplo, contoso.com y www.contoso.com pueden apuntar al mismo perfil de Traffic Manager. Esto ocurre siempre que el perfil de Traffic Manager solo tenga configurados puntos de conexión externos.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los registros de alias, consulte los artículos siguientes:

- [Tutorial: Configuración de un registro de alias para hacer referencia a una dirección IP pública de Azure](tutorial-alias-pip.md)
- [Tutorial: Configuración de un registro de alias de para admitir nombres de dominio de vértice con Traffic Manager](tutorial-alias-tm.md)
- [Preguntas más frecuentes sobre DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
