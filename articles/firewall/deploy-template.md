---
title: Implementación de Azure Firewall mediante una plantilla
description: Implementación de Azure Firewall mediante una plantilla
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: article
ms.date: 12/01/2018
ms.author: victorh
ms.openlocfilehash: a15ffa23b64f944d8b2c088113bcc29443ce6873
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54038966"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Implementación de Azure Firewall mediante una plantilla

La [plantilla para crear la configuración de un espacio aislado de Azure Firewall](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox) crea un entorno de red de prueba con un firewall. La red tiene una red virtual (VNet) con tres subredes: *AzureFirewallSubnet*, *ServersSubnet* y *JumpboxSubnet*. Cada una de las subredes *ServersSubnet* y *JumpboxSubnet* tiene una única máquina virtual Windows Server de doble núcleo.

El firewall está en la subred *AzureFirewallSubnet* y tiene una colección de reglas de aplicación con una única regla que permite el acceso a *www.microsoft.com*.

Una ruta definida por el usuario señala el tráfico de red desde la subred *ServersSubnet* a través del firewall, en donde se aplican las reglas de firewall.

Para más información sobre Azure Firewall, vea [Implementación y configuración de Azure Firewall mediante Azure Portal](tutorial-firewall-deploy-portal.md).

## <a name="use-the-template-to-deploy-azure-firewall"></a>Uso de la plantilla para implementar Azure Firewall

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

**Para instalar e implementar Azure Firewall con la plantilla**:

1. Acceda a la plantilla en [https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox).
   
1. Lea la introducción y, cuando esté listo para implementar, seleccione **Implementar en Azure**.
   
1. Inicie sesión en Azure Portal si es necesario. 

1. En el portal, en la página **Create a sandbox setup of AzureFirewall** (Crear una configuración de espacio aislado de Azure Firewall), escriba o seleccione los valores siguientes:
   
   - **Grupo de recursos**: seleccione **Crear nuevo**, escriba un nombre para el grupo de recursos y seleccione **Aceptar**. 
   - **Nombre de la red virtual**: escriba un nombre para la red virtual nueva. 
   - **Nombre de usuario administrador**: escriba un nombre de usuario para la cuenta de usuario de administrador.
   - **Contraseña de administrador**: escriba una contraseña de administrador. 
   
1. Consulte los términos y condiciones y seleccione **Acepto los términos y condiciones indicados anteriormente**.
   
1. Seleccione **Comprar**.
   
   La creación de los recursos tardará unos minutos. 
   
1. Explore los recursos que se crearon con el firewall. 

Para información sobre la sintaxis y las propiedades de JSON de un firewall en una plantilla, consulte [Microsoft.Network/azureFirewalls](/azure/templates/microsoft.network/azurefirewalls).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede quitar el grupo de recursos, el firewall y todos los recursos relacionados mediante la ejecución del comando de PowerShell [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup). Para quitar un grupo de recursos llamado *MyResourceGroup*, ejecute: 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name MyResourceGroup
```
Si pretende continuar con el tutorial de supervisión de firewall, no quite aún el firewall ni el grupo de recursos. 

## <a name="next-steps"></a>Pasos siguientes

A continuación, puede supervisar los registros de Azure Firewall:

> [!div class="nextstepaction"]
> [Tutorial: Supervisión de los registros de Azure Firewall](./tutorial-diagnostics.md)
