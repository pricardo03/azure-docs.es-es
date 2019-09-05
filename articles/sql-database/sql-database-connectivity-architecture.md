---
title: Arquitectura de conectividad de Azure SQL Database y SQL Data Warehouse | Microsoft Docs
description: En este documento se explica la arquitectura de conectividad de Azure SQL para las conexiones de bases de datos desde dentro o desde fuera de Azure.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 07/02/2019
ms.openlocfilehash: daa9937294cb3ffa594d83fbd56777bc8af8900a
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69981472"
---
# <a name="azure-sql-connectivity-architecture"></a>Arquitectura de conectividad de Azure SQL

En este artículo se explica la arquitectura de conectividad de Azure SQL Database y SQL Data Warehouse, y cómo funcionan los distintos componentes para dirigir el tráfico a una instancia de Azure SQL. La funcionalidad de estos componentes de conectividad consiste en dirigir el tráfico de red a Azure SQL Database o SQL Data Warehouse con clientes que se conectan desde dentro y desde fuera de Azure. En este artículo también se proporcionan ejemplos de script para cambiar cómo se establece la conectividad y, además, se incluyen consideraciones sobre la modificación de la configuración predeterminada de la conectividad.

## <a name="connectivity-architecture"></a>Arquitectura de conectividad

En el siguiente diagrama se proporciona una descripción general de la arquitectura de conectividad de Azure SQL Database.

![Descripción general de la arquitectura](./media/sql-database-connectivity-architecture/connectivity-overview.png)

En los pasos siguientes se describe cómo se establece una conexión a una base de datos de Azure SQL:

- Los clientes se conectan a una puerta de enlace, que tiene una dirección IP pública y escucha en el puerto 1433.
- La puerta de enlace, dependiendo de la directiva de conexión efectiva, redirecciona o envía por proxy el tráfico al clúster de base de datos correcto.
- Dentro, el tráfico del clúster se reenvía a la base de datos de Azure SQL adecuada.

## <a name="connection-policy"></a>Directiva de conexión

Azure SQL Database admite las siguientes tres opciones para la configuración de directiva de conexión de un servidor de SQL Database:

