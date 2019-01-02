---
title: Suministro de configuraciones posteriores a la implementación mediante el uso de extensiones en Azure | Microsoft Docs
description: Aprenda cómo usar las extensiones de plantillas de Azure Resource Manager para proporcionar configuraciones posteriores a la implementación.
services: azure-resource-manager
documentationcenter: na
author: mumian
editor: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: eb46966c3a28b3fa4c2b23668109b7c5d23a609b
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53414345"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Suministro de configuraciones posteriores a la implementación mediante el uso de extensiones en Azure

Las extensiones de plantillas son aplicaciones pequeñas que realizan tareas de automatización y configuración posterior a la implementación en recursos de Azure. El más popular son las extensiones de máquina virtual. Consulte [Características y extensiones de las máquinas virtuales para Windows](../virtual-machines/extensions/features-windows.md) o [Características y extensiones de las máquinas virtuales para Linux](../virtual-machines/extensions/features-linux.md).

## <a name="extensions"></a>Extensiones

Las extensiones existentes son:

- [Microsoft.Compute/virtualMachines/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Microsoft.HDInsight clusters/extensions](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters/extensions)
- [Microsoft.Sql servers/databases/extensions](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) 
- [Microsoft.Web/sites/siteextensions](https://docs.microsoft.com/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Para averiguar las extensiones disponibles, vaya a la [referencia de plantilla](https://docs.microsoft.com/azure/templates/). En **Filtrar por título**, escriba **extensión**.

Para aprender cómo usar estas extensiones, consulte:

- [Tutorial: Implementación de extensiones de máquina virtual con plantillas de Azure Resource Manager](./resource-manager-tutorial-deploy-vm-extensions.md).
- [Tutorial: Importación de archivos BACPAC de SQL con plantillas de Azure Resource Manager](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Implementación de extensiones de máquina virtual con plantillas de Azure Resource Manager](./resource-manager-tutorial-deploy-vm-extensions.md)
