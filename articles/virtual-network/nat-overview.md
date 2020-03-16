---
title: ¿Qué es Azure Virtual Network NAT?
description: Información general sobre las características, recursos, arquitectura e implementación de Virtual Network NAT. Aprenda cómo funciona Virtual Network NAT y cómo usar los recursos de puerta de enlace de NAT en la nube.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to learn more about Virtual Network NAT, its NAT gateway resources, and what I can use them for.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/05/2020
ms.author: allensu
ms.openlocfilehash: 205826a6ad952383582f5a8086cbd8b85dbc3794
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78359252"
---
# <a name="what-is-virtual-network-nat-public-preview"></a>¿Qué es Virtual Network NAT (versión preliminar pública)?

La NAT (traducción de direcciones de red) de Virtual Network simplifica la conectividad a Internet de solo salida para redes virtuales. Cuando se configura en una subred, todas las conexiones salientes usan las direcciones IP públicas estáticas que se hayan especificado.  La conectividad saliente es posible sin que el equilibrador de carga ni las direcciones IP públicas estén conectados directamente a máquinas virtuales. La NAT está totalmente administrada y es muy resistente.

<!-- 
<img src="./media/nat-overview/flow-map.svg" width="270" align="center">
![Virtual Network NAT](./media/nat-overview/flow-map.svg)
-->

<p align="center">
  <img src="./media/nat-overview/flow-map.svg" width="256" title="Virtual Network NAT">
</p>



*Ilustración:* Virtual Network NAT


