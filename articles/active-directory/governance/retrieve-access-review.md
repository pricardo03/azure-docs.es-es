---
title: Recuperar los resultados de la revisión de acceso para grupos o aplicaciones en las revisiones de acceso de Azure AD | Microsoft Docs
description: Obtenga información sobre cómo recuperar los resultados de la revisión de acceso para los miembros del grupo o el acceso a la aplicación de revisiones de acceso de Azure AD.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2516e61b6b86c3d2cee164cfcb80a1f938922908
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57839651"
---
# <a name="retrieve-access-review-results-for-groups-or-applications-in-azure-ad-access-reviews"></a>Recuperar los resultados de la revisión de acceso para grupos o aplicaciones en las revisiones de acceso de Azure AD

Los administradores pueden usar Azure Active Directory (Azure AD) para [crear una revisión de acceso](create-access-review.md) para miembros de grupo o usuarios asignados en una aplicación.  Un usuario que se encuentra en la **administrador Global**, **usuario administrador**, **Administrador de seguridad** o **lector de seguridad** rol también puede Lea los resultados de una revisión de acceso.  Para asignar usuarios a uno de estos roles, un administrador de roles con privilegios puede usar PIM de Azure AD para que se pueda activar el rol de un usuario o un administrador Global puede permanentemente [asignar un usuario a la función](../fundamentals/active-directory-users-assign-role-azure-portal.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="locating-an-access-review"></a>Búsqueda de una revisión de acceso

Si sabe qué programa contiene la revisión de acceso, vaya a la página de revisiones de acceso, seleccione **Programas** y seleccione el programa que contenga el control de revisión de acceso.  A continuación, seleccione **Controles** y seleccione el control de revisión de acceso. Si hay muchos controles en el programa, puede filtrar para los controles de un tipo específico y ordenar por estado. También puede buscar por el nombre del control de revisión de acceso o el nombre para mostrar del propietario que lo creó. 

Si no sabe qué programa contiene la revisión de acceso, vaya a la página de revisiones de acceso y seleccione **Controles**.  Puede filtrar solo los controles de un tipo específico y ordenar por su estado o también puede buscar por el nombre del control de revisión de acceso o por el nombre para mostrar del propietario que lo creó. 

## <a name="retrieving-the-results-for-a-one-time-access-review"></a>Recuperación de los resultados de una revisión de acceso de una única vez

Si el tipo de periodicidad de la revisión es una única vez, se pueden obtener el progreso de una revisión de acceso activa y los resultados de una revisión de acceso completa en la sección **Resultados**.  Puede escribir el nombre para mostrar o el nombre principal de usuario de un usuario cuyo acceso se está revisando para ver solo el acceso de ese usuario.  Para recuperar todos los resultados de una revisión de acceso completa, haga clic en el botón **Descargar**.

## <a name="retrieving-the-results-for-multiple-instances-of-a-recurring-access-review"></a>Recuperación de los resultados de varias instancias de una revisión de acceso periódica

Para ver el progreso de una revisión de acceso activa que es periódica, haga clic en la sección **Resultados**.  Puede escribir el nombre para mostrar o el nombre principal de usuario de un usuario cuyo acceso se está revisando.

Para ver los resultados de una instancia finalizada de una revisión de acceso que sea recurrente, seleccione **Revisar el historial** y, a continuación, seleccione la instancia específica de la lista de instancias de la revisión de acceso finalizada, en función de las fechas inicial y final de la instancia.   Se pueden obtener los resultados de esta instancia en la sección **Resultados**.  Puede escribir el nombre para mostrar o el nombre principal de usuario de un usuario cuyo acceso se está revisando para ver solo el acceso de ese usuario.  Para recuperar todos los resultados de una instancia finalizada de una revisión de acceso periódica, haga clic en el botón **Descargar**.


## <a name="removing-users-from-an-access-review"></a>Eliminación de usuarios de una revisión de acceso

De forma predeterminada, un usuario eliminado permanecerá eliminado en Azure AD durante 30 días, tiempo durante el cual un administrador puede restaurarlo si es necesario.  Después de 30 días, ese usuario se eliminará definitivamente.  Además, mediante el portal de Azure Active Directory, un administrador global puede [eliminar permanentemente un usuario eliminado recientemente](../fundamentals/active-directory-users-restore.md) explícitamente antes de que se alcance ese período de tiempo.  Una vez que un usuario se ha eliminado permanentemente, los datos posteriores sobre ese usuario se eliminarán de las revisiones de acceso activas.  La información de auditoría de los usuarios eliminados se conserva en el registro de auditoría.

## <a name="next-steps"></a>Pasos siguientes

- [Administración del acceso de los usuarios con las revisiones de acceso de Azure AD](manage-user-access-with-access-reviews.md)
- [Administración del acceso de los invitados con las revisiones de acceso de Azure AD](manage-guest-access-with-access-reviews.md)
- [Administración de los programas y los controles para las revisiones de acceso de Azure AD](manage-programs-controls.md)
- [Creación de una revisión de acceso de grupos o aplicaciones](create-access-review.md)
- [Creación de una revisión de acceso de los usuarios en un rol administrativo de Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md)


