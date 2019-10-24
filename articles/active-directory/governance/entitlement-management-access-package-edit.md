---
title: 'Ocultación o eliminación de un paquete de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Aprenda a ocultar o eliminar un paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.openlocfilehash: ab6f3a86d8e88ee2d7465fd32bde7b7555f1f326
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389179"
---
# <a name="hide-or-delete-an-access-package-in-azure-ad-entitlement-management-preview"></a>Ocultación o eliminación de un paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

De forma predeterminada, los paquetes de acceso son detectables. Esto significa que si una directiva permite que un usuario solicite el paquete de acceso, verá automáticamente que el paquete de acceso se muestra en el portal Mi acceso. Sin embargo, puede cambiar la configuración de **Oculto** de forma que el paquete de acceso no aparezca en el portal Mi acceso del usuario.

En este artículo se describe cómo ocultar o eliminar un paquete de acceso.

## <a name="change-the-hidden-setting"></a>Cambiar el valor Oculto

Siga estos pasos para cambiar la configuración de **Oculto** de un paquete de acceso.

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En la página Información general, haga clic en **Editar**.

1. Configure el valor **Oculto**.

    Si está establecido en **No**, el paquete de acceso se mostrará en el portal Mi acceso del usuario.

    Si está establecido en **Sí**, el paquete de acceso no se mostrará en el portal Mi acceso del usuario. La única manera que un usuario puede ver el paquete de acceso es si tiene el **vínculo directo del portal Mi acceso** al paquete acceso. Para más información, consulte [Uso compartido de un vínculo para solicitar un paquete de acceso](entitlement-management-access-package-settings.md).

## <a name="delete-an-access-package"></a>Eliminación de un paquete de acceso

Solo se puede eliminar un paquete de acceso si no tiene ninguna asignación de usuarios activa. Siga estos pasos para eliminar un paquete de acceso.

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En el menú de la izquierda, haga clic en **Asignaciones** y quite el acceso para todos los usuarios.

1. En el menú de la izquierda, haga clic en **Información general** y luego en **Eliminar**.

1. En el mensaje de eliminación que aparece, haga clic en **Sí**.

## <a name="next-steps"></a>Pasos siguientes

- [Visualización, adición y eliminación de asignaciones en un paquete de acceso](entitlement-management-access-package-assignments.md)
- [Visualización de informes y registros](entitlement-management-reports.md)
