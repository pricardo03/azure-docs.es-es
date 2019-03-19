---
title: Preguntas más frecuentes sobre DNS de Azure
description: Preguntas más frecuentes sobre Azure DNS
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: d0c5260fcc2e7ac2acbeec308c6a0cba7d6a81be
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58098100"
---
# <a name="azure-dns-faq"></a>Preguntas más frecuentes sobre DNS de Azure

## <a name="about-azure-dns"></a>Más información sobre DNS de Azure

### <a name="what-is-azure-dns"></a>¿Qué es Azure DNS?

El Sistema de nombres de dominio (DNS) traduce, o resuelve, el nombre de un sitio web o servicio a su dirección IP. Azure DNS es un servicio de hospedaje para dominios DNS. Proporciona resolución de nombres mediante el uso de la infraestructura de Microsoft Azure. Al hospedar dominios en Azure, puede administrar los registros de DNS con las mismas credenciales, API, herramientas y facturación que con los demás servicios de Azure.

Los dominios DNS de Azure DNS se hospedan en la red global de servidores de nombres DNS de Azure. Este sistema usa redes de difusión por proximidad, con el fin de que cada consulta de DNS la responda el servidor DNS disponible más cercano disponible. Azure DNS ofrece un rendimiento rápido y alta disponibilidad para su dominio.

Azure DNS se basa en Azure Resource Manager. Azure DNS se beneficia de características de Resource Manager, como control de acceso basado en rol, registros de auditoría y bloqueo de recursos. Puede administrar los dominios y registros a través de Azure Portal, cmdlets de Azure PowerShell y la CLI de Azure multiplataforma. Las aplicaciones que requieren la administración automática de DNS pueden integrarse con el servicio a través de los SDK y API REST.

### <a name="how-much-does-azure-dns-cost"></a>¿Cuánto cuesta DNS de Azure?

El modelo de facturación de Azure DNS se basa en el número de zonas DNS hospedadas en Azure DNS. También se basa en el número de consultas de DNS que se reciben. Los descuentos se proporcionan en función del uso.

