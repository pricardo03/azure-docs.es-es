---
title: Introducción a los registros de alias de Azure DNS
description: Introducción a la compatibilidad de los registros de alias en Microsoft Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 05a6a1700de2b8b334b40d9efd9b8d79e9e7241f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957589"
---
# <a name="azure-dns-alias-records-overview"></a>Introducción a los registros de alias de Azure DNS

Los registros de alias de Azure DNS son una cualificación de un conjunto de registros de DNS que le permiten hacer referencia a otros recursos de Azure desde su zona DNS. Por ejemplo, puede crear un conjunto de registros de alias que haga referencia a una dirección IP pública de Azure en lugar de un registro A. Puesto que el conjunto de registros de alias apunta dinámicamente a una instancia de servicio de dirección IP pública de Azure, el conjunto de registros del alias se actualiza ininterrumpidamente durante la resolución DNS.

Se admite un conjunto de registros de alias para los siguientes tipos de registros en una zona de Azure DNS: A, AAAA y CNAME. 

> [!NOTE]
> Los registros de alias de los tipos de registro A o AAAA de Traffic Manager solo se admiten para los tipos de punto de conexión externo. Debe proporcionar la dirección IPv4 o IPv6 (lo ideal son IP estáticas) según corresponda para los puntos de conexión externos en Traffic Manager.

## <a name="capabilities"></a>Capacidades

- **Apuntar a un recurso de dirección IP pública desde un conjunto de registros A/AAAA de DNS**. Puede crear un conjunto de registros A/AAAA y hacer que sea un conjunto de registros de alias que apunte a un recurso de dirección IP pública.
- **Apuntar a un perfil de Traffic Manager desde un conjunto de registros A/AAAA/CNAME de DNS**. Además de poder apuntar al registro CNAME de un perfil de Traffic Manager (por ejemplo: contoso.trafficmanager.net) desde un conjunto de registros CNAME de DNS, ahora también puede apuntar a un perfil de Traffic Manager que tenga puntos de conexión externos, desde un conjunto de registros A o AAAA en su zona de DNS.
   > [!NOTE]
   > Los registros de alias de los tipos de registro A o AAAA de Traffic Manager solo se admiten para los tipos de punto de conexión externo. Debe proporcionar la dirección IPv4 o IPv6 (lo ideal son IP estáticas) según corresponda para los puntos de conexión externos en Traffic Manager.
- **Apuntar a otro conjunto de registros de DNS dentro de la misma zona**. Los registros de alias pueden hacer referencia a otros conjuntos de registros del mismo tipo. Por ejemplo, puede hacer que un conjunto de registros CNAME de DNS sea un alias de otro conjunto de registros CNAME del mismo tipo. Esto es útil si desea que algunos conjuntos de registros sean alias y otros no por cuestiones de comportamiento.

## <a name="scenarios"></a>Escenarios
Hay algunos escenarios comunes para los registros de alias:

### <a name="prevent-dangling-dns-records"></a>Impedir que los registros DNS queden pendientes
Desde dentro de las zonas de Azure DNS, los registros de alias se pueden usar para realizar un seguimiento estrecho del ciclo de vida de los recursos de Azure como, por ejemplo, una dirección IP pública y un perfil de Traffic Manager. Uno de los problemas habituales con los registros DNS tradicionales es el de los "registros pendientes", especialmente en el caso de los registros A/AAAA o CNAME. 

En el caso de un registro de zona DNS tradicional, si la dirección IP de destino o CNAME ya no existen, el registro de la zona DNS no tiene conocimiento de este hecho y, por tanto, debe actualizarse manualmente. En algunas organizaciones, esta actualización manual puede que no se produzca a tiempo o que sea problemática debido a la separación de los roles y los niveles de permisos asociados.

Por ejemplo, es posible que el rol que tiene autoridad para eliminar un registro CNAME o una dirección IP que pertenece a una aplicación no tenga autoridad suficiente para actualizar el registro DNS que apunta a esos destinos. Como resultado, se puede producir un retraso entre el momento en que se elimina la dirección IP o el registro CNAME y el momento en el que se elimina el registro DNS que apunta a este, lo cual podría provocar una interrupción para los usuarios finales.

Los registros de alias eliminan la complejidad asociada a este escenario y ayudan a impedir esas referencias pendientes. Si se ha calificado un registro DNS como registro de alias para que apunte a una dirección IP pública o perfil de Traffic Manager, y si esos recursos subyacentes se han eliminado, el registro de alias DNS también se eliminará al mismo tiempo. Esto garantiza que los usuarios finales nunca sufrirán una interrupción del servicio.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>Actualización automática de zonas DNS cuando cambian las direcciones IP de la aplicación

Al igual que en el escenario anterior, si se mueve una aplicación o se reinicia la máquina virtual subyacente, se actualiza automáticamente un registro de alias cuando cambia la dirección IP del recurso de dirección IP pública subyacente. Puede evitar posibles riesgos de seguridad si los usuarios finales se dirigen a otra aplicación que tenga la dirección IP antigua.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Hospedaje de aplicaciones con equilibrio de carga en el vértice de la zona

El protocolo DNS evita la asignación de cualquier elemento que no sea un registro A o AAAA en el vértice de zona (por ejemplo: contoso.com). Este presenta un problema para los propietarios de aplicaciones que tienen aplicaciones con equilibrio de carga detrás de una instancia de Traffic Manager, ya que no será posible apuntar al perfil de Traffic Manager desde el registro del vértice de la zona. Como consecuencia, los propietarios de aplicaciones se verán obligados a usar una solución alternativa. Por ejemplo, un redireccionamiento en el nivel de aplicación para redirigirlo desde el vértice de la zona a otro dominio (de contoso.com a www.contoso.com). Esto presenta un único punto de error en cuanto a la funcionalidad de la redirección.

Con los registros de alias, este problema ya no existe. Los propietarios de aplicaciones ahora pueden apuntar su registro de vértice de zona a un perfil de Traffic Manager que tenga puntos de conexión externos. Con esta funcionalidad, los propietarios de aplicaciones pueden apuntar al mismo perfil de Traffic Manager que se utiliza para cualquier otro dominio dentro de su zona DNS. Por ejemplo, contoso.com y www.contoso.com pueden apuntar al mismo perfil de Traffic Manager siempre que el perfil de Traffic Manager tenga solo puntos de conexión externos configurados.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los registros de alias, consulte los artículos siguientes:

- [Tutorial: Configuración de un registro de alias para hacer referencia a una dirección IP pública de Azure](tutorial-alias-pip.md)
- [Tutorial: Configuración de un registro de alias de para admitir nombres de dominio de vértice con Traffic Manager](tutorial-alias-tm.md)
- [Preguntas más frecuentes sobre DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
