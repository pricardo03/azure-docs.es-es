---
title: Uso de la puerta de enlace de datos local con orígenes de datos de Azure Virtual Network | Microsoft Docs
description: Aprenda a configurar un servidor para usar una puerta de enlace con orígenes de datos en una red virtual.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 675d8ecd3d6a3310a9b102df37df18bed02df3de
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70958801"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Uso de la puerta de enlace para orígenes de datos en una red virtual de Azure (VNet)

En este artículo se describe la propiedad **AlwaysUseGateway** del servidor que se usa cuando los orígenes de datos se encuentran en una [red virtual de Azure (VNET)](../virtual-network/virtual-networks-overview.md).

## <a name="server-access-to-vnet-data-sources"></a>Acceso del servidor a los orígenes de datos de una red virtual

Si el acceso a sus orígenes de datos se realiza a través de una red virtual, el servidor de Azure Analysis Services debe conectarse, en su propio entorno, a esos orígenes de datos como si estuvieran en el entorno local. Puede configurar la propiedad de servidor **AlwaysUseGateway** para especificar el servidor y acceder a todos los datos de orígenes de datos mediante una [puerta de enlace local](analysis-services-gateway.md). 

Los orígenes de datos de Instancia administrada de Azure SQL Database se ejecutan dentro de la red virtual de Azure con una dirección IP privada. Si el punto de conexión público está habilitado en la instancia, no se requiere una puerta de enlace. Si el punto de conexión público no está habilitado, se requiere una puerta de enlace de datos local y la propiedad AlwaysUseGateway debe establecerse en true.

> [!NOTE]
> Esta propiedad es efectiva únicamente cuando se instala y configura una [puerta de enlace de datos local](analysis-services-gateway.md). La puerta de enlace puede estar en la red virtual.

## <a name="configure-alwaysusegateway-property"></a>Configuración de la propiedad AlwaysUseGateway

1. En SSMS > servidor > **Propiedades** > **General**, seleccione **Show Advanced (All) Properties** (Mostrar propiedades avanzadas [todas]).
2. En **ASPaaS\AlwaysUseGateway**, seleccione **true**.

    ![Propiedad AlwaysUseGateway](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Otras referencias
[Conexión a orígenes de datos locales](analysis-services-gateway.md)   
[Instalación y configuración de una puerta de enlace de datos local](analysis-services-gateway-install.md)   
[Azure Virtual Network (VNET)](../virtual-network/virtual-networks-overview.md)   

