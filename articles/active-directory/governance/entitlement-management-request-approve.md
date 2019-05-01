---
title: 'Aprobar o rechazar solicitudes de acceso de administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo usar el portal de mi acceso para aprobar o rechazar solicitudes a un paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.sourcegitcommit: 9ad75f83bbf0fc4623b7995794f33bbf823b31c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64541531"
---
# <a name="approve-or-deny-access-requests-in-azure-ad-entitlement-management-preview"></a>Aprobar o rechazar solicitudes de acceso de administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> Administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Con la administración de derechos de Azure AD, puede configurar directivas para requerir la aprobación para los paquetes de acceso y elija uno o más aprobadores. Este artículo describe cómo designados aprobadores puede aprobar o rechazar solicitudes para los paquetes de acceso.

## <a name="open-request"></a>Solicitud de apertura

Es el primer paso para aprobar o denegar las solicitudes de acceso buscar y abrir la solicitud de acceso pendientes de aprobación. Hay dos maneras de abrir la solicitud de acceso.

**Función de requisitos previos:** Aprobador

1. Busque un correo electrónico de Microsoft Azure que le pide que apruebe o deniegue una solicitud. Este es un correo electrónico de ejemplo:

    ![Aprobar solicitud de acceso al correo electrónico de paquete](./media/entitlement-management-shared/email-approve-request.png)

1. Haga clic en el **aprobar o denegar la solicitud** vínculo para abrir la solicitud de acceso.

1. Inicie sesión en el portal de mi acceso.

Si no tiene el correo electrónico, puede encontrar las solicitudes de acceso pendientes de aprobación siguiendo estos pasos.

1. Inicie sesión el portal de mi acceso en [ https://myaccess.microsoft.com ](https://myaccess.microsoft.com).

1. En el menú izquierdo, haga clic en **aprobaciones** para ver una lista de las solicitudes de acceso pendientes de aprobación.

1. En el **pendiente** pestaña, busque la solicitud.

## <a name="approve-or-deny-request"></a>Aprobar o denegar la solicitud

Después de abrir una solicitud de acceso pendientes de aprobación, puede ver los detalles que le ayudarán a realizar un aprobar o denegar la decisión.

**Función de requisitos previos:** Aprobador

1. Haga clic en el **vista** vínculo para abrir el panel de solicitudes de acceso.

1. Haga clic en **detalles** para ver detalles sobre la solicitud de acceso.

    Los detalles incluyen el nombre del usuario, organización, tener acceso a las fechas de inicio y finalización, si se proporciona, justificación comercial, cuando se envió la solicitud, y cuando la solicitud expirará.

1. Haga clic en **aprobar** o **denegar**.

1. Si es necesario, especifique un motivo.

    ![Mi portal de acceso: solicitud de acceso](./media/entitlement-management-shared/my-access-approve-request.png)

1. Haga clic en **enviar** para enviar su decisión.

    Si se configura una directiva con varios aprobadores, sólo un aprobador necesita tomar una decisión sobre la aprobación pendiente. Después de un aprobador ha enviado su decisión de la solicitud de acceso, la solicitud se ha completado y ya no está disponible para los otros aprobadores aprobar o denegar la solicitud. La decisión de solicitud y la toma de decisiones en su portal Mi acceso, pueden ver los demás aprobadores. En este momento, aprobación de fase única solo se admite.

    Si ninguno de los aprobadores configurados puede aprobar o denegar la solicitud de acceso, la solicitud expira después de la duración configurada de la solicitud. El usuario obtiene una notificación de que su solicitud de acceso ha expirado y que deba volver a enviar la solicitud de acceso.

## <a name="next-steps"></a>Pasos siguientes

- [Solicitar acceso a un paquete de acceso](entitlement-management-request-access.md)
- [Solicitar notificaciones de correo electrónico y de proceso](entitlement-management-process.md)