- **Redirigir (recomendado):** los clientes establecen conexiones directamente con el nodo que hospeda la base de datos. Para habilitar la conectividad, los clientes tienen que permitir las reglas de firewall de salida a todas las direcciones IP de Azure en la región utilizando los grupos de seguridad de red (NSG) con [etiquetas de servicio](../virtual-network/security-overview.md#service-tags) para los puertos 11000-11999, no solo las direcciones IP de puerta de enlace de Azure SQL Database en el puerto 1433. Como los paquetes van directamente a la base de datos, la latencia y la capacidad de proceso mejoran.
- **Proxy:** en este modo, el proxy procesa todas las conexiones a través de las puertas de enlace de Azure SQL Database. Para habilitar la conectividad, el cliente tiene que tener reglas de firewall de salida que permiten solo las direcciones IP de puerta de enlace de Azure SQL Database (normalmente dos direcciones IP por región). La elección de este modo puede producir una latencia mayor y un rendimiento inferior, según la naturaleza de la carga de trabajo. Es muy recomendable utilizar la directiva de conexión `Redirect` frente a `Proxy` para obtener la menor latencia y el mayor rendimiento.
- **Predeterminado:** esta es la directiva de conexión en vigor en todos los servidores después de su creación, a menos que se modifique explícitamente cambiándola a `Proxy` o `Redirect`. La directiva efectiva depende de si las conexiones se originan desde dentro de Azure (`Redirect`) o fuera de Azure (`Proxy`).

## <a name="connectivity-from-within-azure"></a>Conectividad desde dentro de Azure

Si va a conectarse desde dentro de Azure, las conexiones tienen la directiva de conexión predeterminada `Redirect`. Una directiva `Redirect` significa que después de establecer la sesión TCP en Azure SQL Database, la sesión del cliente se redirige al clúster de base de datos correcto con un cambio en la IP virtual de destino de la puerta de enlace de Azure SQL Database a la del clúster. Por lo tanto, todos los paquetes posteriores fluyen directamente al clúster, de tal forma que omiten la puerta de enlace de Azure SQL Database. En el siguiente diagrama, se ilustra este flujo de tráfico.

![Descripción general de la arquitectura](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Conectividad desde fuera de Azure

Si va a conectarse desde fuera de Azure, las conexiones tienen la directiva de conexión predeterminada `Proxy`. La directiva `Proxy` establece que la sesión TCP se realice a través de la puerta de enlace de Azure SQL Database y que todos los paquetes posteriores fluyan a través de la puerta de enlace. En el siguiente diagrama, se ilustra este flujo de tráfico.

![Descripción general de la arquitectura](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Direcciones IP de la puerta de enlace de Azure SQL Database

En la tabla siguiente se muestran las direcciones IP de las puertas de enlace por región. Para conectarse a una base de datos de Azure SQL, debe permitir el tráfico de red desde y hacia **todas** las puertas de enlace de la región.

En el siguiente artículo se describen los detalles de cómo se migrará el tráfico a las nuevas puertas de enlace de regiones específicas: [Migración de tráfico de Azure SQL Database a puertas de enlace más recientes](sql-database-gateway-migration.md)


| Nombre de región          | Direcciones IP de puerta de enlace |
| --- | --- |
| Centro de Australia    | 20.36.105.0 |
| Centro de Australia 2   | 20.36.113.0 |
| Este de Australia       | 13.75.149.87, 40.79.161.1 |
| Sudeste de Australia | 191.239.192.109, 13.73.109.251 |
| Sur de Brasil         | 104.41.11.5,191.233.200.14 |
| Centro de Canadá       | 40.85.224.249      |
| Este de Canadá          | 40.86.226.166      |
| Centro de EE. UU.           | 13.67.215.62, 52.182.137.15 , 23.99.160.139, 104.208.16.96 | 
| Este de China           | 139.219.130.35     |
| Este de China 2         | 40.73.82.1         |
| Norte de China          | 139.219.15.17      |
| Norte de China 2        | 40.73.50.0         |
| Asia oriental            | 191.234.2.139, 52.175.33.150,13.75.32.4 |
| East US              | 40.121.158.30, 40.79.153.12, 191.238.6.43, 40.78.225.32 |
| Este de EE. UU. 2            | 40.79.84.180, 52.177.185.181, 52.167.104.0,  191.239.224.107, 104.208.150.3 | 
| Centro de Francia       | 40.79.137.0, 40.79.129.1 |
| Centro de Alemania      | 51.4.144.100       |
| Nordeste de Alemania   | 51.5.144.179       |
| India central        | 104.211.96.159     |
| Sur de India          | 104.211.224.146    |
| India occidental           | 104.211.160.80     |
| Este de Japón           | 13.78.61.196, 40.79.184.8, 13.78.106.224 , 191.237.240.43, 40.79.192.5 | 
| Oeste de Japón           | 104.214.148.156, 40.74.100.192 | 191.238.68.11,40.74.97.10 | 
| Corea Central        | 52.231.32.42       |
| Corea del Sur          | 52.231.200.86      |
| Centro-Norte de EE. UU     | 23.96.178.199, 23.98.55.75, 52.162.104.33 |
| Europa del Norte         | 40.113.93.91, 191.235.193.75, 52.138.224.1 | 
| Norte de Sudáfrica   | 102.133.152.0      |
| Oeste de Sudáfrica    | 102.133.24.0       |
| Centro-Sur de EE. UU     | 13.66.62.124 , 23.98.162.75, 104.214.16.32   | 
| Sudeste de Asia      | 104.43.15.0 , 23.100.117.95, 40.78.232.3   | 
| Centro de Emiratos Árabes Unidos          | 20.37.72.64        |
| Norte de Emiratos Árabes Unidos            | 65.52.248.0        |
| Sur de Reino Unido 2             | 51.140.184.11      |
| Oeste de Reino Unido              | 51.141.8.11        |
| Centro occidental de EE.UU.      | 13.78.145.25       |
| Europa occidental          | 40.68.37.158, 191.237.232.75, 104.40.168.105  |
| Oeste de EE. UU.              | 104.42.238.205, 23.99.34.75, 13.86.216.196   |
| Oeste de EE. UU. 2            | 13.66.226.202      |
|                      |                    |

## <a name="change-azure-sql-database-connection-policy"></a>Cambio de la directiva de conexión de Azure SQL Database

Para cambiar la directiva de conexión de Azure SQL Database de un servidor de Azure SQL Database, use el comando [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).

- Si la directiva de conexión se establece en `Proxy`, todos los paquetes de red fluyen a través de la puerta de enlace de Azure SQL Database. Para esta configuración, debe permitir el tráfico saliente solo a la dirección IP de la puerta de enlace de Azure SQL Database. El uso de la configuración de `Proxy` ofrece más latencia que la de `Redirect`.
- Si la configuración de la directiva de conexión es `Redirect`, todos los paquetes de red fluyen directamente al clúster de base de datos. Para esta configuración, debe permitir el tráfico saliente a varias direcciones IP.

## <a name="script-to-change-connection-settings-via-powershell"></a>Script para cambiar la configuración de la conexión a través de PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager de PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos. El script siguiente requiere el [módulo de Azure PowerShell](/powershell/azure/install-az-ps).

El siguiente script de PowerShell muestra cómo cambiar la directiva de conexión.

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>Script para cambiar la configuración de la conexión a través de la CLI de Azure

> [!IMPORTANT]
> Este script requiere la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="azure-cli-in-a-bash-shell"></a>CLI de Azure en un shell de Bash

> [!IMPORTANT]
> Este script requiere la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

El siguiente script de CLI muestra cómo cambiar la directiva de conexión en un shell de Bash.

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
ids="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $ids

# Update connection policy
az resource update --ids $ids --set properties.connectionType=Proxy
```

### <a name="azure-cli-from-a-windows-command-prompt"></a>CLI de Azure desde un símbolo del sistema de Windows

> [!IMPORTANT]
> Este script requiere la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

En el siguiente script de la CLI se muestra cómo cambiar la directiva de conexión desde un símbolo del sistema de Windows (con la CLI de Azure instalada).

```azurecli
# Get SQL Server ID and set URI
FOR /F "tokens=*" %g IN ('az sql server show --resource-group myResourceGroup-571418053 --name server-538465606 --query "id" -o tsv') do (SET sqlserverid=%g/connectionPolicies/Default)

# Get current connection policy
az resource show --ids %sqlserverid%

# Update connection policy
az resource update --ids %sqlserverid% --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Pasos siguientes

- Para información sobre cómo cambiar la directiva de conexión de Azure SQL Database para un servidor de Azure SQL Database, consulte [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- Para obtener información sobre el comportamiento de conexión de Azure SQL Database para clientes que usan ADO.NET 4.5 o una versión posterior, vea [Puertos más allá de 1433 para ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Para obtener información general sobre el desarrollo de aplicaciones, vea [Introducción al desarrollo de aplicaciones en SQL Database](sql-database-develop-overview.md).
