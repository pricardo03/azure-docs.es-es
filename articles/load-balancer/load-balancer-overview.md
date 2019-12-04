---
title: ¿Qué es Azure Load Balancer?
titleSuffix: Azure Load Balancer
description: Información general sobre las características, la arquitectura y la implementación del Equilibrador de carga de Azure Aprenda cómo funciona el equilibrador de carga y aproveche sus ventajas en la nube.
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
ms.date: 11/21/2019
ms.author: allensu
ms.openlocfilehash: 335549f4ccae01fa36921e0e4668fa15e8b33835
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2019
ms.locfileid: "74423908"
---
# <a name="what-is-azure-load-balancer"></a>¿Qué es Azure Load Balancer?

Azure Load Balancer permite escalar las aplicaciones y crear alta disponibilidad para sus servicios. Load Balancer admite escenarios de entrada y de salida, proporciona baja latencia y alto rendimiento, y puede escalar hasta millones de flujos para todas las aplicaciones TCP y UDP.

Load Balancer distribuye nuevos flujos entrantes que llegan al front-end del Load Balancer a las instancias del grupo de servidores back-end, de acuerdo con las reglas y los sondeos de estado especificados.

Un Load Balancer público puede proporcionar conexiones salientes para máquinas virtuales (VM) dentro de la red virtual mediante la traducción de sus direcciones IP privadas a direcciones IP públicas.

Azure Load Balancer está disponible en dos planes de tarifa o *SKUs*: Básico y Estándar. Hay diferencias en la escala, las características y los precios. También se pueden crear cualquier escenario con la Load Balancer Básico con Standard Load Balancer, aunque los enfoques difieren ligeramente. Cuando obtenga información sobre Load Balancer, familiarícese con los conceptos básicos y las diferencias específicas de cada SKU.

## <a name="why-use-load-balancer"></a>¿Por qué usar Load Balancer?

Azure Load Balancer sirve para:

