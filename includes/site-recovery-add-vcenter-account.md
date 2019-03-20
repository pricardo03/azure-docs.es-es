---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: cd18d71d26410767a2d3119c12a1339bdc84bd33
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58115428"
---
1. En el servidor de configuración, inicie la herramienta CSPSConfigtool.exe. Esta herramienta está disponible como acceso directo en el escritorio y se encuentra en la carpeta *ubicación de instalación*\home\svsystems\bin.
2. Haga clic en **Administrar cuentas** > **Agregar cuenta**.

    ![Agregar cuenta](./media/site-recovery-add-vcenter-account/credentials1.png)
3. En **Detalles de la cuenta**, agregue la cuenta que se utilizará para la detección automática.

    ![Detalles](./media/site-recovery-add-vcenter-account/credentials2.png)

    > [!Note]
   > El nombre de la cuenta podría tardar 15 minutos o más en aparecer en el portal. Para que se actualice inmediatamente, haga clic en **Servidores de configuración** > ***nombre de servidor*** > **Actualizar servidor**.
