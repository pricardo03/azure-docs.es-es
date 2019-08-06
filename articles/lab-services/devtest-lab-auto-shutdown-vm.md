---
title: Configuración del apagado automático de una máquina virtual en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo configurar el apagado automático para una máquina virtual (VM) de modo que está se apague automáticamente cuando no se esté usando.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2019
ms.author: spelluru
ms.openlocfilehash: 934e8fd71c901c89f328c777103a8cb39bf21ac4
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68361276"
---
# <a name="configure-autoshutdown-settings-for-a-vm-in-azure-devtest-labs"></a>Configuración del apagado automático de una máquina virtual en Azure DevTest Labs
Azure DevTest Labs permite controlar los costos y desperdiciar lo mínimo posible en sus laboratorios gracias a la posibilidad de administrar políticas (configuración) en cada uno de ellos. En este artículo le mostramos cómo configurar la directiva de apagado automático para una cuenta de laboratorio y cómo configurar el apagado automático para un laboratorio de la cuenta de laboratorio. Para ver cómo establecer cada una de las directivas de laboratorio, vea [Definición de directivas de laboratorio en Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="autoshutdown-policy-for-a-lab"></a>Directiva de apagado automático para un laboratorio
Como propietario del laboratorio, puede configurar una programación de apagado para todas las máquinas virtuales de su laboratorio. Al hacerlo, puede ahorrar costos derivados de la ejecución de máquinas que no se usan (inactivas). Puede aplicar una directiva de apagado en todas las máquinas del laboratorio a nivel central, pero también ahorrarles a los usuarios del laboratorio el trabajo de configurar una programación para sus máquinas individuales. Esta característica le permite definir la directiva en la programación del laboratorio: desde no ofrecer ningún control a ofrecer control completo a los usuarios del laboratorio. Como propietario del laboratorio, puede configurar esta directiva mediante los pasos siguientes:

1. En la página principal del laboratorio, seleccione **Configuration and policies** (Configuración y directivas).
2. Seleccione **Auto shutdown policy** (Directiva de apagado automático) en la sección **Schedules** (Programaciones) del menú izquierdo.
3. Seleccione una de las opciones. En las siguientes secciones, se proporcionan más detalles de estas opciones: La directiva establecida se aplica solo a las nuevas máquinas virtuales creadas en el laboratorio y no a las máquinas virtuales ya existentes. 

    ![Opciones de la directiva de apagado automático](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

## <a name="configure-autoshutdown-settings-for-a-lab"></a>Configuración del apagado automático para un laboratorio
La directiva de apagado automático le ayuda a minimizar la pérdida del laboratorio, ya que permite especificar la hora de apagado de la máquina virtual de este laboratorio.

Para ver (y cambiar) las directivas de un laboratorio, siga estos pasos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
3. En la lista de laboratorios, seleccione el laboratorio que desee.   
4. Seleccione **Configuration and policies** (Directivas y configuración).

    ![Panel de configuración de directivas](./media/devtest-lab-set-lab-policy/policies-menu.png)
5. En el panel **Configuration and policies** (Configuración y directivas) del laboratorio, seleccione **Auto shutdown** (Apagado automático) en **Schedules** (Programaciones).
   
    ![Apagado automático](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
6. Seleccione **Activado** para habilitar esta directiva, y **Desactivado** para deshabilitarla.
7. Si habilita esta directiva, especifique la hora local (y la zona horaria) para apagar todas las máquinas virtuales del laboratorio actual.
8. Especifique **Sí** o **No** en la opción de enviar una notificación 15 minutos antes de la hora especificada para el apagado automático. Si elige **Sí**, escriba un punto de conexión de URL del webhook o una dirección de correo electrónico para especificar dónde quiere que se publique o se envíe la notificación. El usuario recibe la notificación y se le ofrece la opción de retrasar el apagado. Para obtener más información, consulte la sección [Notificaciones](#notifications). 
9. Seleccione **Guardar**.

    De manera predeterminada, una vez que se habilite, esta directiva se aplica a todas las máquinas virtuales del laboratorio actual. Para quitar esta configuración de una máquina virtual específica, abra el panel de administración de la máquina virtual y cambie la configuración del **apagado automático**.

## <a name="configure-autoshutdown-settings-for-a-vm"></a>Configuración del apagado automático para una máquina virtual

### <a name="user-sets-a-schedule-and-can-opt-out"></a>El usuario establece una programación y puede excluirse de ella
Si se establece esta opción de directiva para el laboratorio, los usuarios pueden invalidar la programación del laboratorio o no participar de ella. Esta opción concede a los usuarios del laboratorio control completo sobre la programación del apagado automático de sus máquinas virtuales. Los usuarios del laboratorio no verán ningún cambio en su página de programación de apagado de la máquina virtual.

![Opciones de la directiva de apagado automático: 1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>El usuario establece una programación y no puede excluirse de ella
Si se establece esta opción de directiva para el laboratorio, los usuarios pueden invalidar la programación del laboratorio. Sin embargo, no se pueden excluir de la directiva de apagado automático. Esta opción garantiza que todas las máquinas del laboratorio están bajo una programación de apagado automático. Los usuarios del laboratorio pueden actualizar la programación de apagado automático de sus máquinas virtuales y configurar notificaciones de apagado.

![Opciones de la directiva de apagado automático: 2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>El usuario no tiene control sobre la programación establecida por el administrador del laboratorio
Si se establece esta opción de directiva para el laboratorio, los usuarios no pueden invalidar la programación del laboratorio ni dejar de participar de ella. Esta opción ofrece al administrador del laboratorio control total sobre la programación de cada máquina del laboratorio. Los usuarios del laboratorio solo pueden configurar notificaciones de apagado automático para sus máquinas virtuales.

![Opciones de la directiva de apagado automático: 3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="notifications"></a>Notificaciones
Una vez que el propietario del laboratorio haya configurado el apagado automático, las notificaciones se enviarán a los usuarios del laboratorio 15 minutos antes de que se active el apagado automático si alguna de sus VM se ve afectada. Esta opción proporciona a los usuarios del laboratorio la oportunidad de guardar su trabajo antes del apagado. La notificación también les proporciona vínculos para cada VM para que puedan realizar las siguientes acciones:

- Omitir el apagado automático de ese momento
- Posponer el apagado automático durante una o dos horas, para que puedan seguir trabajando en la VM.

La notificación se envía a través del punto de conexión del webhook configurado o mediante una dirección de correo electrónico que especificaron los propietarios del laboratorio en la configuración del apagado automático. Los webhooks le permiten crear o configurar integraciones que se suscriben a ciertos eventos. Cuando se activa uno de esos eventos, DevTest Labs enviará una carga HTTP POST a la dirección URL configurada del webhook. Para obtener más información sobre los webhooks, consulte [Creación de un webhook o una función de API de Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

Le recomendamos que use webhooks porque son ampliamente compatibles con varias aplicaciones (por ejemplo, Slack, Azure Logic Apps, etc.) y le permiten implementar su propio método para enviar notificaciones. Para ver un ejemplo de cómo recibir la notificación de apagado automático desde correos electrónicos con Azure Logic Apps, consulte [Crear una aplicación lógica que reciba notificaciones por correo electrónico](devtest-lab-auto-shutdown.md#create-a-logic-app-that-receives-email-notifications). 


## <a name="next-steps"></a>Pasos siguientes
Consulte [Administración de las directivas de apagado automático para un laboratorio en Azure DevTest Labs](devtest-lab-auto-shutdown.md).
