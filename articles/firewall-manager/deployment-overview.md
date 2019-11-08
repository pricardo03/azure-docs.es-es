---
title: Información general sobre la implementación de Azure Firewall Manager (versión preliminar)
description: Conozca los pasos de implementación de alto nivel necesarios para Azure Firewall Manager (versión preliminar)
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 10/25/2019
ms.author: victorh
ms.openlocfilehash: df87e652d2969d4ae12e97a2b455648cf39382c3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488261"
---
# <a name="azure-firewall-manager-preview-deployment-overview"></a>Información general sobre la implementación de Azure Firewall Manager (versión preliminar)

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Hay más de una forma de implementar la versión preliminar de Azure Firewall Manager, pero se recomienda el siguiente proceso general.

## <a name="prerequisites"></a>Requisitos previos

> [!IMPORTANT]
> La versión preliminar de Azure Firewall Manager se debe habilitar explícitamente mediante el comando `Register-AzProviderFeature` de PowerShell.
>Desde el símbolo del sistema de PowerShell, ejecute los siguientes comandos:
>
>```azure-powershell
>connect-azaccount
>Register-AzProviderFeature -FeatureName AllowCortexSecurity -ProviderNamespace Microsoft.Network
>```
>El registro de la característica tarda en completarse un máximo de 30 minutos. Ejecute el siguiente comando para comprobar el estado del registro:
>
>`Get-AzProviderFeature -FeatureName AllowCortexSecurity -ProviderNamespace Microsoft.Network`



## <a name="general-deployment-process"></a>Proceso de implementación general

1. Creación de una arquitectura de concentrador y radio

   - Cree un centro virtual protegido mediante Azure Firewall Manager y agregue conexiones de red virtual.<br>*or*<br>
   - Cree un centro de conectividad de Virtual WAN y agregue conexiones de red virtual.
2. Selección de proveedores de seguridad

   - Se realiza al crear un centro virtual protegido.<br>*or*<br>
   - Convierta un centro de conectividad de Virtual WAN en un centro virtual seguro.
3. Creación de una directiva de firewall y asociación con su concentrador

   - Solo se aplica si se usa Azure Firewall.
   - Las directivas de seguridad como servicio (SECaaS) de terceros se configuran a través de la experiencia de administración de asociados.
4. Configuración de los valores de la ruta para enrutar el tráfico a su centro protegido

   - Enrute fácilmente el tráfico al centro protegido para filtrar y registrar rutas definidas por el usuario (UDR) en redes virtuales de radios mediante la página de configuración de rutas del centro virtual protegido.

> [!NOTE]
> - No se puede tener más de un concentrador por red WAN virtual por región. Sin embargo, es posible agregar varias redes WAN virtuales en la región para lograrlo.
> - No se pueden tener espacios de direcciones IP superpuestos para los concentradores de una vWAN.
> - Las conexiones de red virtual del concentrador deben estar en la misma región que el concentrador.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Protección de una red en la nube con la versión preliminar de Azure Firewall Manager en Azure Portal](secure-cloud-network.md)