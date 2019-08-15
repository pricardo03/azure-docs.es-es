---
title: Información general de hosts dedicados de Azure para máquinas virtuales | Microsoft Docs
description: Obtenga más información sobre cómo se pueden usar los hosts dedicados de Azure para implementar máquinas virtuales.
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/25/2019
ms.author: cynthn
ms.openlocfilehash: 011bfeb337f3c04b2d9041abedac50affe1f86b0
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68977258"
---
# <a name="preview-azure-dedicated-hosts"></a>Vista previa: Hosts dedicados de Azure

Azure Dedicated Host es un servicio que proporciona servidores físicos (capaces de hospedar una o varias máquinas virtuales) dedicados a una suscripción a Azure. Los hosts dedicados son los mismos servidores físicos que se usan en nuestros centros de datos y se proporcionan como un recurso. Puede aprovisionar hosts dedicados dentro de una región, zona de disponibilidad y dominio de error. Después, puede colocar las máquinas virtuales directamente en los hosts aprovisionados, en la configuración que más se ajuste a sus necesidades.

[!INCLUDE [virtual-machines-common-dedicated-hosts-preview](../../../includes/virtual-machines-common-dedicated-hosts-preview.md)]

[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]

## <a name="next-steps"></a>Pasos siguientes

- Puede implementar un host dedicado mediante [Azure PowerShell](dedicated-hosts-powershell.md), el [portal](dedicated-hosts-portal.md) y la [CLI de Azure](../linux/dedicated-hosts-cli.md).

- [Aquí](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) encontrará una plantilla de ejemplo en la que se usan zonas y dominios de error para obtener la máxima resistencia en una región.
