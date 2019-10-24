---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: msaburnley
ms.service: active-directory
ms.topic: include
ms.date: 10/15/2019
ms.author: ajburnle
ms.custom: include file
ms.openlocfilehash: 88e11adadcc06875964146ad2046828267258038
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389375"
---
## <a name="lifecycle"></a>Ciclo de vida

En la sección **Ciclo de vida**, especificará cuándo expira la asignación de un usuario para el paquete de acceso. También puede especificar si los usuarios pueden extender sus asignaciones.

1. En la sección **Expiración**, establezca **Access package assignments expires** (La asignación de paquetes de acceso expira) en **En la fecha**, **Número de días** o **Nunca**.

    Para la opción **En la fecha**, seleccione una fecha de expiración en el futuro.

    Para la opción **Número de días**, especifique un número entre 0 y 3660 días.

    En función de lo que seleccione, la asignación de un usuario para el paquete de acceso expirará en una fecha concreta, un número determinado de días después de la aprobación o nunca.

1. Haga clic en **Mostrar configuración de expiración avanzada** para mostrar la configuración adicional.

    ![Paquete de acceso - Configuración de la expiración de la directiva](./media/active-directory-entitlement-management-lifecycle-policy/expiration.png)

1. Para permitir que el usuario amplíe sus asignaciones, establezca **Permitir que los usuarios extiendan el acceso** en **Sí**.

    Si se permiten las extensiones en la directiva, el usuario recibirá un correo electrónico 14 días y 1 día antes de que se establezca la expiración de su asignación de paquete de acceso en el que se le solicitará que amplíe la asignación.

    Si se extiende el acceso de un usuario, no podrán solicitar el paquete de acceso después de la fecha de extensión especificada (la fecha está establecida en la zona horaria del usuario que ha creado la directiva).

1. Para requerir la aprobación para conceder una extensión, establezca **Requerir aprobación para conceder extensión** en **Sí**.

    Se utilizará la misma configuración de aprobación que se especificó en la pestaña **Solicitudes**.

1. Haga clic en **Siguiente** o en **Actualizar**.
