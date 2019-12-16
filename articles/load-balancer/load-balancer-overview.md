---
title: ¿Qué es Azure Load Balancer?
titleSuffix: Azure Load Balancer
description: Información general sobre las características, la arquitectura y la implementación del Equilibrador de carga de Azure Aprenda cómo funciona Load Balancer y cómo aprovechar sus ventajas en la nube.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/05/2019
ms.author: allensu
ms.openlocfilehash: 50cb61394043bb8d0e67cae2aea8be4285f3432c
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74926277"
---
# <a name="what-is-azure-load-balancer"></a>¿Qué es Azure Load Balancer?

El *equilibrio de carga* hace referencia a la distribución eficaz de la carga o del tráfico de red entrante en un grupo de recursos o servidores de back-end. Azure ofrece [varias opciones de equilibrio de carga](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) para que pueda elegir la que mejor se ajuste a sus necesidades. En este documento se analiza Azure Load Balancer.

Azure Load Balancer opera en la capa cuatro del modelo de interconexión de sistema abierto (OSI). Es el único punto de contacto de los clientes. Load Balancer distribuye los nuevos flujos de entrada que llegan a su front-end a las instancias del grupo back-end y para ello usa las reglas de equilibrio de carga y los sondeos de estado especificados. Las instancias del grupo de back-end pueden ser instancias de Azure Virtual Machines o de un conjunto de escalado de máquinas virtuales. 

Azure Load Balancer permite escalar las aplicaciones y crear servicios con disponibilidad. Load Balancer admite escenarios de entrada y de salida, proporciona baja latencia y alto rendimiento, y se puede escalar hasta millones de flujos para todas las aplicaciones TCP y UDP.

