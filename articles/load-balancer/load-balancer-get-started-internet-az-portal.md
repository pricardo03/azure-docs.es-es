---
title: Creación de Load Balancer con redundancia de zona en el front-end con Azure Portal
titleSuffix: Azure Load Balancer
description: Aprenda a crear un equilibrador Standard Load Balancer público con redundancia de zona en el front-end para la dirección IP pública mediante Azure Portal
services: load-balancer
documentationcenter: na
author: asudbring
manager: kumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: allensu
ms.openlocfilehash: bd85e062b2b45abc32269b94ce678ebf63fb3fc1
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2020
ms.locfileid: "75894444"
---
#  <a name="create-a-standard-load-balancer-with-zone-redundant-frontend-using-azure-portal"></a>Creación de un equilibrador Standard Load Balancer con redundancia de zona en el front-end mediante Azure Portal

Este artículo le ayudará a crear una instancia pública de [Standard Load Balancer](https://aka.ms/azureloadbalancerstandard) con front-end con redundancia de zona mediante una dirección IP pública estándar. De manera predeterminada, una dirección IP de front-end única en un equilibrador de carga estándar tiene redundancia de zona.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

> [!NOTE]
>  El soporte técnico para las zonas de disponibilidad está disponible para recursos y regiones de Azure, y familias de tamaños de máquina virtual seleccionados. Para más información sobre cómo empezar a trabajar y qué recursos, regiones y familias de tamaños de máquina virtual de Azure puede probar con las zonas de disponibilidad, consulte la [introducción a las zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview). Para soporte técnico, eche un vistazo a [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) o [Creación de una solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

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



