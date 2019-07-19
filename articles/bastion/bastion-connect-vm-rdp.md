---
title: Conexión a una máquina virtual Windows con Azure Bastion | Microsoft Docs
description: En este artículo, descubra cómo usar Azure Bastion para conectarse a una máquina virtual de Azure que ejecuta Windows.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 376b7042a513dd50647dc8f88bf1de70f65bb21c
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478402"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion-preview"></a>Conexión a una máquina virtual Windows con Azure Bastion (versión preliminar)

En este artículo se muestra cómo establecer una conexión segura y sin problemas mediante RDP con las máquinas virtuales Windows de una red virtual de Azure con Azure Bastion. Puede conectarse a una máquina virtual directamente desde Azure Portal. Cuando se utiliza Azure Bastion, las máquinas virtuales no requieren un cliente, un agente ni software adicional. Para más información sobre Azure Bastion, consulte la página de [información general](bastion-overview.md).

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Antes de empezar

Asegúrese de haber configurado un host de Azure Bastion para la red virtual donde reside la máquina virtual. Para más información, consulte [Create an Azure Bastion host](bastion-create-host-portal.md) (Creación de un host de Azure Bastion). Cuando el servicio Bastion se aprovisiona e implementa en la red virtual, puede usarlo para conectarse a cualquier máquina virtual de esta red virtual. En esta versión preliminar, Bastion presupone que usa RDP para conectarse a una máquina virtual Windows y SSH para conectarse a las máquinas virtuales Linux. Para información sobre la conexión a una máquina virtual Linux, consulte el artículo sobre cómo [conectarse a una máquina virtual Linux](bastion-connect-vm-ssh.md).

Para crear una conexión, se requieren los siguientes roles:

* Rol de lector en la máquina virtual
* Rol de lector en la tarjeta de interfaz de red con la dirección IP privada de la máquina virtual
* Rol de lector en el recurso de Azure Bastion

## <a name="rdp"></a>Conexión mediante RDP

1. Use [este vínculo](https://aka.ms/BastionHost) para abrir la página del portal de la versión preliminar de Azure Bastion. Vaya a la máquina virtual a la que quiere conectarse y, luego, haga clic en **Conectar**. La máquina virtual debe ser una máquina virtual Windows cuando se usa una conexión RDP.

    ![Conexión de una máquina virtual](./media/bastion-connect-vm-rdp/connect.png)

1. Después de hacer clic en Conectar, aparecerá una barra lateral con tres pestañas: RDP, SSH y Bastion. Si se aprovisionó Bastion para la red virtual, la pestaña Bastion aparece activa de manera predeterminada. Si no aprovisionó Bastion para la red virtual, puede hacer clic en el vínculo para configurarlo. Para instrucciones sobre la configuración, consulte el artículo sobre la [configuración de Bastion](bastion-create-host-portal.md). Si no aparece **Bastion** es porque no ha abierto el portal de la versión preliminar. Abra el portal con este [vínculo a la versión preliminar](https://aka.ms/BastionHost).

    ![Conexión de una máquina virtual](./media/bastion-connect-vm-rdp/bastion.png)

1. En la ficha Bastion, escriba el nombre de usuario y la contraseña para la máquina virtual y, luego, haga clic en **Connect** (Conectar). La conexión RDP con esta máquina virtual a través de Bastion se abrirá directamente en Azure Portal (a través de HTML5) mediante el puerto 443 y el servicio Bastion.

    ![Conexión de una máquina virtual](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Pasos siguientes

Lea el artículo sobre [preguntas frecuentes de Bastion](bastion-faq.md).
