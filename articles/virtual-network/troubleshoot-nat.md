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
ms.date: 03/04/2020
ms.author: allensu
ms.openlocfilehash: d56cd3b3d286d69a51d8cc14eb8020343cf7295a
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302991"
---
# <a name="troubleshoot-azure-virtual-network-nat-connectivity-problems"></a>Solución de los problemas de conectividad de Azure Virtual Network NAT

En este artículo se ayuda a los administradores a diagnosticar y resolver problemas de conectividad cuando se usa Virtual Network NAT.

>[!NOTE] 
>En este momento, Virtual Network NAT está disponible como versión preliminar pública. Actualmente solo está disponible en un conjunto limitado de [regiones](nat-overview.md#region-availability). Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

## <a name="problems"></a>Problemas

- [Agotamiento de la arquitectura de redes de sistemas](#snat-exhaustion).
- [Error en el ping de ICMP](#icmp-ping-is-failing).

Para resolver estos problemas, siga los pasos de la siguiente sección.

## <a name="resolution"></a>Solución

### <a name="snat-exhaustion"></a>Agotamiento de la arquitectura de redes de sistemas

Un solo [recurso de puerta de enlace de NAT](nat-gateway-resource.md) admite entre 64 000 y 1 millón de flujos simultáneos.  Cada dirección IP proporciona 64 000 puertos de SNAT al inventario disponible. Se pueden usar un máximo de 16 direcciones IP por recurso de puerta de enlace de NAT.  El mecanismo de la arquitectura de redes de sistemas se describe [aquí](nat-gateway-resource.md#source-network-address-translation) de forma más detallada.

#### <a name="steps"></a>Pasos:

1. Investigue la forma en que la aplicación crea conectividad saliente (por ejemplo la revisión del código o la captura de paquetes). 
2. Determine si esta actividad es el comportamiento esperado o si la aplicación no se comporta correctamente.  Use [métricas](nat-metrics.md) en Azure Monitor para apoyar sus conclusiones.
3. Evalúe si se siguen los patrones adecuados.
4. Evalúe si el agotamiento de puertos de SNAT debe mitigarse con la asignación de más direcciones IP a un recurso de puerta de enlace de NAT.

#### <a name="design-pattern"></a>Patrón de diseño:

Aproveche la reutilización de las conexiones y la agrupación de conexiones siempre que sea posible.  Este patrón evitará problemas de agotamiento total de los recursos y el resultado será un comportamiento predecible. Las primitivas de estos patrones se pueden encontrar en muchas bibliotecas y marcos de desarrollo.
- Considere la posibilidad de usar [patrones de sondeo asincrónicos](https://docs.microsoft.com/azure/architecture/patterns/async-request-reply) para las operaciones de ejecución prolongada, con el fin de liberar recursos de conexión para otras operaciones.
- Los flujos de larga duración (como las conexiones TCP reutilizadas) deberían usar paquetes keepalive de TCP o de la capa de la aplicación para evitar que los sistemas intermedios superen el tiempo de espera.
- Se deben usar [patrones de reintento](https://docs.microsoft.com/azure/architecture/patterns/retry) correctos para evitar reintentos o ráfagas agresivos durante un error transitorio o la recuperación de un error.
La creación de una conexión TCP para cada operación HTTP (también conocidas como "conexiones atómicas") es un antipatrón.  Las conexiones atómicas evitarán que su aplicación se escale de forma correcta y malgastarán recursos.  Canalice siempre varias operaciones a la misma conexión.  Su aplicación aumentará la velocidad de las transacciones y reducirá los costos de los recursos.  Cuando la aplicación usa un cifrado de la capa de transporte (por ejemplo, TLS), se produce un considerable costo asociado con el procesamiento de nuevas conexiones.  Para conocer otros patrones de procedimientos recomendados, consulte [Patrones de diseño en la nube de Azure](https://docs.microsoft.com/azure/architecture/patterns/).

#### <a name="mitigations"></a>Mitigaciones

La conectividad saliente se puede escalar como se indica a continuación:

| Escenario | Mitigación |
|---|---|
| Percibe una contención de los puertos SNAT y un agotamiento de estos en periodos de mucho uso. | Determine si puede agregar recursos con direcciones IP públicas o recursos con prefijos IP públicos. Esta incorporación permitirá que haya un máximo de 16 direcciones IP en la puerta de enlace de NAT. También proporcionará más inventario para los puertos SNAT disponibles (64 000 por dirección IP) y le permitirá escalar aún más su escenario.|
| Ya se le han proporcionado 16 direcciones IP y sigue sufriendo un agotamiento de puertos SNAT. | Distribuya el entorno de aplicaciones entre varias subredes y proporcione un recurso de puerta de enlace de NAT para cada subred. |

>[!NOTE]
>Es importante saber por qué se produce el agotamiento de SNAT. Asegúrese de que usa los patrones correctos para escenarios escalables y confiables.  La incorporación de más puertos SNAT a un escenario sin conocer la causa de la demanda sería el último recurso. Si desconoce los motivos por los que su escenario supone una mayor presión para el inventario de puertos SNAT, la incorporación de más puertos SNAT al inventario mediante la adición de más direcciones IP lo único que hará será retrasar el error de agotamiento cuando la aplicación escale.  Es posible que esté ocultando otras ineficacias y antipatrones.

### <a name="icmp-ping-is-failing"></a>Error en el ping de ICMP

[Virtual Network NAT](nat-overview.md) admite los protocolos UDP y TCP de IPv4. ICMP no se admite, por lo que cabe esperar que se produzca un error.  En su lugar, use pruebas de conexión TCP (por ejemplo, "ping de TCP") y pruebas de la capa de la aplicación específicas de UDP para asegurar la conectividad de un extremo a otro.

La siguiente tabla se puede usar como punto de partida para saber qué herramientas se deben usar para iniciar las pruebas.

| Sistema operativo | Prueba de conexión TCP genérica | Prueba de la capa de la aplicación de TCP | UDP |
|---|---|---|---|
| Linux | nc (prueba de conexión genérica) | curl (prueba de la capa de la aplicación de TCP) | específica de la aplicación |
| Windows | [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) | [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) de PowerShell | específica de la aplicación |

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [Virtual Network NAT](nat-overview.md).
- Obtenga información sobre los [recursos de puerta de enlace de NAT](nat-gateway-resource.md)
- Obtenga información acerca de las [métricas y alertas de los recursos de puerta de enlace NAT](nat-metrics.md).
