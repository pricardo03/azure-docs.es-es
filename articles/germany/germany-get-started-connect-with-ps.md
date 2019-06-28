---
title: Conexión a Azure Alemania mediante PowerShell | Microsoft Docs
description: Información acerca de la administración de suscripciones en Azure Alemania mediante PowerShell
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: ralfwi
ms.openlocfilehash: 38ff91fe9ac50a85d684895d0ccb6333f6257284
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033547"
---
# <a name="connect-to-azure-germany-by-using-powershell"></a>Conexión a Azure Alemania mediante PowerShell
Para usar Azure PowerShell con Azure Alemania, es preciso conectarse a Azure Alemania, en lugar de a Azure global. Azure PowerShell se puede usar para administrar una suscripción de gran tamaño a través de un script o para acceder a características que no están actualmente disponibles en Azure Portal. Si ha usado PowerShell en Azure global, básicamente es lo mismo. Las diferencias en Azure Alemania son:

* Conexión a la cuenta
* Nombres de región

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!NOTE]
> Si aún no ha usado PowerShell, consulte [Introducción a Azure PowerShell](/powershell/azure/overview).

Al iniciar PowerShell, tiene que indicar a Azure PowerShell que se conecte a Azure Alemania mediante la especificación de un parámetro de entorno. El parámetro garantiza que PowerShell se conecta a los puntos de conexión correctos. La colección de puntos de conexión se determina cuando se establece la conexión con la cuenta. Diferentes API requieren versiones diferentes del conmutador de entorno:

| Tipo de conexión | Get-Help |
| --- | --- |
| Comandos de [Azure (modelo de implementación clásica)](/powershell/azure) |`Add-AzureAccount -Environment AzureGermanCloud` |
| Comandos de [Azure (modelo de implementación de Resource Manager)](/powershell/azure) |`Connect-AzAccount -EnvironmentName AzureGermanCloud` |
| Comandos de [Azure Active Directory (modelo de implementación clásica)](/previous-versions/azure/jj151815(v=azure.100)) |`Connect-MsolService -AzureEnvironment AzureGermanyCloud` |
| Comandos de [Azure Active Directory (modelo de implementación Resource Manager)](https://msdn.microsoft.com/library/azure/mt757189.aspx) |`Connect-AzureAD -AzureEnvironmentName AzureGermanyCloud` |

También puede usar el modificador `Environment` al conectarse a una cuenta de almacenamiento mediante `New-AzStorageContext` y, después, especificar `AzureGermanCloud`.

## <a name="determining-region"></a>Determinación de la región
Después de estar conectado, hay una diferencia adicional: las regiones que se usan como destino de un servicio. Cada servicio en la nube de Azure tiene una región diferente. Puede verlas en la página de disponibilidad del servicio. Normalmente, se usa la región en el parámetro `Location` de un comando.


| Nombre común | Nombre para mostrar | Nombre de la ubicación |
| --- | --- | --- |
| Centro de Alemania |`Germany Central` | `germanycentral` |
| Noreste de Alemania |`Germany Northeast` | `germanynortheast` |


> [!NOTE]
> Como ocurre con PowerShell para Azure global, puede usar el nombre para mostrar o el de ubicación para el parámetro `Location`.
>
>

Si desea validar las regiones disponibles en Azure Alemania, puede ejecutar los siguientes comandos e imprimir la lista actual. Para las implementaciones clásicas, use el primer comando. Para las implementaciones con Resource Manager, use el segundo comando.

    Get-AzureLocation
    Get-AzLocation

Si desea conocer los entornos disponibles en Azure, puede ejecutar:

    Get-AzureEnvironment
    Get-AzEnvironment

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de cómo conectarse a Azure Alemania, consulte los siguientes recursos:

* [Conexión a Azure Alemania mediante la CLI de Azure](./germany-get-started-connect-with-cli.md)
* [Conexión a Azure Alemania mediante Visual Studio](./germany-get-started-connect-with-vs.md)
* [Conexión a Azure Alemania mediante Azure Portal](./germany-get-started-connect-with-portal.md)




