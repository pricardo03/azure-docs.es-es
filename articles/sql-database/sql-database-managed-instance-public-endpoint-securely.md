---
title: Protección de puntos de conexión público de instancia administrada, Azure SQL Database instancia administrada | Microsoft Docs
description: Usar puntos de conexión públicos de forma segura en Azure con instancia administrada
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto, carlrab
manager: digimobile
origin.date: 04/16/2019
ms.date: 04/29/2019
ms.openlocfilehash: 9d5a3d18e8a7d3c5a6cb08e16e74dd4fbda9ca31
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61315059"
---
# <a name="using-azure-sql-database-managed-instance-securely-with-public-endpoint"></a>Con Azure SQL Database administra la instancia de forma segura con el punto de conexión público

Se pudo habilitar la instancia administrada para proporcionar conectividad de usuario a través de Azure SQL Database [punto de conexión público](../virtual-network/virtual-network-service-endpoints-overview.md). Este artículo proporcionan instrucciones para hacer esta configuración más segura.

## <a name="scenarios"></a>Escenarios

Instancia administrada proporciona un punto de conexión privado para habilitar la conectividad desde dentro de su red virtual. La opción predeterminada es proporcionar aislamiento máximo. Sin embargo, hay escenarios donde se necesita una conexión de punto de conexión público:

- Integración con ofertas de varios inquilinos sola PaaS.
- Rendimiento máximo mayor de intercambio de datos mediante VPN.
- Las directivas de empresa prohíben PaaS dentro de las redes corporativas.

## <a name="deploying-managed-instance-for-public-endpoint-access"></a>Implementar instancia administrada para el acceso de punto de conexión público

Aunque no es obligatorio, es el modelo de implementación comunes para una instancia administrada con acceso de punto de conexión público crear la instancia en una red virtual aislada dedicada. En esta configuración, la red virtual se usa solo para el aislamiento de clúster virtual. No es relevante si el espacio de direcciones IP de instancia administrada se superpone con un espacio de direcciones IP de red corporativa.

## <a name="securing-data-in-motion"></a>Proteger los datos en movimiento

Siempre se cifra el tráfico de datos de instancia administrada si el controlador cliente admite el cifrado. Los datos entre la instancia administrada y otras máquinas virtuales de Azure o servicios de Azure nunca abandona la red troncal de Azure. Si hay una instancia administrada a una conexión de red local, se recomienda usar Expressroute con emparejamiento de Microsoft. Expressroute le ayudará a evitar mover datos a través de Internet pública (conectividad privada de la instancia administrada, solo emparejamiento privado puede usarse).

## <a name="locking-down-inbound-and-outbound-connectivity"></a>Bloquear la conectividad entrante y saliente

El siguiente diagrama muestra las configuraciones de seguridad recomendada.

![managed-instance-vnet.png](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

Instancia administrada tiene un [dirección de punto de conexión público dedicada](sql-database-managed-instance-find-management-endpoint-ip-address.md). Esta dirección IP debe establecerse en el firewall de salida del lado cliente y las reglas del grupo de seguridad de red para limitar la conectividad de salida.

Para asegurarse de que el tráfico a la instancia administrada es procedentes de orígenes de confianza, se recomienda conectarse desde orígenes con direcciones IP conocidas. Limitar el acceso para el punto de conexión público de instancia administrada en el puerto 3342 mediante un grupo de seguridad de red.

Cuando los clientes deben iniciar una conexión desde una red local, asegúrese de que la dirección de origen se convierte en un conjunto conocido de direcciones IP. Si no se puede lograr que (por ejemplo, los trabajadores móviles que se va a un escenario típico), se recomienda para usar [Point-to-site VPN conexiones y un punto de conexión privada](sql-database-managed-instance-configure-p2s.md).

Si se inician las conexiones de Azure, se recomienda que el tráfico procede conocido asignado [VIP](../virtual-network/virtual-networks-reserved-public-ip.md) (por ejemplo, máquinas virtuales). Para facilitar la administración de direcciones IP virtuales, los clientes podría usar [prefijo de dirección IP pública](../virtual-network/public-ip-address-prefix.md).