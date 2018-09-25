---
title: Implementación de Azure Firewall mediante una plantilla
description: Implementación de Azure Firewall mediante una plantilla
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: d32e6e29c287d140c28206743e36dc025b26158b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991341"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Implementación de Azure Firewall mediante una plantilla

Esta plantilla crea un firewall y un entorno de red de prueba. La red tiene una red virtual, con tres subredes: *AzureFirewallSubnet*, *ServersSubnet* y *JumpboxSubnet*. Las subredes ServersSubnet y JumpboxSubnet tienen un servidor de Windows Server de dos núcleos en ellas.

El firewall está en AzureFirewallSubnet y se configura con una colección de reglas de aplicación con una única regla que permite el acceso a www.microsoft.com.

Se crea una ruta definida por el usuario que señala el tráfico de red desde ServersSubnet a través del firewall, en donde se aplican las reglas de firewall.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="template-location"></a>Ubicación de la plantilla

La plantilla se encuentra en:

[https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox)

Lea la introducción y, cuando esté listo para implementar, haga clic en **Implementar en Azure**.

## <a name="clean-up-resources"></a>Limpieza de recursos

En primer lugar, explore los recursos que se crearon con el firewall y, a continuación, cuando ya no sean necesarios, puede usar el comando [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos, el firewall y todos los recursos relacionados.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="next-steps"></a>Pasos siguientes

A continuación, puede supervisar los registros de Azure Firewall:

- [Tutorial: Supervisión de los registros de Azure Firewall](./tutorial-diagnostics.md)

