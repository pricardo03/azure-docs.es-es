---
title: 'Proceso de solicitud y notificaciones por correo electrónico en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Información acerca del proceso de solicitud para un paquete de acceso y de cuándo se envían notificaciones por correo electrónico en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.date: 05/30/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: aede5e315141251026867f7028ebf989d44da4d5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66473062"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management-preview"></a>Proceso de solicitud y notificaciones por correo electrónico en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Cuando un usuario envía una solicitud a un paquete de acceso, se inicia un proceso para proporcionar esa solicitud. La administración de derechos de Azure AD también envía notificaciones por correo electrónico a los aprobadores y solicitantes cuando se producen eventos clave durante el proceso.

En este artículo se describen el proceso de solicitud y las notificaciones por correo electrónico que se envían.

## <a name="request-process"></a>Proceso de solicitud

Un usuario que necesite tener acceso a un paquete de acceso puede enviar una solicitud de acceso. Según la configuración de la directiva, la solicitud puede requerir una aprobación. Cuando se aprueba una solicitud, comienza un proceso para asignar al usuario acceso a cada recurso del paquete de acceso. El siguiente diagrama muestra una visión general del proceso y de los diferentes estados.

![Diagrama del proceso de aprobación](./media/entitlement-management-process/request-process.png)

| Estado | DESCRIPCIÓN |
| --- | --- |
| Enviado | El usuario envía una solicitud. |
| Pendiente de aprobación | Si la directiva para un paquete de acceso requiere aprobación, la solicitud queda pendiente de aprobación. |
| Expirada | Si ningún aprobador aprueba una solicitud dentro del tiempo de espera de la solicitud de aprobación, la solicitud expira. Para intentarlo de nuevo, el usuario tendrá que volver a enviar la solicitud. |
| Denegado | El aprobador rechaza una solicitud. |
| Aprobado | El aprobador aprueba una solicitud. |
| Entrega | **No** se ha asignado al usuario acceso a todos los recursos del paquete de acceso. Si se trata de un usuario externo, el usuario aún no ha accedido al directorio de recursos ni aceptado la petición de permisos. |
| Delivered (Entregado) | Se ha asignado al usuario acceso a todos los recursos del paquete de acceso. |
| Acceso extendido | Si se permiten extensiones en la directiva, el usuario extiende la asignación. |
| Acceso expirado | El acceso del usuario al paquete de acceso ha expirado. Para obtener acceso de nuevo, el usuario tendrá que enviar una solicitud. |

## <a name="email-notifications"></a>Notificaciones por correo electrónico

Si es un aprobador, se le enviarán notificaciones por correo electrónico cuando tenga que aprobar una solicitud de acceso y cuando esta se haya completado. Si es un solicitante, se le enviarán notificaciones por correo electrónico que indiquen el estado de su solicitud. El siguiente diagrama muestra cuándo se envían estas notificaciones por correo electrónico.

![Proceso de correo electrónico de la administración de derechos](./media/entitlement-management-process/email-notifications.png)

En la tabla siguiente se proporcionan más detalles sobre cada una de estas notificaciones por correo electrónico.

| # | Asunto del correo electrónico | Cuándo se envía | Se envía a |
| --- | --- | --- | --- |
| 1 | Acción requerida: revisión de la solicitud de acceso del *[solicitante]* al *[paquete de acceso]* el *[fecha]* | Cuando un solicitante envía una solicitud para un paquete de acceso | Todos los aprobadores |
| 2 | Acción requerida: revisión de la solicitud de acceso del *[solicitante]* al *[paquete de acceso]* el *[fecha]* | X días antes de la fecha de expiración de la solicitud de aprobación | Todos los aprobadores |
| 3 | Notificación de estado: la solicitud de acceso del *[solicitante]* al *[paquete de acceso]* ha expirado | Cuando los aprobadores no aprueben o denieguen una solicitud de acceso dentro de la duración de la solicitud | Solicitante |
| 4 | Notificación de estado: la solicitud de acceso del *[solicitante]* al *[paquete de acceso]* se ha completado | Cuando el primer aprobador aprueba o deniega una solicitud de acceso | Todos los aprobadores |
| 5 | Se le ha denegado el acceso al *[paquete de acceso]* | Cuando se deniega a un solicitante el acceso al paquete acceso | Solicitante |
| 6 | Ya tiene acceso al *[paquete de acceso]*  | Cuando se concede acceso a un solicitante a todos los recursos del paquete de acceso | Solicitante |
| 7 | El acceso al *[paquete de acceso]* expira en X días | X días antes de que expire el acceso al paquete de acceso del solicitante | Solicitante |
| 8 | Su acceso al *[paquete de acceso]* ha expirado | Cuando expire el acceso a un paquete de acceso del solicitante | Solicitante |

