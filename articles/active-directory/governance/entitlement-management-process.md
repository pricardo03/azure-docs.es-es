---
title: 'Solicitar notificaciones de correo electrónico y el proceso de administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre el proceso de solicitud para un paquete de acceso y cuándo se envían notificaciones por correo electrónico en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.date: 04/26/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ab18c8f165fc30636cd05091be1181743f9972d
ms.sourcegitcommit: 8a681ba0aaba07965a2adba84a8407282b5762b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873650"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management-preview"></a>Solicitar notificaciones de correo electrónico y el proceso de administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> Administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Cuando un usuario envía una solicitud a un paquete de acceso, se inicia un proceso para proporcionar esa solicitud. Administración de derechos de Azure AD también envía notificaciones por correo electrónico a los aprobadores y solicitantes cuando se producen eventos clave durante el proceso.

En este artículo se describe el proceso de solicitud y las notificaciones de correo electrónico que se envían.

## <a name="request-process"></a>Proceso de solicitud

Un usuario que necesite tener acceso a un paquete de acceso puede enviar una solicitud de acceso. Según la configuración de la directiva, la solicitud puede requerir una aprobación. Cuando se aprueba una solicitud, comienza un proceso asignar al usuario acceso a cada recurso en el paquete de acceso. El siguiente diagrama muestra una visión general del proceso y los diferentes Estados.

![Diagrama del proceso de aprobación](./media/entitlement-management-process/request-process.png)

| Estado | DESCRIPCIÓN |
| --- | --- |
| Enviado | El usuario envía una solicitud. |
| Pendiente de aprobación | Si la directiva para un paquete de acceso requiere aprobación, mueve a una solicitud pendiente de aprobación. |
| Expirada | Si no hay aprobadores revisión una solicitud en el tiempo de espera de solicitud de aprobación, finalice la solicitud. Para intentarlo de nuevo, el usuario tendrá que volver a enviar su solicitud. |
| Denegada | Aprobador rechaza una solicitud. |
| Aprobado | Aprobador aprueba una solicitud. |
| Entrega | Usuario tiene **no** sido asignado acceso a todos los recursos en el paquete de acceso. Si se trata de un usuario externo, el usuario aún no tiene acceso el directorio de recursos y acepta el símbolo del sistema de permisos. |
| Delivered (Entregado) | Usuario ha se ha asignado acceso a todos los recursos en el paquete de acceso. |
| Acceso a extendidas | Si se permiten las extensiones en la directiva, el usuario extiende la asignación. |
| Acceso expirado | Acceso de usuario para el paquete de acceso expiró. Para obtener acceso de nuevo, el usuario tendrá que enviar una solicitud. |

## <a name="email-notifications"></a>Notificaciones por correo electrónico

Si es un aprobador, se envían notificaciones por correo electrónico cuando se necesita aprobar una solicitud de acceso y cuando se haya completado una solicitud de acceso. Si eres un solicitante, se envían notificaciones por correo electrónico que indican el estado de la solicitud. El siguiente diagrama muestra cuando estas notificaciones por correo electrónico se envía.

![Proceso de correo electrónico de administración de derechos](./media/entitlement-management-process/email-notifications.png)

En la tabla siguiente proporciona más detalles sobre cada una de estas notificaciones por correo electrónico.

| # | Asunto del correo electrónico | Cuando envía | Enviado a |
| --- | --- | --- | --- |
| 1 | Acción requerida: Solicitud de revisión de acceso de *[solicitante]* a *[paquete de acceso]* por *[date]* | Cuando un solicitante envía una solicitud para un paquete de acceso | Todos los aprobadores |
| 2 | Acción requerida: Solicitud de revisión de acceso de *[solicitante]* a *[paquete de acceso]* por *[date]* | Tiempo de espera de solicitud X días antes de la aprobación | Todos los aprobadores |
| 3 | Notificación de estado: *[solicitante]* de solicitud de acceso a *[paquete de acceso]* ha expirado | Cuando los aprobadores no aprobar o denegar una solicitud de acceso dentro de la duración de la solicitud | Solicitante |
| 4 | Notificación de estado: *[solicitante]* solicitud de acceso a *[paquete de acceso]* se ha completado | Cuando el primer aprobador aprueba o deniega una solicitud de acceso | Todos los aprobadores |
| 5 | Se ha denegado el acceso a *[package acceso]* | Cuando un solicitante ha denegado el acceso al paquete acceso | Solicitante |
| 6 | Ahora tiene acceso a *[package acceso]*  | Cuando un solicitante se ha concedido acceso a todos los recursos en el paquete de acceso | Solicitante |
| 7 | El acceso a *[paquete de acceso]* expira en X días | X días antes de acceso del solicitante al paquete acceso expira | Solicitante |
| 8 | El acceso a *[paquete de acceso]* ha expirado | Cuando expira el acceso del solicitante a un paquete de acceso | Solicitante |

