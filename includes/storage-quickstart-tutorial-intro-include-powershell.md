---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 31ef8577a2304091fc4df1b394555c4b30fcf96e
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166101"
---
## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en la suscripción a Azure con el comando `Connect-AzureRmAccount` y siga las instrucciones de la pantalla.

```powershell
Connect-AzureRmAccount
```

Si no sabe qué ubicación desea usar, puede enumerar las ubicaciones disponibles. Cuando se muestre la lista, busque la que desee usar. En este ejemplo se usa **eastus**. Almacénelo en una variable y úsela para que pueda cambiar este valor en un solo lugar.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos de Azure con [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure. 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Cree una cuenta de almacenamiento de uso general estándar con replicación de LRS usando [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount); a continuación, recupere el contexto de la cuenta de almacenamiento que define la cuenta de almacenamiento que se usará. Cuando actúa en una cuenta de almacenamiento, hace referencia al contexto en lugar de proporcionar varias veces las credenciales. En este ejemplo se crea una cuenta de almacenamiento denominada *mystorageaccount* con el cifrado de blob y el almacenamiento con redundancia local (LRS) (habilitados de forma predeterminada).

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```
