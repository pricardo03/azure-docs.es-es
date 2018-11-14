---
title: Reactivación de los espacios de nombres deshabilitados de Azure Access Control Service (ACS)
description: Aprenda a buscar y habilitar los espacios de nombres de Azure Access Control Service (ACS) y solicite una extensión para mantenerlos habilitados hasta el 4 de febrero de 2019.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/07/2018
ms.author: celested
ms.reviewer: jlu
ms.custom: aaddev
ms.openlocfilehash: 298afda7549690a9ea0314bff63a714be50a33b9
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019976"
---
# <a name="how-to-reactivate-disabled-access-control-service-namespaces"></a>Reactivación de los espacios de nombres deshabilitados de Access Control Service

En noviembre de 2017, anunciamos que Microsoft Azure Access Control Service (ACS), un servicio de Azure Active Directory (Azure AD), se va a retirar el 7 de noviembre de 2018.

Desde entonces, hemos enviado varios correos electrónicos al correo electrónico de administración de las suscripciones de la ACS sobre la retirada de ACS 12 meses, 9 meses, 6 meses, 3 meses, 1 mes, 2 semanas, 1 semana y 1 día antes de la fecha de retirada del 7 de noviembre de 2018.

El 3 de octubre de 2018 anunciamos (mediante un correo electrónico y [una entrada de blog](https://azure.microsoft.com/blog/one-month-retirement-notice-access-control-service/)) una oferta de extensión para los clientes que no puedan terminar su migración antes del 7 de noviembre de 2018. El anuncio también incluye instrucciones para solicitar la extensión.

## <a name="why-your-namespace-is-disabled"></a>¿Por qué se deshabilita el espacio de nombres?

Si no ha optado por la extensión, empezaremos a deshabilitar los espacios de nombres de ACS a partir del 7 de noviembre de 2018. Si no recibió las comunicaciones y aún así desea optar por la extensión hasta el 4 de febrero de 2019, siga las instrucciones de las siguientes secciones.

> [!NOTE]
> Debe ser un administrador de la suscripción para ejecutar los comandos de PowerShell y solicitar una extensión.

## <a name="find-and-enable-your-acs-namespaces"></a>Búsqueda y habilitación de los espacios de nombres de ACS

Puede utilizar PowerShell de ACS para enumerar todos los espacios de nombre de ACS y reactivar los que se han desactivado.

1. Descargue e instale PowerShell de ACS:
    1. Vaya a la Galería de PowerShell y descargue [Acs.Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2).
    1. Instale el módulo:

        ```powershell
        Install-Module -Name Acs.Namespaces
        ```

    1. Obtenga una lista de todos los comandos posibles:

        ```powershell
        Get-Command -Module Acs.Namespaces
        ```

        Para obtener ayuda sobre un comando específico, ejecute:

        ```
        Get-Help [Command-Name] -Full
        ```
    
        donde `[Command-Name]` es el nombre del comando de ACS.
1. Enumere las suscripciones de Azure disponibles con el cmdlet **Get AcsSubscription**.
1. Enumere los espacios de nombres de ACS con el cmdlet **Get AcsNamespace**.
1. Confirme que los espacios de nombres están deshabilitados mediante la confirmación de que `State` es `Disabled`.

    [![Confirmación de que los espacios de nombres están deshabilitados](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png)](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png#lightbox)

    También puede usar `nslookup {your-namespace}.accesscontrol.windows.net` para confirmar si el dominio está todavía activo.

1. Habilite los espacios de nombres de ACS con el cmdlet **Enable-AcsNamespace**.

    Una vez que haya habilitado los espacios de nombres, puede solicitar una extensión para que los espacios de nombres no vuelvan a deshabilitarse antes del 4 de febrero de 2019. Después de esa fecha, se producirá un error en todas las solicitudes a ACS.

## <a name="request-an-extension"></a>Solicitud de una extensión

1. Vaya al portal de administración del espacio de nombres de ACS accediendo a `https://{your-namespace}.accesscontrol.windows.net`.
1. Seleccione el botón **Leer términos** para leer los [Términos de uso actualizados](https://azure.microsoft.com/support/legal/access-control/), que le llevarán a una página con dichos términos de uso actualizados.

    [![Selección del botón Leer términos](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png#lightbox)

1. Seleccione **Solicitar extensión** en el banner en la parte superior de la página. Solo se habilitará el botón después de leer los [Términos de uso actualizados](https://azure.microsoft.com/support/legal/access-control/).

    [![Selección del botón Solicitar extensión](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png#lightbox)

1. Después de registrar la solicitud de extensión, la página se actualizará con un nuevo banner en la parte superior de la página.

    [![Página actualizada con banner actualizado](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png#lightbox)

## <a name="help-and-support"></a>Ayuda y soporte técnico

- Si se encuentra con algún problema después de seguir este procedimiento, póngase en contacto con el [soporte técnico de Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
- Si tiene preguntas o comentarios sobre la retirada de ACS, póngase en contacto con nosotros en acsfeedback@microsoft.com.

## <a name="next-steps"></a>Pasos siguientes

- Revise la información sobre la retirada de ACS en [Procedimiento para la migración desde Azure Access Control Service](active-directory-acs-migration.md).
