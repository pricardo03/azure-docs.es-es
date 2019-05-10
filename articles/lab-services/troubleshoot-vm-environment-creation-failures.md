---
title: Solución de problemas de errores de creación de máquina virtual y el entorno de Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo solucionar problemas de máquina virtual (VM) y errores de creación del entorno de Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2019
ms.author: spelluru
ms.openlocfilehash: 7baa5e4c113e6c21c6123ac7c8399533a7dfb358
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410298"
---
# <a name="troubleshoot-virtual-machine-vm-and-environment-creation-failures-in-azure-devtest-labs"></a>Solucionar problemas de máquina virtual (VM) y errores de creación del entorno de Azure DevTest Labs
DevTest Labs proporciona advertencias si un nombre de equipo no es válido o si va a infringir las directivas de laboratorio. A veces, verá rojo `X` junto a su laboratorio de máquina virtual o entorno de estado que le informa de que algo ha ido mal.  Este artículo proporciona algunos trucos que puede usar para buscar el problema subyacente y, con suerte, evitar el problema en el futuro.

## <a name="portal-notifications"></a>Notificaciones del portal
Si utilizas el portal de Azure, el primer lugar donde mirar es el **panel notificaciones**.  Panel de las notificaciones, disponible en la barra de comandos principal haciendo clic en el **icono de campana**, le indicará si el laboratorio de máquina virtual o entorno de creación fue correcto o no.  Si se produjo un error, verá el mensaje de error asociado al error de creación. Estos detalles permiten aún más a menudo información que le ayudarán a resolver el problema. En el ejemplo siguiente, no se pudo crear la máquina virtual debido a la falta de núcleos. El mensaje detallado le enseña a corregir el problema y solicitar un aumento de cuota de núcleos.

![Notificación en Azure portal](./media/troubleshoot-vm-environment-creation-failures/portal-notification.png)


## <a name="activity-logs"></a>Registros de actividad
Examine los registros de actividad si está investigando un error en algún momento después de intentar la creación de la máquina virtual o el entorno. En esta sección se muestra cómo buscar los registros de las máquinas virtuales y entornos.

## <a name="activity-logs-for-virtual-machines"></a>Registros de actividad para máquinas virtuales

1. En la página principal para el laboratorio, seleccione la máquina virtual para iniciar el **Máquina Virtual** página.
2. En el **Máquina Virtual** página, en el **supervisión** sección del menú izquierdo, seleccione **registro de actividad** para ver todos los registros asociados con la máquina virtual.
3. En los elementos de registro de actividad, seleccione la operación con errores. Normalmente, se llama a la operación con error `Write Virtualmachines`.
4. En el panel derecho, cambie a la pestaña JSON. Consulte los detalles en la vista JSON del registro.

    ![Registro de actividad para una máquina virtual](./media/troubleshoot-vm-environment-creation-failures/vm-activity-log.png)
5. Examine el registro de JSON de hasta que encuentre el `statusMessage` propiedad. Proporciona el mensaje de error principal y la información más detallada, si procede. El siguiente JSON es un error de ejemplo para el núcleo entre comillas superado visto anteriormente en este artículo.

    ```json
    "properties": {
        "statusCode": "Conflict",
        "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"The resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"OperationNotAllowed\",\"message\":\"Operation results in exceeding quota limits of Core. Maximum allowed: 100, Current in use: 100, Additional requested: 8. Please read more about quota increase at https://aka.ms/corequotaincrease.\"}]}}",
    },
    ```

## <a name="activity-log-for-an-environment"></a>Registro de actividad para un entorno

Para ver el registro de actividad para la creación de un entorno, siga estos pasos:

1. En la página principal para el laboratorio, seleccione **configuración y directivas** en el menú izquierdo.
2. en el **configuración y directivas** página, seleccione **los registros de actividad** en el menú.
3. Busque el error en la lista de actividades en el registro y selecciónelo.
4. En el panel derecho, cambie a la pestaña JSON y busque el **mensaje de estado**.

    ![Registro de actividad de entorno](./media/troubleshoot-vm-environment-creation-failures/envirionment-activity-log.png)

## <a name="resource-manager-template-deployment-logs"></a>Registros de implementación de plantilla de Resource Manager
Si su entorno o la máquina virtual se creó mediante la automatización, hay un último lugar para buscar información de error. Es el registro de implementación de plantilla de Azure Resource Manager. Cuando se crea un recurso de laboratorio mediante la automatización, a menudo se hace a través de la implementación de una plantilla de Azure Resource Manager. Consulte[ https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates ](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates) para plantillas de Azure Resource Manager de ejemplo que crea recursos de DevTest Labs.

Para ver los registros de implementación de plantilla de laboratorio, siga estos pasos:

1. Iniciar la página del grupo de recursos en el que existe el laboratorio.
2. Seleccione **implementaciones** en el menú izquierdo bajo **configuración**.
3. Busque las implementaciones con un estado de error y selecciónelo.
4. En el **implementación** página, seleccione **detalles de la operación** vínculo para la operación con errores.
5. Ver detalles sobre la operación con error en la **detalles de la operación** ventana.

## <a name="next-steps"></a>Pasos siguientes
Consulte [solución de problemas de errores de artefactos](devtest-lab-troubleshoot-artifact-failure.md)