---
title: Creación de Load Balancer con redundancia de zona en el front-end con Azure Portal
titlesuffix: Azure Load Balancer
description: Aprenda a crear un equilibrador Standard Load Balancer público con redundancia de zona en el front-end para la dirección IP pública mediante Azure Portal
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: allensu
ms.openlocfilehash: a875fc3446944b8a39f43b29440ab1d7179b847d
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275078"
---
#  <a name="create-a-standard-load-balancer-with-zone-redundant-frontend-using-azure-portal"></a>Creación de un equilibrador Standard Load Balancer con redundancia de zona en el front-end mediante Azure Portal

Este artículo le ayudará a crear un servicio [Standard Load Balancer](https://aka.ms/azureloadbalancerstandard) público con front-end con redundancia de zona mediante una dirección IP pública estándar. De manera predeterminada, una dirección IP de front-end única en un equilibrador de carga estándar tiene redundancia de zona.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

> [!NOTE]
>  El soporte técnico para las zonas de disponibilidad está disponible para recursos y regiones de Azure, y familias de tamaños de máquina virtual seleccionados. Para más información sobre cómo empezar a trabajar y qué recursos, regiones y familias de tamaños de máquina virtual de Azure puede probar con las zonas de disponibilidad, consulte la [introducción a las zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview). Para soporte técnico, eche un vistazo a [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) o [Creación de una solicitud de soporte técnico de Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure 

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-zone-redundant-load-balancer"></a>Creación de un equilibrador de carga con redundancia de zona

1. Desde un explorador, navegue a Azure Portal: [https://portal.azure.com](https://portal.azure.com) e inicie sesión con su cuenta de Azure.
2. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Equilibrador de carga**.
3. En la página **Crear equilibrador de carga**, en **Nombre**, escriba **myLoadBalancer**.
4. En **Tipo**, seleccione **Público**.
5. En la SKU, seleccione **Estándar**.
6. Haga clic en **Dirección IP pública**, haga clic en **Crear nuevo** y en la página **Crear dirección IP pública**, debajo del nombre, escriba **myPublicIPStandard**.
    >[!NOTE] 
    > La dirección IP pública creada en este paso es de SKU estándar y, de forma predeterminada, tiene redundancia de zona. 
8. En **Ubicación**, seleccione **Este de EE. UU. 2** y haga clic en **Aceptar**. El equilibrador de carga empieza entonces a implementarse y este proceso tarda unos minutos en completarse correctamente.

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Load Balancer Estándar y zonas de disponibilidad](load-balancer-standard-availability-zones.md).