>[!NOTE] 
>En este momento, Virtual Network NAT está disponible como versión preliminar pública. Actualmente solo está disponible en un conjunto limitado de [regiones](#region-availability). Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

## <a name="static-ip-addresses-for-outbound-only"></a>Direcciones IP estáticas de solo salida

Se puede definir la conectividad de salida para todas las subredes con NAT.  Distintas subredes dentro de la misma red virtual pueden tener diferentes NAT. Para configurar una subred, es preciso especificar qué [recurso de puerta de enlace de NAT](./nat-gateway-resource.md) se debe usar. Los flujos de salida UDP y TCP de todas las instancias de máquina virtual usarán NAT. 

NAT es compatible con los [recursos de dirección IP pública](./virtual-network-ip-addresses-overview-arm.md#standard) o los [recursos de prefijos IP públicos](./public-ip-address-prefix.md), o una combinación de ambos de una SKU estándar.  Puede usar un prefijo de IP pública directamente o distribuir las direcciones IP públicas del prefijo entre varios recursos de puerta de enlace de NAT. NAT limpiará todo el tráfico hacia el intervalo de direcciones IP del prefijo.  Ahora es fácil crear listas blancas de IP de las implementaciones.

Todo el tráfico de salida para la subred lo procesa NAT automáticamente sin que el cliente tenga que configurar nada.  No se necesitan rutas definidas por el usuario. NAT tiene prioridad sobre otros [escenarios de salida](../load-balancer/load-balancer-outbound-connections.md) y reemplaza el destino de Internet predeterminado de una subred.

## <a name="on-demand-snat-with-multiple-ip-addresses-for-scale"></a>SNAT a petición con varias direcciones IP para el escalado

NAT usa la "traducción de direcciones de red de puertos" (PNAT o PAT) y se recomienda para la mayoría de cargas de trabajo. Las cargas de trabajo divergentes o dinámicas se pueden acomodar fácilmente con la asignación de flujos de salida a petición. Se evita un exceso de planeamiento y asignación previas, y en último término, el sobreaprovisionamiento de recursos de salida. Los recursos de los puertos SNAT se comparten, por lo que están disponibles en todas las subredes que usen algún recurso de puerta de enlace de NAT concreto y se proporcionan cuando se necesitan.

Una dirección IP pública conectada a NAT proporciona hasta 64 000 flujos concurrentes para UDP y TCP. Puede empezar con una sola dirección IP y escalar verticalmente hasta 16 direcciones IP públicas.

NAT permite crear flujos desde la red virtual a Internet. El tráfico de retorno de Internet solo se permite en respuesta a un flujo activo.

A diferencia del SNAT de salida del equilibrador de carga, NAT no tiene restricciones sobre cuál de las IP privadas de una instancia de máquina virtual puede realizar conexiones de salida.  Las configuraciones de IP secundarias pueden crear una conexión a Internet de salida con NAT.

## <a name="coexistence-of-inbound-and-outbound"></a>Coexistencia de entrada y salida

NAT es compatible con los siguientes recursos de SKU estándar:

- [Equilibrador de carga](../load-balancer/load-balancer-overview.md)
- [Dirección IP pública](../virtual-network/virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- [Prefijo de IP pública](../virtual-network/public-ip-address-prefix.md)

Si se usan junto con NAT, estos recursos proporcionan conectividad de Internet entrante a sus subredes. NAT proporciona toda la conectividad de Internet que sale desde sus subredes.

Tanto NAT como las características compatibles de la SKU estándar conocen la dirección en que se inició el flujo. Los escenarios de entrada y salida pueden coexistir. Estos escenarios recibirán las traducciones de direcciones de red correctas porque estas características conocen la dirección del flujo. 

<!-- 
<img src="./media/nat-overview/flow-direction4.svg" width="500" align="center">
![Virtual Network NAT flow direction](./media/nat-overview/flow-direction4.svg)
-->
<p align="center">
  <img src="./media/nat-overview/flow-direction4.svg" width="512" title="Dirección del flujo de Virtual Network NAT">
</p>

*Ilustración: Dirección del flujo de Virtual Network NAT*

## <a name="fully-managed-highly-resilient"></a>Totalmente administrado y muy resistente

NAT se escala de forma horizontal completamente desde el principio. No se precisa ninguna operación de aumento ni de escalabilidad horizontal.  Azure administra automáticamente la operación de NAT.  NAT siempre tiene varios dominios de error y puede soportar varios errores sin que se interrumpa el servicio.

## <a name="tcp-reset-for-unrecognized-flows"></a>TCP Reset para flujos no reconocidos

El lado privado de NAT envía paquetes de TCP Reset para realizar intentos de comunicación sobre una conexión TCP que no existe. Un ejemplo son las conexiones que han alcanzado el tiempo de espera de inactividad. El siguiente paquete recibido devolverá un TCP Reset a la dirección IP privada para señalar y forzar el cierre de la conexión.

El lado público de NAT no genera paquetes de TCP Reset ni ningún otro tipo de tráfico.  Solo se emite el tráfico generado por la red virtual del cliente.

## <a name="configurable-idle-timeout"></a>Tiempo de espera de inactividad configurable

Se usa un tiempo de espera de inactividad predeterminado de 4 minutos que se puede aumentar hasta 120. Cualquier actividad de un flujo puede restablecer también el temporizador de actividad, lo que incluye mensajes TCP Keepalive.

## <a name="regional-or-zone-isolation-with-availability-zones"></a>Aislamiento regional o por zona con zonas de disponibilidad

De manera predeterminada, NAT tiene un ámbito regional. Al crear escenarios de [zonas de disponibilidad](../availability-zones/az-overview.md), se puede aislar en una zona específica (implementación zonal).

<!-- 
<img src="./media/nat-overview/az-directions.svg" width="500" align="center">
![Virtual Network NAT with availability zones](./media/nat-overview/az-directions.svg)
-->

<p align="center">
  <img src="./media/nat-overview/az-directions.svg" width="512" title="Virtual Network NAT con zonas de disponibilidad">
</p>

*Ilustración: Virtual Network NAT con zonas de disponibilidad*

## <a name="multi-dimensional-metrics-for-observability"></a>Métricas multidimensionales que favorecen la observación

La operación de la NAT se puede supervisar a través de métricas multidimensionales que se exponen en Azure Monitor. Estas métricas se pueden usar no solo para observar el uso, sino también para solucionar problemas.  Los recursos de la puerta de enlace de NAT exponen las siguientes métricas:
- Bytes
- Paquetes
- Paquetes descartados
- Total de conexiones SNAT
- Transiciones de estado de conexiones SNAT por intervalo.

<!-- "ADD when PM is done" Learn more about [NAT gateway metrics](./nat-gateway-metrics.md) -->

## <a name="sla"></a>Contrato de nivel de servicio

En disponibilidad general, la disponibilidad mínima de la ruta de datos de NAT es del 99,9 %.

## <a name = "region-availability"></a>Disponibilidad regional

NAT está disponible actualmente en estas regiones:

- Oeste de Europa
- Japón Oriental
- Este de EE. UU. 2
- Oeste de EE. UU.
- Oeste de EE. UU. 2
- Centro-oeste de EE. UU.

## <a name = "enable-preview">Participación en la versión preliminar pública</a>

Las suscripciones deben estar registradas para permitir la participación en la versión preliminar pública.  La participación requiere un proceso de dos pasos y a continuación se proporcionan instrucciones tanto para la CLI de Azure como para Azure PowerShell.  La activación puede tardar varios minutos en completarse.

### <a name="azure-cli"></a>Azure CLI

1. Registre la suscripción a la versión preliminar pública

    ```azurecli-interactive
      az feature register --namespace Microsoft.Network --name AllowNatGateway
    ```

2. Active el registro

    ```azurecli-interactive
      az provider register --namespace Microsoft.Network
    ```

### <a name="azure-powershell"></a>Azure PowerShell

1. Registre la suscripción a la versión preliminar pública

    ```azurepowershell-interactive
      Register-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowNatGateway
    ```

2. Active el registro

    ```azurepowershell-interactive
      Register-AzResourceProvider -ProviderNamespace Microsoft.Network
    ```

## <a name="pricing"></a>Precios

La puerta de enlace de NAT se factura con dos medidores independientes:

| Medidor | Tarifa |
| --- | --- |
| Horas de recursos | (0,045 dólares/hora) |
| Datos procesados | 0,045 dólares/GB |

Las horas de recursos cuentan el tiempo de existencia de un recurso de puerta de enlace de NAT.
Los datos procesados cuentan todo el tráfico procesado por un recurso de puerta de enlace de NAT.

Durante la versión preliminar pública, se ofrece un descuento del 50 % en los precios.

## <a name="support"></a>Soporte técnico

El soporte técnico de NAT se proporciona a través de los canales normales de soporte.

## <a name="feedback"></a>Comentarios

Queremos saber cómo podemos mejorar el servicio. Comparta sus [comentarios sobre la versión preliminar pública](https://aka.ms/natfeedback).  También puede proponer lo que debemos crear después (y votar por ello) en [UserVoice para NAT](https://aka.ms/natuservoice).

## <a name="limitations"></a>Limitaciones

* NAT es compatible con la dirección IP pública de la SKU estándar, el prefijo de IP pública y los recursos del equilibrador de carga.   Ni los recursos básicos (por ejemplo, el equilibrador de carga básico) ni los productos derivados de ellos son compatibles con NAT.  Los recursos básicos se deben colocar en una subred que no esté configurada con NAT.
* Se admite la familia de direcciones IPv4.  NAT no interactúa con la familia de direcciones IPv6.  NAT no se puede implementar en una subred con prefijo IPv6.
* El registro de flujos de grupos de seguridad de red no se admite cuando se usa NAT.
* NAT no puede abarcar varias redes virtuales.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [recursos de puerta de enlace de NAT](./nat-gateway-resource.md).
* [Indíquenos qué crear a continuación para Virtual Network NAT en UserVoice](https://aka.ms/natuservoice).
* [Proporcione comentarios sobre la versión preliminar pública](https://aka.ms/natfeedback).
