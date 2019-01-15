---
title: Ejemplos de plantilla de Azure Resource Manager para una red virtual | Microsoft Docs
description: Conozca las diferentes plantillas de Azure Resource Manager disponibles con las que se pueden implementar redes virtuales de Azure.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/13/2018
ms.author: jdial
ms.openlocfilehash: a16feffea6ac64423398b995499f0e6a60182636
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54039595"
---
# <a name="azure-resource-manager-template-samples-for-virtual-network"></a>Ejemplos de plantilla de Azure Resource Manager para una red virtual

En la tabla siguiente se incluyen vínculos a ejemplos de plantilla de Azure Resource Manager. Las plantillas se pueden implementar mediante Azure [Portal](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json), la [CLI](../azure-resource-manager/resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de Azure o Azure [PowerShell](../azure-resource-manager/resource-group-template-deploy.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Para aprender a crear sus propias plantillas, consulte [Creación e implementación de la primera plantilla de Azure Resource Manager](../azure-resource-manager/resource-manager-create-first-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json) y [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Para la sintaxis y las propiedades de JSON que se usan en una plantilla, consulte [Tipos de recursos de Microsoft.Network](/azure/templates/microsoft.network/allversions).


| | |
|----|----|
|[Creación de una red virtual con dos subredes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)| Crea una red virtual con dos subredes.|
|[Enrutamiento del tráfico por una aplicación virtual de red](https://github.com/Azure/azure-quickstart-templates/tree/master/201-userdefined-routes-appliance)| Crea una red virtual con tres subredes. Implementa una máquina virtual en cada una de las subredes. Crea una tabla de rutas que contiene las rutas para dirigir el tráfico de una subred a otra a través de la máquina virtual de la tercera subred. Asocia la tabla de rutas a una de las subredes.|
|[Creación de un punto de conexión de servicio de red virtual para Azure Storage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-2subnets-service-endpoints-storage-integration)|Crea una nueva red virtual con dos subredes y una interfaz de red en cada subred. Habilita un punto de conexión de servicio en Azure Storage para una de las subredes y protege una nueva cuenta de almacenamiento en dicha subred.|
|[Conexión de dos redes virtuales](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-to-vnet-peering)| Crea dos redes virtuales y un emparejamiento de redes virtuales entre ellas.|
|[Creación de una máquina virtual con varias direcciones IP](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-multiple-ipconfig)| Crea una máquina virtual Windows o Linux con varias direcciones IP.|
