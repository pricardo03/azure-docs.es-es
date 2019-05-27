---
title: ¿Qué es Azure Standard Load Balancer?
titlesuffix: Azure Load Balancer
description: Introducción a las características de Azure Load Balancer Estándar
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/28/2019
ms.author: kumud
ms.openlocfilehash: 266630cb7c9601af69073a6c9beb7d7ada9b8034
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957470"
---
# <a name="azure-standard-load-balancer-overview"></a>Introducción a Azure Standard Load Balancer

Azure Load Balancer permite escalar las aplicaciones y ofrecer una alta disponibilidad de los servicios. Load Balancer se puede usar para escenarios de entrada y de salida, proporciona baja latencia y alta capacidad de proceso, y puede escalar hasta millones de flujos para todas las aplicaciones TCP y UDP. 

Este artículo se centra en Load Balancer Estándar.  Para obtener información más general acerca de Azure Load Balancer, consulte también [Información general de Load Balancer](load-balancer-overview.md).

## <a name="what-is-standard-load-balancer"></a>¿Qué es Load Balancer Estándar?

Load Balancer Estándar es un nuevo producto de equilibrador de carga para todas las aplicaciones TCP y UDP, con un conjunto de características ampliado y más pormenorizado que Load Balancer Básico.  Aunque hay muchas similitudes, es importante que se familiarice con las diferencias, tal y como se describe en este artículo.

Puede usar Load Balancer Estándar como un equilibrador de carga público o interno. Una máquina virtual se puede conectar a recurso de Load Balancer interno o público.

Las funciones de un recurso de Load Balancer se expresan siempre como un front-end, una regla, un sondeo de estado y una definición de grupo de back-end.  Un recurso puede contener varias reglas. Las máquinas virtuales se pueden colocar en el grupo de back-end; para ello, se especifica el grupo de back-end desde el recurso NIC de la máquina virtual.  Este parámetro se pasa a través del perfil de la red y se amplia al usar conjuntos de escalado de máquina virtual.

Un aspecto clave es el ámbito de la red virtual del recurso.  Load Balancer Básico existe dentro del ámbito de un conjunto de disponibilidad, mientras que Load Balancer Estándar está totalmente integrado en el ámbito de una red virtual y se aplican todos los conceptos de red virtual.

Los recursos de Load Balancer son objetos mediante los cuales se puede expresar cómo debe Azure programar su infraestructura multiinquilino para lograr el escenario que se desea crear.  No hay ninguna relación directa entre los recursos de Load Balancer y la infraestructura real; crear un equilibrador de carga no crea una instancia y su capacidad está siempre disponible por lo que no es necesario tener en cuenta retrasos por escalado o inicio. 

>[!NOTE]
> Azure ofrece un conjunto de soluciones de equilibrio de carga completamente administradas para sus escenarios.  Si desea finalización de conexiones TLS ("descarga SSL") o procesamiento de capas de aplicación por solicitud HTTP/HTTPS, consulte [Application Gateway](../application-gateway/application-gateway-introduction.md).  Si desea equilibrio de carga de DNS global, consulte [Traffic Manager](../traffic-manager/traffic-manager-overview.md).  Sus escenarios pueden beneficiarse de extremo a extremo de la combinación de estas soluciones según sea necesario.

## <a name="why-use-standard-load-balancer"></a>¿Por qué usar Load Balancer Estándar?

Load Balancer estándar permite escalar las aplicaciones y crear alta disponibilidad para las implementaciones a pequeña escala en arquitecturas grandes y complejas de varias zonas.

Consulte la tabla siguiente para obtener información general de las diferencias entre Load Balancer Estándar y Load Balancer Básico:

