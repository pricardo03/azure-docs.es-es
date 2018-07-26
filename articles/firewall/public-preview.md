---
title: Habilitación de la versión preliminar pública de Azure Firewall
description: Uso de Azure PowerShell para habilitar la versión preliminar pública de Azure Firewall
author: vhorne
ms.service: firewall
services: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: 263b16a419b5ff20a9b6d62860385f92c2a18f9c
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992545"
---
# <a name="enable-the-azure-firewall-public-preview"></a>Habilitación de la versión preliminar pública de Azure Firewall

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

## <a name="enable-using-azure-powershell"></a>Habilitar con Azure PowerShell

Para habilitar la versión preliminar pública de Azure Firewall, utilice los siguientes comandos de Azure PowerShell:

```PowerShell
Register-AzureRmProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Register-AzureRmProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```

Se tarda hasta 30 minutos en completar el registro de características. Puede comprobar el estado del registro mediante la ejecución de los siguientes comandos de Azure PowerShell:

```PowerShell

Get-AzureRmProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Get-AzureRmProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```
Una vez completado el registro, ejecute el siguiente comando:

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal](tutorial-firewall-deploy-portal.md)

