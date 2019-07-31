---
title: Trabajo con máquinas virtuales y grupos de seguridad de red en Azure Bastion | Microsoft Docs
description: En este artículo se describe cómo incorporar acceso a grupos de seguridad de red con Azure Bastion.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 5312ad2593e732f4c84eb67ed263bc9e4666a67a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594191"
---
# <a name="working-with-nsg-access-and-azure-bastion-preview"></a>Trabajo con acceso a grupos de seguridad de red y Azure Bastion (versión preliminar)

Al trabajar con Azure Bastion, puede usar grupos de seguridad de red (NSG). Para más información, consulte [Grupos de seguridad](../virtual-network/security-overview.md). 

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

![Arquitectura](./media/bastion-nsg/nsg_architecture.png)

En este diagrama:

* El host de Bastion se implementa en la red virtual.
* El usuario se conecta a Azure Portal con cualquier explorador HTML5.
* El usuario selecciona la máquina virtual a la que conectarse.
* Con un solo clic, la sesión RDP/SSH se abre en el explorador.
* No se requiere ninguna dirección IP pública en la máquina virtual de Azure.

## <a name="nsg"></a>Grupos de seguridad de red

* **AzureBastionSubnet:** Azure Bastion se implementa en la subred AzureBastionSubnet específica.  
    * **Tráfico de entrada procedente de Internet pública:** Azure Bastion creará una dirección IP pública que necesita el puerto 443 habilitado en la dirección IP pública para el tráfico de entrada. El puerto 3389/22 no tiene que abrirse en la subred AzureBastionSubnet.
    * **Tráfico de salida a máquinas virtuales de destino:** Azure Bastion se comunicará con las máquinas virtuales de destino a través de la dirección IP privada. Los grupos de seguridad de red tienen que permitir el tráfico de salida a otras subredes de VM de destino.
* **Subred de VM de destino:** Se trata de una subred que contiene la máquina virtual de destino a la que quiere conectarse mediante RDP/SSH.
    * **Tráfico de entrada procedente de Azure Bastion:** Azure Bastion se comunicará con la máquina virtual de destino a través de la dirección IP privada. Los puertos RDP/SSH (puertos 3389 y 22, respectivamente) tienen que abrirse en la máquina virtual de destino a través de la dirección IP privada.

## <a name="apply"></a>Aplicación de grupos de seguridad de red en AzureBastionSubnet

Si aplica grupos de seguridad de red a la subred **AzureBastionSubnet**, permita las dos etiquetas de servicio siguientes para la infraestructura y el plano de control de Azure:

* **GatewayManager (solo Resource Manager)** : esta etiqueta denota los prefijos de dirección del servicio Azure Gateway Manager. Si especifica GatewayManager como valor, el tráfico a GatewayManager se permite o se deniega.  Si va a crear grupos de seguridad de red en AzureBastionSubnet, habilite la etiqueta GatewayManager para el tráfico entrante.

* **AzureCloud (solo Resource Manager)** : esta etiqueta denota el espacio de direcciones IP de Azure e incluye todas las direcciones IP públicas del centro de datos. Si especifica AzureCloud como valor, el tráfico a las direcciones IP públicas de Azure se permite o deniega. Si quiere permitir el acceso solo a AzureCloud en una región determinada, puede especificarla. Por ejemplo, si solo quiere permitir el acceso a AzureCloud en la región Este de EE. UU., puede especificar AzureCloud.EastUS como etiqueta de servicio. Si va a crear grupos de seguridad de red en AzureBastionSubnet, habilite la etiqueta AzureCloud para el tráfico saliente.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Bastion, consulte las [preguntas frecuentes](bastion-faq.md).
