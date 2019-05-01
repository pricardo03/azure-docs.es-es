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
ms.date: 04/03/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: f76dd423cb3f7fbae6cc88d064e49dc2d56f1a1c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60766064"
---
# <a name="app-service-environment-management-addresses"></a>Direcciones de administración de App Service Environment

App Service Environment (ASE) es una implementación de un solo inquilino de Azure App Service que se ejecuta en Azure Virtual Network (VNet).  Mientras que el ASE se ejecutan en la red virtual, todavía debe ser accesible desde un número de direcciones IP dedicadas que usan Azure App Service para administrar el servicio.  En el caso de un ASE, el tráfico de administración atraviesa la red controlada por el usuario. Si este tráfico está bloqueado o no está correctamente dirigido, se suspenderá la instancia de ASE. Para más información sobre las dependencias de redes de ASE, consulte [Consideraciones de redes y App Service Environment][networking]. Para obtener información general sobre ASE puede consultar [Introducción a App Service Environment][intro].

Todas las instancias de ASE tienen una IP virtual pública por la que entra el tráfico de administración. El tráfico de administración entrante desde estas direcciones entra por los puertos 454 y 455 en la IP virtual pública de su instancia de ASE. Este documento enumera la direcciones de origen de App Service para el tráfico de administración de la instancia de ASE. Estas direcciones también están en la etiqueta de servicio de IP denominada AppServiceManagement.

Las direcciones que se indica a continuación pueden configurarse en una tabla de rutas para evitar problemas de enrutamientos asimétricos con el tráfico de administración. Las rutas actúan sobre el tráfico en el nivel IP y no tiene un conocimiento de la dirección del tráfico o que el tráfico es una parte de un mensaje de respuesta TCP. Si es distinta de la dirección a que se envió la dirección de respuesta para una solicitud TCP, tiene un problema de enrutamiento asimétrico. Para evitar problemas de enrutamiento asimétricos con el tráfico de administración de ASE, deberá asegurarse de que las respuestas se envían desde la misma dirección que se enviaron a. Para obtener más información sobre cómo configurar su ASE para que funcione en un entorno donde el tráfico saliente se envía de forma local, lea [Configuración de App Service Environment con tunelización forzada][forcedtunnel].

## <a name="list-of-management-addresses"></a>Lista de direcciones de administración ##

| Region | Direcciones |
|--------|-----------|
| Todas las regiones públicas | 13.64.115.203, 13.75.127.117, 13.94.141.115, 13.94.143.126, 13.94.149.179, 23.102.135.246, 23.102.188.65, 40.83.120.64, 40.83.121.56, 40.83.125.161, 40.124.47.188, 52.151.25.45, 52.165.152.214, 52.165.153.122, 52.165.154.193, 52.165.158.140, 52.174.22.21, 52.178.177.147, 52.178.184.149, 52.178.190.65, 52.178.195.197, 52.187.56.50, 52.187.59.251, 52.187.63.19, 52.187.63.37, 52.224.105.172, 52.225.177.153, 65.52.14.230, 65.52.172.237, 65.52.193.203, 70.37.57.58, 70.37.89.222, 104.44.129.141, 104.44.129.243, 104.44.129.255, 104.44.134.255, 104.208.54.11, 157.55.176.93, 157.55.208.185, 191.236.154.88 |
| Microsoft Azure Government | 23.97.29.209, 13.72.53.37, 13.72.180.105, 23.97.0.17, 23.97.16.184 |

## <a name="configuring-a-network-security-group"></a>Configuración de un grupo de seguridad de red

Con los grupos de seguridad de red, no es necesario preocuparse por las direcciones individuales o mantener su propia configuración. Hay una etiqueta de servicio IP denominada AppServiceManagement que se mantiene al día con todas las direcciones. Para usar esta etiqueta de servicio IP en el NSG, vaya al portal, abra la interfaz de usuario de grupos de seguridad de red y se seleccionan las reglas de seguridad de entrada. Si tiene una regla para el tráfico de administración de entrada ya existente, editarla. Si este grupo de seguridad de red no se creó con el ASE o si es totalmente nuevo, seleccione **Agregar**. En la lista desplegable Origen, seleccione **Etiqueta de servicio**.  En la etiqueta de servicio de origen, seleccione **AppServiceManagement**. Establezca los intervalos de puertos de origen para \*destino en **Cualquiera**, los intervalos de puerto de destino en **454-455**, protocolo en**TCP** y acción en **Permitir**. Si está realizando la regla, debe establecer la prioridad. 

![Creación de un grupo de seguridad de red con la etiqueta de servicio][1]

