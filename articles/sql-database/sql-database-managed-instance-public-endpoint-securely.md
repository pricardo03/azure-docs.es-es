---
title: Protección de puntos de conexión público de instancia administrada, Azure SQL Database instancia administrada | Microsoft Docs
description: Usar puntos de conexión públicos de forma segura en Azure con una instancia administrada
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 05/08/2019
ms.openlocfilehash: f06677b66c8f6586fec8cc5dfe97b1515b741e9c
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65470304"
---
# <a name="use-an-azure-sql-database-managed-instance-securely-with-public-endpoints"></a>Usar una instancia administrada de Azure SQL Database de forma segura con puntos de conexión públicos

Las instancias administradas pueden proporcionar conectividad de usuario a través de Azure SQL Database [puntos de conexión públicos](../virtual-network/virtual-network-service-endpoints-overview.md). En este artículo se explica cómo realizar esta configuración más segura.

## <a name="scenarios"></a>Escenarios

Una instancia administrada de SQL Database proporciona un punto de conexión privada para permitir la conectividad desde dentro de su red virtual. La opción predeterminada es proporcionar aislamiento máximo. Sin embargo, hay escenarios donde es necesario proporcionar una conexión de punto de conexión público:

- La instancia administrada debe integrarse con multi-tenant solo plataforma como servicio (PaaS) las ofertas.
- Necesita un mayor rendimiento de intercambio de datos que es posible cuando se usa una VPN.
- Las directivas de empresa prohíben PaaS dentro de las redes corporativas.

## <a name="deploy-a-managed-instance-for-public-endpoint-access"></a>Implementar una instancia administrada para el acceso de punto de conexión público

Aunque no es obligatorio, es el modelo de implementación comunes para una instancia administrada con acceso de punto de conexión público crear la instancia en una red virtual aislada dedicada. En esta configuración, la red virtual se usa solo para el aislamiento de clúster virtual. No importa si el espacio de direcciones IP de la instancia administrada se superpone con el espacio de direcciones IP de la red corporativa.

## <a name="secure-data-in-motion"></a>Proteger los datos en movimiento

Siempre se cifra el tráfico de datos de instancia administrada si el controlador cliente admite el cifrado. Datos enviados entre la instancia administrada y otras máquinas virtuales o servicios de Azure nunca abandonan la red troncal de Azure. Si hay una conexión entre la instancia administrada y una red local, se recomienda que usar Azure ExpressRoute con emparejamiento de Microsoft. ExpressRoute le ayuda a evitar mover datos a través de internet pública. Para conectividad privada de la instancia administrada, se puede usar solo emparejamiento privado.

## <a name="lock-down-inbound-and-outbound-connectivity"></a>Bloquear la conectividad entrante y saliente

El siguiente diagrama muestra las configuraciones de seguridad recomendadas:

![Configuraciones de seguridad para bloquear la conectividad entrante y saliente](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

Una instancia administrada tiene un [dirección de punto de conexión público dedicada](sql-database-managed-instance-find-management-endpoint-ip-address.md). En el firewall de salida del lado cliente y en las reglas del grupo de seguridad de red, establezca esta dirección IP de punto de conexión público para limitar la conectividad de salida.

Para asegurarse de que el tráfico a la instancia administrada es procedentes de orígenes de confianza, se recomienda conectarse desde orígenes con direcciones IP conocidas. Usar un grupo de seguridad de red para limitar el acceso para el punto de conexión público de instancia administrada en el puerto 3342.

Cuando los clientes deben iniciar una conexión desde una red local, asegúrese de que la dirección de origen se convierte en un conjunto conocido de direcciones IP. Si no puede hacerlo (por ejemplo, un recurso móvil que se va a un escenario típico), se recomienda usar [conexiones VPN de punto a sitio y un punto de conexión privada](sql-database-managed-instance-configure-p2s.md).

Si se inician las conexiones de Azure, se recomienda que el tráfico que proviene de una cuenta conocida asignada [dirección IP virtual](../virtual-network/virtual-networks-reserved-public-ip.md) (por ejemplo, una máquina virtual). Para facilitar la administración de direcciones IP virtuales (VIP) sea más fácil, es posible que desee usar [prefijos de direcciones IP públicos](../virtual-network/public-ip-address-prefix.md).

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a configurar el punto de conexión público para administrar instancias: [Configurar el punto de conexión público](sql-database-managed-instance-public-endpoint-configure.md)
