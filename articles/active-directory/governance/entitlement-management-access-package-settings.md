---
title: 'Compartición de un vínculo para solicitar un paquete de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo compartir un vínculo para solicitar un paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 91b98df1f93991d4781283bc38b02dc20d11268d
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391971"
---
# <a name="share-link-to-request-an-access-package-in-azure-ad-entitlement-management-preview"></a>Compartición de un vínculo para solicitar un paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La mayoría de los usuarios del directorio pueden iniciar sesión en el portal Mi acceso y ver automáticamente una lista de paquetes de acceso que pueden solicitar. Pero para los usuarios de socios comerciales externos que aún no están en el directorio, deberá enviarles un vínculo que pueden usar para solicitar un paquete de acceso. 

Siempre que el catálogo para el paquete de acceso esté [habilitado para los usuarios externos](entitlement-management-catalog-create.md) y tenga una [directiva para el directorio del usuario externo](entitlement-management-access-package-request-policy.md), este puede utilizar el vínculo del portal Mi acceso para solicitar el paquete de acceso.

## <a name="share-link-to-request-an-access-package"></a>Compartición de un vínculo para solicitar un paquete de acceso

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En la página Información general, copie el **vínculo del portal Mi acceso**.

    ![Información general del paquete acceso: vínculo del portal Mi acceso](./media/entitlement-management-shared/my-access-portal-link.png)

    Es importante que copie todo el vínculo al portal Mi acceso cuando lo envíe a un partner comercial interno. De este modo, se garantiza que el partner obtendrá acceso al portal del directorio para realizar su solicitud. El vínculo comienza con `myaccess`, incluye una sugerencia de directorio y termina con un identificador de paquete de acceso.

    `https://myaccess.microsoft.com/@<directory_hint>#/access-packages/<access_package_id>`

1. Envíe por correo electrónico o envíe el vínculo a su socio comercial externo. Este puede compartir el vínculo con sus usuarios para solicitar el paquete de acceso.

## <a name="next-steps"></a>Pasos siguientes

- [Solicitud de acceso a un paquete de acceso](entitlement-management-request-access.md)
- [Aprobación o denegación de solicitudes de acceso](entitlement-management-request-approve.md)