### <a name="access-request-emails"></a>Mensajes de correo electrónico de solicitud de acceso

Cuando un solicitante envía una solicitud de acceso para un paquete de acceso que está configurado para requerir aprobación, todos los aprobadores configurados en la directiva reciben una notificación por correo electrónico con los detalles de la solicitud. Los detalles incluyen el nombre del solicitante, la organización, las fechas de inicio y fin del acceso (si se proporcionan), la justificación comercial, cuándo se envió la solicitud y cuándo expirará. El mensaje de correo electrónico incluye un vínculo donde los aprobadores pueden aprobar o denegar la solicitud de acceso. Esta es una notificación por correo electrónico de ejemplo que se envía a un aprobador cuando un solicitante envía una solicitud de acceso.

![Mensaje de correo electrónico de revisión de solicitud de acceso](./media/entitlement-management-shared/email-approve-request.png)

### <a name="approved-or-denied-emails"></a>Mensajes de correo electrónico de aprobación o denegación

Los solicitantes recibirán una notificación cuando su solicitud de acceso esté aprobada y disponible para el acceso, o cuando se deniegue su solicitud de acceso. Cuando un aprobador reciba una solicitud de acceso enviada por un solicitante, puede aprobar o denegar la solicitud de acceso. El aprobador debe agregar una justificación comercial que apoye su decisión.

Cuando se aprueba una solicitud de acceso, la administración de derechos inicia el proceso de conceder al solicitante acceso a cada uno de los recursos del paquete de acceso. Después de que se haya concedido al solicitante acceso a todos los recursos del paquete de acceso, se le envía una notificación por correo electrónico para informarle que se ha aprobado su solicitud de acceso y que ya tiene acceso al paquete de acceso. Esta es una notificación por correo electrónico de ejemplo que se envía a un solicitante cuando se le concede acceso al paquete de acceso.

Cuando se deniega una solicitud de acceso, se envía una notificación por correo electrónico al solicitante. Esta es una notificación por correo electrónico de ejemplo que se envía a un solicitante cuando se le deniega la solicitud de acceso.

### <a name="expired-access-request-emails"></a>Mensajes de correo electrónico de solicitud de acceso expirada

Los solicitantes reciben una notificación cuando ha expirado su solicitud de acceso. Cuando un solicitante envía una solicitud de acceso, la solicitud tiene una duración de solicitud después de la cual expira. Si no hay aprobadores que envíen una decisión de aprobación o denegación, la solicitud sigue en estado de pendiente de aprobación. Cuando la solicitud alcanza la duración de expiración configurada, expira y los aprobadores ya no la pueden aprobar ni rechazar. En este caso, la solicitud pasa al estado expirado. No se puede aprobar ni denegar una solicitud expirada. Se envía al solicitante una notificación por correo electrónico que le indica que su solicitud de acceso ha expirado y que la debe volver a enviar. Esta es una notificación por correo electrónico de ejemplo que se envía a un solicitante cuando su solicitud de acceso ha expirado.

![Mensaje de correo electrónico de solicitud de acceso expirada](./media/entitlement-management-process/email-expired-access-request.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solicitud de acceso a un paquete de acceso](entitlement-management-request-access.md)
- [Aprobación o denegación de solicitudes de acceso](entitlement-management-request-approve.md)
