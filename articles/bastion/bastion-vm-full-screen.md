---
title: Cambio de la vista de sesión de máquina virtual a pantalla completa en Azure Bastion | Microsoft Docs
description: En este artículo, aprenda a cambiar la vista a pantalla completa.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 6a063d6c8891133126924bfb934770f7818db71a
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191255"
---
# <a name="change-to-full-screen-view-for-a-vm-session-azure-bastion-preview"></a>Cambio a la vista de pantalla completa para una sesión de máquina virtual: Azure Bastion (versión preliminar)

Este artículo le ayuda a cambiar la vista de la máquina virtual a pantalla completa y de nuevo en el explorador. Antes de trabajar con una máquina virtual, asegúrese de que ha seguido los pasos indicados en [Creación de un host de Bastion](bastion-create-host-portal.md). Después, conéctese a la máquina virtual con la que quiere trabajar mediante [RDP](bastion-connect-vm-rdp.md) o [SSH](bastion-connect-vm-ssh.md).

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="launch-the-clipboard-tool"></a>Inicie la herramienta del Portapapeles

Durante la sesión remota, inicie la paleta de herramientas de acceso al Portapapeles de Bastion seleccionando las dos flechas que se encuentran en la parte central izquierda de la sesión.

![herramientas](./media/bastion-vm-manage/left.png)

## <a name="select-full-screen"></a>Seleccionar la pantalla completa

Seleccione el botón **FullScreen** para cambiar la sesión a una experiencia de pantalla completa. Cuando cambie, la sesión se reiniciará en pantalla completa.

![pantalla completa](./media/bastion-vm-manage/full-screen.png)
 
## <a name="next-steps"></a>Pasos siguientes

Lea el artículo sobre [preguntas frecuentes de Bastion](bastion-faq.md).
Aprenda a [copiar y pegar](bastion-vm-copy-paste.md) entre máquinas virtuales de Azure.