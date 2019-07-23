---
title: 'Solicitud de acceso a un paquete de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo usar el portal Mi acceso para solicitar acceso a un paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/19/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39a50240b4360c5b4adcd6020c2b80b0f06315f7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64541561"
---
# <a name="request-access-to-an-access-package-in-azure-ad-entitlement-management-preview"></a>Solicitud de acceso a un paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="sign-in-to-the-my-access-portal"></a>Inicio de sesión en el portal Mi acceso

El primer paso es iniciar sesión en el portal Mi acceso, donde puede solicitar acceso a un paquete de acceso.

**Rol necesario:** Solicitante

1. Busque un correo electrónico o un mensaje del jefe de proyecto o el director comercial con el que trabaja. El correo electrónico debe incluir un vínculo al paquete acceso al que necesitará acceso. El vínculo comenzará por:

    `https://myaccess.microsoft.com`

1. Abra el vínculo.

1. Inicie sesión en el portal Mi acceso.

    Asegúrese de usar la cuenta de su organización. Si no está seguro, póngase en contacto con el jefe de proyecto o el director comercial.

## <a name="request-an-access-package"></a>Solicitud de un paquete de acceso

Una vez que haya encontrado el paquete de acceso en el portal Mi acceso, puede enviar una solicitud.

**Rol necesario:** Solicitante

1. Haga clic en la marca de verificación para seleccionar el paquete de acceso.

    ![Portal Mi acceso: paquetes de acceso](./media/entitlement-management-shared/my-access-access-packages.png)

1. Haga clic en **Solicitar acceso** para abrir el panel Solicitar acceso.

1. Si se muestra el cuadro **Justificación comercial**, escriba una justificación para necesitar acceso.

1. Si la opción **¿Se trata de una solicitud para un período específico?** está habilitada, seleccione **Sí** o **No**.

1. Si es necesario, especifique las fechas de inicio y finalización.

    ![Portal Mi acceso: solicitud de acceso](./media/entitlement-management-shared/my-access-request-access.png)

1. Cuando termine, haga clic en **Enviar** para enviar la solicitud.

1. Haga clic en **Historial de solicitudes** para ver una lista de sus solicitudes con el estado.

    Si el paquete de acceso requiere aprobación, la solicitud se encuentra en estado pendiente de aprobación.

## <a name="cancel-a-request"></a>Cancelación de una solicitud

Si envía una solicitud de acceso y esta aún se encuentra en estado **pendiente de aprobación**, puede cancelarla.

**Rol necesario:** Solicitante

1. En el portal Mi acceso, a la izquierda, haga clic en **Historial de solicitudes** para ver una lista de sus solicitudes con el estado.

1. Haga clic en el vínculo **Ver** de la solicitud que quiere cancelar.

1. Si la solicitud aún se encuentra en estado **pendiente de aprobación**, puede hacer clic en **Cancelar solicitud** para cancelarla.

    ![Portal Mi acceso: Cancelar solicitud](./media/entitlement-management-request-access/my-access-cancel-request.png)

1. Haga clic en **Historial de solicitudes** para confirmar la cancelación de la solicitud.

## <a name="select-a-policy"></a>Selección de una directiva

Si está solicitando acceso a un paquete de acceso que tiene varias directivas aplicables, es posible que se le pida que seleccione una directiva. Por ejemplo, un administrador de paquetes de acceso puede configurar un paquete de acceso con dos directivas para dos grupos de empleados internos. La primera directiva podría permitir el acceso durante 60 días y requerir aprobación. La segunda directiva podría permitir el acceso durante 2 días y no requerir aprobación. Si se encuentra con este escenario, debe seleccionar la directiva que quiere utilizar.

**Rol necesario:** Solicitante

## <a name="next-steps"></a>Pasos siguientes

- [Aprobación o denegación de acceso](entitlement-management-request-approve.md)
- [Solicitud de notificaciones de proceso y correo electrónico](entitlement-management-process.md)
