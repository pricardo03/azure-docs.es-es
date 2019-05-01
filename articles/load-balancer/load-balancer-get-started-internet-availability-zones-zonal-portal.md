---
title: Creación de una instancia de Load Balancer con front-end de zona mediante Azure Portal
titlesuffix: Azure Load Balancer
description: Aprenda a crear una instancia de Standard Load Balancer con front-end de zona con Azure Portal
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: c81ff5ea330c4c0ba26a92a3b5399cfa961e4b2b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60753518"
---
#  <a name="create-a-standard-load-balancer-with-zonal-frontend-using-azure-portal"></a>Creación de un equilibrador Standard Load Balancer con el front-end de zona mediante Azure Portal

En este artículo se le ayudará a crear una instancia pública de [Standard Load Balancer](https://aka.ms/azureloadbalancerstandard) con una configuración de IP de front-end de zona. Para saber cómo funcionan las zonas de disponibilidad con Load Balancer Estándar, consulte [Load Balancer Estándar y zonas de disponibilidad](load-balancer-standard-availability-zones.md). 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

> [!NOTE]
> El soporte técnico para las zonas de disponibilidad está disponible para recursos y regiones de Azure, y familias de tamaños de máquina virtual seleccionados. Para más información sobre cómo empezar a trabajar y qué recursos, regiones y familias de tamaños de máquina virtual de Azure puede probar con las zonas de disponibilidad, consulte la [introducción a las zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview). Para soporte técnico, eche un vistazo a [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) o [Creación de una solicitud de soporte técnico de Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure 

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-load-balancer-with-zonal-frontend-ip-address"></a>Creación de un equilibrador de carga con dirección IP de front-end zonal

1. Desde un explorador, navegue a Azure Portal: [https://portal.azure.com](https://portal.azure.com) e inicie sesión con su cuenta de Azure.
2. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Equilibrador de carga**.
3. En la página **Crear equilibrador de carga**, en **Nombre**, escriba **myLoadBalancer**.
4. En **Tipo**, seleccione **Público**.
5. En la SKU, seleccione **Estándar**.
6. Haga clic en **Elegir una dirección IP pública** y en **Crear nuevo** y, en la página de **creación de una dirección IP pública**, en el nombre, escriba **myPublicIPZonal**, en la SKU, seleccione **Estándar** y en la zona de disponibilidad, seleccione **1**.
    
>[!NOTE] 
> La dirección IP pública creada en este paso es de SKU estándar de forma predeterminada.

1. En **Grupo de recursos**, haga clic en **Crear nuevo** y escriba **myResourceGroupZLB** como nombre del grupo de recursos.
1. En **Ubicación**, seleccione **Europa Occidental** y haga clic en **Aceptar**. El equilibrador de carga empieza entonces a implementarse y este proceso tarda unos minutos en completarse correctamente.

    ![creación de una instancia de Standard Load Balancer con redundancia de zona con Azure Portal](./media/load-balancer-get-started-internet-availability-zones-zonal-portal/load-balancer-zonal-frontend.png)


## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Load Balancer Estándar y zonas de disponibilidad](load-balancer-standard-availability-zones.md).



