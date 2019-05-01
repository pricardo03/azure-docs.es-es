---
title: Configurar el punto de conexión público - Azure SQL Database managed instancia | Microsoft Docs
description: Obtenga información sobre cómo configurar un punto de conexión público para la instancia administrada
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 04/26/2019
ms.openlocfilehash: ea16efbb846f21ec7c3fa39b2efeac741d8f8ce0
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64928368"
---
# <a name="configure-public-endpoint-in-azure-sql-database-managed-instance"></a>Configurar el punto de conexión público en instancia administrada de Azure SQL Database

Punto de conexión público para un [instancia administrada](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) habilita el acceso de datos a la instancia administrada desde fuera de la [red virtual](../virtual-network/virtual-networks-overview.md). Es posible tener acceso a la instancia administrada de servicios de Azure de varios inquilinos como Power BI, Azure App Service o una red local. Al usar el punto de conexión público en una instancia administrada, no es necesario usar una VPN, que puede ayudar a evitar problemas de rendimiento VPN.

En este artículo, aprenderá cómo:

> [!div class="checklist"]
> - Habilitar punto de conexión público para la instancia administrada en Azure portal
> - Habilitar punto de conexión público para la instancia administrada con PowerShell
> - Configurar el grupo de seguridad de red de instancia administrada para permitir el tráfico al punto de conexión público de instancia administrada
> - Obtener la cadena de conexión de punto de conexión público de instancia administrada

## <a name="permissions"></a>Permisos

Debido a la confidencialidad de datos que se encuentra en una instancia administrada, la configuración para habilitar el punto de conexión público de instancia administrada requiere un proceso de dos pasos. Esta medida de seguridad se ajusta a la separación de tareas (SoD):

- Habilitación de punto de conexión público en una instancia administrada debe hacerse mediante el Administrador de la instancia administrada. El Administrador de la instancia administrada se puede encontrar en **Introducción** página SQL administra recursos de la instancia.
- Que permita el tráfico mediante un grupo de seguridad de red que debe realizarse por un administrador de red. Para obtener más información, consulte [permisos de grupo de seguridad de red](../virtual-network/manage-network-security-group.md#permissions).

## <a name="enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal"></a>Habilitación de punto de conexión público para una instancia administrada en Azure portal

1. Inicie el portal de Azure en <https://portal.azure.com/.>
1. Abra el grupo de recursos con la instancia administrada y seleccione el **instancia administrada de SQL** que desea configurar el punto de conexión público en.
1. En el **seguridad** la configuración, seleccione el **red Virtual** ficha.
1. En la página de configuración de red Virtual, seleccione **habilitar** y, a continuación, el **guardar** icono para actualizar la configuración.

![mi-vnet-config.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-config.png)

## <a name="enabling-public-endpoint-for-a-managed-instance-using-powershell"></a>Habilitación de punto de conexión público para una instancia administrada mediante PowerShell

### <a name="enable-public-endpoint"></a>Habilitar punto de conexión público

Ejecute los comandos de PowerShell siguientes. Reemplace **identificador_de_suscripción** con el identificador de suscripción. Reemplace también **rg-name** con el grupo de recursos para la instancia administrada y reemplace **mi nombre** con el nombre de la instancia administrada.

```powershell
Install-Module -Name Az

Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount

# Use your subscription ID in place of subscription-id below

Select-AzSubscription -SubscriptionId {subscription-id}

# Replace rg-name with the resource group for your managed instance, and replace mi-name with the name of your managed instance

$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}

$mi = $mi | Set-AzSqlInstance -PublicDataEndpointEnabled $true -force
```

### <a name="disable-public-endpoint"></a>Deshabilitar punto de conexión público

Para deshabilitar el punto de conexión público con PowerShell, se ejecutaría el comando siguiente (y también, no olvide cerrar el NSG para el puerto de entrada 3342 si tiene configurada):

```powershell
Set-AzSqlInstance -PublicDataEndpointEnabled $false -force
```

## <a name="allow-public-endpoint-traffic-on-the-network-security-group"></a>Permitir el tráfico de punto de conexión público en el grupo de seguridad de red

1. Si tiene la página de configuración de la instancia administrada sigue abierta, vaya a la **Introducción** ficha. En caso contrario, vuelva a su **instancia administrada de SQL** recursos. Seleccione el **subred de la red Virtual** vínculo, que le llevará a la página de configuración de red Virtual.

    ![mi-overview.png](media/sql-database-managed-instance-public-endpoint-configure/mi-overview.png)

1. Seleccione el **subredes** pestaña en el panel izquierdo de configuración de la red Virtual y anote el **grupo de seguridad** para la instancia administrada.

    ![mi-vnet-subnet.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-subnet.png)

1. Vuelva al grupo de recursos que contiene la instancia administrada. Debería ver el **grupo de seguridad de red** nombre se ha indicado anteriormente. Seleccione el nombre para ir a la página de configuración del grupo de seguridad de red.

1. Seleccione el **reglas de seguridad de entrada** ficha, y **agregar** una regla que tiene mayor prioridad que el **deny_all_inbound** regla con la siguiente configuración: </br> </br>

    |Configuración  |Valor sugerido  |DESCRIPCIÓN  |
    |---------|---------|---------|
    |**Origen**     |Cualquier dirección IP o la etiqueta de servicio         |<ul><li>Para servicios de Azure como Power BI, seleccione la etiqueta de servicio de nube de Azure</li> <li>Para su equipo o máquina virtual de Azure, use la dirección IP de NAT</li></ul> |
    |**Intervalos de puerto de origen**     |*         |Deje esta opción para * (any) como puertos de origen suelen ser impredecible asignada de forma dinámica y como tal, |
    |**Destino**     |Cualquiera         |Salir de destino como cualquiera para permitir el tráfico en la subred de instancia administrada |
    |**Intervalos de puerto de destino**     |3342         |Puerto de destino de ámbito a 3342, que es el punto de conexión TDS público de instancia administrada |
    |**Protocolo**     |TCP         |Administra la instancia utiliza el protocolo TCP de TDS |
    |**Acción**     |PERMITIR         |Permitir el tráfico entrante a instancia administrada mediante el punto de conexión público |
    |**Prioridad**     |1300         |Asegúrese de que esta regla es la prioridad más alta que la **deny_all_inbound** regla |

    ![mi-nsg-rules.png](media/sql-database-managed-instance-public-endpoint-configure/mi-nsg-rules.png)

    > [!NOTE]
    > Puerto 3342 se usa para el punto de conexión público las conexiones a instancia administrada y no se puede cambiar en este momento.

## <a name="obtaining-the-managed-instance-public-endpoint-connection-string"></a>Obtener la cadena de conexión de punto de conexión público de instancia administrada

1. Vaya a la página de configuración de instancia administrada de SQL que se ha habilitado para el punto de conexión público. Seleccione el **las cadenas de conexión** pestaña bajo el **configuración** configuración.
1. Tenga en cuenta que el nombre de host del punto de conexión público se incluye en el formato < mi_name >. **pública**. < dns_zone >. database.windows.net y que el puerto usado para la conexión es 3342.

    ![mi-public-endpoint-conn-string.png](media/sql-database-managed-instance-public-endpoint-configure/mi-public-endpoint-conn-string.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [con Azure SQL Database administra la instancia de forma segura con el punto de conexión público](sql-database-managed-instance-public-endpoint-securely.md).