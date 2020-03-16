---
title: Solución de los problemas de conectividad de Azure Virtual Network NAT
titleSuffix: Azure Virtual Network NAT troubleshooting
description: Solucione los problemas de Azure Virtual Network NAT.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to troubleshoot Virtual Network NAT.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/05/2020
ms.author: allensu
ms.openlocfilehash: c629b3425cd095a6ac9d305b5cd6de58ed9d572a
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78674324"
---
# <a name="troubleshoot-azure-virtual-network-nat-connectivity-problems"></a>Solución de los problemas de conectividad de Azure Virtual Network NAT

En este artículo se ayuda a los administradores a diagnosticar y resolver problemas de conectividad cuando se usa Virtual Network NAT.

>[!NOTE] 
>En este momento, Virtual Network NAT está disponible como versión preliminar pública. Actualmente solo está disponible en un conjunto limitado de [regiones](nat-overview.md#region-availability). Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

## <a name="problems"></a>Problemas

* [Agotamiento de SNAT](#snat-exhaustion)
* [Error en el ping de ICMP](#icmp-ping-is-failing)
* [Errores de conectividad](#connectivity-failures)
* [Coexistencia de IPv6](#ipv6-coexistence)

Para resolver estos problemas, siga los pasos de la siguiente sección.

## <a name="resolution"></a>Solución

### <a name="snat-exhaustion"></a>Agotamiento de la arquitectura de redes de sistemas

Un solo [recurso de puerta de enlace de NAT](nat-gateway-resource.md) admite entre 64 000 y 1 millón de flujos simultáneos.  Cada dirección IP proporciona 64 000 puertos de SNAT al inventario disponible. Se pueden usar un máximo de 16 direcciones IP por recurso de puerta de enlace de NAT.  El mecanismo de la arquitectura de redes de sistemas se describe [aquí](nat-gateway-resource.md#source-network-address-translation) de forma más detallada.

Con frecuencia, la causa principal del agotamiento de SNAT es un antipatrón para la manera en que se establece y administra la conectividad saliente.  Consulte detenidamente esta sección.

#### <a name="steps"></a>Pasos

1. Investigue la forma en que la aplicación crea conectividad saliente (por ejemplo la revisión del código o la captura de paquetes). 
2. Determine si esta actividad es el comportamiento esperado o si la aplicación no se comporta correctamente.  Use [métricas](nat-metrics.md) en Azure Monitor para apoyar sus conclusiones. Use la categoría "Error" para la métrica de las conexiones SNAT.
3. Evalúe si se siguen los patrones adecuados.
4. Evalúe si el agotamiento de puertos de SNAT debe mitigarse con la asignación de más direcciones IP a un recurso de puerta de enlace de NAT.

#### <a name="design-patterns"></a>Patrones de diseño

Aproveche la reutilización de las conexiones y la agrupación de conexiones siempre que sea posible.  Estos patrones evitarán problemas de agotamiento total de los recursos y el resultado será un comportamiento predecible, confiable y escalable. Las primitivas de estos patrones se pueden encontrar en muchas bibliotecas y marcos de desarrollo.

_**Solución:**_ Uso de patrones adecuados

- Considere la posibilidad de usar [patrones de sondeo asincrónicos](https://docs.microsoft.com/azure/architecture/patterns/async-request-reply) para las operaciones de ejecución prolongada, con el fin de liberar recursos de conexión para otras operaciones.
- Los flujos de larga duración (como las conexiones TCP reutilizadas) deberían usar paquetes keepalive de TCP o de la capa de la aplicación para evitar que los sistemas intermedios superen el tiempo de espera.
- Se deben usar [patrones de reintento](https://docs.microsoft.com/azure/architecture/patterns/retry) correctos para evitar reintentos o ráfagas agresivos durante un error transitorio o la recuperación de un error.
La creación de una conexión TCP para cada operación HTTP (también conocidas como "conexiones atómicas") es un antipatrón.  Las conexiones atómicas evitarán que su aplicación se escale de forma correcta y malgastarán recursos.  Canalice siempre varias operaciones a la misma conexión.  Su aplicación aumentará la velocidad de las transacciones y reducirá los costos de los recursos.  Cuando la aplicación usa un cifrado de la capa de transporte (por ejemplo, TLS), se produce un considerable costo asociado con el procesamiento de nuevas conexiones.  Para conocer otros patrones de procedimientos recomendados, consulte [Patrones de diseño en la nube de Azure](https://docs.microsoft.com/azure/architecture/patterns/).

#### <a name="possible-mitigations"></a>Posibles remedios

_**Solución:**_ Escale la conectividad saliente como se indica a continuación:

| Escenario | Evidencia |Mitigación |
|---|---|---|
| Percibe una contención de los puertos SNAT y un agotamiento de estos en periodos de mucho uso. | La categoría "Error" para la [métrica](nat-metrics.md) de Conexiones SNAT en Azure Monitor muestra los errores transitorios o persistentes a lo largo del tiempo y un volumen de conexión elevado.  | Determine si puede agregar recursos con direcciones IP públicas o recursos con prefijos IP públicos. Esta incorporación permitirá que haya un máximo de 16 direcciones IP en la puerta de enlace de NAT. También proporcionará más inventario para los puertos SNAT disponibles (64 000 por dirección IP) y le permitirá escalar aún más su escenario.|
| Ya se le han proporcionado 16 direcciones IP y sigue sufriendo un agotamiento de puertos SNAT. | Error al intentar agregar una dirección IP adicional. El número total de direcciones IP de los recursos de dirección IP pública o de prefijo de IP pública supera un total de 16. | Distribuya el entorno de aplicaciones entre varias subredes y proporcione un recurso de puerta de enlace de NAT para cada subred.  Vuelva a evaluar los patrones de diseño para optimizar según la [guía](#design-patterns) anterior. |

>[!NOTE]
>Es importante saber por qué se produce el agotamiento de SNAT. Asegúrese de que usa los patrones correctos para escenarios escalables y confiables.  La incorporación de más puertos SNAT a un escenario sin conocer la causa de la demanda sería el último recurso. Si desconoce los motivos por los que su escenario supone una mayor presión para el inventario de puertos SNAT, la incorporación de más puertos SNAT al inventario mediante la adición de más direcciones IP lo único que hará será retrasar el error de agotamiento cuando la aplicación escale.  Es posible que esté ocultando otras ineficacias y antipatrones.

### <a name="icmp-ping-is-failing"></a>Error en el ping de ICMP

[Virtual Network NAT](nat-overview.md) admite los protocolos UDP y TCP de IPv4. ICMP no se admite, por lo que cabe esperar que se produzca un error.  

_**Solución:**_ En su lugar, use pruebas de conexión TCP (por ejemplo, "ping de TCP") y pruebas de la capa de la aplicación específicas de UDP para asegurar la conectividad de un extremo a otro.

La siguiente tabla se puede usar como punto de partida para saber qué herramientas se deben usar para iniciar las pruebas.

| Sistema operativo | Prueba de conexión TCP genérica | Prueba de la capa de la aplicación de TCP | UDP |
|---|---|---|---|
| Linux | nc (prueba de conexión genérica) | curl (prueba de la capa de la aplicación de TCP) | específica de la aplicación |
| Windows | [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) | [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) de PowerShell | específica de la aplicación |

### <a name="connectivity-failures"></a>Errores de conectividad

Los problemas de conectividad con [Virtual Network NAT](nat-overview.md) pueden deberse a varios problemas:

* El [agotamiento de SNAT](#snat-exhaustion) transitorio o persistente de la puerta de enlace NAT
* Errores transitorios en la infraestructura de Azure 
* Errores transitorios en la ruta de acceso entre Azure y el destino público de Internet 
* Errores transitorios o persistentes en el destino de Internet público.

Use herramientas como la siguiente para la validación de la conectividad. [No se admite el ping de ICMP](#icmp-ping-is-failing).

| Sistema operativo | Prueba de conexión TCP genérica | Prueba de la capa de la aplicación de TCP | UDP |
|---|---|---|---|
| Linux | nc (prueba de conexión genérica) | curl (prueba de la capa de la aplicación de TCP) | específica de la aplicación |
| Windows | [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) | [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) de PowerShell | específica de la aplicación |

#### <a name="snat-exhaustion"></a>Agotamiento de la arquitectura de redes de sistemas

Revise la sección sobre el [agotamiento de SNAT](#snat-exhaustion) en este artículo.

#### <a name="azure-infrastructure"></a>Infraestructura de Azure

Aunque Azure supervisa y opera su infraestructura con gran cuidado, pueden producirse errores transitorios, ya que no hay ninguna garantía de que las transmisiones sean sin pérdida.  Utilice patrones de diseño que permitan las retransmisiones SYN para las aplicaciones TCP. Use tiempos de espera de conexión lo bastante prolongados como para permitir la retransmisión TCP SYN a fin de reducir los efectos transitorios causados por la pérdida de un paquete SYN.

_**Solución:**_

* Compruebe el [agotamiento de SNAT](#snat-exhaustion).
* El parámetro de configuración de una pila TCP que controla el comportamiento de la retransmisión de SYN se denomina RTO ([tiempo de espera de la retransmisión](https://tools.ietf.org/html/rfc793)). El valor de RTO es ajustable, pero normalmente es de 1 segundo o más, de forma predeterminada, con retroceso exponencial.  Si el tiempo de espera de la conexión de la aplicación es demasiado corto (por ejemplo, 1 segundo), es posible que vea tiempos de espera de conexión esporádicos.  Aumente el tiempo de espera de conexión de la aplicación.
* Si observa tiempos de espera más largos e inesperados con comportamientos de aplicación predeterminados, abra un caso de soporte técnico para solucionar el problema.

No se recomienda reducir artificialmente el tiempo de espera de la conexión TCP ni ajustar el parámetro RTO.

#### <a name="public-internet-transit"></a>Tránsito público por Internet

La probabilidad de errores transitorios aumenta con una ruta de acceso más larga al destino y más sistemas intermedios. Se espera que los errores transitorios puedan aumentar la frecuencia en la [infraestructura de Azure](#azure-infrastructure). 

Siga las mismas instrucciones que en la sección [Infraestructura de Azure](#azure-infrastructure).

#### <a name="internet-endpoint"></a>Punto de conexión de Internet

Las secciones anteriores se aplican además de las consideraciones relacionadas con el punto de conexión de Internet con el que se establece la comunicación. Otros factores que pueden afectar al éxito de la conectividad son:

* Administración del tráfico en el lado de destino, incluido
- Limitación de la tasa de API impuesta por el lado de destino
- Formas de tráfico de la capa de transporte o de mitigación de DDoS volumétricas
* Firewall u otros componentes en el destino 

Normalmente, se necesitan capturas de paquetes en el origen además de en el destino (si está disponible) para determinar lo que está teniendo lugar.

_**Solución:**_

* Compruebe el [agotamiento de SNAT](#snat-exhaustion). 
* Valide la conectividad a un punto de conexión en la misma región o en otra parte para la comparación.  
* Si va a crear pruebas de gran volumen o de tasa de transacciones, explore si la reducción de la velocidad reduce la aparición de errores.
* Si la tasa de cambio afecta a la tasa de errores, compruebe si es posible que se haya alcanzado el límite de velocidad de la API u otras restricciones en el lado de destino.
* Si la investigación no es concluyente, abra un caso de soporte técnico para solucionar el problema.

#### <a name="tcp-resets-received"></a>Restablecimientos TCP recibidos

Si observa que se reciben restablecimientos TCP (paquetes TCP RST) en la máquina virtual de origen, puede que los haya generado la puerta de enlace NAT en el lado privado para los flujos que no se reconocen como en curso.  Una posible razón es que la conexión TCP haya agotado el tiempo de espera.  Puede ajustar el tiempo de espera de inactividad entre 4 minutos y un máximo de 120 minutos.

Los restablecimientos TCP no se generan en el lado público de los recursos de puerta de enlace NAT. Si recibe restablecimientos TCP en el lado de destino, los genera la pila de la máquina virtual de origen y no el recurso de puerta de enlace NAT.

_**Solución:**_

* Revise las recomendaciones de [patrones de diseño](#design-patterns).  
* Abra un caso de soporte técnico para solucionar el problema si es necesario.

### <a name="ipv6-coexistence"></a>Coexistencia de IPv6

[Virtual Network NAT](nat-overview.md) admite los protocolos UDP y TCP de IPv4, y la implementación en una [subred con prefijo IPv6 no es compatible](nat-overview.md#limitations).

_**Solución:**_ Implemente la puerta de enlace NAT en una subred sin prefijo IPv6.

Puede indicar el interés en funcionalidades adicionales mediante [UserVoice de Virtual Network NAT](https://aka.ms/natuservoice).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [Virtual Network NAT](nat-overview.md).
* Obtenga información sobre los [recursos de puerta de enlace de NAT](nat-gateway-resource.md)
* Obtenga información acerca de las [métricas y alertas de los recursos de puerta de enlace NAT](nat-metrics.md).
* [Indíquenos qué crear a continuación para Virtual Network NAT en UserVoice](https://aka.ms/natuservoice).