Una instancia de **[Load Balancer pública](#publicloadbalancer)** puede proporcionar conexiones salientes para máquinas virtuales (VM) dentro de la red virtual, para lo que traduce de sus direcciones IP privadas a direcciones IP públicas. Las instancias públicas de Load Balancer se usan para equilibrar la carga del tráfico de Internet en las máquinas virtuales.

Se puede usar una instancia **[interna (o privada) de Load Balancer](#internalloadbalancer)** en escenarios donde solo se necesitan direcciones IP privadas en el front-end. Las instancias internas de Load Balancer se usan para equilibrar la carga del tráfico dentro de una red virtual. También puede tener acceso a un servidor front-end de Load Balancer desde una red local en un escenario híbrido.

## <a name="load-balancer-components"></a>Componentes de Load Balancer
* **Configuraciones de direcciones IP de front-end**: la dirección IP pública de Load Balancer. Es el punto de contacto de los clientes. Pueden ser direcciones IP públicas o privadas, lo que crea instancias de Load Balancer públicas o internas, respectivamente.
* **Grupo de back-end**: el grupo de máquinas virtuales o las instancias del conjunto de escalado de máquinas virtuales que van a atender la solicitud entrante. Para escalar de forma rentable, con el fin de satisfacer grandes volúmenes de procedimientos recomendados para el procesamiento de tráfico entrante, generalmente se recomienda agregar más instancias al grupo de back-end. Load Balancer se reconfigura al instante de forma automática mediante el escalado o la reducción vertical de instancias. Al agregar o quitar máquinas virtuales del grupo de servidores back-end, el equilibrador de carga se vuelve a configurar sin necesidad de operaciones adicionales en el recurso del equilibrador de carga.
* **Sondeos de estado**: los sondeos de estado se usan para determinar el estado de las instancias del grupo de back-end. Puede definir el umbral incorrecto de los sondeos de estado. Cuando un sondeo no responde, el equilibrador de carga deja de enviar nuevas conexiones a las instancias incorrectas. Un error de sondeo no afecta a las conexiones existentes. La conexión continúa hasta que la aplicación finaliza el flujo, se agota el tiempo de espera de inactividad o se apaga la máquina virtual. Load Balancer proporciona diferentes tipos de sondeo de estado para los puntos de conexión TCP, HTTP y HTTPS. Para más información, consulte [Tipos de sondeo](load-balancer-custom-probe-overview.md#types).
* **Reglas de equilibrio de carga**: las reglas de equilibrio de carga son las que indican a Load Balancer lo que se debe hacer en cada momento. 
* **Reglas NAT de entrada**: las reglas NAT de entrada reenvían el tráfico desde un puerto específico de una dirección IP de front-end específica a un puerto específico de una instancia de back-end específica dentro de la red virtual. El reenvío de puertos se realiza mediante la misma distribución basada en hash que el equilibrio de carga. Los escenarios comunes para esta funcionalidad son las sesiones Protocolo de escritorio remoto (RDP) o Secure Shell (SSH) en las instancias de máquina virtual individuales dentro de una red virtual de Azure. Puede asignar varios puntos de conexión internos a puertos en la misma dirección IP de front-end. Puede usar las direcciones IP de front-end para administrar de forma remota las máquinas virtuales sin un cuadro de salto adicional.
* **Reglas de salida**: una regla de salida configura una NAT de salida para todas las máquinas virtuales o instancias identificadas por el grupo de back-end que se van a traducir para el front-end.

## <a name="load-balancer-concepts"></a>Conceptos de Load Balancer

Load Balancer proporciona las siguientes funcionalidades básicas para las aplicaciones TCP y UDP:

* **Algoritmo de equilibrio de carga**: con Azure Load Balancer puede crear una regla de equilibrio de carga para distribuir el tráfico que llega al front-end a las instancias del grupo de back-end. Load Balancer usa un algoritmo hash para la distribución de los flujos de entrada y reescribe los encabezados de los flujos en las instancias del grupo de back-end. Un servidor está disponible para recibir nuevos flujos cuando el sondeo de estado indica un punto de conexión de back-end correcto.
De forma predeterminada, Load Balancer usa un hash de tupla 5. El hash incluye la dirección IP de origen, el puerto de origen, la dirección IP de destino, el puerto de destino y el número de protocolo IP para asignar flujos a los servidores disponibles. Puede crear afinidad en una dirección IP de origen mediante un hash de tupla 2 o 3 para una regla determinada. Todos los paquetes del mismo flujo de paquetes llegan a la misma instancia subyacente al servidor front-end con equilibrio de carga. Cuando el cliente inicia un nuevo flujo desde la misma IP de origen, se cambia el puerto de origen. En consecuencia, el hash 5-tupla puede hacer que el tráfico vaya a otro punto de conexión de back-end.
Para más información, consulte [Configurar el modo de distribución para Azure Load Balancer](load-balancer-distribution-mode.md). En la siguiente imagen se muestra la distribución basada en hash:

  ![Distribución basada en hash](./media/load-balancer-overview/load-balancer-distribution.png)

  *Ilustración: distribución basada en hash*

* **Independencia y transparencia de aplicaciones**: Load Balancer no interactúa directamente con TCP o UDP ni con la capa de aplicación. Se puede admitir cualquier escenario de aplicación TCP o UDP. Load Balancer no finaliza ni se originan flujos, interactúe con la carga del flujo o proporcione cualquier función de puerta de enlace de capa de aplicación. Los protocolos de enlace de protocolo siempre se producen directamente entre el cliente y la instancia del grupo de back-end. Una respuesta a un flujo de entrada siempre es una respuesta de una máquina virtual. Cuando llega el flujo a la máquina virtual, también se conserva la dirección IP de origen original.
  * A cada punto de conexión solo le responde una máquina virtual. Por ejemplo, un protocolo de enlace TCP se realiza siempre entre el cliente y la máquina virtual de back-end seleccionada. Una respuesta a una solicitud a un front-end es una respuesta generada por una máquina virtual de back-end. Cuando se valida correctamente la conectividad con un front-end, se valida la conectividad de un extremo a otro con al menos una máquina virtual de back-end.
  * Las cargas de la aplicación son transparentes para Load Balancer. Se puede admitir cualquier aplicación UDP o TCP.
  * Dado que Load Balancer no interactúa con la carga TCP y proporciona descarga TLS, puede crear escenarios cifrados de un extremo a otro. El uso de Load Balancer aumenta la escalabilidad horizontal de las aplicaciones TLS al finalizar la conexión TLS en la propia máquina virtual. Por ejemplo, la capacidad para claves de sesión TLS solo está limitada por el tipo y número de máquinas virtuales que se agregan al grupo de servidores back-end.

* **Conexiones de salida (SNAT)** : Todos los flujos salientes desde direcciones IP privadas dentro de la red virtual a direcciones IP públicas en Internet se pueden traducir en una dirección IP de front-end del Load Balancer. Cuando un front-end público está asociado a una máquina virtual de back-end mediante una regla de equilibrio de carga, Azure traduce las conexiones salientes a la dirección IP de front-end pública. Esta configuración tiene las siguientes ventajas:
  * Actualización sencilla y recuperación ante desastres de servicios, ya que el front-end se puede asignar dinámicamente a otra instancia del servicio.
  * Facilita la administración de la lista de control de acceso (ACL). Las ACL que se expresan como direcciones IP de front-end no cambian a medida que los servicios se escalan o bajan horizontalmente o se vuelve a implementar. La traducción de las conexiones salientes a un número menor de direcciones IP que las máquinas reduce la carga de la implementación de listas de destinatarios seguras.
Para más información, consulte [Las conexiones salientes en Azure](load-balancer-outbound-connections.md).
Standard Load Balancer tiene capacidades adicionales específicas de SKU más allá de estos aspectos fundamentales, tal y como se describe a continuación.

## <a name="skus"></a> Comparación de las SKU de Load Balancer

Load Balancer admite las SKU básica y estándar. Estas SKU difieren en la escala, las características y los precios del escenario. Cualquier escenario que sea posible con el Load Balancer Básico se puede crear con Standard Load Balancer. Las API de ambas SKU son similares y se invocan a través de la especificación de una SKU. La API que admite SKU para Load Balancer y la dirección IP pública está disponible con la `2017-08-01` API. Ambas SKU comparten la misma estructura y API generales.

La configuración completa del escenario podría diferir ligeramente en función de la SKU. La documentación de Load Balancer indica si un artículo se aplica únicamente a una SKU específica. Para comparar y comprender las diferencias, consulte la siguiente tabla. Para más información, consulte [Información general sobre Standard Load Balancer de Azure](load-balancer-standard-overview.md).

>[!NOTE]
> Microsoft recomienda Standard Load Balancer.
Las máquinas virtuales independientes, los conjuntos de disponibilidad y los conjuntos de escalado de máquinas virtuales solo se pueden conectar a una SKU, nunca a ambas. Load Balancer y la SKU de la dirección IP pública deben coincidir cuando se usan con direcciones IP públicas. Load Balancer y las SKU de IP públicas no son mutables.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Para más información, consulte [Límites del equilibrador de carga](https://aka.ms/lblimits). Para más información de Load Balancer Estándar, consulte los artículos de [introducción](load-balancer-standard-overview.md), [precios](https://aka.ms/lbpricing) y [Acuerdo de Nivel de Servicio](https://aka.ms/lbsla).

## <a name="load-balancer-types"></a>Tipos de Load Balancer

### <a name = "publicloadbalancer"></a>Equilibrador de carga público

Una Load Balancer pública asigna la dirección IP pública y el puerto del tráfico entrante a la dirección IP privada y al puerto de la máquina virtual. Load Balancer asigna el tráfico de la otra manera en torno al tráfico de respuesta de la máquina virtual. Puede distribuir determinados tipos de tráfico entre varias máquinas virtuales o servicios aplicando reglas de equilibrio de carga. Por ejemplo, puede distribuir la carga del tráfico de solicitudes web entre varios servidores web.

>[!NOTE]
>Solo puede implementar un Load Balancer público y un Load Balancer interno por conjunto de disponibilidad.

En la ilustración siguiente se muestra un punto de conexión con equilibrio de carga para el tráfico web que se comparte entre tres máquinas virtuales para el puerto público y TCP 80. Estas tres máquinas virtuales se encuentran en un conjunto con equilibrio de carga.

![Ejemplo de equilibrador de carga público](./media/load-balancer-overview/IC727496.png)

*Ilustración: Equilibrio del tráfico web mediante un Load Balancer público*

Los clientes de Internet envían solicitudes de páginas web a la dirección IP pública de una aplicación web en el puerto TCP 80. Azure Load Balancer distribuye las solicitudes entre las tres máquinas virtuales del conjunto de carga equilibrada. Para más información sobre los algoritmos de Load Balancer, consulte [Conceptos de Load Balancer](load-balancer-overview.md##load-balancer-concepts).

Azure Load Balancer distribuye el tráfico de red equitativamente entre varias instancias de máquina virtual de forma predeterminada. También puede configurar la afinidad de la sesión. Para más información, consulte [Configurar el modo de distribución para Azure Load Balancer](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a>Equilibrador de carga interno

Un Load Balancer interno solo dirige el tráfico a los recursos que están dentro de una red virtual o que usan una VPN para acceder a la infraestructura de Azure, a diferencia de un Load Balancer público. La infraestructura de Azure restringe el acceso a las direcciones IP de front-end con equilibrio de carga de una red virtual. Las direcciones IP de front-end y las redes virtuales no se exponen nunca directamente a un punto de conexión de Internet. Las aplicaciones de línea de negocio internas se ejecutan en Azure y se accede a ellas desde Azure o desde recursos locales.

Un equilibrador de carga interno permite los siguientes tipos de equilibrio de carga:

* **Dentro de una red virtual**: Equilibrio de carga de las máquinas virtuales de la red virtual a un conjunto de máquinas virtuales que se encuentran en la misma red virtual.
* **En una red virtual entre entornos locales**: Equilibrio de carga desde equipos locales a un conjunto de máquinas virtuales que se encuentran en la misma red virtual.
* **En aplicaciones de niveles múltiples**: Equilibrio de carga para aplicaciones de niveles múltiples accesibles desde Internet en las que los niveles de back-end no son accesibles desde Internet. Los niveles de back-end requieren un equilibrio de carga de tráfico en el nivel accesible desde Internet. Consulte la ilustración siguiente.
* **En aplicaciones de línea de negocio**: equilibrio de carga para las aplicaciones de línea de negocio hospedadas en Azure sin requerir hardware ni software adicional de equilibrador de carga. Este escenario incluye servidores locales que se encuentran en el conjunto de equipos cuyo tráfico tiene equilibrio de carga.

![Ejemplo de equilibrador de carga interno](./media/load-balancer-overview/IC744147.png)

*Ilustración: Equilibrar las aplicaciones de niveles múltiples mediante Load Balancer público e interno*

## <a name="pricing"></a>Precios

Se cobra el uso de Standard Load Balancer.

* Número de reglas de equilibrio de carga y de salida configuradas. Las reglas NAT de entrada no cuentan en el número total de reglas.
* Cantidad de datos procesados de entrada y salida independientes de las reglas.

Para más información sobre los precios de Standard Load Balancer, consulte [Precios de Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/).

Load Balancer Básico se ofrece sin cargo.

## <a name="sla"></a>Contrato de nivel de servicio

Para más información sobre el contrato de nivel de servicio de Standard Load Balancer, consulte [Contrato de nivel de servicio para Load Balancer](https://aka.ms/lbsla).

## <a name="limitations"></a>Limitaciones

* Load Balancer proporciona equilibrio de carga y reenvío de puertos para protocolos TCP o UDP específicos. Las reglas de equilibrio de carga y las reglas NAT de entrada admiten TCP y UDP, pero no otros protocolos IP, incluido ICMP.

  Load Balancer no finaliza, responde o interactúa de otro modo con la carga de un flujo UDP o TCP. No es un proxy. La validación correcta de la conectividad con un front-end debe realizarse en banda con el mismo protocolo que se usa en una regla NAT de entrada o de equilibrio de carga. Al menos una de las máquinas virtuales debe generar una respuesta para que un cliente vea una respuesta de un front-end.

  No recibir una respuesta en banda del Load Balancer front-end indica que ninguna máquina virtual podría responder. Nada puede interactuar con un front-end Load Balancer sin que una máquina virtual pueda responder. Este principio se aplica también a las conexiones salientes donde el puerto de enmascaramiento SNAT solo se admite para TCP y UDP. Cualquier otro protocolo IP, incluido ICMP, produce un error. Asigne una dirección IP pública de nivel de instancia para mitigar este problema. Para más información, consulte [Descripción de SNAT y PAT](load-balancer-outbound-connections.md#snat).

* Los equilibradores de carga internos no convierten las conexiones salientes originadas en el front-end de una Load Balancer interna porque ambos están en un espacio de direcciones IP privado. Los equilibradores de carga públicos proporcionan [conexiones salientes](load-balancer-outbound-connections.md) desde direcciones IP privadas dentro de la red virtual a direcciones IP públicas. En el caso de los equilibradores de carga internos, este enfoque evita el agotamiento del puerto SNAT potencial en un espacio de direcciones IP interno único, donde no se requiere traducción.

  Un efecto secundario es que si un flujo de salida de una máquina virtual del grupo de servidores back-end intenta un flujo hacia el front-end del Load Balancer interno en su grupo _y_ se asigna a sí mismo, las dos piernas del flujo no coinciden. Dado que no coinciden, se produce un error en el flujo. El flujo se realiza correctamente si el flujo no se ha asignado a la misma máquina virtual del grupo de servidores back-end que creó el flujo al front-end.

  Cuando el flujo se vuelve a asignar a sí mismo, el flujo de salida parece originarse desde la máquina virtual hasta el front-end y el flujo de entrada correspondiente parece provenir de la máquina virtual. Desde el punto de vista del sistema operativo invitado, las partes entrantes y salientes del mismo flujo no coinciden dentro de la máquina virtual. La pila TCP no reconocerá estas mitades del mismo flujo como parte del mismo flujo. El origen y el destino no coinciden. Cuando el flujo se asigna a cualquier otra máquina virtual del grupo de back-end, las mitades del flujo coinciden y la máquina virtual puede responder al flujo.

  El síntoma de este escenario es el tiempo de espera de conexión intermitente cuando el flujo vuelve al mismo back-end que originó el flujo. Las soluciones alternativas comunes incluyen la inserción de una capa de proxy detrás del Load Balancer interno y el uso de reglas de estilo de Direct Server Return (DSR). Para más información, consulte [Varios front-ends para Azure Load Balancer](load-balancer-multivip-overview.md).

  Puede combinar una Load Balancer interna con cualquier proxy de terceros o usar el [Application Gateway](../application-gateway/application-gateway-introduction.md) interno para escenarios de proxy con HTTP/HTTPS. Aunque podría usar un Load Balancer público para mitigar este problema, el escenario resultante es propenso a [agotamiento de SNAT](load-balancer-outbound-connections.md#snat). Evite este segundo enfoque a menos que se administre con cuidado.

* En general, no se admite el reenvío de fragmentos IP en reglas de equilibrio de carga. No se admite la fragmentación IP de paquetes UDP y TCP en reglas de equilibrio de carga. Las reglas de equilibrio de carga de puertos de alta disponibilidad se pueden usar para reenviar fragmentos IP existentes. Para más información, consulte [Información general sobre los puertos de alta disponibilidad](load-balancer-ha-ports-overview.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte [Creación de una instancia de Standard Load Balancer](quickstart-load-balancer-standard-public-portal.md) para empezar a usar una instancia de Load Balancer: crear una, crear máquinas virtuales con una extensión de IIS personalizada instalada y equilibrar la carga de la aplicación web entre las máquinas virtuales.
