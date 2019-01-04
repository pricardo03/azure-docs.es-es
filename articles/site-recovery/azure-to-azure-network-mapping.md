---
title: Asignación de redes virtuales entre dos regiones de Azure en Azure Site Recovery | Microsoft Docs
description: Azure Site Recovery coordina la replicación, la conmutación por error y la recuperación de máquinas virtuales y servidores físicos. Obtenga información sobre la conmutación por error en Azure o en un centro de datos secundario.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: 6140687d583534d21ee50652811c2fd1624a5cf5
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840459"
---
# <a name="set-up-network-mapping-and-ip-addressing-for-vnets"></a>Configuración de la asignación de red y el direccionamiento IP para redes virtuales

En este artículo se describe cómo asignar dos instancias de redes virtuales de (VNet) ubicadas en regiones de Azure diferentes y cómo configurar el direccionamiento IP entre redes. La asignación de red garantiza que se crea una máquina virtual replicada en la región de Azure de destino en la red virtual que está asignada a la red virtual de la máquina virtual de origen.

## <a name="prerequisites"></a>Requisitos previos

Antes de asignar redes, debe tener [redes virtuales de Azure](../virtual-network/virtual-networks-overview.md) en las regiones de Azure de origen y destino. 

## <a name="set-up-network-mapping"></a>Configuración de la asignación de red

Asigne las redes de la siguiente manera:

1. En **Infraestructura de Site Recovery**, haga clic en **+Asignación de red**.

    ![ Crear una asignación de red](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)

3. En **Agregar asignación de red**, seleccione las ubicaciones de origen y destino. En nuestro ejemplo, la máquina virtual de origen se ejecuta en la región Asia Oriental y se replica en la región Sudeste Asiático.

    ![Seleccionar origen y destino ](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)
3. Ahora puede crear una asignación de red en el directorio opuesto. En nuestro ejemplo, el origen será ahora Sudeste Asiático y el destino será Asia Oriental.

    ![Panel para agregar asignaciones de red: selección de las ubicaciones de origen y destino de la red de destino](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-networks-when-you-enable-replication"></a>Asignación de redes al habilitar la replicación

Si aún no ha preparado la asignación de red antes de configurar la recuperación ante desastres para máquinas virtuales de Azure, puede especificar una red de destino en el momento de [configurar y habilitar la replicación](azure-to-azure-how-to-enable-replication.md). Al hacerlo, ocurre lo siguiente:

- En función del destino que ha seleccionado, Site Recovery crea automáticamente asignaciones de red desde la región de origen a la región de destino y desde la región de destino a la región de origen.
- De forma predeterminada, Site Recovery crea una red en la región de destino que es idéntica a la red de origen. Site Recovery agrega el sufijo **-asr** al nombre de la red de origen. Puede personalizar la red de destino.
- Si ya se ha realizado la asignación de red, no puede modificar la red virtual de destino al habilitar la replicación. Para cambiar la red virtual de destino, debe modificar la asignación de red existente.
- Si modifica la asignación de red de la región A a la región B, asegúrese de modificar también la asignación de red de la región B a la A.

## <a name="specify-a-subnet"></a>Especificación de una subred

La subred de la máquina virtual de destino se selecciona en función del nombre de la subred de la máquina virtual de origen.

- Si una subred con el mismo nombre que el de la subred de la máquina virtual de origen está disponible en la red de destino, se establece esa subred para la máquina virtual de destino.
- Si no hay ninguna subred con el mismo nombre en la red de destino, se establece la primera red en orden alfabético como subred de destino.
- Puede modificar esto en la configuración de **Proceso y red** de la máquina virtual.

    ![Ventana Propiedades de Compute de Proceso y red](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="set-up-ip-addressing-for-target-vms"></a>Configuración del direccionamiento IP para las máquinas virtuales de destino

La dirección IP de cada NIC de una máquina virtual de destino se configura como sigue:

- **DHCP**: si la NIC de la máquina virtual de origen usa DHCP, la NIC de la máquina virtual de destino también se establece para usar DHCP.
- **Static IP address** (Dirección IP estática): si la NIC de la máquina virtual de origen usa direccionamiento IP estático, la NIC de la máquina virtual de destino también usará una dirección IP estática.


## <a name="ip-address-assignment-during-failover"></a>Asignación de direcciones IP durante la conmutación por error

**Subredes de origen y destino** | **Detalles**
--- | ---
Mismo espacio de direcciones | La dirección IP de la NIC de la máquina virtual de origen se establece como la dirección IP de la NIC de la máquina virtual de destino.<br/><br/> Si la dirección no está disponible, la siguiente dirección IP disponible se establece como la dirección IP de destino.
Distinto espacio de direcciones<br/><br/> La siguiente dirección IP disponible en la subred de destino se establece como la dirección de la NIC de la máquina virtual de destino.



## <a name="ip-address-assignment-during-test-failover"></a>Asignación de direcciones IP durante la conmutación por error de prueba

**Red de destino** | **Detalles**
--- | ---
La red de destino es la red virtual de conmutación por error | - La dirección IP de destino es estática, pero no la misma dirección IP que se reserva para la conmutación por error.<br/><br/>  - La dirección IP asignada será la siguiente dirección disponible del final del intervalo de direcciones de subred.<br/><br/> Por ejemplo:  si la dirección IP de origen es la 10.0.0.19 y la red de conmutación por error usa el intervalo 10.0.0.0/24, la siguiente dirección IP asignada a la máquina virtual de destino es la 10.0.0.254.
La red de destino no es la red virtual de conmutación por error | - La dirección IP de destino será estática con la misma dirección IP reservada para la conmutación por error.<br/><br/>  - Si la misma dirección IP ya está asignada, la dirección IP es la siguiente disponible del final del intervalo de direcciones de subred.<br/><br/> Por ejemplo:  si la dirección IP estática de origen es la 10.0.0.19 y la conmutación por error está en una red que no es la red de conmutación por error, con el intervalo 10.0.0.0/24, la dirección IP estática de destino será la 10.0.0.0.19 si está disponible y, en caso contrario, será la 10.0.0.254.

- La red virtual de conmutación por error es la red de destino que seleccionó al configurar la recuperación ante desastres.
- Se recomienda que utilice siempre una red que no sea de producción para la conmutación por error de prueba.
- Puede modificar la dirección IP de destino en la configuración de **Proceso y red** de la máquina virtual.


## <a name="next-steps"></a>Pasos siguientes

- Revise la [Guía de redes](site-recovery-azure-to-azure-networking-guidance.md) para la recuperación ante desastres de máquinas virtuales de Azure.
- [Más información](site-recovery-retain-ip-azure-vm-failover.md) sobre la conservación de direcciones IP después de una conmutación por error.

Si la red de destino elegida es la red virtual de conmutación por error y el segundo punto dice "Si la red de destino elegida es diferente de la red virtual de conmutación por error, pero tiene el mismo intervalo de subred que la red virtual de conmutación por error"