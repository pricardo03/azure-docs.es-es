---
title: 'Proceso de solicitud y notificaciones por correo electrónico en la administración de derechos de Azure AD: Azure Active Directory'
description: Información acerca del proceso de solicitud para un paquete de acceso y de cuándo se envían notificaciones por correo electrónico en la administración de derechos de Azure Active Directory.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/30/2019
ms.author: ajburnle
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: e34d2c69cc808552a3b0c604804f3cd2597b379b
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2019
ms.locfileid: "73199967"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management"></a>Proceso de solicitud y notificaciones por correo electrónico en la administración de derechos de Azure AD

Cuando un usuario envía una solicitud a un paquete de acceso, se inicia un proceso para proporcionar esa solicitud de acceso. La administración de derechos de Azure AD envía notificaciones por correo electrónico a los aprobadores y solicitantes cuando, durante el proceso, se producen eventos clave. En este artículo se describen el proceso de solicitud y las notificaciones por correo electrónico que se envían.

## <a name="request-process"></a>Proceso de solicitud

Un usuario que necesite tener acceso a un paquete de acceso puede enviar una solicitud de acceso. Según la configuración de la directiva, la solicitud puede requerir una aprobación. Cuando se aprueba una solicitud, comienza un proceso para asignar al usuario acceso a cada recurso del paquete de acceso. El siguiente diagrama muestra una visión general del proceso y de los diferentes estados:

![Diagrama del proceso de aprobación](./media/entitlement-management-process/request-process.png)

| State | DESCRIPCIÓN |
| --- | --- |
| Enviado | El usuario envía una solicitud. |
| Pendiente de aprobación | Si la directiva para un paquete de acceso requiere aprobación, la solicitud queda pendiente de aprobación. |
| Expirada | Si ningún aprobador aprueba una solicitud dentro del tiempo de espera de la solicitud de aprobación, la solicitud expira. Para intentarlo de nuevo, el usuario tendrá que volver a enviar la solicitud. |
| Denegado | El aprobador rechaza una solicitud. |
| Aprobado | El aprobador aprueba una solicitud. |
| Entrega | **No** se ha asignado al usuario acceso a todos los recursos del paquete de acceso. Si se trata de un usuario externo, el usuario puede que aún no haya accedido al directorio de recursos ni aceptado la petición de consentimiento. |
| Delivered (Entregado) | Se ha asignado al usuario acceso a todos los recursos del paquete de acceso. |
| Acceso extendido | Si se permiten extensiones en la directiva, el usuario extiende la asignación. |
| Acceso expirado | El acceso del usuario al paquete de acceso ha expirado. Para obtener acceso de nuevo, el usuario tendrá que enviar una solicitud. |

## <a name="email-notifications"></a>Notificaciones por correo electrónico

Si es un aprobador, se le enviarán notificaciones por correo electrónico cuando tenga que aprobar una solicitud de acceso y cuando esta se haya completado. Si es un solicitante, se le enviarán notificaciones por correo electrónico que indiquen el estado de su solicitud.

