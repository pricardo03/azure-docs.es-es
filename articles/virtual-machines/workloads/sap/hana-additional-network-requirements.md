---
title: Requisitos de red adicionales para SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Requisitos de red adicionales para SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ee89553846309bd96f3cad3648e337488cf07d3
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44167633"
---
# <a name="additional-network-requirements"></a>Requisitos de red adicionales

Es posible que tenga requisitos de red adicionales como parte de la implementación de instancias grandes de HANA. En este artículo se muestran los siguientes requisitos de red:
- [Incorporación de más direcciones IP o subredes ](#adding-more-ip-addresses-or-subnets)
- [Incorporación de redes virtuales](#adding-vnets)
- [Aumento del ancho de banda del circuito de ExpressRoute](#increasing-expressroute-circuit-bandwidth)
- [Incorporación de un circuito de ExpressRoute adicional](#adding-an-additional-expressroute-circuit)
- [Eliminación de una subred](#deleting-a-subnet)
- [Eliminación de una red virtual](#deleting-a-vnet)
- [Eliminación de un circuito de ExpressRoute](#deleting-an-expressroute-circuit)


## <a name="adding-more-ip-addresses-or-subnets"></a>incorporación de más direcciones IP o subredes

Use Azure Portal, PowerShell o la CLI al agregar más direcciones IP o subredes.

En este caso, se recomienda agregar el nuevo intervalo de direcciones IP como un nuevo intervalo al espacio de direcciones de la red virtual en lugar de generar un nuevo intervalo agregado. En cualquier caso, debe enviar este cambio a Microsoft para permitir la conectividad desde el nuevo intervalo de direcciones IP a las unidades de HANA Instancias grandes del cliente. Puede abrir una solicitud de soporte técnico de Azure para obtener el nuevo espacio de direcciones de red virtual agregado. Después de recibir la confirmación, realice los pasos siguientes.

Para crear una subred adicional desde Azure Portal, consulte el artículo [Creación de una red virtual mediante Azure Portal](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network) y para crearla desde PowerShell, consulte [Creación de una red virtual mediante PowerShell](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network).

## <a name="adding-vnets"></a>Incorporación de redes virtuales

Después de conectarse inicialmente a una o varias redes virtuales de Azure, puede agregar otras que tengan acceso a SAP HANA en Azure (Instancias grandes). En primer lugar, envíe una solicitud de soporte técnico de Azure que incluya tanto la información específica para identificar la implementación de Azure en particular como los intervalos de direcciones IP del espacio de direcciones de la red virtual de Azure. A continuación, SAP HANA en Azure Service Management, proporciona la información necesaria para conectar las redes virtuales y ExpressRoute adicionales. Para cada red virtual necesita una clave de autorización única a fin de conectar el circuito de ExpressRoute a HANA (Instancias grandes).

Pasos para agregar una nueva red virtual de Azure:

1. Realice el primer paso del proceso de incorporación; vea la sección _Creación de una red virtual de Azure_.
2. Realice el segundo paso del proceso de incorporación; vea la sección _Creación de una subred de puerta de enlace_.
3. Para conectar las redes virtuales adicionales al circuito de ExpressRoute de HANA (Instancias grandes), abra una solicitud de soporte técnico de Azure con información sobre la nueva red virtual y solicite una nueva clave de autorización.
4. Cuando reciba la notificación de que la autorización está completa, use la información de autorización proporcionada por Microsoft para realizar el tercer paso del proceso de incorporación; vea la sección _Vinculación de redes virtuales_.

## <a name="increasing-expressroute-circuit-bandwidth"></a>Aumento del ancho de banda del circuito de ExpressRoute

Consulte con SAP HANA en Azure Service Management. Si se le recomienda aumentar el ancho de banda del circuito de ExpressRoute de SAP HANA en Azure (Instancias grandes), cree una solicitud de soporte técnico de Azure. Puede solicitar un aumento del ancho de banda de un solo circuito de hasta 10 Gbps. Recibirá una notificación cuando la operación se complete; no es necesaria ninguna acción adicional para habilitar esta velocidad superior en Azure.

## <a name="adding-an-additional-expressroute-circuit"></a>Incorporación de un circuito de ExpressRoute adicional

Consulte con SAP HANA en Azure Service Management; si se le recomienda agregar otro circuito de ExpressRoute, abra una solicitud de soporte técnico de Azure para crear un circuito de ExpressRoute y obtener información de autorización para conectarse a él. Para que la administración de servicios de SAP HANA en Azure proporcione la autorización, es necesario definir el espacio de direcciones que se va a usar en las redes virtuales antes de realizar la solicitud.

Una vez creado el nuevo circuito y terminada la configuración de administración de servicios de SAP HANA en Azure, recibirá una notificación con la información que necesita para continuar. Siga los pasos antes indicados para crear y conectar la nueva red virtual para este circuito adicional. No puede conectar redes virtuales de Azure a este circuito adicional si ya están conectadas a otro circuito de ExpressRoute de SAP HANA en Azure (Instancias grandes) en la misma región de Azure.

## <a name="deleting-a-subnet"></a>Eliminación de una subred

Para quitar una subred de red virtual, se puede usar Azure Portal, PowerShell o la CLI. Si el intervalo de direcciones IP de la red virtual de Azure o el espacio de direcciones de la red virtual de Azure fuesen intervalos agregados, no son necesarias acciones con Microsoft. Eso salvo que la red virtual todavía esté propagando el espacio de direcciones de la ruta BGP que incluye la subred eliminada. Si ha definido el intervalo de direcciones IP de la red virtual de Azure o el espacio de direcciones de la red virtual de Azure como varios intervalos de direcciones IP, de los cuales uno está asignado a la subred eliminada, debe eliminarlo del espacio de direcciones de la red virtual y, posteriormente, informar a SAP HANA en Azure Service Management para que lo elimine de los intervalos con los que SAP HANA en Azure (Instancias grandes) tiene permitido comunicarse.

Para eliminar una subred, consulte [Eliminación de una subred](../../../virtual-network/virtual-network-manage-subnet.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-subnet) para más información sobre la creación de subredes.

## <a name="deleting-a-vnet"></a>Eliminación de una red virtual

Para eliminar una red virtual, consulte [Eliminación de una red virtual](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-virtual-network). SAP HANA en Azure Service Management quita las autorizaciones existentes en el circuito ExpressRoute de SAP HANA en Azure (Instancias grandes) y elimina el intervalo de direcciones IP de la red virtual de Azure o el espacio de direcciones de la red virtual de Azure para la comunicación con HANA Instancias grandes.

Una vez eliminada la red virtual, abra una solicitud de soporte técnico de Azure para proporcionar los intervalos de espacios de direcciones IP que se van a eliminar.

Para asegurarse de que se quita todo, elimine los elementos siguientes:

- La conexión de ExpressRoute, la puerta de enlace de la red virtual, la dirección IP pública de la puerta de enlace de la red virtual y la red virtual

## <a name="deleting-an-expressroute-circuit"></a>Eliminación de un circuito de ExpressRoute

Para quitar un circuito de ExpressRoute adicional de SAP HANA en Azure (Instancias grandes), abra una solicitud de soporte técnico de Azure en la administración de servicios de SAP HANA en Azure y solicite que se elimine. En la suscripción de Azure, puede eliminar o mantener la red virtual, según sea necesario. Sin embargo, debe eliminar la conexión entre el circuito de ExpressRoute de HANA Instancias grandes y la puerta de enlace de la red virtual enlazada.

Si también desea quitar una red virtual, siga las instrucciones de la sección anterior, Eliminación de una red virtual.

**Pasos siguientes**

- Consulte [Procedimiento para instalar y configurar SAP HANA en Azure (instancias grandes)](hana-installation.md).
