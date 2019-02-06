---
title: Dirección del tráfico de Azure a Azure SQL Database y SQL Data Warehouse | Microsoft Docs
description: En este documento, se explica la arquitectura de conectividad de Azure SQL Database y SQL Data Warehouse desde dentro o desde fuera de Azure.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: de31ab4e617b872239c1b83324e5b8d52b0b4094
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469127"
---
# <a name="azure-sql-connectivity-architecture"></a>Arquitectura de conectividad de Azure SQL

En este artículo se explica la arquitectura de conectividad de Azure SQL Database y SQL Data Warehouse, y cómo funcionan los distintos componentes para dirigir el tráfico a una instancia de Azure SQL. La funcionalidad de estos componentes de conectividad consiste en dirigir el tráfico de red a Azure SQL Database o SQL Data Warehouse con clientes que se conectan desde dentro y desde fuera de Azure. En este artículo también se proporcionan ejemplos de script para cambiar cómo se establece la conectividad y, además, se incluyen consideraciones sobre la modificación de la configuración predeterminada de la conectividad.

> [!IMPORTANT]
> **[Cambio próximo] Para las conexiones de punto de conexión de servicio a los servidores Azure SQL, un comportamiento de conectividad `Default` cambia a `Redirect`.**
>
> El cambio entrará en vigor en todas las regiones el 2 de enero de 2019 como muy tarde.
>
> Para evitar que la conectividad a través de un punto de conexión de servicio se interrumpa en los entornos existentes a causa de este cambio, utilizamos telemetría para lo siguiente:
> - Para los servidores cuyo acceso a través de puntos de conexión de servicio hemos detectado antes del cambio, cambiamos el tipo de conexión a `Proxy`.
> - Para todos los demás servidores, el tipo de conexión se cambiará por `Redirect`.
>
> Los usuarios del punto de conexión de servicio todavía pueden verse afectados en los escenarios siguientes:
> - La aplicación se conecta a un servidor existente con poca frecuencia, por lo que la telemetría no capturó la información acerca de dicha aplicación.
> - La lógica de implementación automatizada crea un servidor de SQL Database suponiendo que es el comportamiento predeterminado de las conexiones de punto de conexión de servicio es `Proxy`.
>
> Si no se pudieron establecer conexiones de punto de conexión de servicio al servidor de Azure SQL y sospecha que este cambio le afecta, compruebe que el tipo de conexión esté establecido explícitamente en `Redirect`. Si es el caso, debe abrir las reglas del firewall de la máquina virtual y los grupos de seguridad de red (NSG) para todas las direcciones IP de Azure en la región que pertenecen a la [etiqueta de servicio](../virtual-network/security-overview.md#service-tags) Sql para los puertos 11000-12000. Si esta no es una opción en su caso, cambie el servidor explícitamente a `Proxy`.
> [!NOTE]
> Este tema se aplica al servidor de Azure SQL y tanto a las bases de datos de SQL Database como a SQL Data Warehouse que se crean en el servidor de Azure SQL. Para simplificar, SQL Database se utiliza cuando se hace referencia tanto a SQL Database como a SQL Data Warehouse.

## <a name="connectivity-architecture"></a>Arquitectura de conectividad

En el siguiente diagrama se proporciona una descripción general de la arquitectura de conectividad de Azure SQL Database.

![Descripción general de la arquitectura](./media/sql-database-connectivity-architecture/connectivity-overview.png)

En los pasos siguientes se describe cómo se establece una conexión a una instancia de Azure SQL Database:

- Los clientes se conectan a una puerta de enlace, que tiene una dirección IP pública y escucha en el puerto 1433.
- La puerta de enlace, dependiendo de la directiva de conexión efectiva, redirecciona o envía por proxy el tráfico al clúster de base de datos correcto.
- En la base de datos, el tráfico del clúster se reenvía a la instancia de Azure SQL Database adecuada.

## <a name="connection-policy"></a>Directiva de conexión

Azure SQL Database admite las siguientes tres opciones para la configuración de directiva de conexión de un servidor de SQL Database:

- **Redirigir (recomendado):** los clientes establecen conexiones directamente con el nodo que hospeda la base de datos. Para habilitar la conectividad, los clientes tienen que permitir las reglas de firewall de salida a todas las direcciones IP de Azure en la región utilizando los grupos de seguridad de red (NSG) con [etiquetas de servicio](../virtual-network/security-overview.md#service-tags) para los puertos 11000-12000, no solo las direcciones IP de puerta de enlace de Azure SQL Database en el puerto 1433. Como los paquetes van directamente a la base de datos, la latencia y la capacidad de proceso mejoran.
- **Proxy:** en este modo, el proxy procesa todas las conexiones a través de las puertas de enlace de Azure SQL Database. Para habilitar la conectividad, el cliente tiene que tener reglas de firewall de salida que permiten solo las direcciones IP de puerta de enlace de Azure SQL Database (normalmente dos direcciones IP por región). La elección de este modo puede producir una latencia mayor y un rendimiento inferior, según la naturaleza de la carga de trabajo. Es muy recomendable utilizar la directiva de conexión `Redirect` frente a `Proxy` para obtener la menor latencia y el mayor rendimiento.
- **Predeterminado:** esta es la directiva de conexión en vigor en todos los servidores después de su creación, a menos que se modifique explícitamente cambiándola a `Proxy` o `Redirect`. La directiva efectiva depende de si las conexiones se originan desde dentro de Azure (`Redirect`) o fuera de Azure (`Proxy`).

## <a name="connectivity-from-within-azure"></a>Conectividad desde dentro de Azure

Si va a conectarse desde dentro de Azure, las conexiones tienen la directiva de conexión predeterminada `Redirect`. Una directiva `Redirect` significa que después de establecer la sesión TCP en Azure SQL Database, la sesión del cliente se redirige al clúster de base de datos correcto con un cambio en la IP virtual de destino de la puerta de enlace de Azure SQL Database a la del clúster. Por lo tanto, todos los paquetes posteriores fluyen directamente al clúster, de tal forma que omiten la puerta de enlace de Azure SQL Database. En el siguiente diagrama, se ilustra este flujo de tráfico.

![Descripción general de la arquitectura](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Conectividad desde fuera de Azure

Si va a conectarse desde fuera de Azure, las conexiones tienen la directiva de conexión predeterminada `Proxy`. La directiva `Proxy` establece que la sesión TCP se realice a través de la puerta de enlace de Azure SQL Database y que todos los paquetes posteriores fluyan a través de la puerta de enlace. En el siguiente diagrama, se ilustra este flujo de tráfico.

![Descripción general de la arquitectura](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Direcciones IP de la puerta de enlace de Azure SQL Database

Para conectarse a una base de datos SQL de Azure desde recursos locales, debe permitir el tráfico de red saliente a la puerta de enlace de Azure SQL Database en su región de Azure. Las conexiones solo se establecen a través de la puerta de enlace al conectarse en modo `Proxy`, que es el valor predeterminado para las conexiones desde recursos locales.

En la tabla siguiente se enumeran las direcciones IP principales y secundarias de la puerta de enlace de Azure SQL Database para todas las regiones de datos. En algunas regiones, hay dos direcciones IP. En estas regiones, la dirección IP principal es la dirección IP actual de la puerta de enlace y la dirección IP secundaria es una dirección IP de conmutación por error. La dirección de conmutación por error es la dirección a la que se puede mover el servidor para mantener la alta disponibilidad del servicio. En estas regiones, se recomienda que permita el tráfico saliente a ambas direcciones IP. La dirección IP secundaria es propiedad de Microsoft y no escucha en ningún servicio hasta que Azure SQL Database la activa para aceptar conexiones.

| Nombre de región | Dirección IP principal | Dirección IP secundaria |
| --- | --- |--- |
| Este de Australia | 13.75.149.87 | 40.79.161.1 |
| Sudeste de Australia | 191.239.192.109 | 13.73.109.251 |
| Sur de Brasil | 104.41.11.5 | |
| Centro de Canadá | 40.85.224.249 | |
| Este de Canadá | 40.86.226.166 | |
| Centro de EE. UU. | 23.99.160.139 | 13.67.215.62 |
| Este de China 1 | 139.219.130.35 | |
| Este de China 2 | 40.73.82.1 | |
| Norte de China 1 | 139.219.15.17 | |
| Norte de China 2 | 40.73.50.0 | |
| Asia oriental | 191.234.2.139 | 52.175.33.150 |
| Este de EE. UU. 1 | 191.238.6.43 | 40.121.158.30 |
| Este de EE. UU. 2 | 191.239.224.107 | 40.79.84.180 * |
| Centro de Francia | 40.79.137.0 | 40.79.129.1 |
| Centro de Alemania | 51.4.144.100 | |
| Nordeste de Alemania | 51.5.144.179 | |
| India central | 104.211.96.159 | |
| Sur de India | 104.211.224.146 | |
| India occidental | 104.211.160.80 | |
| Este de Japón | 191.237.240.43 | 13.78.61.196 |
| Oeste de Japón | 191.238.68.11 | 104.214.148.156 |
| Corea Central | 52.231.32.42 | |
| Corea del Sur | 52.231.200.86 |  |
| Centro-Norte de EE. UU | 23.98.55.75 | 23.96.178.199 |
| Europa del Norte | 191.235.193.75 | 40.113.93.91 |
| Centro-Sur de EE. UU | 23.98.162.75 | 13.66.62.124 |
| Sudeste de Asia | 23.100.117.95 | 104.43.15.0 |
| Norte del Reino Unido | 13.87.97.210 | |
| Sur de Reino Unido 1 | 51.140.184.11 | |
| Sur del Reino Unido 2 | 13.87.34.7 | |
| Oeste de Reino Unido | 51.141.8.11 | |
| Centro occidental de EE.UU. | 13.78.145.25 | |
| Europa occidental | 191.237.232.75 | 40.68.37.158 |
| Oeste de EE. UU. 1 | 23.99.34.75 | 104.42.238.205 |
| Oeste de EE. UU. 2 | 13.66.226.202 | |
||||

\* **NOTA:** La zona del *Este de EE. UU. 2* también tiene una dirección IP terciaria de `52.167.104.0`.

## <a name="change-azure-sql-database-connection-policy"></a>Cambio de la directiva de conexión de Azure SQL Database

Para cambiar la directiva de conexión de Azure SQL Database de un servidor de Azure SQL Database, use el comando [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).

- Si la directiva de conexión se establece en `Proxy`, todos los paquetes de red fluyen a través de la puerta de enlace de Azure SQL Database. Para esta configuración, debe permitir el tráfico saliente solo a la dirección IP de la puerta de enlace de Azure SQL Database. El uso de la configuración de `Proxy` ofrece más latencia que la de `Redirect`.
- Si la configuración de la directiva de conexión es `Redirect`, todos los paquetes de red fluyen directamente al clúster de base de datos. Para esta configuración, debe permitir el tráfico saliente a varias direcciones IP.

## <a name="script-to-change-connection-settings-via-powershell"></a>Script para cambiar la configuración de la conexión a través de PowerShell

> [!IMPORTANT]
> Este script requiere el [módulo Azure PowerShell](/powershell/azure/install-az-ps).

El siguiente script de PowerShell muestra cómo cambiar la directiva de conexión.

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzureRmSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzureRmResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzureRmResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>Script para cambiar la configuración de la conexión a través de la CLI de Azure

> [!IMPORTANT]
> Este script requiere la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

El siguiente script de CLI muestra cómo cambiar la directiva de conexión.

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $id

# Update connection policy
az resource update --ids $id --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Pasos siguientes

- Para información sobre cómo cambiar la directiva de conexión de Azure SQL Database para un servidor de Azure SQL Database, consulte [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- Para obtener información sobre el comportamiento de conexión de Azure SQL Database para clientes que usan ADO.NET 4.5 o una versión posterior, vea [Puertos más allá de 1433 para ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Para obtener información general sobre el desarrollo de aplicaciones, vea [Introducción al desarrollo de aplicaciones en SQL Database](sql-database-develop-overview.md).
