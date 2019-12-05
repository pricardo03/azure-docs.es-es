---
title: Compatibilidad de Azure Resource Manager con Load Balancer
description: En este artículo, usará Azure PowerShell y plantillas con Azure Load Balancer.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/19/2019
ms.author: allensu
ms.openlocfilehash: 57a60a65dee995695224aa1b451e848ea8768ab1
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74215388"
---
# <a name="azure-resource-manager-support-with-azure-load-balancer"></a>Compatibilidad de Azure Resource Manager con Load Balancer



Azure Resource Manager es el marco de administración de servicios preferido en Azure. Azure Load Balancer puede administrarse ahora mediante las herramientas y las API basadas en Azure Resource Manager.

## <a name="concepts"></a>Conceptos

Con Resource Manager, Azure Load Balancer contiene los siguientes recursos secundarios:

* Configuración de dirección IP front-end: un equilibrador de carga puede incluir una o varias direcciones IP front-end, conocidas también como IP virtuales (VIP). Estas direcciones IP sirven como entrada para el tráfico.
* Grupo de direcciones back-end: este grupo es una colección de direcciones IP asociadas a las tarjetas de interfaz de red (NIC) de máquina virtual a las que se distribuye la carga.
* Reglas de equilibrio de carga: una propiedad de regla asigna una combinación dada de dirección IP y puerto front-end a un conjunto de combinaciones de direcciones IP y puerto back-end. Un solo equilibrador de carga puede tener varias reglas de equilibrio de carga. Cada regla es una combinación de una dirección IP y un puerto front-end y una dirección IP y un puerto back-end asociados a las máquinas virtuales.
* Sondeos: los sondeos permiten realizar un seguimiento del estado de las instancias de máquina virtual. Si se produce un error en un sondeo de estado, la instancia de VM se sacará automáticamente de la rotación.
* Reglas NAT entrantes: las reglas NAT que definen el tráfico entrante que fluye a través de la dirección IP front-end y se distribuye a la dirección IP back-end.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Plantillas de inicio rápido

El Administrador de recursos de Azure le permite aprovisionar sus aplicaciones mediante una plantilla declarativa. En una plantilla, puede implementar varios servicios junto con sus dependencias. Use la misma plantilla para implementar su aplicación de forma repetida durante cada fase de su ciclo de vida.

Las plantillas pueden incluir definiciones para:
* **Máquinas virtuales**
* **Redes virtuales**
* **Conjuntos de disponibilidad**
* **Interfaces de red (NIC)**
* **Cuentas de almacenamiento**
* **Equilibradores de carga**
* **Grupos de seguridad de red**
* **Direcciones IP públicas** 

Con las plantillas puede crear todo lo que necesita para una aplicación compleja. El archivo de plantilla se puede proteger en el sistema de administración de contenidos para controlar versiones y colaborar.

[Más información sobre las plantillas](../azure-resource-manager/resource-manager-template-walkthrough.md)

[Más información sobre los recursos de red](../networking/networking-overview.md)

Para las plantillas de inicio rápido que usan Azure Load Balancer, eche un vistazo al [repositorio de GitHub](https://github.com/Azure/azure-quickstart-templates) que contiene un conjunto de plantillas generadas por la comunidad.

Ejemplos de plantillas:

* [Dos máquinas virtuales en un Equilibrador de carga y reglas de equilibrio de carga](https://go.microsoft.com/fwlink/?LinkId=544799)
* [Dos máquinas virtuales en una red virtual con un equilibrador de carga interno y reglas del equilibrador de carga](https://go.microsoft.com/fwlink/?LinkId=544800)
* [Dos máquinas virtuales en un equilibrador de carga y configuración de reglas NAT en el equilibrador de carga](https://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Configuración del Equilibrador de carga de Azure con PowerShell o CLI

Introducción a los cmdlets de Azure Resource Manager, las herramientas de línea de comandos y las API REST

* [cmdlets de Redes de Azure](https://docs.microsoft.com/powershell/module/az.network#networking) pueden usarse para crear un equilibrador de carga.
* [Cómo crear un Equilibrador de carga mediante el Administrador de recursos de Azure](load-balancer-get-started-ilb-arm-ps.md)
* [Uso de CLI de Azure con el Administrador de recursos de Azure](../xplat-cli-azure-resource-manager.md)
* [API de REST del Equilibrador de carga](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Pasos siguientes

[Empiece a crear un equilibrador de carga accesible desde Internet](load-balancer-get-started-internet-arm-ps.md) y configure el tipo de [modo de distribución](load-balancer-distribution-mode.md) para un comportamiento especifico del tráfico de red.

Aprenda a administrar la [configuración de opciones de tiempo de espera de inactividad de TCP para el equilibrador de carga](load-balancer-tcp-idle-timeout.md). Esta configuración es importante cuando la aplicación necesita mantener las conexiones activas para servidores detrás de un equilibrador de carga.
