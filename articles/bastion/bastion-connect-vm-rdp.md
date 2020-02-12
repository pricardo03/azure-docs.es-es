---
title: Conexión a una máquina virtual Windows con Azure Bastion | Microsoft Docs
description: En este artículo, descubra cómo usar Azure Bastion para conectarse a una máquina virtual de Azure que ejecuta Windows.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: 7dad6a517341f83f693e1e7e1f7d27e899e00f7e
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76990494"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Conexión a una máquina virtual Windows con Azure Bastion

En este artículo se muestra cómo establecer una conexión segura y sin problemas mediante RDP con las máquinas virtuales Windows de una red virtual de Azure con Azure Bastion. Puede conectarse a una máquina virtual directamente desde Azure Portal. Cuando se utiliza Azure Bastion, las máquinas virtuales no requieren un cliente, un agente ni software adicional. Para más información sobre Azure Bastion, consulte la página de [información general](bastion-overview.md).

## <a name="before-you-begin"></a>Antes de empezar

Asegúrese de haber configurado un host de Azure Bastion para la red virtual donde reside la máquina virtual. Para más información, consulte [Create an Azure Bastion host](bastion-create-host-portal.md) (Creación de un host de Azure Bastion). Cuando el servicio Bastion se aprovisiona e implementa en la red virtual, puede usarlo para conectarse a cualquier máquina virtual de esta red virtual.

Bastion presupone que usa RDP para conectarse a una máquina virtual Windows y SSH para conectarse a las máquinas virtuales Linux. Para información sobre la conexión a una máquina virtual Linux, consulte el artículo sobre cómo [conectarse a una máquina virtual Linux](bastion-connect-vm-ssh.md).

### <a name="required-roles"></a>Roles necesarios
Para crear una conexión, se requieren los siguientes roles:

* Rol de lector en la máquina virtual
* Rol de lector en la tarjeta de interfaz de red con la dirección IP privada de la máquina virtual
* Rol de lector en el recurso de Azure Bastion

### <a name="ports"></a>Puertos

Para conectarse a la máquina virtual Windows a través de RDP, debe tener abiertos los siguientes puertos en esta máquina virtual:

* Puertos de entrada: RDP (3389)

## <a name="rdp"></a>Conexión mediante RDP

1. Abra [Azure Portal](https://portal.azure.com). Vaya a la máquina virtual a la que quiere conectarse y, luego, haga clic en **Conectar**. La máquina virtual debe ser una máquina virtual Windows cuando se usa una conexión RDP.

   ![Conexión de una máquina virtual](./media/bastion-connect-vm-rdp/connect.png)
1. Después de hacer clic en Conectar, aparecerá una barra lateral con tres pestañas: RDP, SSH y Bastion. Si se aprovisionó Bastion para la red virtual, la pestaña Bastion aparece activa de manera predeterminada. Si no aprovisionó Bastion para la red virtual, puede hacer clic en el vínculo para configurarlo. Para instrucciones sobre la configuración, consulte el artículo sobre la [configuración de Bastion](bastion-create-host-portal.md).

   ![Conexión de una máquina virtual](./media/bastion-connect-vm-rdp/bastion.png)
1. En la ficha Bastion, escriba el nombre de usuario y la contraseña para la máquina virtual y, luego, haga clic en **Connect** (Conectar). La conexión RDP con esta máquina virtual a través de Bastion se abrirá directamente en Azure Portal (a través de HTML5) mediante el puerto 443 y el servicio Bastion.

   ![Conexión de una máquina virtual](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Pasos siguientes

Lea el artículo sobre [preguntas frecuentes de Bastion](bastion-faq.md).