## <a name="configuring-a-route-table"></a>Configuración de una tabla de rutas

Las direcciones de administración pueden colocarse en una tabla de rutas con un próximo salto de internet para asegurarse de que todo el tráfico de administración de entrada es capaz de recorrer la misma ruta de acceso. Estas rutas se necesitan al configurar la tunelización forzada. Para crear la tabla de rutas, puede usar el portal, PowerShell o la CLI de Azure.  Los comandos para crear una tabla de rutas mediante la CLI de Azure desde un símbolo del sistema de PowerShell se encuentran a continuación. 

    $rg = "resource group name"
    $rt = "route table name"
    $location = "azure location"
    az network route-table create --name $rt --resource-group $rg --location $location
    az network route-table route create -g $rg --route-table-name $rt -n 13.64.115.203 --next-hop-type Internet --address-prefix 13.64.115.203/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.75.127.117 --next-hop-type Internet --address-prefix 13.75.127.117/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.141.115 --next-hop-type Internet --address-prefix 13.94.141.115/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.143.126 --next-hop-type Internet --address-prefix 13.94.143.126/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.149.179 --next-hop-type Internet --address-prefix 13.94.149.179/32
    az network route-table route create -g $rg --route-table-name $rt -n 23.102.135.246 --next-hop-type Internet --address-prefix 23.102.135.246/32
    az network route-table route create -g $rg --route-table-name $rt -n 23.102.188.65 --next-hop-type Internet --address-prefix 23.102.188.65/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.120.64 --next-hop-type Internet --address-prefix 40.83.120.64/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.121.56 --next-hop-type Internet --address-prefix 40.83.121.56/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.125.161 --next-hop-type Internet --address-prefix 40.83.125.161/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.124.47.188 --next-hop-type Internet --address-prefix 40.124.47.188/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.151.25.45 --next-hop-type Internet --address-prefix 52.151.25.45/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.152.214 --next-hop-type Internet --address-prefix 52.165.152.214/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.153.122 --next-hop-type Internet --address-prefix 52.165.153.122/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.154.193 --next-hop-type Internet --address-prefix 52.165.154.193/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.158.140 --next-hop-type Internet --address-prefix 52.165.158.140/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.174.22.21 --next-hop-type Internet --address-prefix 52.174.22.21/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.177.147 --next-hop-type Internet --address-prefix 52.178.177.147/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.184.149 --next-hop-type Internet --address-prefix 52.178.184.149/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.190.65 --next-hop-type Internet --address-prefix 52.178.190.65/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.195.197 --next-hop-type Internet --address-prefix 52.178.195.197/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.56.50 --next-hop-type Internet --address-prefix 52.187.56.50/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.59.251 --next-hop-type Internet --address-prefix 52.187.59.251/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.63.19 --next-hop-type Internet --address-prefix 52.187.63.19/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.63.37 --next-hop-type Internet --address-prefix 52.187.63.37/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.224.105.172 --next-hop-type Internet --address-prefix 52.224.105.172/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.225.177.153 --next-hop-type Internet --address-prefix 52.225.177.153/32
    az network route-table route create -g $rg --route-table-name $rt -n 65.52.14.230 --next-hop-type Internet --address-prefix 65.52.14.230/32
    az network route-table route create -g $rg --route-table-name $rt -n 65.52.172.237 --next-hop-type Internet --address-prefix 65.52.172.237/32
    az network route-table route create -g $rg --route-table-name $rt -n 65.52.193.203 --next-hop-type Internet --address-prefix 65.52.193.203/32
    az network route-table route create -g $rg --route-table-name $rt -n 70.37.57.58 --next-hop-type Internet --address-prefix 70.37.57.58/32
    az network route-table route create -g $rg --route-table-name $rt -n 70.37.89.222 --next-hop-type Internet --address-prefix 70.37.89.222/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.141 --next-hop-type Internet --address-prefix 104.44.129.141/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.243 --next-hop-type Internet --address-prefix 104.44.129.243/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.255 --next-hop-type Internet --address-prefix 104.44.129.255/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.134.255 --next-hop-type Internet --address-prefix 104.44.134.255/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.208.54.11 --next-hop-type Internet --address-prefix 104.208.54.11/32
    az network route-table route create -g $rg --route-table-name $rt -n 157.55.176.93 --next-hop-type Internet --address-prefix 157.55.176.93/32
    az network route-table route create -g $rg --route-table-name $rt -n 157.55.208.185 --next-hop-type Internet --address-prefix 157.55.208.185/32
    az network route-table route create -g $rg --route-table-name $rt -n 191.236.154.88 --next-hop-type Internet --address-prefix 191.236.154.88/32

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