### <a name="review-access-request-emails"></a>Correos electrónicos de solicitud de acceso de revisión

Cuando un solicitante envía una solicitud de acceso de un paquete de acceso que está configurado para requerir la aprobación, configurados en la directiva de todos los aprobadores reciben una notificación de correo electrónico con los detalles de la solicitud. Los detalles incluyen el nombre del solicitante, organización, tener acceso a las fechas de inicio y finalización, si se proporciona, justificación comercial, cuando se envió la solicitud, y cuando la solicitud expirará. El correo electrónico incluye un vínculo donde los aprobadores pueden aprobar o denegar la solicitud de acceso. Aquí es una notificación de correo electrónico de ejemplo que se envía a un aprobador cuando un solicitante envía una solicitud de acceso.

![Correo electrónico de solicitud de acceso de revisión](./media/entitlement-management-shared/email-approve-request.png)

### <a name="approved-or-denied-emails"></a>Correos electrónicos aprobados y denegados

Los solicitantes se le notificará cuando su solicitud de acceso está aprobado y disponibles para el acceso, o cuando se ha denegado su solicitud de acceso. Cuando un aprobador revisa una solicitud de acceso enviada por un solicitante, pueden aprobar o denegar la solicitud de acceso. El aprobador debe agregar una justificación comercial para su decisión.

Cuando se aprueba una solicitud de acceso, administración de derechos inicia el proceso de otorgar el solicitante un acceso a cada uno de los recursos en el paquete de acceso. Después de que el solicitante se ha concedido acceso a todos los recursos en el paquete de acceso, se envía una notificación por correo electrónico al solicitante que se ha aprobado su solicitud de acceso y que ahora tienen acceso al paquete acceso. Aquí es una notificación de correo electrónico de ejemplo que se envía al solicitante cuando se les concede acceso a un paquete de acceso.

Cuando se deniega una solicitud de acceso, se envía una notificación por correo electrónico al solicitante. Aquí es una notificación de correo electrónico de ejemplo que se envía al solicitante cuando se ha denegado su solicitud de acceso.

### <a name="expired-access-request-emails"></a>Caducado correos electrónicos de solicitud de acceso

Los solicitantes reciben una notificación cuando ha expirado su solicitud de acceso. Cuando un solicitante envía una solicitud de acceso, la solicitud tiene una duración de la solicitud después del cual caduca. Si no hay ningún aprobadores que enviar una decisión de aprobar o denegar, continúa la solicitud permanezca en un estado pendiente de aprobación. Cuando la solicitud alcanza su duración de expiración configurado, la solicitud expire y ya no ser aprobada o rechazada por los aprobadores. En este caso, la solicitud entra en un estado expirado. Ya no se puede apruebe o deniegue una solicitud expirada. Se envía una notificación de correo electrónico al solicitante que su solicitud de acceso ha expirado y que necesitan para volver a enviar la solicitud de acceso. Aquí es una notificación de correo electrónico de ejemplo que se envía al solicitante cuando ha expirado su solicitud de acceso.

![Expiró el correo electrónico de solicitud de acceso](./media/entitlement-management-process/email-expired-access-request.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solicitar acceso a un paquete de acceso](entitlement-management-request-access.md)
- [Aprobar o rechazar solicitudes de acceso](entitlement-management-request-approve.md)
