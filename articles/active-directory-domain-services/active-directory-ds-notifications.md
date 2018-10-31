---
title: 'Azure Active Directory Domain Services: configuración de notificaciones | Microsoft Docs'
description: Configuración de notificaciones de Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: b9af1792-0b7f-4f3e-827a-9426cdb33ba6
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2018
ms.author: ergreenl
ms.openlocfilehash: f5049d6092bfc6b3f70ebafdc501e02ad9fe7936
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49433211"
---
# <a name="notification-settings-in-azure-ad-domain-services"></a>Configuración de notificaciones de Azure AD Domain Services

Las notificaciones de Azure AD Domain Services podrán actualizarse en cuanto se detecta una alerta de estado en el dominio administrado.  

Esta característica solo está disponible para los dominios administrados que no están en redes virtuales clásicas.


## <a name="how-to-check-your-azure-ad-domain-services-email-notification-settings"></a>Comprobación de la configuración de las notificaciones por correo electrónico de Azure AD Domain Services

1. Vaya a la [página de Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) en Azure Portal.
2. Elija el dominio administrado de la tabla.
3. En el panel de navegación izquierdo, elija **Configuración de notificaciones**.

En la página se muestran todos los destinatarios de correo electrónico para las notificaciones por correo electrónico de Azure AD Domain Services.

## <a name="what-does-an-email-notification-look-like"></a>¿Qué aspecto tiene una notificación por correo electrónico?

La imagen siguiente es un ejemplo de una notificación por correo electrónico:

![Notificación por correo electrónico de ejemplo](.\media\active-directory-domain-services-alerts\email-alert.png)

El correo electrónico especifica el dominio administrado en el que se muestra la alerta, así como la hora de detección y un vínculo a la página de estado de Azure AD Domain Services en Azure Portal.

> [!WARNING]
> Asegúrese siempre de que el correo electrónico procede de un remitente comprobado de Microsoft antes de hacer clic en vínculos de sus correos electrónicos. Los correos electrónicos siempre proceden de la dirección azure-noreply@microsoft.com.
>


## <a name="why-would-i-receive-email-notifications"></a>¿Por qué recibiré notificaciones por correo electrónico?

Azure AD Domain Services envía notificaciones por correo electrónico de actualizaciones importantes en su dominio.  Estas notificaciones solo informan de cuestiones urgentes que afectarán a su servicio y que deben solucionarse de inmediato. Cada notificación por correo electrónico desencadena una alerta en el dominio administrado. Estas alertas también aparecerán en Azure Portal y pueden verse en la [página de estado de Azure AD Domain Services](active-directory-ds-check-health.md).

Azure AD Domain Services no envía correos electrónicos a esta lista para fines de ventas, actualizaciones o anuncios.

## <a name="when-will-i-receive-email-notifications"></a>¿Cuándo recibiré notificaciones por correo electrónico?

Se enviará una notificación inmediatamente cuando haya una [nueva alerta](active-directory-ds-troubleshoot-alerts.md) en el dominio administrado. Si no se resuelve la alerta, se enviará una notificación por correo electrónico como recordatorio cada cuatro días.

## <a name="who-should-receive-the-email-notifications"></a>¿Quién recibe las notificaciones por correo electrónico?


 Se recomienda que la lista de destinatarios de correo electrónico de Azure AD Domain Services se componga de las personas que puedan administrar y realizar cambios en el dominio administrado. Debe considerar a las personas de esta lista de correo electrónico como los primeros en responder ante cualquier problema detectado. Si tiene más de cinco correos electrónicos adicionales que quiere agregar, se recomienda crear una lista de distribución para agregarlos a la lista de notificaciones.

Puede agregar hasta cinco correos electrónicos adicionales para las notificaciones relacionadas con Azure AD Domain Services. Además, puede elegir que todos los administradores globales de su directorio y todos los miembros del grupo "Administradores de controladores de dominio de AAD" reciban las notificaciones por correo electrónico de Azure AD Domain Services. Azure AD Domain Services solo enviará notificaciones a un máximo de 100 direcciones de correo electrónico, incluida la lista de los administradores globales y de administradores de controladores de dominio de AAD.


## <a name="how-to-add-an-additional-email-recipient"></a>Pasos para agregar a un destinatario de correo electrónico adicional

> [!WARNING]
> Al cambiar la configuración de notificaciones, estará modificando también las de todo el dominio administrado, no solo las de usted.

1. Vaya a la página [Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) en Azure Portal.
2. Haga clic en el dominio administrado.
3. En el panel de navegación izquierdo, haga clic en **Configuración de notificaciones**.
4. Para agregar un correo electrónico, escriba la dirección de correo electrónico en la tabla de destinatarios adicionales.
5. Haga clic en "Guardar" en el panel de navegación superior.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

#### <a name="i-received-an-email-notification-for-an-alert-but-when-i-logged-on-to-the-azure-portal-there-was-no-alert-what-happened"></a>He recibido una notificación por correo electrónico de una alerta, pero cuando inicié sesión en Azure Portal no había ninguna. ¿Qué ha ocurrido?

Si se resuelve una alerta, la alerta desaparece de Azure Portal. El motivo más probable es que alguien que recibe notificaciones de correo electrónico resolviera la alerta en el dominio administrado, o bien que Azure AD Domain Services la resolviera automáticamente.


#### <a name="why-can-i-not-edit-the-notification-settings"></a>¿Por qué no puedo editar la configuración de notificaciones?

Si no puede acceder a la página de configuración de notificaciones en Azure Portal, significa que no tiene los permisos para editar Azure AD Domain Services. Debe ponerse en contacto con el administrador global con el fin de obtener permisos para editar los recursos de Azure AD Domain Services o quitarlos de la lista de destinatarios.

#### <a name="i-dont-seem-to-be-receiving-email-notifications-even-though-i-provided-my-email-address-why"></a>Parece que no recibo notificaciones por correo electrónico, a pesar de que he proporcionado mi dirección de correo electrónico. ¿Por qué?

Compruebe la carpeta de correo no deseado para buscar la notificación y asegúrese de incluir en la lista blanca al remitente (azure-noreply@microsoft.com).

## <a name="next-steps"></a>Pasos siguientes
- [Resuelva alertas en el dominio administrado](active-directory-ds-troubleshoot-alerts.md)
- [Más información sobre Azure AD Domain Services](active-directory-ds-overview.md)
- [Póngase en contacto con el equipo de productos](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Ponerse en contacto con nosotros
Póngase en contacto con el equipo de productos de Active Directory Domain Services para [compartir comentarios u obtener asistencia](active-directory-ds-contact-us.md).
