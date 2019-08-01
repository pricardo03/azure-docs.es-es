---
title: Guardado en la lista de IP seguras de las direcciones URL de Azure Portal | Microsoft Docs
description: Agregar estas direcciones URL a la omisión del servidor proxy para comunicarse con Azure Portal y sus servicios
services: azure-portal
keywords: ''
author: kfollis
ms.author: kfollis
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 3747ca7504e1a8a6bbeb6237c1b3cb2e5e4afb5b
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667469"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Guardado en la lista de IP seguras de las direcciones URL de Azure Portal en el servidor proxy o firewall

Para lograr un buen rendimiento y conectividad entre la red de área local o extensa y la nube de Azure, configure los dispositivos de seguridad locales de modo que omitan las restricciones de seguridad para las direcciones URL de Azure Portal. Con frecuencia, los administradores de red implementan servidores proxy, firewalls u otros dispositivos para ayudar con la seguridad y tener control sobre cómo los usuarios acceden a Internet. Sin embargo, las reglas diseñadas para proteger a los usuarios a veces pueden bloquear o ralentizar el tráfico de Internet empresarial legítimo, como las comunicaciones entre usted y Azure. Para optimizar la conectividad entre la red y Azure Portal y sus servicios, se recomienda agregar las direcciones URL de Azure Portal a la lista de IP seguras.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Direcciones URL de Azure Portal para la omisión del proxy

Agregue la siguiente lista de direcciones URL al servidor proxy o firewall para permitir el tráfico de red a estos puntos de conexión y omitir las restricciones:

* *.aadcdn.microsoftonline-p.com
* *.aimon.applicationinsights.io
* *.azure.com
* *.azuredatalakestore.net
* *.azureedge.net
* *.exp.azure.com
* *.ext.azure.com
* *.gfx.ms
* *.account.microsoft.com
* *.hosting.portal.azure.net
* *.marketplaceapi.microsoft.com
* *.microsoftonline.com
* *.msauth.net
* *.msftauth.net
* *.portal.azure.com
* *.portalext.visualstudio.com
* *.sts.microsoft.com
* *.vortex.data.microsoft.com
* *.vscommerce.visualstudio.com
* *.vssps.visualstudio.com
* \* .windows.net
* *.wpc.azureedge.net

> [!NOTE]
> El tráfico a estos puntos de conexión usa puertos TCP estándar para HTTP (80) y HTTPS (443).
>
>
## <a name="next-steps"></a>Pasos siguientes

* ¿Necesita guardar direcciones IP en la lista de IP seguras? Descargue la lista de [intervalos IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653).
* Otro servicios de Microsoft usan direcciones URL y direcciones IP adicionales para la conectividad. Para optimizar la conectividad de red de los servicios de Microsoft 365, consulte [Set up your network for Office 365](/office365/enterprise/set-up-network-for-office-365) (Configuración de la red para Office 365).