En los diagramas siguientes se muestra cuándo se envían estas notificaciones por correo electrónico a los aprobadores o al solicitante. Haga referencia a la tabla [notificaciones por correo electrónico](entitlement-management-process.md#email-notifications-table) para buscar el número correspondiente a las notificaciones de correo electrónico que se muestran en los diagramas.

### <a name="primary-approvers-and-alternate-approvers"></a>Aprobadores principales y aprobadores alternativos
En el diagrama siguiente se muestra la experiencia de los aprobadores principales y los aprobadores alternativos, así como las notificaciones por correo electrónico que reciben durante el proceso de solicitud:

![Flujo de proceso de aprobadores principales y alternativos](./media/entitlement-management-process/primary-approvers-and-alternate-with-escalation-flow.png)

### <a name="requestors"></a>Solicitantes
En el diagrama siguiente se muestra la experiencia de los aprobadores, así como las notificaciones por correo electrónico que reciben durante el proceso de solicitud:

![Flujo del proceso del solicitante](./media/entitlement-management-process/requestor-approval-and-expiration-request-flow.png)

### <a name="email-notifications-table"></a>Tabla de notificaciones por correo electrónico
En la tabla siguiente se proporcionan más detalles sobre cada una de estas notificaciones por correo electrónico. Para administrar estos mensajes de correo electrónico, puede usar reglas. En Outlook, por ejemplo, puede crear reglas para trasladar los correos electrónicos a una carpeta si el asunto contiene palabras de esta tabla:

| # | Asunto del correo electrónico | Cuándo se envía | Se envía a |
| --- | --- | --- | --- |
| 1 | Acción requerida: Aprobar o denegar la solicitud reenviada por *[fecha]* | Este correo electrónico se enviará a los aprobadores alternativos de la fase 1 (después de escalar la solicitud) para realizar la acción. | Aprobador alternativo de la fase 1 |
| 2 | Acción requerida: Aprobar o denegar una solicitud por *[date]* | Este correo electrónico se enviará a los aprobadores principales de la fase 1, si la escalación está deshabilitada, para realizar la acción. | Aprobador principal de la fase 1 |
| 3 | Recordatorio: Aprobar o denegar la solicitud por *[fecha]* para *[solicitante]* | Este correo electrónico se enviará a los aprobadores principales de la fase 1, si la escalación está deshabilitada, para realizar la acción, solo cuando no hayan hecho ninguna acción. | Aprobador principal de la fase 1 |
| 4 | Aprobar o denegar la solicitud por *[hora]* el *[fecha]* | Este correo electrónico se enviará a los aprobadores principales de la fase 1 (si la escalación está habilitada) para realizar la acción. | Aprobador principal de la fase 1 |
| 5 | Se requiere recordatorio de la acción: Aprobar o denegar la solicitud por *[fecha]* para *[solicitante]* | Este correo electrónico se enviará a los aprobadores principales de la fase 1, si la escalación está habilitada, para realizar la acción, solo cuando no hayan hecho ninguna acción. | Aprobador principal de la fase 1 |
| 6 | La solicitud expiró para *[access_package]* | Una vez que la solicitud haya expirado, este correo electrónico se enviará a los aprobadores principales de la fase 1 o a los aprobadores alternativos de la fase 1, de una solicitud de una sola fase o de varias fases. | Aprobador principal de la fase 1, aprobador alternativo de la fase 1 |
| 7 | Solicitud aprobada para el *[solicitante]* para *[access_package]* | Este correo electrónico se enviará a los aprobadores principales de la fase 1 o a los aprobadores alternativos de la fase 1, tras la finalización de una solicitud. | Aprobador principal de la fase 1, aprobador alternativo de la fase 1 |
| 8 | Solicitud aprobada para el *[solicitante]* para *[access_package]* | Este correo electrónico se enviará a los aprobadores principales de la fase 1 o a los aprobadores alternativos de la fase 1, de una solicitud de la fase 2, solo cuando la fase 1 esté aprobada. | Aprobador principal de la fase 1, aprobador alternativo de la fase 1 |
| 9 | Se denegó la solicitud a *[access_package]* | Este correo electrónico se enviará al solicitante solo cuando se deniegue su solicitud | Solicitante |
| 10 | Expiró su solicitud a *[access_package]* | Una vez que la solicitud haya expirado, este correo electrónico se enviará al solicitante al final de la fase 1, de una solicitud de una sola fase o de varias fases. | Solicitante |
| 18 | Ahora tiene acceso a *[access_package]* | Este correo electrónico se enviará a los usuarios finales para que empiecen a usar su acceso. | Solicitante |
| 19 | Extender el acceso a *[access_package]* por *[fecha]* | Este correo electrónico se enviará a los usuarios finales antes de que expire su acceso. | Solicitante |
| 20 | Ha finalizado el acceso a *[access_package]* | Este correo electrónico se enviará a los usuarios finales después de que expire su acceso. | Solicitante |

### <a name="access-request-emails"></a>Mensajes de correo electrónico de solicitud de acceso

Cuando un solicitante envía una solicitud de acceso para un paquete de acceso que está configurado para requerir aprobación, todos los aprobadores agregados a la directiva reciben una notificación por correo electrónico con los detalles de la solicitud. Los detalles incluyen el nombre del solicitante, la organización, las fechas de inicio y fin del acceso (si se proporcionan), la justificación comercial, cuándo se envió la solicitud y cuándo expirará.

El mensaje de correo electrónico incluye un vínculo a MyAccess donde los aprobadores pueden aprobar o denegar la solicitud de acceso. Esta es una notificación por correo electrónico de ejemplo que se envía a un aprobador cuando un solicitante envía una solicitud de acceso:

![Correo electrónico de solicitud de aprobación para un paquete de acceso](./media/entitlement-management-shared/approver-request-email.png)

También se envía una notificación por correo electrónico a los aprobadores principales con un recordatorio para tomar medidas y tomar una decisión sobre la solicitud. Este es un ejemplo de correo electrónico de la notificación que reciben los aprobadores principales para recordarles que tomen medidas:

![Mensaje de correo electrónico del recordatorio de la solicitud de acceso](./media/entitlement-management-process/approver-access-request-reminder-email.png)

### <a name="alternate-approver-request-emails"></a>Correos electrónicos de solicitud para el aprobador alternativo

Si está habilitado el reenvío a aprobadores alternativos, según la directiva de reenvío, y si la solicitud todavía está pendiente, se reenviará la solicitud. El aprobador alternativo recibirá un correo electrónico de notificación para aprobar o denegar la solicitud. Este es un ejemplo de correo electrónico de la notificación que reciben los aprobadores alternativos:

![Correo electrónico de solicitud para el aprobador alternativo](./media/entitlement-management-process/alternate-approver-email-fwd-request.png)

Tanto los aprobadores principales como los alternativos pueden aprobar o denegar la solicitud.

### <a name="approved-or-denied-emails"></a>Mensajes de correo electrónico de aprobación o denegación

Los solicitantes recibirán una notificación cuando su solicitud de acceso esté aprobada y disponible para el acceso, o cuando se deniegue su solicitud de acceso. Cuando un aprobador reciba una solicitud de acceso enviada por un solicitante, puede aprobar o denegar la solicitud de acceso. El aprobador debe agregar una justificación comercial que apoye su decisión. Este es un ejemplo de correo electrónico que se envía a los aprobadores principales o alternativos una vez aprobada una solicitud:

![Mensaje de correo electrónico de revisión de solicitud de acceso](./media/entitlement-management-process/approver-request-email-approved.png)

Cuando se aprueba una solicitud de acceso y se aprovisiona su acceso, se envía una notificación por correo electrónico al solicitante de que ahora tiene acceso al paquete de acceso. Este es un ejemplo de una notificación por correo electrónico que se envía a un solicitante cuando se le concede acceso al paquete de acceso:

![Mensaje de correo electrónico de solicitud de acceso expirada](./media/entitlement-management-process/requestor-email-approved.png)

Cuando se deniega una solicitud de acceso, se envía una notificación por correo electrónico al solicitante. Este es un ejemplo de una notificación por correo electrónico que se envía a un solicitante cuando se le deniega la solicitud de acceso:

![Correo electrónico de solicitud denegada al solicitante](./media/entitlement-management-process/requestor-email-denied.png)

### <a name="expired-access-request-emails"></a>Mensajes de correo electrónico de solicitud de acceso expirada

Las solicitudes de acceso pueden expirar si ningún aprobador ha aprobado o denegado la solicitud. 

La solicitud expira cuando se llega la fecha de expiración configurada; los aprobadores ya no la pueden aprobar ni rechazar. Este es un ejemplo de correo electrónico de la notificación que reciben todos los aprobadores principales y alternativos:

 ![Mensaje de correo electrónico de solicitud de acceso expirada para los aprobadores](./media/entitlement-management-process/approver-request-email-expired.png)

 Se envía al solicitante una notificación por correo electrónico que le indica que su solicitud de acceso ha expirado y que debe volver a enviarla. Este es un ejemplo de una notificación por correo electrónico que se envía a un solicitante cuando su solicitud de acceso ha expirado:

![Mensaje de correo electrónico de solicitud de acceso expirada para el aprobador](./media/entitlement-management-process/requestor-email-request-expired.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solicitud de acceso a un paquete de acceso](entitlement-management-request-access.md)
- [Aprobación o denegación de solicitudes de acceso](entitlement-management-request-approve.md)
