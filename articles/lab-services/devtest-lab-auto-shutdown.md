---
title: Administración de directivas de parada automática de máquinas en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo establecer la directiva de parada automática de máquinas para un laboratorio para que las máquinas virtuales se cierran automáticamente cuando no están en uso.
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
ms.date: 05/17/2019
ms.author: spelluru
ms.openlocfilehash: 9adf8dd4a5a3c469ed130b29308a0d828aee40bf
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873996"
---
# <a name="manage-autoshutdown-policies-for-a-lab-in-azure-devtest-labs"></a>Administración de directivas de parada automática de máquinas en un laboratorio en Azure DevTest Labs
Azure DevTest Labs permite controlar los costos y desperdiciar lo mínimo posible en sus laboratorios gracias a la posibilidad de administrar políticas (configuración) en cada uno de ellos. Este artículo muestra cómo configurar la directiva de parada automática de máquinas para una cuenta de laboratorio y configure las opciones de desconexión automática para un laboratorio en la cuenta de laboratorio. Para ver cómo establecer cada una de las directivas de laboratorio, vea [Definición de directivas de laboratorio en Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="set-auto-shutdown-policy-for-a-lab"></a>Establecer directiva de apagado automático para un laboratorio
Como propietario del laboratorio, puede configurar una programación de apagado para todas las máquinas virtuales de su laboratorio. Al hacerlo, puede ahorrar costos derivados de la ejecución de máquinas que no se usan (inactivas). Puede aplicar una directiva de apagado en todas las máquinas del laboratorio a nivel central, pero también ahorrarles a los usuarios del laboratorio el trabajo de configurar una programación para sus máquinas individuales. Esta característica le permite definir la directiva en la programación del laboratorio: desde no ofrecer ningún control a ofrecer control completo a los usuarios del laboratorio. Como propietario del laboratorio, puede configurar esta directiva mediante los pasos siguientes:

1. En la página principal del laboratorio, seleccione **Configuration and policies** (Configuración y directivas).
2. Seleccione **Auto shutdown policy** (Directiva de apagado automático) en la sección **Schedules** (Programaciones) del menú izquierdo.
3. Seleccione una de las opciones. Las secciones siguientes proporcionan más detalles sobre estas opciones: La directiva establecida se aplica solo a nuevas máquinas virtuales creadas en el laboratorio y no a las máquinas virtuales ya existentes. 

    ![Automáticamente cierra correctamente las opciones de directiva](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

## <a name="configure-auto-shutdown-settings"></a>Configurar opciones de apagado automático
La directiva de parada automática de máquinas ayuda a minimizar la pérdida del laboratorio, ya que permite especificar el tiempo que apagan las máquinas virtuales de este laboratorio.

Para ver (y cambiar) las directivas de un laboratorio, siga estos pasos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
3. En la lista de laboratorios, seleccione el laboratorio que desee.   
4. Seleccione **Configuration and policies** (Directivas y configuración).

    ![Panel de configuración de directivas](./media/devtest-lab-set-lab-policy/policies-menu.png)
5. En el laboratorio **configuración y directivas** panel, seleccione **apagado automático** en **programaciones**.
   
    ![Parada automática de máquinas](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
6. Seleccione **Activado** para habilitar esta directiva, y **Desactivado** para deshabilitarla.
7. Si habilita esta directiva, especifique la hora local (y la zona horaria) para apagar todas las máquinas virtuales del laboratorio actual.
8. Especificar **Sí** o **No** para la opción Enviar una notificación 15 minutos anterior a la hora de parada automática de máquinas especificada. Si elige **Sí**, escriba un punto de conexión de URL del webhook o una dirección de correo electrónico para especificar dónde quiere que se publique o se envíe la notificación. El usuario recibe la notificación y se le ofrece la opción de retrasar el apagado. Para obtener más información, consulte el [notificaciones](#notifications) sección. 
9. Seleccione **Guardar**.

    De manera predeterminada, una vez que se habilite, esta directiva se aplica a todas las máquinas virtuales del laboratorio actual. Para quitar esta configuración de una máquina virtual específica, abra Panel de administración de la máquina virtual y cambie su **parada automática de máquinas** configuración.

### <a name="user-sets-a-schedule-and-can-opt-out"></a>El usuario establece una programación y puede excluirse de ella
Si establece esta directiva para el laboratorio, los usuarios del laboratorio pueden excluirse de la programación del laboratorio o invalidarla. Esta opción concede a los usuarios del laboratorio control completo sobre la programación del apagado automático de sus máquinas virtuales. Los usuarios del laboratorio no verán ningún cambio en su página de programación de apagado de la máquina virtual.

![Auto apagar la opción de directiva - 1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>El usuario establece una programación y no puede excluirse de ella
Si establece esta directiva para el laboratorio, los usuarios del laboratorio pueden invalidar la programación del laboratorio. Sin embargo, no se pueden excluir de la directiva de apagado automático. Esta opción garantiza que todas las máquinas del laboratorio están bajo una programación de apagado automático. Los usuarios del laboratorio pueden actualizar la programación de apagado automático de sus máquinas virtuales y configurar notificaciones de apagado.

![Auto apagar la opción de directiva - 2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>El usuario no tiene control sobre la programación establecida por el administrador del laboratorio
Si establece esta directiva para el laboratorio, los usuarios del laboratorio no pueden excluirse de la programación del laboratorio ni invalidarla. Esta opción ofrece al administrador del laboratorio control total sobre la programación de cada máquina del laboratorio. Los usuarios del laboratorio solo pueden configurar notificaciones de apagado automático para sus máquinas virtuales.

![Auto apagar la opción de directiva - 3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="notifications"></a>Notificaciones
Una vez establecido por el propietario del laboratorio de desconexión automática, las notificaciones se enviarán a los usuarios del laboratorio 15 minutos antes de la parada automática de máquinas desencadenada si cualquiera de sus máquinas virtuales se verán afectadas. Esta opción proporciona una oportunidad de guardar su trabajo antes del apagado de los usuarios del laboratorio. La notificación también proporciona vínculos para cada máquina virtual para las siguientes acciones:

- Omitir la parada automática de máquinas de este momento
- Posponer la parada automática de máquinas para una hora o 2 horas, para que puedan seguir trabajando en la máquina virtual.

Se envía una notificación a través del extremo de enlace web configurado o una dirección de correo electrónico especificada por los propietarios de laboratorio en la configuración de parada automática de máquinas. Los Webhooks permiten crear o configurar integraciones que se suscriben a determinados eventos. Cuando se desencadene uno de esos eventos, DevTest Labs le enviará una carga de HTTP POST a la URL configurada del webhook. Para obtener más información sobre los webhooks, consulte [Creación de un webhook o una función de API de Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

Se recomienda usar enlaces web porque ampliamente es compatible con varias aplicaciones (por ejemplo, Slack, Azure Logic Apps y así sucesivamente.) y le permite implementar su propia manera para enviar notificaciones. Por ejemplo, este artículo le guiará a través de cómo obtener notificaciones de parada automática de máquinas de los correos electrónicos con Azure Logic Apps. En primer lugar, vamos a ir rápidamente a través de los pasos básicos para habilitar la notificación de parada automática de máquinas en el laboratorio.   

## <a name="create-a-logic-app-that-receives-email-notifications"></a>Crear una aplicación lógica que recibe notificaciones de correo electrónico
[Azure Logic Apps](../logic-apps/logic-apps-overview.md) proporciona muchos conectores de fábrica que simplifica la integración de un servicio con otros clientes, como Office 365 y twitter. En el nivel alto, los pasos para configurar una aplicación lógica para la notificación por correo electrónico pueden dividirse en cuatro fases: 

- Cree una aplicación lógica. 
- Configurar la plantilla integrada.
- Integrar con su cliente de correo electrónico
- Obtenga la dirección URL del Webhook.

### <a name="create-a-logic-app"></a>Creación de una aplicación lógica
Para empezar, cree una aplicación lógica en su suscripción de Azure mediante los pasos siguientes:

1. Seleccione **+ crear un recurso** en el menú izquierdo, seleccione **integración**y seleccione **aplicación lógica**. 

    ![Nuevo menú de la aplicación lógica](./media/devtest-lab-auto-shutdown/new-logic-app.png)
2. En el **aplicación lógica: creación de** página, siga estos pasos: 
    1. Escriba un **nombre** para la aplicación lógica.
    2. Selección la **suscripción**de Azure.
    3. Cree un **grupo de recursos** nuevo o seleccione uno existente. 
    4. Seleccione un **ubicación** para la aplicación lógica. 

        ![Nueva aplicación lógica - configuración](./media/devtest-lab-auto-shutdown/new-logic-app-page.png)
3. En el **notificaciones**, seleccione **ir al recurso** en la notificación. 

    ![Ir al recurso](./media/devtest-lab-auto-shutdown/go-to-resource.png)
4. Seleccione **Diseñador de aplicaciones lógicas** en **herramientas de implementación** categoría.

    ![Seleccione la solicitud/respuesta HTTP](./media/devtest-lab-auto-shutdown/select-http-request-response-option.png)
5. En el **solicitud-respuesta HTTP** página, seleccione **usar esta plantilla**. 

    ![Seleccione usar esta opción de plantilla](./media/devtest-lab-auto-shutdown/select-use-this-template.png)
6. Copie el siguiente JSON en el **esquema de JSON de cuerpo de solicitud** sección: 

    ```json
    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "properties": {
            "delayUrl120": {
                "type": "string"
            },
            "delayUrl60": {
                "type": "string"
            },
            "eventType": {
                "type": "string"
            },
            "guid": {
                "type": "string"
            },
            "labName": {
                "type": "string"
            },
            "owner": {
                "type": "string"
            },
            "resourceGroupName": {
                "type": "string"
            },
            "skipUrl": {
                "type": "string"
            },
            "subscriptionId": {
                "type": "string"
            },
            "text": {
                "type": "string"
            },
            "vmName": {
                "type": "string"
            }
        },
        "required": [
            "skipUrl",
            "delayUrl60",
            "delayUrl120",
            "vmName",
            "guid",
            "owner",
            "eventType",
            "text",
            "subscriptionId",
            "resourceGroupName",
            "labName"
        ],
        "type": "object"
    }
    ```
    
    ![Esquema JSON de cuerpo de solicitud](./media/devtest-lab-auto-shutdown/request-json.png)
7. Seleccione **+ nuevo paso** en el diseñador y siga estos pasos:
    1. Busque **Office 365 Outlook - enviar un correo electrónico**. 
    2. Seleccione **enviar un correo electrónico** desde **acciones**. 
    
        ![Enviar la opción de correo electrónico](./media/devtest-lab-auto-shutdown/select-send-email.png)
    3. Seleccione **inicie sesión en** para iniciar sesión en su cuenta de correo electrónico. 
    4. Seleccione **TO** campo y seleccione el propietario.
    5. Seleccione **asunto**y un asunto de la notificación de correo electrónico de entrada. Por ejemplo: "Apagado de máquina vmName para laboratorio: labName."
    6. Seleccione **cuerpo**y definir el contenido del cuerpo de notificación por correo electrónico. Por ejemplo: "vmName está programada para apagarse en 15 minutos. Omitir este apagado, haga clic en: DIRECCIÓN URL. Apagado de retraso para una hora: delayUrl60. Apagado de retraso de 2 horas: delayUrl120. "

        ![Esquema JSON de cuerpo de solicitud](./media/devtest-lab-auto-shutdown/email-options.png)
1. Seleccione **Guardar** en la barra de herramientas. Ahora, puede copiar el **URL de HTTP POST**. Seleccione el botón Copiar para copiar la dirección URL en el Portapapeles. 

    ![Dirección URL del WebHook](./media/devtest-lab-auto-shutdown/webhook-url.png)

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre cómo establecer todas las directivas, consulte [definir directivas de laboratorio en Azure DevTest Labs](devtest-lab-set-lab-policy.md).
