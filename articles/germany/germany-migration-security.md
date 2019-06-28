---
title: Migración de recursos de seguridad de Azure desde Azure Alemania a Azure global
description: En este artículo se proporciona información sobre cómo migrar los recursos de seguridad de Azure desde Azure Alemania a Azure global.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 08/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: f411e4fce32e31bfd02db554999e467770409857
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033652"
---
# <a name="migrate-security-resources-to-global-azure"></a>Migración de recursos de seguridad a Azure global

En este artículo se proporciona información que puede ayudarle a migrar los recursos de seguridad de Azure desde Azure Alemania a Azure global.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-active-directory"></a>Azure Active Directory

Para obtener información acerca de cómo migrar Azure Active Directory, consulte [Migración de identidades](./germany-migration-identity.md#azure-active-directory).

## <a name="key-vault"></a>Key Vault

Algunas de las características de Azure Key Vault no se pueden migrar desde Azure Alemania a Azure global.

### <a name="encryption-keys"></a>Claves de cifrado

Las claves de cifrado no se pueden migrar. Cree nuevas claves en la región de destino y, después, úselas para proteger el recurso de destino (por ejemplo, Azure Storage o Azure SQL Database). Migre de forma segura los datos de la región anterior a la nueva.

### <a name="application-secrets"></a>Secretos de aplicación

Los secretos de aplicación son certificados, claves de cuenta de almacenamiento y otros secretos relacionados con la aplicación. Durante una migración, primero cree almacén de claves en Azure global. Luego, realice una de las siguientes acciones:

- Cree nuevos secretos de aplicación.
- Lea los secretos en Azure Alemania actuales y escriba el valor en el nuevo almacén.

```powershell
Get-AzKeyVaultSecret -vaultname mysecrets -name Deploydefaultpw
```

Para obtener más información:

- Actualice sus conocimientos completando los [tutoriales de Key Vault](https://docs.microsoft.com/azure/key-vault/#step-by-step-tutorials).
- Revise [Introducción a Key Vault](../key-vault/key-vault-overview.md).
- Examine los [cmdlets de PowerShell para Key Vault](/powershell/module/az.keyvault/).

## <a name="vpn-gateway"></a>VPN Gateway

En este momento no se admite la migración de una instancia de Azure VPN Gateway desde Azure Alemania a Azure global. Se recomienda crear y configurar una nueva instancia de VPN Gateway en Azure global.

Puede recopilar la información sobre la configuración actual de VPN Gateway mediante el portal o PowerShell. En PowerShell, use un conjunto de cmdlets que comienzan por `Get-AzVirtualNetworkGateway*`.

Asegúrese de que actualice la configuración local. Además, elimine todas las reglas existentes en los intervalos de direcciones IP antiguas después de actualizar el entorno de red de Azure.

Para obtener más información:

- Actualice sus conocimientos completando los [tutoriales de VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/#step-by-step-tutorials).
- Aprenda cómo [crear de una conexión de sitio a sitio](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).
- Revise los cmdlets [Get AzVirtualNetworkGateway](/powershell/module/az.network/get-azvirtualnetworkgateway) de PowerShell.
- Lea la entrada de blob [Create a site-to-site connection](https://blogs.technet.microsoft.com/ralfwi/2017/02/02/connecting-clouds/) (Creación de una conexión de sitio a sitio).
  
## <a name="application-gateway"></a>Application Gateway

En este momento no se admite la migración de una instancia de Azure Application Gateway desde Azure Alemania a Azure global. Se recomienda crear y configurar una nueva puerta de enlace en Azure global.

Puede recopilar la información sobre la configuración actual la puerta de enlace mediante el portal o PowerShell. En PowerShell, use un conjunto de cmdlets que comienzan por `Get-AzApplicationGateway*`.

Para obtener más información:

- Actualice sus conocimientos completando los [tutoriales de Application Gateway](https://docs.microsoft.com/azure/application-gateway/#step-by-step-tutorials).
- Aprenda a [crear una puerta de enlace de aplicaciones](../application-gateway/quick-create-portal.md).
- Revise los cmdlets [Get AzApplicationGateway](/powershell/module/az.network/get-azapplicationgateway) de PowerShell.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre herramientas, técnicas y recomendaciones para migrar los recursos en las siguientes categorías de servicio:

- [Proceso](./germany-migration-compute.md)
- [Redes](./germany-migration-networking.md)
- [Storage](./germany-migration-storage.md)
- [Web](./germany-migration-web.md)
- [Bases de datos](./germany-migration-databases.md)
- [Analytics](./germany-migration-analytics.md)
- [IoT](./germany-migration-iot.md)
- [Integración](./germany-migration-integration.md)
- [Identidad](./germany-migration-identity.md)
- [Herramientas de administración](./germany-migration-management-tools.md)
- [Elementos multimedia](./germany-migration-media.md)

