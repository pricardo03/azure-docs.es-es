---
title: Habilitar y solicitar acceso just-in-time para aplicaciones administradas de Azure
description: Describe cómo los editores de aplicaciones administradas de Azure solicitan acceso just-in-time a una aplicación administrada.
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: ea933f5382cb42c01de523326b094c1813401132
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481780"
---
# <a name="enable-and-request-just-in-time-access-for-azure-managed-applications"></a>Habilitar y solicitar acceso just-in-time para aplicaciones administradas de Azure

Los consumidores de la aplicación administrada pueden ser reacios a conceder acceso permanente al grupo de recursos administrados. Como publicador de una aplicación de administrador, es preferible que los consumidores saben exactamente cuando lo necesita tener acceso a los recursos administrados. Para ofrecer a los clientes mayor control sobre la concesión de acceso a los recursos administrados, Azure Managed Applications proporciona una característica denominada acceso de just-in-time (JIT), que se encuentra actualmente en versión preliminar.

Acceso JIT le permite solicitar acceso con privilegios elevados a recursos de la aplicación administrada para solucionar problemas o mantenimiento. Siempre tendrá acceso de solo lectura a los recursos, pero puede tener mayor acceso durante un período de tiempo específico.

El flujo de trabajo de concesión de acceso es:

1. Agregar una aplicación administrada en marketplace y especificar que el acceso JIT está disponible.

1. Durante la implementación, el consumidor permite el acceso JIT a esa instancia de la aplicación administrada.

1. Después de la implementación, el consumidor puede cambiar la configuración para el acceso JIT.

1. Enviar una solicitud de acceso cuando se necesita solucionar problemas o actualizar los recursos administrados.

1. El consumidor aprueba su solicitud.

En este artículo se centra en las acciones que realizar los publicadores para habilitar el acceso JIT y enviar las solicitudes. Para obtener información acerca de la aprobación de solicitudes de acceso JIT, consulte [aprobar el acceso just-in-time en aplicaciones administradas de Azure](approve-just-in-time-access.md).

## <a name="add-jit-access-step-to-ui"></a>Agregar paso de acceso JIT a la interfaz de usuario

El archivo CreateUiDefinition.json es exactamente igual que el archivo de interfaz de usuario que cree para el acceso permanente, excepto que debe incluir un paso que permite a los consumidores habilitar acceso JIT. Para más información sobre la publicación de su primera aplicación administrada que ofrece en Azure Marketplace, consulte [aplicaciones administradas de Azure en Marketplace](publish-marketplace-app.md).

Para admitir la funcionalidad JIT de la oferta, agregue el siguiente contenido al archivo CreateUiDefinition.json:

En "pasos":

```json
{
    "name": "jitConfiguration",
    "label": "JIT Configuration",
    "subLabel": {
        "preValidation": "Configure JIT settings for your application",
        "postValidation": "Done"
    },
    "bladeTitle": "JIT Configuration",
    "elements": [
        {
          "name": "jitConfigurationControl",
          "type": "Microsoft.Solutions.JitConfigurator",
          "label": "JIT Configuration"
        }
    ]
}
```
 
En "outputs":

```json
"jitAccessPolicy": "[parse(concat('{\"jitAccessEnabled\":', string(steps('jitConfiguration').jitConfigurationControl.jitEnabled), ',\"jitApprovalMode\":\"', steps('jitConfiguration').jitConfigurationControl.jitApprovalMode, '\",\"maximumJitAccessDuration\":\"', steps('jitConfiguration').jitConfigurationControl.maxAccessDuration, '\",\"jitApprovers\":', string(steps('jitConfiguration').jitConfigurationControl.approvers), '}'))]"
```

> [!NOTE]
> Acceso JIT está en versión preliminar. El esquema de configuración de JIT podría cambiar en futuras iteraciones.

## <a name="enable-jit-access"></a>Habilitar acceso JIT

Al definir su oferta en marketplace, asegúrese de que habilitar acceso JIT.

1. Inicie sesión en el [Cloud Partner portal de publicación](https://cloudpartner.azure.com).

1. Proporcione valores para publicar la aplicación administrada en marketplace. ¿Seleccione **Sí** para **habilitar acceso JIT?**

   ![Habilitar acceso Just-In-Time](./media/request-just-in-time-access/marketplace-enable.png)

Se ha agregado un paso de configuración JIT a la interfaz de usuario y se ha habilitado el acceso JIT en la oferta de marketplace. Cuando los consumidores de implementación la aplicación administrada, pueden [activar el acceso JIT para su instancia](approve-just-in-time-access.md#enable-during-deployment).

## <a name="request-access"></a>Solicitar acceso

Cuando necesite tener acceso a los recursos administrados del consumidor, enviar una solicitud para un rol específico, el tiempo y la duración. El consumidor, a continuación, debe aprobar la solicitud.

Para enviar una solicitud de acceso JIT:

1. Seleccione **acceso JIT** para la aplicación administrada que necesite tener acceso.

1. Seleccione **Roles elegibles**y seleccione **activar** en la columna de acción para el rol que desea.

   ![Activar la solicitud de acceso](./media/request-just-in-time-access/send-request.png)

1. En el **activar rol** formulario, seleccione una hora de inicio y la duración de su rol para que estén activos. Seleccione **Activate** para enviar la solicitud.

   ![Activar acceso](./media/request-just-in-time-access/activate-access.png) 

1. Ver las notificaciones para ver que la nueva solicitud JIT se ha enviado correctamente al consumidor.

   ![Notification](./media/request-just-in-time-access/in-progress.png)

   Ahora, debe esperar para que el consumidor [aprobar su solicitud](approve-just-in-time-access.md#approve-requests).

1. Para ver el estado de todas las solicitudes JIT para una aplicación administrada, seleccione **acceso JIT** y **historial solicitar**.

   ![Ver estado](./media/request-just-in-time-access/view-status.png)

## <a name="known-issues"></a>Problemas conocidos

El identificador de entidad de seguridad de la cuenta que solicita acceso JIT debe incluirse explícitamente en la definición de aplicación administrada. La cuenta no puede incluirse solo a través de un grupo que se especifica en el paquete. Esta limitación se corregirá en una versión futura.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de la aprobación de solicitudes para el acceso JIT, consulte [aprobar el acceso just-in-time en aplicaciones administradas de Azure](approve-just-in-time-access.md).
