---
title: 'Revisión de acceso de grupos o aplicaciones: Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo realizar una revisión de acceso de los miembros del grupo o el acceso a la aplicación de revisiones de acceso de Azure Active Directory.
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
ms.date: 05/22/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: bae204ec1789f227150adc560d4a292404d23b7e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113287"
---
# <a name="complete-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Revisión de acceso de los grupos o las revisiones de acceso de las aplicaciones de Azure AD

Como administrador, [crear una revisión de acceso de grupos o aplicaciones](create-access-review.md) y revisores [realizar la revisión de acceso](perform-access-review.md). En este artículo se describe cómo ver los resultados de la revisión de acceso y se aplican los resultados.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="prerequisites"></a>Requisitos previos

- Azure AD Premium P2
- Administrador global, Administrador de usuarios, Administrador de seguridad o lector de seguridad

Para obtener más información, consulte [qué usuarios deben tener licencias?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="view-an-access-review"></a>Ver una revisión de acceso

Puede seguir el progreso como los revisores las realizan.

1. Inicie sesión en Azure portal y abra el [página de revisiones de acceso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. En el menú izquierdo, haga clic en **las revisiones de acceso**.

1. En la lista, haga clic en una revisión de acceso.

    Para ver una serie de acceso revisiones, vaya a la revisión de acceso y encontrará futuras repeticiones en las revisiones programada.

    En el **Introducción** página, puede ver el progreso. Derechos de acceso no se cambian en el directorio hasta que se complete la revisión.

    ![Progreso de revisiones de acceso](./media/complete-access-review/overview-progress.png)

1. Si desea detener una revisión de acceso antes ha alcanzado la fecha de finalización programada, haga clic en el **detener** botón.

    Al detener una revisión, los revisores ya no podrán proporcionar respuestas. No puede reiniciar una revisión una vez detenida.

1. Si ya no está interesado en la revisión de acceso, puede eliminarlo haciendo clic en el **eliminar** botón.

## <a name="apply-the-changes"></a>Aplicación de cambios

Si **aplicar automáticamente los resultados al recurso** estuviera habilitado y según las selecciones realizadas en **configuración de finalización**, auto-aplicar will se ejecutará después de la fecha de finalización de la revisión o al detener manualmente la revisión.

Si **aplicar automáticamente los resultados al recurso** no estaba habilitado para la revisión, haga clic en **aplicar** para aplicar los cambios manualmente. Si se denegó el acceso de un usuario en la revisión, al hacer clic en **aplicar**, Azure AD quitará la asignación de pertenencia o aplicación.

![Aplicar cambios de la revisión de acceso](./media/complete-access-review/apply-changes.png)

El estado de la revisión cambiará de **completado** a través de los estados intermedios como **Applying** y, finalmente, al estado **resultado aplicado**. Debería esperar ver a los usuarios denegados, si es que los hay, eliminados de la pertenencia al grupo o la asignación de aplicaciones en unos minutos.

Una revisión de aplicación automática configurada o la selección de **Aplicar** no tiene ningún efecto en un grupo que se origina en un directorio local o en un grupo dinámico. Si desea cambiar un grupo que se origina en un directorio local, descargue los resultados y aplique esos cambios a la representación del grupo en ese directorio.

## <a name="retrieve-the-results"></a>Recuperación de los resultados

Para ver los resultados de una revisión de acceso de un solo uso, haga clic en el **resultados** página. Para ver solo acceso de un usuario, en el cuadro de búsqueda, escriba el nombre para mostrar o nombre principal de usuario de un usuario cuyo acceso se ha revisado.

![Recuperar los resultados de una revisión de acceso](./media/complete-access-review/retrieve-results.png)

Para ver el progreso de una revisión de acceso activa que se repite, haga clic en el **resultados** página.

Para ver los resultados de una instancia completada de una revisión de acceso que se repite, haga clic en **revisar el historial**, a continuación, seleccione la instancia específica de la lista de instancias de revisiones de acceso completa, en función de inicio de la instancia y la fecha de finalización. Los resultados de esta instancia pueden obtenerse a partir del **resultados** página.

Para recuperar todos los resultados de una revisión de acceso, haga clic en el **descargar** botón. El archivo CSV resultante puede verse en Excel o en otros programas que abren archivos CSV codificados en UTF-8.

## <a name="remove-users-from-an-access-review"></a>Quitar usuarios de una revisión de acceso

 De forma predeterminada, un usuario eliminado permanecerá eliminado en Azure AD durante 30 días, tiempo durante el cual un administrador puede restaurarlo si es necesario.  Después de 30 días, ese usuario se eliminará definitivamente.  Además, mediante el portal de Azure Active Directory, un administrador global puede [eliminar permanentemente un usuario eliminado recientemente](../fundamentals/active-directory-users-restore.md) explícitamente antes de que se alcance ese período de tiempo.  Una vez que un usuario se ha eliminado permanentemente, los datos posteriores sobre ese usuario se eliminarán de las revisiones de acceso activas.  La información de auditoría de los usuarios eliminados se conserva en el registro de auditoría.

## <a name="next-steps"></a>Pasos siguientes

- [Administración del acceso de los usuarios con las revisiones de acceso de Azure AD](manage-user-access-with-access-reviews.md)
- [Administración del acceso de los invitados con las revisiones de acceso de Azure AD](manage-guest-access-with-access-reviews.md)
- [Creación de una revisión de acceso de grupos o aplicaciones](create-access-review.md)
- [Creación de una revisión de acceso de los usuarios en un rol administrativo de Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md)
