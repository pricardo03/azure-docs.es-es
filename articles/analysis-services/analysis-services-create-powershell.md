---
title: 'Inicio rápido: Creación de un servidor de Azure Analysis Services mediante PowerShell | Microsoft Docs'
description: Obtenga información sobre cómo crear un servidor de Azure Analysis Services mediante PowerShell
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 01bd8b5d1f8ed0d78f3331b4150df37ef0a1049e
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54426789"
---
# <a name="quickstart-create-a-server---powershell"></a>Inicio rápido: Creación de un servidor: PowerShell

Este inicio rápido describe el uso de PowerShell desde la línea de comandos para crear un servidor de Azure Analysis Services de su suscripción.

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción de Azure**: visite [Evaluación gratuita de Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) para crear una cuenta.
- **Azure Active Directory**: la suscripción debe estar asociada a un inquilino de Azure Active Directory y debe tener una cuenta en ese directorio. Para más información, consulte [Permisos de usuario y autenticación](analysis-services-manage-users.md).
- **Módulo Azure PowerShell, versión 4.0 o posterior**. Para encontrar la versión, ejecute ` Get-Module -ListAvailable AzureRM`. Si necesita instalarla o actualizarla, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).

## <a name="import-azurermanalysisservices-module"></a>Importación del módulo AzureRm.AnalysisServices

Para crear un servidor en su suscripción, use el módulo del componente [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices). Cargue el módulo AzureRm.AnalysisServices en la sesión de PowerShell.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en su suscripción de Azure mediante el comando [Command-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount). Siga las instrucciones que aparecen en pantalla.

```powershell
Connect-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Creación de un grupo de recursos

Un [grupo de recursos de Azure](../azure-resource-manager/resource-group-overview.md) es un contenedor lógico en el que se implementan y se administran los recursos de Azure como grupo. Cuando cree el servidor, deberá especificar un grupo de recursos de su suscripción. Si aún no dispone de un grupo de recursos, puede crearlo mediante el comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). En el siguiente ejemplo, se crea un grupo de recursos denominado `myResourceGroup` en la región del oeste de EE. UU.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

## <a name="create-a-server"></a>Creación de un servidor

Cree un nuevo servidor mediante el comando [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). En el ejemplo siguiente se crea un servidor denominado myServer en myResourceGroup, en la región oeste de Estados Unidos, en el nivel de D1 (gratis), y se especifica philipc@adventureworks.com como administrador del servidor.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myserver" -Location WestUS -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede quitar el servidor de la suscripción mediante el comando [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). Si va a seguir con otros inicios rápidos y tutoriales de esta colección, no quite el servidor. En el ejemplo siguiente se quita el servidor creado en el paso anterior.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido aprendió a crear un servidor en la suscripción de Azure mediante PowerShell. Ahora que tiene un servidor, puede protegerlo si configura un firewall de servidor (opcional). También puede agregar un modelo de datos de ejemplo básico al servidor directamente desde el portal. Tener un modelo de ejemplo es útil al aprender cómo configurar los roles de la base de datos del modelo y probar las conexiones de cliente. Para más información, puede seguir el tutorial para agregar un modelo de ejemplo.

> [!div class="nextstepaction"]
> [Inicio rápido: Configuración del firewall del servidor: Portal](analysis-services-qs-firewall.md)      
> [!div class="nextstepaction"]
> [Tutorial: Incorporación de un modelo de ejemplo al servidor](analysis-services-create-sample-model.md)