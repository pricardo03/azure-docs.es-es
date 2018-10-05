---
title: Direcciones de administración de Azure App Service Environment
description: Enumera las direcciones de administración utilizadas en App Service Environment
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: ccompy
ms.openlocfilehash: 590679daff20f9c469fb8fcfcc0fbbad77f91b5b
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162869"
---
# <a name="app-service-environment-management-addresses"></a>Direcciones de administración de App Service Environment

Una instancia de Azure App Service Environment (ASE) es una implementación de Azure App Service en una subred de Azure Virtual Network (VNet).  La instancia de ASE debe ser accesible desde el plano de administración usado por Azure App Service.  Este tráfico de administración de ASE atraviesa la red controlada por el usuario. Si este tráfico está bloqueado o no está correctamente dirigido, se suspenderá la instancia de ASE. Para más información sobre las dependencias de redes de ASE, consulte [Consideraciones de redes y App Service Environment][networking]. Para obtener información general sobre ASE puede consultar [Introducción a App Service Environment][intro].

Este documento enumera la direcciones de origen de App Service para el tráfico de administración de la instancia de ASE y sirve para dos fines importantes.  

1. Puede usar estas direcciones para crear grupos de seguridad de red para bloquear el tráfico entrante.  
2. Puede crear rutas con estas direcciones para admitir implementaciones de túnel forzadas. Para obtener más información sobre cómo configurar su ASE para que funcione en un entorno donde el tráfico saliente se envía de forma local, lea [Configuración de App Service Environment con tunelización forzada][forcedtunnel].

Todas las instancias de ASE tienen una IP virtual pública por la que entra el tráfico de administración. El tráfico de administración entrante desde estas direcciones entra por los puertos 454 y 455 en la IP virtual pública de su instancia de ASE.  

## <a name="list-of-management-addresses"></a>Lista de direcciones de administración ##

| Region | Direcciones |
|--------|-----------|
| Todas las regiones públicas | 70.37.57.58, 157.55.208.185, 52.174.22.21, 13.94.149.179, 13.94.143.126, 13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141, 23.102.188.65, 191.236.154.88, 13.64.115.203, 65.52.193.203, 70.37.89.222, 52.224.105.172, 23.102.135.246 |
| Microsoft Azure Government (Fairfax o MAG) | 23.97.29.209, 13.72.53.37, 13.72.180.105, 23.97.0.17, 23.97.16.184 |

## <a name="get-your-management-addresses-from-api"></a>Obtención de direcciones de administración de la API ##

Puede enumerar las direcciones de administración que coinciden con el ASE con la siguiente llamada API.

    get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01

La API devuelve un documento JSON que incluye todas las direcciones entrantes para su ASE. La lista de direcciones incluye las direcciones de administración, la dirección IP virtual que utiliza su ASE y el intervalo de direcciones de subred de ASE propio.  

Para llamar a la API con [armclient](http://github.com/projectkudu/ARMClient), use los siguientes comandos pero sustituya el identificador de suscripción, el grupo de recursos y el nombre de ASE.  

    armclient login
    armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
