---
title: ¿Qué es Azure Load Balancer?
titleSuffix: Azure Load Balancer
description: Información general sobre las características, la arquitectura y la implementación del Equilibrador de carga de Azure Aprenda cómo funciona Load Balancer y cómo usarlo en la nube.
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
ms.date: 1/14/2019
ms.author: allensu
ms.openlocfilehash: dc986d40d50b93720c87ba36d265ed3044b0abc9
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76045377"
---
# <a name="what-is-azure-load-balancer"></a>¿Qué es Azure Load Balancer?

El *equilibrio de carga* hace referencia a la distribución uniforme de la carga (el tráfico de red entrante) en un grupo de recursos o servidores de back-end. Azure ofrece [varias opciones de equilibrio de carga](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) para que pueda elegir la que mejor se ajuste a sus necesidades. En este documento se analiza Azure Load Balancer.

Azure Load Balancer opera en la capa cuatro del modelo de interconexión de sistema abierto (OSI). Es el único punto de contacto de los clientes. Load Balancer distribuye nuevos flujos entrantes que llegan al front-end del equilibrador de carga a las instancias del grupo de servidores back-end. Estos flujos están de acuerdo con las reglas de equilibrio de carga y los sondeos de estado configurados. Las instancias del grupo de back-end pueden ser instancias de Azure Virtual Machines o de un conjunto de escalado de máquinas virtuales.


Un **[equilibrador de carga público](./concepts-limitations.md#publicloadbalancer)** puede proporcionar conexiones de salida para máquinas virtuales dentro de la red virtual. Estas conexiones se realizan mediante la traducción de sus direcciones IP privadas a direcciones IP públicas. Las instancias públicas de Load Balancer se usan para equilibrar la carga del tráfico de Internet en las máquinas virtuales.

Un **[equilibrador de carga interno (o privado)](./concepts-limitations.md#internalloadbalancer)** se usa cuando se necesitan direcciones IP privadas solo en el front-end. Los equilibradores de carga internos se usan para equilibrar la carga del tráfico dentro de una red virtual. También se puede acceder a un servidor front-end del equilibrador de carga desde una red local en un escenario híbrido.

<div align="center">
  <img src='./media/load-balancer-overview/IC744147.png'>
</div>

*Ilustración: Equilibrar las aplicaciones de niveles múltiples mediante Load Balancer público e interno*

Para más información sobre los componentes individuales del equilibrador de carga, consulte [Componentes y limitaciones de Azure Load Balancer](./concepts-limitations.md).

>[!NOTE]
> Microsoft recomienda [Standard Load Balancer](./load-balancer-standard-overview.md).
Las máquinas virtuales independientes, los conjuntos de disponibilidad y los conjuntos de escalado de máquinas virtuales solo se pueden conectar a una SKU, nunca a ambas. Load Balancer y la SKU de la dirección IP pública deben coincidir cuando se usan con direcciones IP públicas. Load Balancer y las SKU de IP públicas no son mutables.

## <a name="why-use-azure-load-balancer"></a>Uso de Azure Load Balancer
Con Azure Load Balancer puede escalar las aplicaciones y crear servicios con alta disponibilidad. Load Balancer admite escenarios de entrada y salida. Una instancia de Load Balancer proporciona baja latencia y alto rendimiento, y puede escalar hasta millones de flujos para todas las aplicaciones TCP y UDP.

Entre los escenarios clave que puede realizar con Azure Load Balancer se incluyen:

- Equilibrio de carga del tráfico **[interno](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-manage-portal)** y **[externo](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-internal-portal)** a las máquinas virtuales de Azure.

- Aumento de la disponibilidad mediante la distribución de recursos **[en zonas](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zonal-portal)** y **[a través de ellas](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zone-redundant-portal)** .

- Configuración de la **[conectividad de salida ](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections)** para máquinas virtuales de Azure.

- Uso de **[sondeos de estado](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)** para supervisar los recursos con equilibrio de carga.

- Empleo del **[desvío de puertos](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)** para acceder a las máquinas virtuales de una red virtual mediante la dirección IP pública y el puerto.

- Habilitación de la compatibilidad con el **[equilibrio de carga](https://docs.microsoft.com/azure/virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell)** de **[IPv6](https://docs.microsoft.com/azure/virtual-network/ipv6-overview)** .

- Aprovechamiento de las ventajas de **[métricas y diagnósticos](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics)** para Azure Load Balancer con **[Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview)** .

- Servicios de equilibrio de carga en **[varios puertos, varias direcciones IP, o en ambos](https://docs.microsoft.com/azure/load-balancer/load-balancer-multivip-overview)** .

- Desplazamiento de los recursos **[internos](https://docs.microsoft.com/azure/load-balancer/move-across-regions-internal-load-balancer-portal)** y **[externos](https://docs.microsoft.com/azure/load-balancer/move-across-regions-external-load-balancer-portal)** del equilibrador de carga por las regiones de Azure.

- Equilibrio de carga del flujo de TCP y UDP en todos los puertos simultáneamente mediante los **[puertos de alta disponibilidad](https://docs.microsoft.com/azure/load-balancer/load-balancer-ha-ports-overview)** .

## <a name="pricing"></a>Precios

Se cobra el uso de Standard Load Balancer.

* Número de reglas de equilibrio de carga y de salida configuradas. Las reglas NAT de entrada no cuentan en el número total de reglas.
* Cantidad de datos procesados de entrada y salida independientes de las reglas.

Para más información sobre los precios de Standard Load Balancer, consulte [Precios de Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/).

Load Balancer Básico se ofrece sin cargo.

## <a name="sla"></a>Contrato de nivel de servicio

Para más información sobre el contrato de nivel de servicio de Standard Load Balancer, consulte [Contrato de nivel de servicio para Load Balancer](https://aka.ms/lbsla).

## <a name="next-steps"></a>Pasos siguientes

Consulte el artículo sobre cómo [crear una instancia de Standard Load Balancer pública](quickstart-load-balancer-standard-public-portal.md) para empezar a usar un equilibrador de carga.

Para más información acerca de las limitaciones y los componentes de Azure Load Balancer, consulte [Conceptos y limitaciones de Azure Load Balancer](./concepts-limitations.md).
