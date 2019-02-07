---
title: Configuración de una red virtual para Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este artículo se describe cómo configurar una red virtual y una subred existentes en las que se puede implementar la Instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 01/15/2019
ms.openlocfilehash: c806357cfb5cbcc67185473e490ebc1f37a12838
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564279"
---
# <a name="configure-an-existing-virtual-network-for-azure-sql-database-managed-instance"></a>Configuración de una red virtual para Instancia administrada de Azure SQL Database

Instancia administrada de Azure SQL Database se debe implementar dentro de una [red virtual](../virtual-network/virtual-networks-overview.md) de Azure y la subred dedicada solo a Instancias administradas. Puede usar la red virtual y la subred existentes si se configuran de acuerdo con [los requisitos de red virtual de Instancia administrada](sql-database-managed-instance-connectivity-architecture.md#network-requirements).

Si uno de los casos siguientes se aplica a usted, puede validar y modificar la red mediante el uso del script que se explica en este artículo:

* Tiene una nueva subred que aún no está configurada.
* No está seguro de que la subred cumple los [requisitos](sql-database-managed-instance-connectivity-architecture.md#network-requirements).
* Quiere comprobar si la subred cumple con los [requisitos de red](sql-database-managed-instance-connectivity-architecture.md#network-requirements) una vez realizados los cambios.


> [!Note]
> Puede crear una Instancia administrada solo en redes virtuales creadas mediante el modelo de implementación de Azure Resource Manager. No se admiten las redes virtuales de Azure creadas mediante el modelo de implementación clásica. Calcule el tamaño de la subred siguiendo las instrucciones del artículo [Determinación del tamaño de subred para instancias administradas](sql-database-managed-instance-determine-size-vnet-subnet.md). No se puede cambiar el tamaño de la subred después de implementar los recursos que contiene.

## <a name="validate-and-modify-an-existing-virtual-network"></a>Validación y modificación de una red virtual existente

Si desea crear una instancia administrada en una subred existente, es aconsejable usar el siguiente script de PowerShell para preparar la subred:

```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```

El script prepara la subred en tres pasos:

1. Validación: se validan la subred y la red virtuales seleccionadas para garantizar el cumplimiento de los requisitos de red de Instancia administrada.
2. Confirmación: se muestra al usuario el conjunto de cambios que deben realizarse para preparar la subred para la implementación de Instancia administrada. También se solicitará su consentimiento.
3. Preparación: la subred y la red virtuales se configuran adecuadamente.

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada?](sql-database-managed-instance.md)
- Para obtener un tutorial que muestre cómo crear una red virtual, crear una instancia administrada y restaurar una base de datos desde una copia de seguridad de base de datos, consulte [Creación de una Instancia administrada de Azure SQL Database](sql-database-managed-instance-get-started.md).
- Para ver información relativa a los distintos problemas de DNS, consulte [Configuración de un DNS personalizado](sql-database-managed-instance-custom-dns.md).
