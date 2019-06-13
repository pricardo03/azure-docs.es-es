---
title: 'Inicio rápido: Creación de una instancia de Standard Load Balancer: plantilla de Azure Resource Manager'
titlesuffix: Azure Load Balancer
description: En este inicio rápido se muestra cómo crear una instancia de Standard Load Balancer mediante la plantilla de Azure Resource Manager.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Standard Load Balancer by using Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2019
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 0e47560ca43b23f4779da701f3e6f11f53a6b1ce
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66480407"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-by-using-azure-resource-manager-template"></a>Inicio rápido: Creación de una instancia de Standard Load Balancer para cargar máquinas virtuales de equilibrio de carga mediante una plantilla de Azure Resource Manager

El equilibrio de carga proporciona un mayor nivel de disponibilidad y escala, ya que distribuye las solicitudes entrantes entre varias máquinas virtuales. Puede implementar una plantilla de Azure Resource Manager para crear un equilibrador de carga en las máquinas virtuales (VM) de equilibrio de carga. En este tutorial rápido se muestra cómo equilibrar la carga de máquinas virtuales mediante Load Balancer Estándar.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-standard-load-balancer"></a>Creación de un equilibrador de carga estándar

En esta sección, se crea una instancia de Standard Load Balancer que ayuda a equilibrar la carga de las máquinas virtuales. La versión Estándar de Load Balancer solo admite direcciones IP públicas estándar. Cuando se crea una instancia de Load Balancer Estándar, también se debe crear una nueva dirección IP pública estándar que se configura como front-end (denominado *LoadBalancerFrontend* de forma predeterminada) para dicha instancia. Hay muchos métodos que se pueden usar para crear un equilibrador de carga estándar. En este inicio rápido, usará Azure PowerShell para implementar una [plantilla de Resource Manager](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-load-balancer-standard-create/azuredeploy.json). Las plantillas de Resource Manager son archivos JSON que definen los recursos que necesita para implementar la solución. Para entender los conceptos asociados a la implementación y administración de sus soluciones de Azure, consulte la [documentación de Azure Resource Manager](/azure/azure-resource-manager/). Para encontrar más plantillas relacionadas con Azure Load Balancer, consulte [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

Para implementar la plantilla, seleccione **Pruébelo** para abrir Azure Cloud Shell y pegue el siguiente script de PowerShell en la ventana del shell. Para pegar el código, haga clic con el botón derecho en la ventana del shell y seleccione **Pegar**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name with 12 or less letters or numbers that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUserName = Read-Host -Prompt "Enter the virtual machine administrator account name"
$adminPassword = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-load-balancer-standard-create/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -location $location -adminUsername $adminUsername -adminPassword $adminPassword
```

Observe que el nombre del grupo de recursos es el nombre del proyecto con **rg** anexado. Necesitará el nombre del grupo de recursos en la sección siguiente.  Los recursos tardan en crearse unos minutos.

## <a name="test-the-load-balancer"></a>Prueba de la instancia de Load Balancer

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Seleccione **Grupos de recursos** en el panel izquierdo.
1. Seleccione el grupo de recursos que creó en la última sección.  El nombre del grupo de recursos predeterminado es el nombre del proyecto con **rg** anexado.
1. Seleccione el equilibrador de carga.  Hay solo un equilibrador de carga. El nombre predeterminado es el nombre del proyecto con **-lb** anexado.
1. Copie la dirección IP pública (solo la parte de la dirección IP) y, luego, péguela en la barra de direcciones del explorador. La página predeterminada del servidor web de IIS se muestra en el explorador.

   ![Servidor web de IIS](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Para ver cómo Load Balancer distribuye el tráfico entre las tres máquinas virtuales, puede forzar la actualización del explorador web desde la máquina cliente.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando no los necesite, elimine el grupo de recursos, Load Balancer y todos los recursos relacionados. Para ello, seleccione el grupo de recursos que contiene el equilibrador de carga desde Azure Portal y, luego, seleccione **Eliminar grupo de recursos**.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado una instancia de Standard Load Balancer, le ha asociado máquinas virtuales, ha configurado la regla de tráfico de Load Balancer y el sondeo de estado y, después, ha probado la instancia de Load Balancer. Para más información acerca de Azure Load Balancer, diríjase a los tutoriales correspondientes.

> [!div class="nextstepaction"]
> [Tutoriales de Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
