---
title: Conectarse a una red del mismo nivel en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo conectar su red de laboratorio con otra red como un punto. Por ejemplo, conectar la red de la escuela o universidad locales con redes virtuales del laboratorio en Azure.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: c9b305beae1b385d4714e3a80e6843c7e76a4f60
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410998"
---
# <a name="connect-your-labs-network-with-a-peer-virtual-network-in-azure-lab-services"></a>Conexión de red de su laboratorio con una red virtual del mismo nivel en Azure Lab Services 
En este artículo se proporciona información sobre el emparejamiento de la red de laboratorios con otra red. 

## <a name="overview"></a>Información general
Emparejamiento de redes virtuales permite conectar sin problemas redes virtuales de Azure. Una vez emparejadas, a efectos de conectividad las redes virtuales aparecen como una sola. El tráfico entre máquinas virtuales en las redes virtuales emparejadas se enruta a través de la infraestructura de red troncal de Microsoft, gran parte como el tráfico se enruta entre máquinas virtuales en la misma red virtual a través de direcciones IP privadas solo. Para más información, consulte [Emparejamiento de redes virtuales](../../virtual-network/virtual-network-peering-overview.md).

Puede que necesite conectar la red de su laboratorio con una red virtual del mismo nivel en algunos escenarios, incluidas las siguientes:

- Las máquinas virtuales en el laboratorio tiene el software que se conecta a servidores de licencias local para adquirir la licencia
- Las máquinas virtuales en el laboratorio necesita acceso a conjuntos de datos (o cualquier otro archivo) en recursos compartidos de red de la universidad. 

Determinadas redes locales están conectadas a Azure Virtual Network ya sea a través de [ExpressRoute](../../expressroute/expressroute-introduction.md) o [puerta de enlace de red Virtual](../../vpn-gateway/vpn-gateway-about-vpngateways.md). Estos servicios se deben configurar fuera de Azure Lab Services. Para más información acerca de cómo conectar una red local a Azure mediante ExpressRoute, consulte [información general sobre ExpressRoute]) (.. /expressroute/expressroute-Introduction.MD). Para conectividad local mediante una puerta de enlace Virtual de red, la puerta de enlace especificado red virtual y la cuenta de laboratorio debe estar en la misma región.

## <a name="configure-at-the-time-of-lab-account-creation"></a>Configurar en el momento de creación de la cuenta de laboratorio
Durante la creación de cuenta de laboratorio nuevo, puede elegir una red virtual existente que se muestra en el **red virtual del mismo nivel** lista desplegable. La red virtual seleccionada es connected(peered) al laboratorio creado con la cuenta de laboratorio. Todas las máquinas virtuales en laboratorios que se crean después de hacer este cambio tendría acceso a los recursos en la red virtual emparejada. 

![Seleccione la red virtual del mismo nivel](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer.png)

> [!NOTE]
> Para obtener instrucciones detalladas paso a paso para crear una cuenta de laboratorio, consulte [configurar una cuenta de laboratorio](tutorial-setup-lab-account.md)


## <a name="configure-after-the-lab-is-created"></a>Configurar una vez creado el laboratorio
La misma propiedad se puede habilitar desde el **configuración Labs** pestaña de la **cuenta de laboratorio** página si no ha configurado una red del mismo nivel en el momento de creación de la cuenta de laboratorio. Cambio realizado en esta configuración se aplica solo a los laboratorios que se crean después del cambio. Como puede ver en la imagen, puede habilitar o deshabilitar **red virtual del mismo nivel** para laboratorios en la cuenta de laboratorio. 

![Habilitar o deshabilitar la red virtual de emparejamiento de una vez creado el laboratorio](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer-existing-lab.png) 

Al seleccionar una red virtual para la **red virtual del mismo nivel** campo, el **permitir creador de laboratorio para elegir la ubicación de laboratorio** opción está deshabilitada. Es porque laboratorios en la cuenta de laboratorio deben estar en la misma región que la cuenta de laboratorio para que puedan conectarse con los recursos de la red virtual del mismo nivel. 

> [!IMPORTANT]
> Este cambio de configuración se aplica solo a los laboratorios que se crean después de realizar el cambio, no a los laboratorios existentes. 


## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)