* Equilibre la carga del tráfico entrante de Internet a las máquinas virtuales. Esta configuración se conoce como [equilibrador de carga interno](#publicloadbalancer).
* Equilibrio de carga del tráfico entre máquinas virtuales dentro de una red virtual. También puede acceder a un servidor front-end de Load Balancer desde una red local en un escenario híbrido. Ambos escenarios utilizan una configuración que se conoce como [equilibrador de carga interno](#internalloadbalancer).
* Reenviar el tráfico a un puerto específico de máquinas virtuales determinadas con reglas de traducción de direcciones de red (NAT) de entrada.
* Proporcionar [conectividad de salida](load-balancer-outbound-connections.md) para máquinas virtuales dentro de la red virtual mediante un equilibrador de carga público.

>[!NOTE]
> Azure ofrece un conjunto de soluciones de equilibrio de carga completamente administradas para sus escenarios. Si busca la finalización del protocolo de Seguridad de la Capa de Transporte (TLS) o la solicitud por HTTP/HTTPS, el procesamiento de la capa de aplicación, consulte [¿Qué es Azure Application Gateway?](../application-gateway/overview.md). Si busca el equilibrio de carga de DNS global, consulte [¿Qué es Traffic Manager?](../traffic-manager/traffic-manager-overview.md). Los escenarios de un extremo a otro pueden beneficiarse de la combinación de estas soluciones.
>
> Para obtener una comparación de las opciones de equilibrio de carga de Azure, consulte [Información general sobre las opciones de equilibrio de carga en Azure](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview).

## <a name="what-are-load-balancer-resources"></a>¿Qué son los recursos de Load Balancer?

Los recursos de Load Balancer son objetos que especifican cómo debe Azure programar su infraestructura de varios inquilinos para lograr el escenario que desea crear. No hay ninguna relación directa entre los recursos de Load Balancer y la infraestructura real. Al crear un equilibrador de carga no se crea una instancia y siempre hay capacidad disponible.

Un recurso de Load Balancer puede ser equilibrador de carga público o un equilibrador de carga interno. Las funciones del recurso de Load Balancer se definen mediante un front-end, una regla, un sondeo de estado y una definición de grupo de back-end. Las máquinas virtuales se colocan en el grupo de back-end mediante la especificación del grupo de back-end desde la máquina virtual.

## <a name="fundamental-load-balancer-features"></a>Características fundamentales de Load Balancer

Load Balancer proporciona las siguientes funcionalidades básicas para las aplicaciones TCP y UDP:

* **Equilibrio de carga**

  Con Azure Load Balancer, puede crear una regla de equilibrio de carga para distribuir el tráfico que llega en el front-end a las instancias del grupo de servidores back-end. Load Balancer usa un algoritmo hash para la distribución de los flujos de entrada y reescribe los encabezados de los flujos en las instancias del grupo de back-end. Un servidor está disponible para recibir nuevos flujos cuando el sondeo de estado indica un punto de conexión de back-end correcto.

  De forma predeterminada, Load Balancer usa un hash de tupla 5. El hash incluye la dirección IP de origen, el puerto de origen, la dirección IP de destino, el puerto de destino y el número de protocolo IP para asignar flujos a los servidores disponibles. Puede crear afinidad en una dirección IP de origen mediante un hash de tupla 2 o 3 para una regla determinada. Todos los paquetes del mismo flujo de paquetes llegan a la misma instancia subyacente al servidor front-end con equilibrio de carga. Cuando el cliente inicia un nuevo flujo desde la misma IP de origen, se cambia el puerto de origen. Como resultado, el hash de tupla 5 puede hacer que el tráfico vaya a un punto de conexión de back-end diferente.

  Para más información, consulte [Configurar el modo de distribución para Azure Load Balancer](load-balancer-distribution-mode.md). En la siguiente imagen se muestra la distribución basada en hash:

  ![Distribución basada en hash](./media/load-balancer-overview/load-balancer-distribution.png)

  *Ilustración: distribución basada en hash*

* **Reenvío de puertos**

  Con Load Balancer, puede crear una regla NAT de entrada. Esta regla NAT reenvía el tráfico desde un puerto específico de una dirección IP de front-end específica a un puerto específico de una instancia de back-end específica dentro de la red virtual. Este reenvío se realiza mediante la misma distribución basada en hash como equilibrio de carga. Los escenarios comunes para esta funcionalidad son las sesiones Protocolo de escritorio remoto (RDP) o Secure Shell (SSH) en las instancias de máquina virtual individuales dentro de una red virtual de Azure.
  
  Puede asignar varios puntos de conexión internos a puertos en la misma dirección IP de front-end. Puede usar las direcciones IP de front-end para administrar de forma remota las máquinas virtuales sin un cuadro de salto adicional.

* **Independencia y transparencia de las aplicaciones**

  Load Balancer no interactúa directamente con TCP o UDP ni con la capa de aplicación. Se puede admitir cualquier escenario de aplicación TCP o UDP. Load Balancer no finaliza ni se originan flujos, interactúe con la carga del flujo o proporcione cualquier función de puerta de enlace de capa de aplicación. Los protocolos de enlace de protocolo siempre se producen directamente entre el cliente y la instancia del grupo de back-end. Una respuesta a un flujo de entrada siempre es una respuesta de una máquina virtual. Cuando llega el flujo a la máquina virtual, también se conserva la dirección IP de origen original.

  * A cada punto de conexión solo le responde una máquina virtual. Por ejemplo, un protocolo de enlace TCP se realiza siempre entre el cliente y la máquina virtual de back-end seleccionada. Una respuesta a una solicitud a un front-end es una respuesta generada por una máquina virtual de back-end. Cuando se valida correctamente la conectividad con un front-end, se valida la conectividad de un extremo a otro con al menos una máquina virtual de back-end.
  * Las cargas de la aplicación son transparentes para Load Balancer. Se puede admitir cualquier aplicación UDP o TCP. En el caso de cargas de trabajo que requieren el procesamiento de solicitudes HTTP o la manipulación de cargas de nivel de aplicación, como el análisis de direcciones URL HTTP, debe usar un equilibrador de carga de nivel 7 como [Application Gateway](https://azure.microsoft.com/services/application-gateway).
  * Dado que Load Balancer no interactúa con la carga TCP y proporciona descarga TLS, puede crear escenarios cifrados de un extremo a otro. El uso de Load Balancer aumenta la escalabilidad horizontal de las aplicaciones TLS al finalizar la conexión TLS en la propia máquina virtual. Por ejemplo, la capacidad para claves de sesión TLS solo está limitada por el tipo y número de máquinas virtuales que se agregan al grupo de servidores back-end. Si requiere la descarga de SSL, el tratamiento del nivel de aplicación o desea delegar la administración de certificados en Azure, use en su lugar un equilibrador de carga de nivel 7 de Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway).

* **Reconfiguración automática**

  Load Balancer se reconfigura inmediatamente al escalar o reducir instancias verticalmente. La adición o eliminación de máquinas virtuales del grupo de back-end reconfigura el Load Balancer sin operaciones adicionales en el recurso de Load Balancer.

* **Sondeos de mantenimiento**

  Load Balancer usa los sondeos de mantenimiento que el usuario define para determinar el mantenimiento de las instancias en el grupo de servidores back-end. Cuando un sondeo no responde, el equilibrador de carga deja de enviar nuevas conexiones a las instancias incorrectas. Un error de sondeo no afecta a las conexiones existentes. La conexión continúa hasta que la aplicación finaliza el flujo, se agota el tiempo de espera de inactividad o se apaga la máquina virtual.

  Load Balancer proporciona diferentes tipos de sondeo de estado para los puntos de conexión TCP, HTTP y HTTPS. Para más información, consulte [Tipos de sondeo](load-balancer-custom-probe-overview.md#types).

  Al usar los servicios en la nube clásica, se permite un tipo adicional: [agente invitado](load-balancer-custom-probe-overview.md#guestagent). Un agente invitado debe considerarse un sondeo de estado de último recurso. Microsoft no los recomienda si hay otras opciones disponibles.

* **Conexiones de salida (SNAT)**

  Todos los flujos salientes desde direcciones IP privadas dentro de la red virtual a direcciones IP públicas en Internet se pueden traducir en una dirección IP de front-end del Load Balancer. Cuando un front-end público está asociado a una máquina virtual de back-end mediante una regla de equilibrio de carga, Azure traduce las conexiones salientes a la dirección IP de front-end pública. Esta configuración tiene las siguientes ventajas:

  * Actualización sencilla y recuperación ante desastres de servicios, ya que el front-end se puede asignar dinámicamente a otra instancia del servicio.
  * Facilita la administración de la lista de control de acceso (ACL). Las ACL que se expresan como direcciones IP de front-end no cambian a medida que los servicios se escalan o bajan horizontalmente o se vuelve a implementar. La traducción de las conexiones salientes a un número menor de direcciones IP que las máquinas reduce la carga de la implementación de listas de destinatarios seguras.

  Para más información, consulte [Las conexiones salientes en Azure](load-balancer-outbound-connections.md).

Standard Load Balancer tiene capacidades adicionales específicas de SKU más allá de estos aspectos fundamentales, tal y como se describe a continuación.

## <a name="skus"></a> Comparación de las SKU de Load Balancer

Load Balancer admite las SKU básica y estándar. Estas SKU difieren en la escala, las características y los precios del escenario. Cualquier escenario que sea posible con el Load Balancer Básico se puede crear con Standard Load Balancer. Las API de ambas SKU son similares y se invocan a través de la especificación de una SKU. La API que admite SKU para Load Balancer y la dirección IP pública está disponible con la `2017-08-01` API. Ambas SKU comparten la misma estructura y API generales.

La configuración completa del escenario podría diferir ligeramente en función de la SKU. La documentación de Load Balancer indica si un artículo se aplica únicamente a una SKU específica. Para comparar y comprender las diferencias, consulte la siguiente tabla. Para más información, consulte [Información general sobre Standard Load Balancer de Azure](load-balancer-standard-overview.md).

>[!NOTE]
> Los nuevos diseños deben adoptar Load Balancer Estándar.

Las máquinas virtuales independientes, los conjuntos de disponibilidad y los conjuntos de escalado de máquinas virtuales solo se pueden conectar a una SKU, nunca a ambas. Load Balancer y la SKU de la dirección IP pública deben coincidir cuando se usan con direcciones IP públicas. Load Balancer y las SKU de IP públicas no son mutables.

El procedimiento recomendado es especificar las SKU explícitamente. En este momento, se están realizando los cambios necesarios mínimos. Si no se especifica una SKU, el valor predeterminado es la `2017-08-01` versión de API  de la SKU básica.

>[!IMPORTANT]
>Standard Load Balancer es un nuevo producto de Load Balancer. En gran medida, es un superconjunto de Load Balancer Básico, pero hay diferencias importantes entre los dos productos. Cualquier escenario posible con Load Balancer Básico puede crearse también con Load Balancer Estándar. Si ya usa el Load Balancer Básico, compárelo con Standard Load Balancer para comprender los cambios más recientes en su comportamiento.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Para más información, consulte [Límites del equilibrador de carga](https://aka.ms/lblimits). Para más información de Load Balancer Estándar, consulte los artículos de [introducción](load-balancer-standard-overview.md), [precios](https://aka.ms/lbpricing) y [Acuerdo de Nivel de Servicio](https://aka.ms/lbsla).

## <a name="concepts"></a>Conceptos

### <a name = "publicloadbalancer"></a>Equilibrador de carga público

Una Load Balancer pública asigna la dirección IP pública y el puerto del tráfico entrante a la dirección IP privada y al puerto de la máquina virtual. Load Balancer asigna el tráfico de la otra manera en torno al tráfico de respuesta de la máquina virtual. Puede distribuir determinados tipos de tráfico entre varias máquinas virtuales o servicios aplicando reglas de equilibrio de carga. Por ejemplo, puede distribuir la carga del tráfico de solicitudes web entre varios servidores web.

>[!NOTE]
>Solo puede implementar un Load Balancer público y un Load Balancer interno por conjunto de disponibilidad.

En la ilustración siguiente se muestra un punto de conexión con equilibrio de carga para el tráfico web que se comparte entre tres máquinas virtuales para el puerto público y TCP 80. Estas tres máquinas virtuales se encuentran en un conjunto con equilibrio de carga.

![Ejemplo de equilibrador de carga público](./media/load-balancer-overview/IC727496.png)

*Ilustración: Equilibrio del tráfico web mediante un Load Balancer público*

Los clientes de Internet envían solicitudes de páginas web a la dirección IP pública de una aplicación web en el puerto TCP 80. Azure Load Balancer distribuye las solicitudes entre las tres máquinas virtuales del conjunto de carga equilibrada. Para más información acerca de los algoritmos de Load Balancer, consulte [Características fundamentales de Load Balancer](load-balancer-overview.md##fundamental-load-balancer-features).

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

Consulte [Crear un Load Balancer Básico](quickstart-create-basic-load-balancer-portal.md) para empezar a usar un Load Balancer: crear uno, crear máquinas virtuales con una extensión de IIS personalizada instalada y equilibrar la carga de la aplicación web entre las máquinas virtuales.
