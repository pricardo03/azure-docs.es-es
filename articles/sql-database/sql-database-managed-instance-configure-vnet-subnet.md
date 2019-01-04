---
title: Configuración de una red virtual y una subred existentes para Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este tema se describe cómo configurar una red virtual y una subred existentes en las que se puede implementar Instancia administrada de Azure SQL Database.
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
ms.date: 09/20/2018
ms.openlocfilehash: 53aba5192ddf57598965fcfe0db5f2b18423c7e9
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53345874"
---
# <a name="configure-an-existing-vnet-for-azure-sql-database-managed-instance"></a>Configuración de una red virtual para Instancia administrada de Azure SQL Database

Instancia administrada de Azure SQL Database se debe implementar dentro de una [red virtual](../virtual-network/virtual-networks-overview.md) de Azure y la subred dedicada solo a instancias administradas. Puede usar la red virtual y la subred existentes si se configuran de acuerdo con [los requisitos de red virtual de Instancia administrada](sql-database-managed-instance-connectivity-architecture.md#network-requirements). 

Si tiene una nueva subred que aún no está configurada, no está seguro si la subred cumple los [requisitos](sql-database-managed-instance-connectivity-architecture.md#network-requirements) o desea comprobar si la subred sigue siendo compatible con los [requisitos de red](sql-database-managed-instance-connectivity-architecture.md#network-requirements) después de los cambios que ha realizado, puede validar y modificar la red mediante el script se explica en esta sección. 

  > [!Note]
  > Una Instancia administrada solo se puede crear en redes virtuales de Resource Manager. No se admiten las redes virtuales de Azure implementadas mediante el modelo de implementación clásico. Asegúrese de que para calcular el tamaño de la subred sigue las directrices de la sección en la que se [determina el tamaño de la subred de las instancias administradas](#determine-the-size-of-subnet-for-managed-instances), ya que una vez que se implementan los recursos en una subred no es posible cambiar el tamaño de esta.

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
La preparación de la subred se realiza en tres sencillos pasos:

1. Validación: la subred y la red virtuales seleccionadas se validan para garantizar el cumplimiento de los requisitos de red de Instancia administrada.
2. Confirmación: se muestra al usuario el conjunto de cambios que deben realizarse para preparar la subred para la implementación de Instancia administrada y se le solicita su autorización.
3. Preparación: la subred y la red virtuales se configuran adecuadamente.

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada de SQL Database (versión preliminar)?](sql-database-managed-instance.md)
- Para obtener un tutorial que muestre cómo crear una red virtual, crear una instancia administrada y restaurar una base de datos desde una copia de seguridad de base de datos, consulte [Creación de una Instancia administrada de Azure SQL Database](sql-database-managed-instance-get-started.md).
- Para ver información relativa a los distintos problemas de DNS, consulte [Configuración de un DNS personalizado](sql-database-managed-instance-custom-dns.md)
