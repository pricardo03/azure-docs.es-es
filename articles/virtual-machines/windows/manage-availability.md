---
title: Administración de la disponibilidad de las máquinas virtuales Windows en Azure
description: Aprenda a utilizar varias máquinas virtuales para garantizar la alta disponibilidad de su aplicación Windows en Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 02351953-7b6a-4657-b9e1-de2ea8f6aa05
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 11/27/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1f9914b84b63f271c7dd7d1b8f7dbc3b69511605
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2019
ms.locfileid: "74561184"
---
# <a name="manage-the-availability-of-windows-virtual-machines-in-azure"></a>Administración de la disponibilidad de las máquinas virtuales Windows en Azure 

Aprenda a configurar y administrar varias máquinas virtuales para garantizar la alta disponibilidad de la aplicación de Windows en Azure. También puede [administrar la disponibilidad de las máquinas virtuales Linux](../linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Para obtener instrucciones sobre cómo crear y utilizar conjuntos de disponibilidad al usar el modelo de implementación clásica, consulte [Configuración de un conjunto de disponibilidad para máquinas virtuales en el modelo de implementación clásica](classic/configure-availability-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

[!INCLUDE [virtual-machines-common-manage-availability](../../../includes/virtual-machines-common-manage-availability.md)]

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre el equilibrio de carga de las máquinas virtuales, consulte [Equilibrio de carga para servicios de infraestructura de Azure](tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Vista de arquitecturas de referencia para ejecutar aplicaciones de N niveles en SQL Server en IaaS

* [Aplicación Windows de N niveles en Azure con SQL Server](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/n-tier-sql-server)
* [Ejecución de una aplicación de N niveles en varias regiones de Azure para lograr alta disponibilidad](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)
