---
title: Direcciones de administración de App Service Environment - Azure
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
ms.date: 01/16/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 632fa14bd96eaee2ca58b59dd855584c1fd961e8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58010094"
---
# <a name="app-service-environment-management-addresses"></a>Direcciones de administración de App Service Environment

Una instancia de Azure App Service Environment (ASE) es una implementación de Azure App Service en una subred de Azure Virtual Network (VNet).  La instancia de ASE debe ser accesible desde el plano de administración usado por Azure App Service.  Este tráfico de administración de ASE atraviesa la red controlada por el usuario. Si este tráfico está bloqueado o no está correctamente dirigido, se suspenderá la instancia de ASE. Para más información sobre las dependencias de redes de ASE, consulte [Consideraciones de redes y App Service Environment][networking]. Para obtener información general sobre ASE puede consultar [Introducción a App Service Environment][intro].

Todas las instancias de ASE tienen una IP virtual pública por la que entra el tráfico de administración. El tráfico de administración entrante desde estas direcciones entra por los puertos 454 y 455 en la IP virtual pública de su instancia de ASE. Este documento enumera la direcciones de origen de App Service para el tráfico de administración de la instancia de ASE. Estas direcciones se encuentran en la etiqueta de servicio denominada AppServiceManagement.

Puede usar la etiqueta de servicio denominada AppServiceManagement en los grupos de seguridad de red con el fin de bloquear el tráfico de administración de entrada para su instancia de ASE.  

Las direcciones que se indican a continuación pueden configurarse en una tabla de rutas. Esto es importante al operar con el ASE en una red virtual con túnel forzado donde, de lo contrario, podría tener un problema de enrutamiento asimétrico. Para obtener más información sobre cómo configurar su ASE para que funcione en un entorno donde el tráfico saliente se envía de forma local, lea [Configuración de App Service Environment con tunelización forzada][forcedtunnel].

## <a name="list-of-management-addresses"></a>Lista de direcciones de administración ##

| Region | Direcciones |
|--------|-----------|
| Todas las regiones públicas | 70.37.57.58, 157.55.208.185, 52.174.22.21, 13.94.149.179, 13.94.143.126, 13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141, 23.102.188.65, 191.236.154.88, 13.64.115.203, 65.52.193.203, 70.37.89.222, 52.224.105.172, 23.102.135.246, 52.225.177.153, 65.52.172.237, 52.151.25.45, 40.124.47.188 |
| Microsoft Azure Government | 23.97.29.209, 13.72.53.37, 13.72.180.105, 23.97.0.17, 23.97.16.184 |

## <a name="configuring-a-network-security-group"></a>Configuración de un grupo de seguridad de red

Con los grupos de seguridad de red no es necesario preocuparse de las direcciones individuales ni mantener su propia configuración. Hay una etiqueta de servicio IP denominada AppServiceManagement que se mantiene al día con todas las direcciones. Para usar esta etiqueta de servicio IP en el grupo de seguridad de red, vaya al portal, abra la interfaz de usuario de grupos de seguridad de red y seleccione las reglas de seguridad de entrada. Si tiene una regla previa para el tráfico de administración de entrada, modifíquela. Si este grupo de seguridad de red no se creó con el ASE o si es totalmente nuevo, seleccione **Agregar**. En la lista desplegable Origen, seleccione **Etiqueta de servicio**.  En la etiqueta de servicio de origen, seleccione **AppServiceManagement**. Establezca los intervalos de puertos de origen para \*destino en **Cualquiera**, los intervalos de puerto de destino en **454-455**, protocolo en**TCP** y acción en **Permitir**. Si está creando la regla, debe establecer la prioridad. 

![Creación de un grupo de seguridad de red con la etiqueta de servicio][1]

## <a name="configuring-a-route-table"></a>Configuración de una tabla de rutas

