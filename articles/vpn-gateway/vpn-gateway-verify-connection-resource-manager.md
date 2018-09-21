---
title: Verificación de una conexión de VPN Gateway | Microsoft Docs
description: En este artículo se explica cómo verificar una conexión de VPN Gateway de red virtual.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2017
ms.author: cherylmc
ms.openlocfilehash: 9dfea33b558b4b568da1fd9ed90402e6c5ce54bf
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45573202"
---
# <a name="verify-a-vpn-gateway-connection"></a>Verificación de una conexión de VPN Gateway

En este artículo se muestra cómo comprobar una conexión de puerta de enlace de VPN para los modelos de implementación clásico y de Resource Manager.

## <a name="azure-portal"></a>Azure Portal

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

Para comprobar el modelo de implementación de Resource Manager usado en una conexión de puerta de enlace de VPN con PowerShell, instale la versión más reciente de los [cmdlets de PowerShell de Azure Resource Manager](/powershell/azure/overview).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

Para comprobar el modelo de implementación de Resource Manager usado en una conexión de puerta de enlace de VPN con la CLI de Azure, instale la versión más reciente de los [comandos de CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 o posterior).

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>Azure Portal (clásico)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (clásico)

Para comprobar el modelo de implementación clásico usado en la conexión de puerta de enlace de VPN con PowerShell, instale las versiones más reciente de los cmdlets de Azure PowerShell. Asegúrese de descargar e instalar el módulo de [Service Management](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets). Use "Add-AzureAccount" para iniciar sesión en el modelo de implementación clásica.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Pasos siguientes

* Puede agregar máquinas virtuales a las redes virtuales. Consulte [Creación de una máquina virtual que ejecuta Windows en Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) para ver los pasos.