---
title: Configurar y aprobar el acceso just-in-time para aplicaciones administradas de Azure
description: Describe cómo los consumidores de Azure Managed Applications aprobación las solicitudes de acceso just-in-time a una aplicación administrada.
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: 55fbf7292ab894cad3de3de9e96ddc96fe0b79b3
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481720"
---
# <a name="configure-and-approve-just-in-time-access-for-azure-managed-applications"></a>Configurar y aprobar el acceso just-in-time para aplicaciones administradas de Azure

Como consumidor de una aplicación administrada, puede que no sea cómodo que proporciona el publicador acceso permanente al grupo de recursos administrados. Para obtener un mayor control sobre la concesión de acceso a los recursos administrados, Azure Managed Applications proporciona una característica denominada acceso de just-in-time (JIT), que se encuentra actualmente en versión preliminar. Permite aprobar cuándo y cuánto del publicador tiene acceso al grupo de recursos. El publicador puede hacer que las actualizaciones necesarias durante ese tiempo, pero cuando ese tiempo, expira el acceso del Editor.

El flujo de trabajo de concesión de acceso es:

1. El publicador agrega una aplicación administrada en marketplace y especifica que el acceso JIT está disponible.

1. Durante la implementación, habilita el acceso JIT para la instancia de la aplicación administrada.

1. Después de la implementación, puede cambiar la configuración para el acceso JIT.

1. El publicador envía una solicitud de acceso.

1. Apruebe la solicitud.

En este artículo se centra en las acciones que los consumidores seguir para habilitar el acceso JIT y aprobar las solicitudes. Para obtener información sobre cómo publicar una aplicación administrada con acceso JIT, consulte [solicitar acceso just-in-time en aplicaciones administradas de Azure](request-just-in-time-access.md).

> [!NOTE]
> Para usar el acceso just-in-time, debe tener un [licencia P2 de Azure Active Directory](../active-directory/privileged-identity-management/subscription-requirements.md).

## <a name="enable-during-deployment"></a>Habilitar durante la implementación

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Buscar una entrada de marketplace para una aplicación administrada con JIT habilitado. Seleccione **Crear**.

1. Al proporcionar valores para la nueva aplicación administrada, el **configuración JIT** paso le permite habilitar o deshabilitar el acceso JIT a la aplicación administrada. Seleccione **Sí** para **habilitar acceso JIT**. Esta opción está seleccionada de forma predeterminada para las aplicaciones administradas que definen con JIT habilitado en el mercado.

   ![Configurar el acceso](./media/approve-just-in-time-access/configure-jit-access.png)

   Solo puede habilitar el acceso JIT durante la implementación. Si selecciona **No**, los anunciantes obtienen acceso permanente al grupo de recursos administrados. No se puede habilitar acceso JIT más adelante.

1. Para cambiar la configuración de aprobación de forma predeterminada, seleccione **Personalizar configuración JIT**.

   ![Personalizar el acceso](./media/approve-just-in-time-access/customize-jit-access.png)

   De forma predeterminada, una aplicación administrada con JIT habilitado tiene las siguientes opciones:

   * Modo de aprobación: automática
   * Duración máxima de acceso: 8 horas
   * Aprobadores: ninguno

   Cuando se establece el modo de aprobación en **automática**, los aprobadores reciben una notificación para cada solicitud, pero la solicitud se aprueba automáticamente. Cuando se establece en **manual**, los aprobadores reciben una notificación para cada solicitud, y uno de ellos debe aprobarlo.

   La duración máxima de activación especifica la cantidad máxima de tiempo que puede solicitar un publicador para el acceso al grupo de recursos administrados.

   La lista de aprobadores es que los usuarios de Azure Active Directory que se pueden aprobar las solicitudes de acceso JIT. Para agregar un aprobador, seleccione **agregar aprobador** y busque el usuario.

   Después de actualizar la configuración, seleccione **guardar**.

## <a name="update-after-deployment"></a>Actualizar después de la implementación

Puede cambiar los valores de cómo se aprueban las solicitudes. Sin embargo, si no habilita el acceso JIT durante la implementación, no puede habilitarlo más adelante.

Para cambiar la configuración de una aplicación administrada implementada:

1. En el portal, seleccione la aplicación de administración.

1. Seleccione **configuración JIT** y cambiar la configuración según sea necesario.

   ![Cambiar la configuración de acceso](./media/approve-just-in-time-access/change-settings.png)

1. Cuando haya terminado, seleccione **Guardar**.

## <a name="approve-requests"></a>Aprobar solicitudes

Cuando el publicador solicita acceso, se le notificará de la solicitud. Puede aprobar las solicitudes de acceso JIT directamente a través de la aplicación administrada, o a través de todas las aplicaciones administradas a través del servicio de Azure AD Privileged Identity Management. Para usar el acceso just-in-time, debe tener un [licencia P2 de Azure Active Directory](../active-directory/privileged-identity-management/subscription-requirements.md).

Para aprobar las solicitudes a través de la aplicación administrada:

1. Seleccione **acceso JIT** para la aplicación administrada y seleccione **aprobar solicitudes**.

   ![Aprobar solicitudes](./media/approve-just-in-time-access/approve-requests.png)
 
1. Seleccione la solicitud para aprobar.

   ![Seleccione la solicitud](./media/approve-just-in-time-access/select-request.png)

1. En el formulario, proporcione la razón para la aprobación y seleccione **aprobar**.

Para aprobar las solicitudes a través de Azure AD Privileged Identity Management:

1. Seleccione **todos los servicios** y empezar a buscar **Azure AD Privileged Identity Management**. Selecciónelo entre las opciones disponibles.

   ![Busque service](./media/approve-just-in-time-access/search.png)

1. Seleccione **aprobar solicitudes**.

   ![Seleccione aprobar solicitudes](./media/approve-just-in-time-access/select-approve-requests.png)

1. Seleccione **aplicaciones administradas de Azure**y seleccione la solicitud para aprobar.

   ![Seleccione las solicitudes](./media/approve-just-in-time-access/view-requests.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo publicar una aplicación administrada con acceso JIT, consulte [solicitar acceso just-in-time en aplicaciones administradas de Azure](request-just-in-time-access.md).