Para más información, vea la [página de precios de DNS de Azure](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>¿Qué es el Acuerdo de Nivel de Servicio de DNS de Azure?

Azure garantiza que las solicitudes de DNS válidas reciben respuesta de al menos un servidor de nombres de Azure DNS el 100 % del tiempo.

Para más información, vea la [página del SLA de DNS de Azure](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>¿Qué es una zona DNS? ¿Es lo mismo que un dominio DNS? 

Un dominio es un nombre único en el sistema de nombres de dominio. Un ejemplo es contoso.com.

Una zona DNS se usa para hospedar los registros DNS de un dominio concreto. Por ejemplo, el dominio contoso.com puede contener varios registros de DNS. Los registros pueden incluir mail.contoso.com para un servidor de correo y www\.contoso.com para un sitio Web. Estos registros se hospedan en la zona DNS contoso.com.

Un nombre de dominio es *solo un nombre*. Una zona DNS es un recurso de datos que contiene los registros de DNS para un nombre de dominio. Puede usar Azure DNS para hospedar una zona DNS y administrar los registros DNS de un dominio en Azure. También proporciona servidores de nombres DNS para responder a consultas de DNS de Internet.

### <a name="do-i-need-to-buy-a-dns-domain-name-to-use-azure-dns"></a>¿Tengo que adquirir un nombre de dominio DNS para usar Azure DNS? 

No necesariamente.

No es necesario comprar un dominio para hospedar una zona DNS en Azure DNS. Se puede crear una zona DNS en cualquier momento sin necesidad de poseer el nombre del dominio. Las consultas de DNS de esta zona solo se resuelven si se dirigen a los servidores de nombres de Azure DNS asignados a la zona.

Para vincular la zona DNS en la jerarquía DNS global, debe comprar el nombre de dominio. Luego, las consultas de DNS desde cualquier lugar del mundo encuentran su zona DNS y responden con sus registros de DNS.

## <a name="azure-dns-features"></a>Características de Azure DNS

### <a name="are-there-any-restrictions-when-using-alias-records-for-a-domain-name-apex-with-traffic-manager"></a>¿Hay alguna restricción en el uso de registros de alias para un vértice de nombre de dominio con Traffic Manager?

Sí. Debe usar direcciones IP públicas estáticas con Azure Traffic Manager. Configure el **Punto de conexión externo** de destino mediante una dirección IP estática. 

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>¿DNS de Azure admite la conmutación por error de puntos de conexión o el enrutamiento basados en DNS?

Traffic Manager proporciona las características de conmutación por error de puntos de conexión y el enrutamiento de tráfico basados en DNS. Traffic Manager es un servicio de Azure independiente que puede utilizarse junto con Azure DNS. Para más información, vea la [Introducción a Traffic Manager](../traffic-manager/traffic-manager-overview.md).

Azure DNS solo admite hospedar dominios DNS estáticos, donde cada consulta de DNS de un registro de DNS determinado siempre recibe la misma respuesta DNS.

### <a name="does-azure-dns-support-domain-name-registration"></a>¿DNS de Azure admite el registro de nombres de dominio?

 No. Azure DNS no admite actualmente la opción de comprar nombres de dominio. Para comprar dominios, debe usar un registrador de nombres de dominio de terceros. El registrador suele cobrar una tarifa anual reducida. Los dominios se pueden hospedar en Azure DNS para la administración de registros de DNS. Para más información, vea [Delegación de un dominio en DNS de Azure](dns-domain-delegation.md).

Se hace seguimiento de la característica para comprar nombres de dominio en el trabajo pendiente de Azure. Use el sitio de comentarios para [registrar su compatibilidad con esta característica](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>¿DNS de Azure admite DNSSEC?

 No. Azure DNS no admite actualmente las extensiones de seguridad del Sistema de nombres de dominio (DNSSEC).

Se hace seguimiento de la característica DNSSEC en el trabajo pendiente de Azure DNS. Use el sitio de comentarios para [registrar su compatibilidad con esta característica](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>¿DNS de Azure admite las transferencias de zona (AXFR/IXFR)?

 No. Azure DNS no admite actualmente las transferencias de zona. Las zonas DNS se pueden [importar a Azure DNS mediante la CLI de Azure](dns-import-export.md). Los registros DNS se administran a través del [Portal de administración de Azure DNS](dns-operations-recordsets-portal.md), la [API REST](https://docs.microsoft.com/powershell/module/azurerm.dns), el [SDK](dns-sdk.md), los [cmdlets de PowerShell](dns-operations-recordsets.md) o la [herramienta CLI](dns-operations-recordsets-cli.md).

Se hace seguimiento de la característica de transferencia de zona en el trabajo pendiente de Azure DNS. Use el sitio de comentarios para [registrar su compatibilidad con esta característica](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>¿DNS de Azure admite los redireccionamientos de dirección URL?

 No. Los servicios de redireccionamiento de dirección URL no son un servicio DNS. Funcionan en el nivel HTTP en lugar del nivel DNS. Algunos proveedores DNS incluyen un servicio de redireccionamiento de dirección URL como parte de su oferta general. Este servicio actualmente no es compatible con Azure DNS.

Se hace seguimiento de la característica de redireccionamiento de dirección URL en el trabajo pendiente de Azure DNS. Use el sitio de comentarios para [registrar su compatibilidad con esta característica](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

### <a name="does-azure-dns-support-the-extended-ascii-encoding-8-bit-set-for-txt-record-sets"></a>¿Azure DNS admite el conjunto de codificación ASCII extendido (8 bits) para conjuntos de registros TXT?

Sí. Azure DNS admite el conjunto de codificación ASCII extendido para conjuntos de registros TXT, pero se debe usar la versión más reciente de las API REST de Azure, los SDK, PowerShell y la CLI. Las versiones anteriores al 1 de octubre de 2017 o al SDK 2.1 no admiten el conjunto de ASCII extendido. 

Por ejemplo, podría proporcionar una cadena como el valor de un registro TXT con el carácter ASCII extendido \128. Un ejemplo es "abcd\128efgh." Azure DNS usa el valor de bytes de este carácter, que es 128, en la representación interna. En el momento de la resolución DNS; este valor de bytes se devolverá en la respuesta. Debe tener en cuenta también que "abc" y "\097\098\099" son intercambiables en lo que respecta a la resolución. 

Seguimos las reglas de secuencias de escape de caracteres maestros de archivos de zona [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) para los registros TXT. Por ejemplo, según la RFC, `\` actúa ahora como secuencia de escape. Si especifica `A\B` como valor del registro TXT, se representa y resuelve simplemente como `AB`. Si realmente desea que el registro TXT sea `A\B` en la resolución, tendrá que volver a utilizar una secuencia de escape con `\`. Por ejemplo, especifique `A\\B`.

Esta compatibilidad no está disponible actualmente para los registros TXT creados en Azure Portal.

## <a name="alias-records"></a>Registros de alias

### <a name="what-are-some-scenarios-where-alias-records-are-useful"></a>¿En qué escenarios son útiles los registros de alias?
Consulte la sección de escenarios en [Introducción a los registros de alias de Azure DNS](dns-alias.md).

### <a name="what-record-types-are-supported-for-alias-record-sets"></a>¿Qué tipos de registros se admiten para los conjuntos de registros de alias?
Los conjuntos de registros de alias se admiten para los siguientes tipos de registros en una zona de Azure DNS:
 
- Una  
- AAAA
- CNAME 

### <a name="what-resources-are-supported-as-targets-for-alias-record-sets"></a>¿Qué recursos se admiten como destinos para los conjuntos de registros de alias?

- **Apuntar a un recurso de dirección IP pública desde un conjunto de registros A/AAAA de DNS**. Puede crear un conjunto de registros D/AAAA y hacer que sea un conjunto de registros de alias que apunte a un recurso de dirección IP pública.
- **Apuntar a un perfil de Traffic Manager desde un conjunto de registros D/AAAA/CNAME de DNS.** Puede apuntar al CNAME de un perfil de Traffic Manager desde un conjunto de registros CNAME de DNS. Un ejemplo es contoso.trafficmanager.net. Ahora, también puede apuntar a un perfil de Traffic Manager que tenga puntos de conexión externos desde un registro D o AAAA establecido en su zona DNS.
- **Apuntar a otro conjunto de registros de DNS dentro de la misma zona**. Los registros de alias pueden hacer referencia a otros conjuntos de registros del mismo tipo. Por ejemplo, puede hacer que un conjunto de registros CNAME de DNS sea un alias de otro conjunto de registros CNAME del mismo tipo. Esta organización resulta útil si desea que algunos conjuntos de registros sean alias y otros no alias.

### <a name="can-i-create-and-update-alias-records-from-the-azure-portal"></a>¿Puedo crear y actualizar registros de alias desde Azure Portal?
Sí. Puede crear o administrar los registros de alias en Azure Portal junto con las API REST de Azure, PowerShell, la CLI y los SDK.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-deleted-when-the-underlying-public-ip-is-deleted"></a>¿Los registros de alias ayudarán a garantizar que mi conjunto de registros de DNS se eliminará cuando se elimine la dirección IP pública subyacente?
Sí. Esta característica es una de las funcionalidades principales de los registros de alias. Ayudan a evitar posibles interrupciones para los usuarios de la aplicación.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-updated-to-the-correct-ip-address-when-the-underlying-public-ip-address-changes"></a>¿Los registros de alias ayudarán a garantizar que mi conjunto de registros de DNS se actualizará a la dirección IP correcta cuando cambie la dirección IP pública subyacente?
Sí. Esta característica es una de las funcionalidades principales de los registros de alias. Ayudan a evitar posibles interrupciones o riesgos de seguridad para las aplicaciones.

### <a name="are-there-any-restrictions-when-using-alias-record-sets-for-a-or-aaaa-records-to-point-to-traffic-manager"></a>¿Hay alguna restricción a la hora de usar conjuntos de registros de alias para que registros A o AAAA apunten a Traffic Manager?
Sí. Para apuntar a un perfil de Traffic Manager como un alias desde un conjunto de registros A o AAAA, el perfil de Traffic Manager solo debe usar puntos de conexión externos. Al crear los puntos de conexión externos en Traffic Manager, proporcione las direcciones IP reales de los puntos de conexión.

### <a name="is-there-an-additional-charge-to-use-alias-records"></a>¿Hay algún cargo adicional por utilizar los registros de alias?
Los registros de alias son una calificación de un conjunto de registros de DNS válido. No hay ninguna facturación adicional por los registros de alias.

## <a name="use-azure-dns"></a>Uso de Azure DNS

### <a name="can-i-cohost-a-domain-by-using-azure-dns-and-another-dns-provider"></a>¿Puedo hospedar de manera conjunta un dominio con Azure DNS y otro proveedor de DNS?

Sí. Azure DNS es compatible con el hospedaje conjunto de dominios con otros servicios DNS.

Para configurar el hospedaje conjunto, modifique los registros de NS para que el dominio apunte a los servidores de nombres de ambos proveedores. Los registros de servidor de nombres (NS) controlan qué proveedores reciben consultas de DNS para el dominio. Puede modificar estos registros de NS en Azure DNS, en el otro proveedor y en la zona primaria. Habitualmente, la zona primera se configura mediante el registrador de nombres de dominio. Para más información sobre la delegación DNS, vea [Delegación de dominios DNS](dns-domain-delegation.md).

Además, asegúrese de que los registros de DNS del dominio estén sincronizados entre ambos proveedores de DNS. Azure DNS actualmente no admite las transferencias de zona DNS. Los registros DNS se deben sincronizar mediante el [Portal de administración de Azure DNS](dns-operations-recordsets-portal.md), [la API REST](https://docs.microsoft.com/powershell/module/azurerm.dns), el [SDK](dns-sdk.md), los [cmdlets de PowerShell](dns-operations-recordsets.md) o la [herramienta CLI](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>¿Tengo que delegar mi dominio en los cuatro servidores de nombres de Azure DNS?

Sí. Azure DNS asigna cuatro servidores de nombres a cada zona DNS. Esta disposición se usa para aislar los errores y aumentar la resistencia. Para poder optar al Acuerdo de Nivel de Servicio de Azure DNS, delegue el dominio a los cuatro servidores de nombres.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>¿Cuáles son los límites de uso de Azure DNS?

Cuando se usa Azure DNS, se aplican estos límites predeterminados.

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>¿Puedo mover una zona de Azure DNS entre los grupos de recursos o entre las suscripciones?

Sí. Las zonas DNS se pueden mover entre grupos de recursos o entre suscripciones.

Si mueve una zona DNS, no hay ningún efecto en las consultas de DNS. Los servidores de nombres asignados a la zona siguen siendo los mismos. Las consultas de DNS se procesan de la manera habitual.

Para obtener más información e instrucciones sobre cómo mover zonas DNS, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>¿Cuánto tardan en surtir efecto los cambios de DNS?

Las nuevas zonas DNS y registros de DNS suelen aparecen rápidamente en los servidores de nombres de Azure DNS. No tarda más allá de unos segundos.

Los cambios en los registros de DNS existentes pueden tardar un poco más. Por lo general, aparecen en los servidores de nombres de Azure DNS en 60 segundos. El almacenamiento en caché de DNS por parte de los clientes de DNS y las resoluciones recursivas de DNS fuera de Azure DNS también pueden afectar cuánto se tarda. Para controlar esta duración de memoria caché, use la propiedad de período de vida (TTL) de cada conjunto de registros.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>¿Cómo puedo proteger mis zonas DNS contra una eliminación accidental?

Azure DNS se administra mediante Azure Resource Manager. Azure DNS se beneficia de las características de control de acceso que proporciona Azure Resource Manager. El control de acceso basado en rol controla qué usuarios tienen acceso de lectura o escritura a conjuntos de registros y zonas DNS. Los bloqueos de recursos evitan la modificación o eliminación accidental de conjuntos de registros y zonas DNS.

Para más información, consulte [Protección de zonas y registros DNS](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>¿Cómo puedo configurar registros de SPF en Azure DNS?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-name-servers-resolve-over-ipv6"></a>¿Se resuelven los servidores de nombres de Azure DNS en IPv6? 

Sí. Los servidores de nombres de Azure DNS son de pila doble. Esto significa que tienen direcciones IPv4 e IPv6. Para encontrar la dirección IPv6 de los servidores de nombres de Azure DNS asignados a la zona DNS, use una herramienta como nslookup. Un ejemplo es `nslookup -q=aaaa <Azure DNS Nameserver>`.

### <a name="how-do-i-set-up-an-idn-in-azure-dns"></a>¿Cómo configuro un IDN en Azure DNS?

Los nombres de dominio internacionalizados (IDN) codifican cada nombre DNS mediante [punycode](https://en.wikipedia.org/wiki/Punycode). Las consultas de DNS se realizan con estos nombres codificados mediante punycode.

Para configurar IDN en Azure DNS, convierta el nombre de la zona o el nombre del conjunto de registros a punycode. Azure DNS no admite actualmente la conversión integrada hacia o desde punycode.

## <a name="private-dns"></a>DNS privado

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

### <a name="does-azure-dns-support-private-domains"></a>¿Azure DNS admite dominios privados?

La compatibilidad con dominios privados se implementa mediante la característica de zonas privadas. Esta característica está actualmente en versión preliminar pública. Las zonas privadas se administran con las mismas herramientas que las zonas de Azure DNS accesibles desde Internet. Se pueden resolver solo desde dentro de las redes virtuales especificadas. Para más información, consulte la [introducción](private-dns-overview.md).

Actualmente no se admiten zonas privadas en Azure Portal.

Para sobre otras opciones de DNS internas de Azure, consulte [Resolución de nombres para las máquinas virtuales e instancias de rol](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="whats-the-difference-between-registration-virtual-network-and-resolution-virtual-network-in-the-context-of-private-zones"></a>¿En qué se diferencian una red virtual de registro y una red virtual de resolución en el contexto de las zonas privadas?

Las redes virtuales se pueden vincular a una zona DNS privada como red virtual de registro o como red virtual de resolución. En cualquier caso, las máquinas virtuales de la red virtual se resuelven correctamente con registros de la zona privada. Con una red virtual de registro, los registros de DNS se registran automáticamente en la zona correspondiente a las máquinas virtuales de la red virtual. Cuando se elimina una máquina virtual de una red virtual de registro, el registro de DNS correspondiente de la zona privada vinculada se quita automáticamente. 

### <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>¿Funcionará Azure DNS Private Zones entre regiones de Azure?

Sí. Las zonas privadas se admiten para la resolución DNS entre redes virtuales de distintas regiones de Azure. Las zonas privadas funcionan incluso sin emparejamiento explícito de las redes virtuales. Todas las redes virtuales se deben especificar como redes virtuales de resolución para la zona privada. Los clientes pueden necesitar que las redes virtuales se emparejen para que el tráfico TCP/HTTP fluya de una región a otra.

### <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Para las zonas privadas, ¿se requiere que las redes virtuales tengan conectividad con Internet?

 No. Las zonas privadas funcionan junto con las redes virtuales. Los clientes las usan para administrar los dominios de las máquinas virtuales u otros recursos dentro y entre redes virtuales. No se requiere conectividad con Internet para la resolución de nombres. 

### <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>¿Se puede usar la misma zona privada para varias redes virtuales para la resolución?

Sí. Los clientes pueden asociar hasta 10 redes virtuales de resolución con una única zona privada.

### <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-resolution-virtual-network-to-a-private-zone"></a>¿Se puede agregar una red virtual que pertenece a otra suscripción como red virtual de resolución a una zona privada?

Sí. Debe tener permiso para la operación de escritura en las redes virtuales y en la zona DNS privada. El permiso de escritura se puede conceder a varios roles de RBAC. Por ejemplo, el rol de RBAC Colaborador de la red virtual clásica tiene permisos de escritura en las redes virtuales. Para más información sobre los roles de RBAC, consulte el artículo sobre [el control de acceso basado en rol](../role-based-access-control/overview.md).

### <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-the-virtual-machines-are-deleted-by-the-customer"></a>¿Se eliminarán automáticamente los registros de DNS de una red virtual registrada automáticamente cuando el cliente elimine las máquinas virtuales?

Sí. Si elimina una máquina virtual dentro de una red virtual de registro, se eliminan automáticamente los registros de DNS que se registraron en la zona. 

### <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-registration-virtual-network-be-deleted-manually"></a>¿Se puede eliminar manualmente un registro de máquina virtual realizado automáticamente en una zona privada de una red virtual de registro?

 No. Los clientes no pueden ver ni editar los registros de DNS de una máquina virtual que se registran automáticamente en una zona privada de una red virtual de registro. Puede sobrescribir los registros de DNS que se registraron automáticamente con un registro de DNS que se creó de manera manual en la zona. La siguiente pregunta y su respuesta abordan este tema.

### <a name="what-happens-when-we-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-registration-virtual-network"></a>¿Qué ocurre cuando se intenta crear manualmente un nuevo registro de DNS en una zona privada que tiene el mismo nombre de host que una máquina virtual existente registrada de manera automática en una red virtual de registro?

Se intenta crear un registro de DNS nuevo en una zona privada con el mismo nombre de host que una máquina virtual existente y registrada de manera automática de una red virtual de registro. Al hacerlo, el nuevo registro de DNS sobrescribe el registro de máquina virtual que se registró automáticamente. Si intenta eliminar nuevamente este registro de DNS que se creó de manera manual de la zona, la eliminación se realizará correctamente. El registro automático vuelve a ocurrir siempre que la máquina virtual siga existiendo y tenga adjunta una dirección IP privada. El registro de DNS se vuelve a crear de manera automática en la zona.

### <a name="what-happens-when-we-unlink-a-registration-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>¿Qué ocurre si desvinculamos una red virtual de registro de una zona privada? ¿Los registros de máquina virtual registrados automáticamente de la red virtual también se quitarán de la zona?

Sí. Para desvincular una red virtual de registro de una zona privada, debe actualizar la zona DNS para quitar la red virtual de registro asociada. En este proceso, los registros de máquina virtual que se registraron automáticamente se quitan de la zona. 

### <a name="what-happens-when-we-delete-a-registration-or-resolution-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-registration-or-resolution--virtual-network-from-the-zone"></a>¿Qué ocurre cuando se elimina una red virtual de registro o de resolución que está vinculada a una zona privada? ¿Tenemos que actualizar manualmente la zona privada para desvincular la red virtual de registro o resolución de la zona?

Sí. Cuando se elimina una red virtual de registro o resolución sin desvincularla primero de una zona privada, la operación de eliminación se realizará correctamente, pero la red virtual no se desvinculará de manera automática de la zona privada, si la hubiera. Debe desvincular manualmente la red virtual de la zona privada. Es por este motivo que debe desvincular la red virtual de la zona privada antes de eliminarla.

### <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>¿Resolución de DNS usando el valor predeterminado (internal.cloudapp.net) FQDN seguirá funcionando incluso cuando una zona privada (por ejemplo, private.contoso.com) está vinculada a una red virtual?

Sí. Las zonas privadas no reemplazan las resoluciones DNS predeterminadas mediante el uso de la zona internal.cloudapp.net proporcionada por Azure. Se ofrece como característica o mejora adicional. Ya sea si confía en la zona internal.cloudapp.net proporcionada por Azure o en su propia zona privada, use el nombre de dominio completo de la zona en la que quiere la resolución. 

### <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>¿Se cambiará el sufijo DNS en las máquinas virtuales de una red virtual vinculada por el de la zona privada?

 No. El sufijo DNS de las máquinas virtuales en la red virtual vinculada permanecerá como el sufijo predeterminado proporcionado por Azure ("*.internal.cloudapp.net"). En las máquinas virtuales, dicho sufijo DNS se puede cambiar manualmente por el de la zona privada. 

### <a name="are-there-any-limitations-for-private-zones-during-this-preview"></a>¿Existen limitaciones para las zonas de privada durante esta versión preliminar?

Sí. Durante la versión preliminar pública, existen las siguientes limitaciones.
* Una red virtual de registro por zona privada.
* Hasta 10 redes virtuales de resolución por zona privada.
* Una red virtual dada solo se vincula a una zona privada como red virtual de registro.
* Una red virtual dada se vincula a hasta 10 zonas privadas como red virtual de resolución.
* Si se especifica una red virtual de registro, los registros de DNS de las máquinas virtuales de esa red virtual que se registraron en la zona privada no se podrán ver ni recuperar desde PowerShell, la CLI o las API. Los registros de la máquina virtual se registran y resuelven de manera correcta.
* El DNS inverso solo funciona en el espacio de direcciones IP privadas de la red virtual de registro.
* El DNS inverso de una dirección IP privada que no se haya registrado en la zona privada devuelve "internal.cloudapp.net" como el sufijo DNS. Este sufijo no se puede resolver. Un ejemplo es una dirección IP privada para una máquina virtual de una red virtual que se vincula como una red virtual de resolución a una zona privada.
* Una red virtual debe estar vacía cuando se vincula por primera vez con una zona privada como red virtual de registro o resolución. La red virtual puede no estar vacía para una futura vinculación como una red virtual de registro o resolución a otras zonas privadas.
* Por ejemplo, no se admite el reenvío condicional para habilitar la resolución entre redes locales y Azure. Obtenga información sobre cómo los clientes pueden disfrutar de este escenario a través de otros mecanismos. Consulte [Resolución de nombres para las máquinas virtuales e instancias de rol](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

### <a name="are-there-any-quotas-or-limits-on-zones-or-records-for-private-zones"></a>¿Existen cuotas o límites de zonas o registros en las zonas privadas?

No hay ningún límite en el número de zonas permitidas por suscripción para las zonas privadas. No hay ningún límite en el número de conjuntos de registros por zona para las zonas privadas. Tanto las zonas públicas como las privadas cuentan para los límites globales de DNS. Para más información, consulte los [límites de suscripción y servicio de Azure](../azure-subscription-service-limits.md#azure-dns-limits)

### <a name="is-there-portal-support-for-private-zones"></a>¿Admite Azure Portal las zonas privadas?

Las zonas privadas ya creadas a través de API, PowerShell, la CLI y los SDK son visibles en Azure Portal. Pero los clientes no pueden crear zonas privadas nuevas ni administrar asociaciones con redes virtuales. En el caso de las redes virtuales asociadas como redes virtuales de registro, los registros de máquina virtual registrados automáticamente no se verán desde el portal. 

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre Azure DNS](dns-overview.md).
<br>
- [Más información sobre cómo usar Azure DNS para dominios privados](private-dns-overview.md).
<br>
- [Más información sobre registros y zonas DNS](dns-zones-records.md).
<br>
- [Introducción a Azure DNS](dns-getstarted-portal.md).

