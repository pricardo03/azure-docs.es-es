---
title: 'Aprobación o denegación de solicitudes de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo usar el portal Mi acceso para aprobar o denegar solicitudes para un paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.date: 04/18/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b2d07638f6c6f153ee3640273fbee5e56df0ab2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64541531"
---
# <a name="approve-or-deny-access-requests-in-azure-ad-entitlement-management-preview"></a>Aprobación o denegación de solicitudes de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Con la administración de derechos de Azure AD, puede configurar directivas para requerir aprobación para los paquetes de acceso y elegir uno o varios aprobadores. En este artículo se describe cómo los aprobadores designados pueden aprobar o denegar solicitudes para paquetes de acceso.

## <a name="open-request"></a>Abrir una solicitud

El primer paso para aprobar o denegar solicitudes de acceso es buscar y abrir la solicitud de acceso pendiente de aprobación. Hay dos maneras de abrir la solicitud de acceso.

**Rol necesario:** Aprobador

1. Buscar un correo electrónico de Microsoft Azure que le pida que apruebe o deniegue una solicitud. Este es un ejemplo de correo electrónico:

    ![Correo electrónico de solicitud de aprobación para un paquete de acceso](./media/entitlement-management-shared/email-approve-request.png)

1. Haga clic en el vínculo **Aprobar o denegar una solicitud** para abrir la solicitud de acceso.

1. Inicie sesión en el portal Mi acceso.

Si no tiene el correo electrónico, puede encontrar las solicitudes de acceso pendientes de aprobación mediante estos pasos.

1. Inicie sesión en el portal Mi acceso en [https://myaccess.microsoft.com](https://myaccess.microsoft.com).

1. En el menú izquierdo, haga clic en **Aprobaciones** para ver una lista de las solicitudes de acceso pendientes de aprobación.

1. En la pestaña **Pendientes**, busque la solicitud.

## <a name="approve-or-deny-request"></a>Aprobar o denegar una solicitud

Después de abrir una solicitud de acceso pendiente de aprobación, puede ver los detalles que le ayudarán a tomar una decisión de aprobación o denegación.

**Rol necesario:** Aprobador

1. Haga clic en el vínculo **Ver** para abrir el panel Solicitud de acceso.

1. Haga clic en **Detalles** para ver más información sobre la solicitud de acceso.

    Los detalles incluyen el nombre de usuario, la organización, las fechas de inicio y fin del acceso (si se proporcionan), la justificación comercial, cuándo se envió la solicitud y cuándo expirará.

1. Haga clic en **Aprobar** o **Denegar**.

1. Si es necesario, escriba un motivo.

    ![Portal Mi acceso: solicitud de acceso](./media/entitlement-management-shared/my-access-approve-request.png)

1. Haga clic en **Enviar** para enviar su decisión.

    Si se configura una directiva con varios aprobadores, solo un aprobador tiene que tomar una decisión sobre la aprobación pendiente. Una vez que un aprobador haya enviado su decisión sobre la solicitud de acceso, la solicitud se completa y ya no está disponible para que los otros aprobadores la aprueben o denieguen. Los otros aprobadores pueden consultar la decisión sobre la solicitud y el aprobador que tomó la decisión en su portal Mi acceso. En este momento, solo se admite la aprobación de fase única.

    Si ninguno de los aprobadores configurados puede aprobar o denegar la solicitud de acceso, la solicitud expira tras la duración de la solicitud configurada. El usuario recibe una notificación que le indica que su solicitud de acceso ha expirado y que debe volver a enviar la solicitud de acceso.

## <a name="next-steps"></a>Pasos siguientes

- [Solicitud de acceso a un paquete de acceso](entitlement-management-request-access.md)
- [Solicitud de notificaciones de proceso y correo electrónico](entitlement-management-process.md)