>[!NOTE]
> Los nuevos diseños deben adoptar Load Balancer Estándar. 

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Consulte los [límites de servicio de Load Balancer](https://aka.ms/lblimits), los [precios](https://aka.ms/lbpricing) y el [SLA](https://aka.ms/lbsla).


### <a name="backend"></a>Grupo de back-end

Los grupos de back-end de Standard Load Balancer se expanden a cualquier recurso de máquina virtual en una red virtual.  Puede contener hasta 1000 instancias de back-end.  Una instancia de back-end es una configuración de IP, que es una propiedad de un recurso de NIC.

El grupo de back-end puede contener máquinas virtuales independientes, conjuntos de disponibilidad o conjuntos de escalado de máquinas virtuales.  También puede combinar recursos en el grupo back-end. Puede combinar hasta 150 recursos en el grupo back-end por recurso de Load Balancer.

A la hora de considerar cómo diseñar el grupo de back-end, puede diseñar el menor número de recursos individuales del grupo de back-end para optimizar aún más la duración de las operaciones de administración.  No hay ninguna diferencia en el rendimiento o la escala en el plano de los datos.

### <a name="probes"></a>Sondeos de mantenimiento
  
Standard Load Balancer permite agregar compatibilidad con los [sondeos de mantenimiento HTTPS](load-balancer-custom-probe-overview.md#httpprobe) [sondeo HTTP con contenedor de Seguridad de la capa de transporte (TLS)] para supervisar con precisión las aplicaciones HTTPS.  

Además, cuando todos los sondeos del grupo de servidores back-end [están desactivados](load-balancer-custom-probe-overview.md#probedown), Standard Load Balancer permite que continúen todas las conexiones TCP establecidas. (Basic Load Balancer terminará todas las conexiones TCP en todas las instancias).

Repase [Sondeos de estado de Load Balancer](load-balancer-custom-probe-overview.md) para más información.

### <a name="az"></a>Zonas de disponibilidad

>[!IMPORTANT]
>Revisión [zonas de disponibilidad](../availability-zones/az-overview.md) para otros temas relacionados, incluida cualquier información específica de la región.

Load Balancer Estándar admite otras funcionalidades en las regiones donde las zonas de disponibilidad están disponibles.  Estas características se agregan a las que Load Balancer Estándar ya proporciona.  Las configuraciones de las zonas de disponibilidad están disponibles para equilibradores de carga estándar públicos e internos.

Los servidores front-end no zonales pasan a tener redundancia de zona de forma predeterminada cuando se implementan en una región con zonas de disponibilidad.   Un front-end con redundancia de zona sobrevive a los errores de la zona y cuenta con infraestructura dedicada en todas las zonas al mismo tiempo. 

Además, puede garantizar un front-end en una zona específica. Un front-end zonal comparte el destino de su zona correspondiente y solo cuenta con infraestructura dedicada en una sola zona.

El equilibrio de carga entre zonas está disponible para el grupo de back-end y cualquier recurso de máquina virtual en una red virtual puede formar parte de un grupo de back-end.

Consulte la [explicación detallada de las funcionalidades relativas a las zonas de disponibilidad](load-balancer-standard-availability-zones.md).

### <a name="diagnostics"></a>Diagnóstico

Load Balancer Estándar proporciona métricas multidimensionales mediante Azure Monitor.  Estas métricas se pueden filtrar, agrupar y desglosar para una dimensión determinada.  Proporcionan una perspectiva actual e histórica del rendimiento y el mantenimiento del servicio.  También se admite Resource Health.  A continuación se muestra una breve descripción de los diagnósticos admitidos:

| Métrica | DESCRIPCIÓN |
| --- | --- |
| Disponibilidad de VIP | Standard Load Balancer usa continuamente la ruta de acceso a los datos desde una región hasta el servidor front-end de Load Balancer y, finalmente, hasta la pila de SDN que respalda la máquina virtual. Siempre que permanezcan las instancias correctas, la medida sigue la misma ruta de acceso que el tráfico con equilibrio de carga de las aplicaciones. También se valida la ruta de acceso a los datos usada por los clientes. La medida es invisible para la aplicación y no interfiere con otras operaciones.|
| Disponibilidad de DIP | Load Balancer Estándar usa un servicio de sondeo de mantenimiento distribuido que supervisa el mantenimiento del punto de conexión de la aplicación de acuerdo con la configuración. Esta métrica proporciona una vista agregada o filtrada por punto de conexión de cada punto de conexión de instancia individual del grupo de Load Balancer.  Puede ver cómo Load Balancer observa el estado de su aplicación según se indica en la configuración de sondeo de estado.
| Paquetes SYN | Load Balancer Estándar no finaliza las conexiones TCP ni interactúa con los flujos de paquetes TCP o UDP. Los flujos y los protocolos de enlace son siempre entre el origen y la instancia de máquina virtual. Para solucionar mejor los escenarios de protocolo TCP, puede hacer uso de estos contadores de paquetes SYN para saber el número de intentos de conexión TCP realizados. La métrica indica el número de paquetes TCP SYN recibidos.|
| Conexiones SNAT | Standard Load Balancer informa del número de flujos salientes enmascarados en el servidor front-end de dirección IP pública. Los puertos SNAT son un recurso agotable. Esta métrica puede proporcionar una indicación de la dependencia que su aplicación tiene de SNAT en los flujos salientes originados.  Los contadores de los flujos de salida de SNAT que se realizaron con éxito y los que tuvieron algún error se notifican y se pueden utilizar para solucionar problemas y comprender el estado de los flujos de salida.|
| Contadores de bytes | Standard Load Balancer informa de los datos procesados por front-end.|
| Contadores de paquetes | Standard Load Balancer informa de los paquetes procesados por front-end.|

Consulte la [explicación detallada de los diagnóstico de Load Balancer Estándar](load-balancer-standard-diagnostics.md).

### <a name="haports"></a>Puertos HA

Load Balancer Estándar admite un nuevo tipo de regla.  

Puede configurar reglas de equilibrio de carga para que la aplicación sea escalable y de gran confiabilidad. Cuando se usa una regla de equilibrio de carga para puertos de alta disponibilidad, Standard Load Balancer ofrece equilibrio de carga por flujo en cada puerto efímero de la dirección IP de front-end de Standard Load Balancer.  La característica es útil para otros escenarios donde no es práctico o no deseable especificar los puertos individuales.

Una regla de equilibrio de carga para puertos de alta disponibilidad permite crear escenarios activo-pasivo o activo-activo n+1 para dispositivos de red virtual y aplicaciones que requieran grandes intervalos de puertos de entrada.  Se puede usar un sondeo de mantenimiento para determinar qué back-ends deberían recibir nuevos flujos.  Puede usar un grupo de seguridad de red para emular un escenario de intervalo de puertos.

>[!IMPORTANT]
> Si va a usar un dispositivo virtual de red, póngase en contacto con su proveedor para consultar si su producto se ha probado con puertos HA, y siga sus instrucciones específicas para la implementación. 

Consulte la [explicación detallada sobre los puertos HA](load-balancer-ha-ports-overview.md).

### <a name="securebydefault"></a>Seguro de forma predeterminada

Load Balancer Estándar está totalmente incorporado a la red virtual.  La red virtual es una red privada, cerrada.  Como las equilibradores de carga estándar y las direcciones IP públicas estándar están diseñadas para permitir el acceso a la red virtual desde fuera de ella, ahora, estos recursos están cerrados a menos que se abran. Esto significa que ahora se usan grupos de seguridad de red (NSG) para dar permisos explícitos al tráfico e incluirlo en una lista de permitidos.  Puede crear su propio centro de datos virtual completo y decidir mediante grupos de seguridad de red cómo y cuándo debe estar disponible.  Si no tiene ningún grupo de seguridad de red en una subred o NIC del recurso de máquina virtual, no se permitirá que el tráfico llegue a este recurso.

Para aprender más sobre los NSG y cómo aplicarlos en su caso, vea [Grupos de seguridad de red](../virtual-network/security-overview.md).

### <a name="outbound"></a> Conexiones de salida

Load Balancer admite escenarios de entrada y salida.  Standard Load Balancer es muy diferente de Basic Load Balancer en relación a las conexiones de salida.

La traducción de direcciones de red de origen (SNAT) se utiliza para asignar las direcciones IP privadas internas de la red virtual a direcciones IP públicas en los front-ends de Load Balancer.

Load Balancer Estándar incorpora un nuevo [algoritmo de SNAT más sólido, escalable y predecible](load-balancer-outbound-connections.md#snat), que permite nuevas funcionalidades, elimina ambigüedades y aplica configuraciones explícitas en lugar de efectos secundarios. Estos cambios son necesarios para que surjan características nuevas. 

Estos son los principios claves que hay que recordar cuando se trabaja con Load Balancer Estándar:

- El recurso de Load Balancer se controla mediante la finalización de reglas.  Toda la programación de Azure se deriva de su configuración.
- Cuando hay varios front-ends disponibles, se utilizan todos ellos y cada front-end multiplica el número de puertos SNAT disponibles.
- Puede elegir y controlar si no desea que un front-end específico se use para las conexiones de salida.
- Los escenarios de salida son explícitos y la conectividad de salida no existe hasta que se especifique.
- Las reglas de equilibrio de carga deducen cómo se programa SNAT. Las reglas de equilibrio de carga son específicas del protocolo. SNAT es específica del protocolo y la configuración debe reflejarlo en lugar de crear un efecto secundario.

#### <a name="multiple-frontends"></a>Varios servidores front-end
Si desea más puertos SNAT porque espera tener o ya tiene una gran demanda de conexiones de salida, puede agregar más puertos SNAT. Para ello, configure más front-ends, reglas y grupos de back-end en los mismos recursos de máquina virtual.

#### <a name="control-which-frontend-is-used-for-outbound"></a>Controlar qué front-end se usa para la salida
Si quiere restringir las conexiones de salida para que se originen únicamente desde una dirección IP de front-end específica, puede deshabilitar la SNAT de salida en la regla que expresa la asignación de salida.

#### <a name="control-outbound-connectivity"></a>Control de la conectividad de salida
Load Balancer Estándar existe en el contexto de la red virtual.  Una red virtual es una red privada aislada.  A menos que exista una asociación con una dirección IP pública, no se permite la conectividad pública.  Puede acceder a los [puntos de conexión de servicio de la red virtual](../virtual-network/virtual-network-service-endpoints-overview.md) porque están dentro de la red virtual y son locales a ella.  Si desea establecer la conectividad de salida a un destino fuera de la red virtual, tiene dos opciones:
- Asignar una dirección IP pública de la SKU Estándar como una dirección IP pública de nivel de instancia al recurso de máquina virtual.
- Colocar el recurso de máquina virtual en el grupo de back-end de un equilibrador de carga estándar público.

Ambos permitirán la conectividad de salida desde la red virtual hacia afuera. 

Si _solo_ tiene un equilibrador de carga estándar interno asociado con el grupo de back-end en el que se encuentra el recurso de máquina virtual, la máquina virtual solo puede acceder a los recursos de red virtual y los [puntos de conexión de servicio de la red virtual](../virtual-network/virtual-network-service-endpoints-overview.md).  Para crear una conexión de salida, siga los pasos descritos en el párrafo anterior.

La conectividad de salida de un recurso de máquina virtual no se asocia a las SKU Estándar, permanece como antes.

Consulte la [explicación detallada sobre las conexiones de salida](load-balancer-outbound-connections.md).

### <a name="multife"></a>Varios servidores front-end
Load Balancer admite varias reglas con varios front-ends.  Load Balancer Estándar incluye también los escenarios de salida.  Los escenarios de salida son básicamente lo contrario de una regla de equilibrio de carga de entrada.  La regla de equilibrio de carga de entrada también crea a una asociación para las conexiones de salida. Standard Load Balancer usa todos los front-ends asociados a un recurso de máquina virtual mediante una regla de equilibrio de carga.  Además, un parámetro en la red de equilibrio de carga permite suprimir una red de equilibrio de carga para la conectividad de salida, lo que permite elegir front-ends específicos o, incluso, ninguno.

Para la comparación, Load Balancer Básico selecciona un único front-end de forma aleatoria y no se puede controlar cuál se selecciona.

Consulte la [explicación detallada sobre las conexiones de salida](load-balancer-outbound-connections.md).

### <a name="operations"></a> Operaciones de administración.

Los recursos de Load Balancer Estándar existen en una plataforma de infraestructura completamente nueva.  Esto permite operaciones de administración más rápidas para las SKU Estándar y tiempos de finalización normalmente inferiores a 30 segundos por recurso de SKU Estándar.  Cuando los grupos de back-end aumentan de tamaño, el tiempo requerido para los cambios en el grupo de back-end también aumenta.

Puede modificar los recursos de Load Balancer Estándar y mover una dirección IP pública estándar de una máquina virtual a otra mucho más rápidamente.

## <a name="migration-between-skus"></a>Migración entre SKU

Las SKU no son mutables. Siga los pasos de esta sección para cambiar de una SKU de recurso a otra.

>[!IMPORTANT]
>Consulte este documento en su totalidad para comprender las diferencias entre las SKU y examine detenidamente su escenario.  Quizás deba realizar cambios adicionales para ajustar su escenario.

### <a name="migrate-from-basic-to-standard-sku"></a>Migración de una SKU básica a una estándar

1. Cree un nuevo recurso estándar (Load Balancer y direcciones IP públicas cuando sea necesario). Vuelva a crear sus reglas y definiciones de sondeo.  Si usaba un protocolo de sondeo 443/tcp anteriormente, considere la posibilidad de cambiarlo a un sondeo HTTPS y agregue una ruta.

2. Cree un grupo de seguridad de red nuevo o actualice uno existente en la NIC o la subred para incluir en la lista de permitidos el tráfico con equilibrio de carga  y cualquier otro tráfico que desee permitir.

3. Quite los recursos de SKU básica (Load Balancer y direcciones IP públicas, según corresponda) de todas las instancias de máquina virtual. Asegúrese de quitar también todas las instancias de máquina virtual de un conjunto de disponibilidad.

4. Asocie todas las instancias de máquina virtual a los nuevos recursos de SKU estándar.

### <a name="migrate-from-standard-to-basic-sku"></a>Migración de SKU estándar a básica

1. Cree un nuevo recurso básico (Load Balancer y direcciones IP públicas cuando sea necesario). Vuelva a crear sus reglas y definiciones de sondeo.  Cambie un sondeo HTTPS a un sondeo TCP a un protocolo de sondeo 443/tcp. 

2. Quite los recursos de SKU estándar (Load Balancer y direcciones IP públicas, según corresponda) de todas las instancias de máquina virtual. Asegúrese de quitar también todas las instancias de máquina virtual de un conjunto de disponibilidad.

3. Asocie todas las instancias de máquina virtual a los nuevos recursos de SKU básica.

>[!IMPORTANT]
>
>Existen limitaciones sobre uso de las SKU básica y estándar.
>
>Los puertos HA y el diagnóstico de la SKU estándar solo están disponibles en la SKU estándar. No puede migrar desde la SKU estándar a la SKU básica y también conservan estas características.
>
>Las SKU Básico y Estándar presentan algunas diferencias, tal y como se describe en este artículo.  Asegúrese de comprenderlas y prepararse para ellas.
>
>En los recursos Load Balancer e IP pública se deben usar SKU coincidentes. No puede tener una combinación de recursos de SKU básica y recursos de SKU estándar. No se pueden asociar máquinas virtuales independientes, máquinas virtuales en un recurso de conjunto de disponibilidad o conjuntos de escalado de máquinas virtuales a ambas SKU simultáneamente.

## <a name="region-availability"></a>Disponibilidad por región

Standard Load Balancer está disponible actualmente en todas las regiones de la nube pública.

## <a name="sla"></a>Acuerdo de Nivel de Servicio

Load Balancer Estándar está disponible con un SLA del 99,99 %.  Consulte el [SLA de Load Balancer Estándar](https://aka.ms/lbsla) para obtener más información.

## <a name="pricing"></a>Precios

Se cobra el uso de Standard Load Balancer.

- Número de reglas de salida y equilibrio de carga configurados (las reglas nat de entrada no cuentan para el número total de reglas)
- Cantidad de datos procesados entrantes y salientes con independencia de la regla. 

Para más información sobre los precios de Load Balancer Estándar, vaya la página [Precios de Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/).

## <a name="limitations"></a>Limitaciones

- Las SKU no son mutables. No se puede cambiar la SKU de un recurso existente.
- Un recurso de máquina virtual independiente, un recurso de conjunto de disponibilidad o un recurso de conjunto de escalado de máquinas virtuales puede hacer referencia únicamente a una SKU, nunca a ambas.
- Una regla de equilibrador de carga no puede abarcar dos redes virtuales.  Los front-ends y sus instancias de back-end relacionadas deben estar ubicados en la misma red virtual.  
- [Mover las operaciones de suscripción](../azure-resource-manager/resource-group-move-resources.md) no se admite para los recursos estándar PIP y LB de SKU.
- Solo se puede acceder a los roles de trabajo web sin una red virtual y otros servicios de plataforma de Microsoft si se usa un equilibrador de carga estándar debido a un efecto secundario del funcionamiento de los servicios previos a la red virtual y otros servicios de plataforma. No debe depender de esto, porque el servicio mismo o la plataforma subyacente puede cambiar sin previo aviso. Siempre debe pensar que necesita crear [conectividad de salida](load-balancer-outbound-connections.md) de manera explícita si lo desea al usar solo un equilibrador de carga estándar interno.
- Load Balancer es un producto TCP o UDP de equilibrio de carga y enrutamiento de puerto para estos protocolos IP específicos.  Las reglas de equilibrio de carga y las reglas de traducción de direcciones de red de entrada son compatibles con TCP y UDP y no con otros protocolos IP, como ICMP. Load Balancer no termina la carga ni responde a ella, ni interactúa con la carga de los flujos UDP o TCP. No es un servidor proxy. La validación correcta de la conectividad con un front-end debe realizarse en banda con el mismo protocolo de equilibrado de carga o la misma regla de traducción de direcciones de red de entrada (TCP o UDP) _y_, para ver una respuesta desde el front-end, al menos una de las máquinas virtuales debe generar una respuesta para un cliente.  Si no se recibe una respuesta en banda desde el front-end de Load Balancer, significa que ninguna máquina virtual puede responder.  No es posible interactuar con el front-end de Load Balancer sin una máquina virtual que pueda responder.  Esto también se aplica a las conexiones salientes donde el [enmascaramiento del puerto SNAT](load-balancer-outbound-connections.md#snat) es solo es compatible con TCP y UDP; los demás protocolos IP (como ICMP) también producirán errores.  Asigne una dirección IP pública en el nivel de instancia para la mitigación.
- A diferencia de los equilibradores de carga públicos que proporcionan [conexiones de salida](load-balancer-outbound-connections.md) al realizar la transición de direcciones IP privadas dentro de la red virtual a direcciones IP públicas, los internos no traducen las conexiones de salida originales al front-end de un equilibrador de carga interno, ya que ambos se encuentran en el espacio de direcciones IP privadas.  Esto evita la posibilidad de agotamiento de SNAT dentro de único espacio de direcciones IP internas donde no se necesita traducción.  El inconveniente es que si un flujo de salida de una máquina virtual del grupo de servidores back-end intenta un flujo al front-end del equilibrador de carga interno donde reside el grupo _y_ se asigna a sí mismo, los lados del flujo no coinciden y se produce un error en el flujo.  Si el flujo no se asignó a la misma máquina virtual en el grupo de servidores back-end que creó el flujo para el front-end, se realizará correctamente.   Cuando se asigna el flujo a sí mismo, el flujo de salida parece que se origina en la máquina virtual del front-end y el flujo de entrada correspondiente, en la propia máquina virtual. Desde el punto de vista de los sistemas operativos invitados, las partes de entrada y salidas del mismo flujo no coinciden en la máquina virtual. La pila TCP no reconocerá estas mitades como parte del mismo flujo, ya que el origen y el destino no coinciden.  Cuando el flujo se asigna a cualquier otra máquina virtual del grupo de servidores back-end, sus mitades coinciden y la máquina virtual responde correctamente al flujo.  El inconveniente de este escenario es la producción de tiempos de espera de conexión intermitentes. Hay varias soluciones alternativas comunes para lograr este escenario de forma confiable (flujos que se originan desde un grupo de servidores back-end hasta el respectivo front-end de Load Balancer interno de los grupos de servidores back-end), por ejemplo la inserción de un proxy de terceros detrás de la instancia de Load Balancer interna o el [uso de reglas de estilo DSR](load-balancer-multivip-overview.md).  Aunque podría utilizar un equilibrador de carga público para la mitigación, el escenario resultante es propenso al [agotamiento de SNAT](load-balancer-outbound-connections.md#snat) y debe evitarse, a menos que se administre con cuidado.

## <a name="next-steps"></a>Pasos siguientes

- Más información en [Standard Load Balancer y Availability Zones](load-balancer-standard-availability-zones.md).
- Más información sobre [sondeos de mantenimiento](load-balancer-custom-probe-overview.md).
- Aprenda más sobre [zonas de disponibilidad](../availability-zones/az-overview.md).
- Más información acerca de los [diagnósticos de Load Balancer Estándar](load-balancer-standard-diagnostics.md).
- Más información acerca de las [métricas multidimensionales admitidas](../azure-monitor/platform/metrics-supported.md#microsoftnetworkloadbalancers) para diagnósticos en [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).
- Más información acerca de cómo usar [Load Balancer para conexiones salientes](load-balancer-outbound-connections.md).
- Obtenga información sobre las [reglas de salida](load-balancer-outbound-rules-overview.md).
- Obtenga información sobre el [restablecimiento de TCP en estado inactivo](load-balancer-tcp-reset.md).
- Más información acerca de [Standard Load Balancer con reglas de equilibrio de carga para puertos HA](load-balancer-ha-ports-overview.md).
- Más información acerca de cómo usar [Load Balancer con varios front-ends](load-balancer-multivip-overview.md).
- Más información acerca de las [redes virtuales](../virtual-network/virtual-networks-overview.md).
- Más información sobre los [grupos de seguridad de red](../virtual-network/security-overview.md).
- Más información acerca de [Puntos de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md).
- Aprenda sobre las demás [funcionalidades de red](../networking/networking-overview.md) clave en Azure.
- Más información acerca de [Load Balancer](load-balancer-overview.md).