Las direcciones de administración pueden colocarse en una tabla de rutas con un próximo salto de internet para asegurarse de que todo el tráfico de administración de entrada es capaz de recorrer la misma ruta de acceso. Estas rutas se necesitan al configurar la tunelización forzada. Para crear la tabla de rutas, puede usar el portal, PowerShell o la CLI de Azure.  Los comandos para crear una tabla de rutas mediante la CLI de Azure desde un símbolo del sistema de PowerShell se encuentran a continuación. 

    $rg = "resource group name"
    $rt = "route table name"
    $location = "azure location"
    az network route-table create --name $rt --resource-group $rg --location $location
    az network route-table route create -g $rg --route-table-name $rt -n 70.37.57.58 --next-hop-type Internet --address-prefix 70.37.57.58/32 
    az network route-table route create -g $rg --route-table-name $rt -n 157.55.208.185 --next-hop-type Internet --address-prefix 157.55.208.185/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.174.22.21 --next-hop-type Internet --address-prefix 52.174.22.21/32 
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.149.179 --next-hop-type Internet --address-prefix 13.94.149.179/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.143.126 --next-hop-type Internet --address-prefix 13.94.143.126/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.141.115 --next-hop-type Internet --address-prefix 13.94.141.115/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.195.197 --next-hop-type Internet --address-prefix 52.178.195.197/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.190.65 --next-hop-type Internet --address-prefix 52.178.190.65/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.184.149 --next-hop-type Internet --address-prefix 52.178.184.149/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.177.147 --next-hop-type Internet --address-prefix 52.178.177.147/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.75.127.117 --next-hop-type Internet --address-prefix 13.75.127.117/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.125.161 --next-hop-type Internet --address-prefix 40.83.125.161/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.121.56 --next-hop-type Internet --address-prefix 40.83.121.56/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.120.64 --next-hop-type Internet --address-prefix 40.83.120.64/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.56.50 --next-hop-type Internet --address-prefix 52.187.56.50/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.63.37 --next-hop-type Internet --address-prefix 52.187.63.37/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.59.251 --next-hop-type Internet --address-prefix 52.187.59.251/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.63.19 --next-hop-type Internet --address-prefix 52.187.63.19/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.158.140 --next-hop-type Internet --address-prefix 52.165.158.140/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.152.214 --next-hop-type Internet --address-prefix 52.165.152.214/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.154.193 --next-hop-type Internet --address-prefix 52.165.154.193/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.153.122 --next-hop-type Internet --address-prefix 52.165.153.122/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.255 --next-hop-type Internet --address-prefix 104.44.129.255/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.134.255 --next-hop-type Internet --address-prefix 104.44.134.255/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.243 --next-hop-type Internet --address-prefix 104.44.129.243/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.141 --next-hop-type Internet --address-prefix 104.44.129.141/32
    az network route-table route create -g $rg --route-table-name $rt -n 23.102.188.65 --next-hop-type Internet --address-prefix 23.102.188.65/32
    az network route-table route create -g $rg --route-table-name $rt -n 191.236.154.88 --next-hop-type Internet --address-prefix 191.236.154.88/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.64.115.203 --next-hop-type Internet --address-prefix 13.64.115.203/32
    az network route-table route create -g $rg --route-table-name $rt -n 65.52.193.203 --next-hop-type Internet --address-prefix 65.52.193.203/32
    az network route-table route create -g $rg --route-table-name $rt -n 70.37.89.222 --next-hop-type Internet --address-prefix 70.37.89.222/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.224.105.172 --next-hop-type Internet --address-prefix 52.224.105.172/32
    az network route-table route create -g $rg --route-table-name $rt -n 23.102.135.246 --next-hop-type Internet --address-prefix 23.102.135.246/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.225.177.153 --next-hop-type Internet --address-prefix 52.225.177.153/32
    az network route-table route create -g $rg --route-table-name $rt -n 65.52.172.237 --next-hop-type Internet --address-prefix 65.52.172.237/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.151.25.45 --next-hop-type Internet --address-prefix 52.151.25.45/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.124.47.188 --next-hop-type Internet --address-prefix 40.124.47.188/32

Una vez creada la tabla de rutas, deberá establecerla en la subred de ASE.  

## <a name="get-your-management-addresses-from-api"></a>Obtención de direcciones de administración de la API ##

Puede enumerar las direcciones de administración que coinciden con el ASE con la siguiente llamada API.

    get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01

La API devuelve un documento JSON que incluye todas las direcciones entrantes para su ASE. La lista de direcciones incluye las direcciones de administración, la dirección IP virtual que utiliza su ASE y el intervalo de direcciones de subred de ASE propio.  

Para llamar a la API con [armclient](https://github.com/projectkudu/ARMClient), use los siguientes comandos pero sustituya el identificador de suscripción, el grupo de recursos y el nombre de ASE.  

    armclient login
    armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01


<!--IMAGES-->
[1]: ./media/management-addresses/managementaddr-nsg.png

<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
