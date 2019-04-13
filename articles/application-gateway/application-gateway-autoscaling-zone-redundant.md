---
title: Escalabilidad automática y puerta de enlace de aplicaciones con redundancia de zona en Azure (versión preliminar pública)
description: En este artículo se presenta la SKU de la Aplicación de Azure v2, que incluye características de escalabilidad automática y redundancia de zona.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 3/6/2019
ms.author: victorh
ms.openlocfilehash: 95b14a0028134e522206f3595bc3b9ebf9aaf396
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548735"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-public-preview"></a>Escalabilidad automática y puerta de enlace de aplicaciones con redundancia de zona (versión preliminar pública)

Application Gateway y Firewall de aplicaciones web (WAF) ahora están disponibles en versión preliminar pública en una nueva SKU v2 que ofrece mejoras de rendimiento y agrega compatibilidad para nuevas características esenciales, como escalabilidad automática, redundancia de zona y compatibilidad con VIP estáticas. La nueva SKU v2 sigue admitiendo las características existentes en la SKU de disponibilidad general, salvo algunas excepciones que se indican en la sección de limitaciones conocidas. La nueva SKU v2 incluye las siguientes mejoras:

- **Escalabilidad automática**: Las implementaciones de Application Gateway o WAF en la SKU de escalado automática pueden escalarse o reducirse verticalmente en función de los cambiantes patrones de la carga de tráfico. La escalabilidad automática también elimina el requisito de tener elegir un tamaño de implementación o un número de instancias durante el aprovisionamiento. Esta SKU ofrece elasticidad es true. En la nueva SKU, Application Gateway puede funcionar con una capacidad fija (escalabilidad automática deshabilitada) y en modo de escalabilidad automática habilitada. El modo de capacidad fija es útil para escenarios con cargas de trabajo coherentes y predecibles. El modo de escalabilidad automática es útil en aplicaciones que tendrán una gran variación en el tráfico de las aplicaciones.

- **Redundancia de zona**: una implementación de Application Gateway o WAF puede abarcar varias zonas de disponibilidad, lo que elimina la necesidad de aprovisionar y anclar instancias de Application Gateway independientes en cada zona con una instancia de Traffic Manager. Puede elegir una o varias zonas donde se implementarán las instancias de Application Gateway, lo que garantiza la resistencia a errores de zona. El grupo de back-end de las aplicaciones se puede distribuir de manera similar entre las zonas de disponibilidad.
- **Mejoras de rendimiento**: la SKU de escalabilidad automática ofrece un rendimiento cinco veces superior en la descarga de SSL en comparación con la SKU de disponibilidad general.
- **Menores tiempos de implementación y actualización**: la SKU de escalabilidad automática proporciona menores tiempos de implementación y actualización en comparación con la SKU de disponibilidad general.
- **VIP estática**: la dirección IP virtual de Application Gateway admite ahora exclusivamente el tipo VIP estática. Esto garantiza que la VIP asociada con application gateway no cambia incluso después del reinicio.

> [!IMPORTANT]
> La SKU de escalabilidad automática y de puerta de enlace de aplicaciones con redundancia de zona actualmente está en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

> [!NOTE]
> El escalado automático y la puerta de enlace de aplicaciones con redundancia de zona SKU ahora admite [sonda de estado predeterminada](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#default-health-probe) para supervisar el estado de todos los recursos de su grupo de back-end y resaltar los miembros de back-end que se consideran automáticamente Insalubre. El quedará de sondeo de estado predeterminada se configura automáticamente para todos esos back-ends que no ha configurado ninguna configuración de sondeo personalizado. Para obtener más información, consulte [sondeos de estado de application gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## <a name="feature-comparison-between-v1-sku-and-v2-sku"></a>Comparación de características entre SKU v1 y v2 SKU

En la tabla siguiente se compara las características disponibles con cada SKU.

|                                                   | v1 SKU   | v2 SKU   |
| ------------------------------------------------- | -------- | -------- |
| Escalado automático                                       |          | &#x2713; |
| Redundancia de zona                                   |          | &#x2713; |
| &nbsp;VIP estático&nbsp;&nbsp;                      |          | &#x2713; |
| Enrutamiento basado en dirección URL                                 | &#x2713; | &#x2713; |
| Hospedaje de varios sitios                             | &#x2713; | &#x2713; |
| Redireccionamiento del tráfico                               | &#x2713; | &#x2713; |
| Firewall de aplicaciones web (WAF)                    | &#x2713; | &#x2713; |
| Terminación de la Capa de sockets seguros (SSL)            | &#x2713; | &#x2713; |
| Cifrado SSL de extremo a otro                         | &#x2713; | &#x2713; |
| Afinidad de sesión                                  | &#x2713; | &#x2713; |
| Páginas de error personalizadas                                | &#x2713; | &#x2713; |
| Vuelva a escribir los encabezados HTTP (S)                           |          | &#x2713; |
| Compatibilidad con WebSocket                                 | &#x2713; | &#x2713; |
| Compatibilidad con HTTP/2                                    | &#x2713; | &#x2713; |
| Purga de la conexión                               | &#x2713; | &#x2713; |
| Azure controlador Ingress de Kubernetes Service (AKS) |          | &#x2713; |

## <a name="supported-regions"></a>Regiones admitidas

La SKU de autoescala está disponible en las siguientes regiones: Centro-norte de EE. UU., Centro-sur de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2, Este de EE. UU., Este de EE. UU. 2, Centro de EE. UU., Europa del Norte, Europa Occidental, Sudeste Asiático, Centro de Francia, Oeste de Reino Unido, Este de Japón, Oeste de Japón.

## <a name="pricing"></a>Precios

Durante la versión preliminar, no hay ningún cargo. Se le facturará por recursos distintos de la puerta de enlace de aplicaciones, como Key Vault, las máquinas virtuales y así sucesivamente.

## <a name="known-issues-and-limitations"></a>Problemas conocidos y limitaciones

|Problema|Detalles|
|--|--|
|Certificado de autenticación|No compatible.<br>Para más información, consulte [Introducción a SSL de un extremo a otro con Application Gateway](ssl-overview.md#end-to-end-ssl-with-the-v2-sku).|
|Combinación de Application Gateway Standard y Standard_v2 en la misma subred|No compatible|
|Ruta definida por el usuario (UDR) en la subred de Application Gateway|No compatible|
|NSG para el intervalo de puertos de entrada| - 65200 a 65535 para la SKU Standard_v2<br>- 65503 a 65534 para la SKU Standard.<br>Para más información, consulte las [preguntas más frecuentes](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet).|
|Registros de rendimiento en Azure Diagnostics|No compatible.<br>Se deben usar las métricas de Azure.|
|Facturación|No hay facturación actualmente.|
|Modo FIPS|Actualmente no se admiten.|
|Solo modo ILB|Actualmente no se admite. Se admiten los modos público e ILB juntos.|
|Integración de Netwatcher|No se admite en la versión preliminar pública.|

## <a name="next-steps"></a>Pasos siguientes
- [Creación de una puerta de enlace de aplicaciones con redundancia de zona, escalabilidad automática y una dirección IP virtual reservada con Azure PowerShell](tutorial-autoscale-ps.md)
- Más información acerca de [Application Gateway](overview.md).
- Más información acerca de [Azure Firewall](../firewall/overview.md).
