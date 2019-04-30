---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 1c3996c3f40da496af0cd795d0873864667a1f19
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104059"
---
## <a name="use-the-azure-portal"></a>Uso de Azure Portal
1. Seleccione la máquina virtual que quiera volver a implementar y el botón *Volver a implementar* en la hoja *Configuración*. Es posible que deba bajar para ver la sección **Soporte y solución de problemas** que contiene el botón Volver a implementar, como en el ejemplo siguiente:

    ![Hoja Máquina virtual de Azure](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. Para confirmar la operación, haga clic en el botón *Volver a implementar*:

    ![Hoja Volver a implementar una máquina virtual](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. El valor de **Estado** de la máquina virtual cambia a *Actualizando*, puesto que la máquina virtual se prepara para implementarse de nuevo, como se muestra en el siguiente ejemplo:

    ![Máquina virtual actualizando](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. El valor de **Estado** pasa luego a *Iniciando* cuando la máquina virtual se inicia en un nuevo host de Azure, como se muestra en el ejemplo siguiente:

    ![Máquina virtual iniciando](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. Cuando la máquina virtual finaliza el proceso de arranque, el **estado** vuelve a *En ejecución*, lo que indica que la máquina virtual se ha vuelto a implementar correctamente:

    ![Máquina virtual en ejecución](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

<!-- Update_Description: update meta properties -->