---
title: 'Solución de problemas de administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información acerca de algunos elementos que debe comprobarse para ayudar a solucionar problemas de administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/25/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ca7955186d6c6fb1dff2acd6a2d5badd1cedaef
ms.sourcegitcommit: 9ad75f83bbf0fc4623b7995794f33bbf823b31c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64541516"
---
# <a name="troubleshoot-azure-ad-entitlement-management-preview"></a>Solución de problemas de administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> Administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se describe algunos elementos que debe comprobarse para ayudar a solucionar problemas de administración de derechos de Azure Active Directory (Azure AD).

## <a name="checklist-for-entitlement-management-administration"></a>Lista de comprobación para la administración de la administración de derechos

* Si recibe un mensaje acceso denegado al configurar la administración de derechos, y es un administrador Global, asegúrese de que el directorio tiene un [licencia Azure AD Premium P2 (o EMS E5)](entitlement-management-overview.md#prerequisites).  
* Si recibe un acceso denegado mensaje al crear o ver los paquetes de acceso, y es miembro de un grupo de creador de catálogo, debe crear un catálogo antes de crear el primer paquete de acceso.

## <a name="checklist-for-adding-a-resource"></a>Lista de comprobación para agregar un recurso

* Si hay usuarios que ya se han asignado a un recurso que desee administrar con un paquete de acceso, asegúrese de que los usuarios están asignados al paquete acceso con una directiva adecuada. Por ejemplo, puede incluir un grupo en un paquete de acceso que ya tiene los usuarios en el grupo. Si esos usuarios en el vínculo es necesario grupo acceso continuación, deben tener una directiva adecuada para los paquetes de acceso para que no pierden el acceso al grupo. Puede asignar el paquete acceso solicitando a los usuarios para solicitar el paquete de acceso que contiene ese recurso, o mediante su asignación directa para el paquete de acceso. Para obtener más información, consulte [editar y administrar un paquete existente de acceso](entitlement-management-access-package-edit.md).

## <a name="checklist-for-request-issues"></a>Lista de comprobación para problemas de solicitud

* Cuando un usuario desea solicitar acceso a un paquete de acceso, asegúrese de que están utilizando la **vínculo del portal Mi acceso** para el paquete de acceso. Para obtener más información, consulte [vínculo del portal copia mi acceso](entitlement-management-access-package-edit.md#copy-my-access-portal-link).

* Cuando un usuario inicia sesión en el portal de mi acceso para solicitar un paquete de acceso, asegúrese de que se autentiquen con sus cuentas profesionales. La cuenta profesional puede ser una cuenta en el directorio de recursos o en un directorio que se incluye en una de las directivas del paquete de acceso. Si la cuenta de usuario no es una cuenta de organización o el directorio no está incluido en la directiva, el usuario no verá el paquete de acceso. Para obtener más información, consulte [solicitar acceso a un paquete acceso](entitlement-management-request-access.md).

* Si un usuario no podrá iniciar sesión en el directorio de recursos, no podrá solicitar acceso en el portal de mi acceso. Para que el usuario pueda solicitar acceso, debe quitar el bloque de inicio de sesión de perfil del usuario. Para quitar el bloque de inicio de sesión en Azure portal, haga clic en **Azure Active Directory**, haga clic en **usuarios**, haga clic en el usuario y, a continuación, haga clic en **perfil**. Editar el **configuración** sección y cambie **Bloquear inicio de sesión** a **No**. Para obtener más información, consulte [agregar o actualizar la información de perfil de un usuario con Azure Active Directory](../fundamentals/active-directory-users-profile-azure-portal.md)

* En el portal Mi acceso, si un usuario es un solicitante y aprobador, no verá su solicitud para un paquete de acceso en el **aprobaciones** página. Este comportamiento es deliberado: un usuario no puede aprobar su solicitud. Asegúrese de que el paquete de acceso están solicitando tiene aprobadores adicionales configurados en la directiva. Para obtener más información, consulte [editar una directiva existente](entitlement-management-access-package-edit.md#edit-an-existing-policy).

* Si un nuevo usuario externo, que no ha registrado anteriormente en el directorio, recibe un paquete de acceso, incluido un sitio de SharePoint Online, se mostrará su paquete de acceso tal como lo entrega no totalmente hasta que su cuenta se aprovisiona en SharePoint Online.

## <a name="next-steps"></a>Pasos siguientes

- [Ver informes de cómo los usuarios tenían acceso en la administración de derechos](entitlement-management-reports.md)
