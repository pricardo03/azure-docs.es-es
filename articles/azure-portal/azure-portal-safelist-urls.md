---
title: Guardado en la lista de IP seguras de las direcciones URL de Azure Portal | Microsoft Docs
description: Agregar estas direcciones URL a la omisión del servidor proxy para comunicarse con Azure Portal y sus servicios
services: azure-portal
keywords: ''
author: kfollis
ms.author: kfollis
ms.date: 09/13/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 78c752423d20d183c561d5bcf0bb95246b84ab49
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71076675"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Guardado en la lista de IP seguras de las direcciones URL de Azure Portal en el servidor proxy o firewall

Para lograr un buen rendimiento y conectividad entre la red de área local o extensa y la nube de Azure, configure los dispositivos de seguridad locales de modo que omitan las restricciones de seguridad para las direcciones URL de Azure Portal. Con frecuencia, los administradores de red implementan servidores proxy, firewalls u otros dispositivos para ayudar con la seguridad y tener control sobre cómo los usuarios acceden a Internet. Sin embargo, las reglas diseñadas para proteger a los usuarios a veces pueden bloquear o ralentizar el tráfico de Internet empresarial legítimo, como las comunicaciones entre usted y Azure. Para optimizar la conectividad entre la red y Azure Portal y sus servicios, se recomienda agregar las direcciones URL de Azure Portal a la lista de IP seguras.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Direcciones URL de Azure Portal para la omisión del proxy

Los puntos de conexión de URL a la lista de IP seguras de Azure Portal son específicos de la nube de Azure donde está implementada la organización. Seleccione la nube y agregue la lista de direcciones URL al servidor proxy o firewall para permitir el tráfico de red a estos puntos de conexión y omitir las restricciones.

#### <a name="public-cloudtabpublic-cloud"></a>[Nube pública](#tab/public-cloud)
```
*.aadcdn.microsoftonline-p.com
*.aka.ms
*.applicationinsights.io
*.azure.com
*.azure.net
*.azureafd.net
*.azure-api.net
*.azuredatalakestore.net
*.azureedge.net
*.loganalytics.io
*.microsoft.com
*.microsoftonline.com
*.microsoftonline-p.com
*.msauth.net
*.msftauth.net
*.trafficmanager.net
*.visualstudio.com
*.windows.net
*.windows-int.net
```

#### <a name="us-government-cloudtabus-government-cloud"></a>[Nube del gobierno de Estados Unidos](#tab/us-government-cloud)
```
*.azure.us
*.loganalytics.us
*.microsoft.us
*.microsoftonline.us
*.msauth.net
*.usgovcloudapi.net
*.usgovtrafficmanager.net
*.windowsazure.us
```

#### <a name="china-government-cloudtabchina-government-cloud"></a>[Nube del gobierno de China](#tab/china-government-cloud)
```
*.azure.cn
*.microsoft.cn
*.microsoftonline.cn
*.chinacloudapi.cn
*.trafficmanager.cn
*.chinacloudsites.cn
*.windowsazure.cn
```
---

> [!NOTE]
> El tráfico a estos puntos de conexión usa puertos TCP estándar para HTTP (80) y HTTPS (443).
>
>
## <a name="next-steps"></a>Pasos siguientes

¿Necesita guardar direcciones IP en la lista de IP seguras? Descargue la lista de intervalos IP del centro de datos de Microsoft Azure para su nube:

* [Internacional](https://www.microsoft.com/download/details.aspx?id=56519)
* [Gobierno de Estados Unidos](http://www.microsoft.com/download/details.aspx?id=57063)
* [Alemania](http://www.microsoft.com/download/details.aspx?id=57064)
* [China](http://www.microsoft.com/download/details.aspx?id=57062)

Otro servicios de Microsoft usan direcciones URL y direcciones IP adicionales para la conectividad. Para optimizar la conectividad de red de los servicios de Microsoft 365, consulte [Set up your network for Office 365](/office365/enterprise/set-up-network-for-office-365) (Configuración de la red para Office 365).
