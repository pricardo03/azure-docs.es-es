---
title: 'Copiado y pegado entre máquinas virtuales: Azure Bastion | Microsoft Docs'
description: Aprenda con este artículo a copiar y pegar entre máquinas virtuales de Azure al usar Bastion.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 9d69d1a9fae258c9546a8c794fc0b0c68b952049
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191335"
---
# <a name="copy-and-paste-to-a-virtual-machine-azure-bastion-preview"></a>Copiado y pegado entre máquinas virtuales: Azure Bastion (versión preliminar)

Este artículo le ayuda a copiar y pegar texto entre máquinas virtuales cuando se usa Azure Bastion. Antes de trabajar con una máquina virtual, asegúrese de que ha seguido los pasos indicados en [Creación de un host de Bastion](bastion-create-host-portal.md). Después, conéctese a la máquina virtual con la que quiere trabajar mediante [RDP](bastion-connect-vm-rdp.md) o [SSH](bastion-connect-vm-ssh.md).

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

En exploradores que admiten el acceso avanzado de la API de Portapapeles, puede copiar y pegar texto entre el dispositivo local y la sesión remota de la misma manera que copia y pega entre aplicaciones en el dispositivo local. En otros exploradores, puede utilizar la paleta de herramientas de acceso al Portapapeles de Bastion.

  ![Autorización del Portapapeles](./media/bastion-vm-manage/allow.png)

Solo se admiten operaciones de copiado y pegado de texto. En operaciones de copiado y pegado directo, es posible que el explorador le pida acceso al Portapapeles al inicializar la sesión de Bastion. Pulse **Permitir** para que la página web puede acceder al Portapapeles.

## <a name="to"></a>Copiado en una sesión remota

Después de conectarse a la máquina virtual con [Azure Portal](https://aka.ms/BastionHost) para obtener la versión preliminar de Bastion:

1. Copie el texto/contenido del dispositivo local al Portapapeles local.
1. Durante la sesión remota, inicie la paleta de herramientas de acceso al Portapapeles de Bastion seleccionando las dos flechas. Las flechas se encuentran en la parte central izquierda de la sesión.

    ![paleta de herramientas](./media/bastion-vm-manage/left.png)

    ![Portapapeles](./media/bastion-vm-manage/clipboard.png)

1. Normalmente, el texto copiado se muestra automáticamente en la paleta de copiado y pegado de Bastion. Si el texto no se encuentra allí, péguelo en el área de texto de la paleta.
1. Cuando el texto esté en el área de texto, podrá pegarlo en la sesión remota.

    ![pegar](./media/bastion-vm-manage/local.png)

## <a name="from"></a>Copiado desde una sesión remota

Después de conectarse a la máquina virtual con [Azure Portal](https://aka.ms/BastionHost) para obtener la versión preliminar de Bastion:

1. Copie el texto/contenido de la sesión remota al Portapapeles remoto (con Ctrl-C).

    ![paleta de herramientas](./media/bastion-vm-manage/remote.png)

1. Durante la sesión remota, inicie la paleta de herramientas de acceso al Portapapeles de Bastion seleccionando las dos flechas. Las flechas se encuentran en la parte central izquierda de la sesión.

    ![Portapapeles](./media/bastion-vm-manage/clipboard2.png)

1. Normalmente, el texto copiado se muestra automáticamente en la paleta de copiado y pegado de Bastion. Si el texto no se encuentra allí, péguelo en el área de texto de la paleta.
1. Cuando el texto esté en el área de texto, podrá pegarlo en el dispositivo local.

    ![pegar](./media/bastion-vm-manage/local2.png)
 
## <a name="next-steps"></a>Pasos siguientes

Lea el artículo sobre [preguntas frecuentes de Bastion](bastion-faq.md).