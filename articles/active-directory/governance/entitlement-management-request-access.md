---
title: 'Solicitar acceso a un paquete de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo usar el portal de mi acceso para solicitar acceso a un paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.sourcegitcommit: 9ad75f83bbf0fc4623b7995794f33bbf823b31c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64541561"
---
# <a name="request-access-to-an-access-package-in-azure-ad-entitlement-management-preview"></a>Solicitar acceso a un paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> Administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="sign-in-to-the-my-access-portal"></a>Inicie sesión en el portal de mi acceso

El primer paso es iniciar sesión en el portal de mi acceso donde puede solicitar acceso a un paquete de acceso.

**Función de requisitos previos:** Solicitante

1. Busque un correo electrónico o un mensaje desde el Administrador de proyecto o business que esté trabajando. El correo electrónico debe incluir un vínculo al paquete acceso a que necesita acceso. El vínculo se iniciará con:

    `https://myaccess.microsoft.com`

1. Abra el vínculo.

1. Inicie sesión en el portal de mi acceso.

    Asegúrese de que usar su cuenta profesional. Si no está seguro, póngase en contacto con su administrador de proyecto o business.

## <a name="request-an-access-package"></a>Solicitud de un paquete de acceso

Una vez haya encontrado el paquete de acceso en el portal de mi acceso, puede enviar una solicitud.

**Función de requisitos previos:** Solicitante

1. Haga clic en la marca de verificación para seleccionar el paquete de acceso.

    ![Mi portal de acceso: paquetes de acceso](./media/entitlement-management-shared/my-access-access-packages.png)

1. Haga clic en **solicitar acceso** para abrir el panel de acceso de la solicitud.

1. Si el **justificación comercial** se muestra el cuadro, escriba una justificación para tener acceso.

1. Si **solicitar durante un período específico?** está habilitado, seleccione **Sí** o **No**.

1. Si es necesario, especifique la fecha de inicio y fecha de finalización.

    ![Mi portal de acceso: solicitud de acceso](./media/entitlement-management-shared/my-access-request-access.png)

1. Cuando termine, haga clic en **enviar** para enviar la solicitud.

1. Haga clic en **historial de solicitudes** para ver una lista de las solicitudes y el estado.

    Si el paquete de acceso requiere aprobación, la solicitud está ahora en un estado pendiente de aprobación.

## <a name="cancel-a-request"></a>Cancelar una solicitud

Si se envía una solicitud de acceso y la solicitud aún está en el **pendiente de aprobación** de estado, puede cancelar la solicitud.

**Función de requisitos previos:** Solicitante

1. En el portal de mi acceso a la izquierda, haga clic en **historial de solicitudes** para ver una lista de las solicitudes y el estado.

1. Haga clic en el **vista** vínculo de la solicitud que desea cancelar.

1. Si la solicitud aún está en el **pendiente de aprobación** de estado, puede hacer clic en **solicitud de cancelación** para cancelar la solicitud.

    ![Mi portal de acceso: solicitud de cancelación](./media/entitlement-management-request-access/my-access-cancel-request.png)

1. Haga clic en **historial de solicitudes** para confirmar la solicitud se canceló.

## <a name="select-a-policy"></a>Selección de una directiva

Si desea solicitar acceso a un paquete de acceso que tiene varias directivas que se aplican, puede que le pida que seleccione una directiva. Por ejemplo, un administrador de paquetes de acceso puede configurar un paquete de acceso con dos directivas para los dos grupos de empleados internos. La primera directiva podría permitir el acceso durante 60 días y requerir la aprobación. La segunda directiva podría permitir el acceso de 2 días y no requieren aprobación. Si se produce esta situación, debe seleccionar la directiva que desea utilizar.

**Función de requisitos previos:** Solicitante

## <a name="next-steps"></a>Pasos siguientes

- [Aprobar o rechazar solicitudes de acceso](entitlement-management-request-approve.md)
- [Solicitar notificaciones de correo electrónico y de proceso](entitlement-management-process.md)
