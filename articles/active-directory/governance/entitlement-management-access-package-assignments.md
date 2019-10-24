---
title: 'Consulta, incorporación y eliminación de asignaciones para un paquete de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Aprenda a consultar, agregar y eliminar asignaciones de un paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.openlocfilehash: 694c8866a69d8488511be1670ba3d3013a4a3423
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72392027"
---
# <a name="view-add-and-remove-assignments-for-an-access-package-in-azure-ad-entitlement-management-preview"></a>Consulta, incorporación y eliminación de asignaciones para un paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En la administración de derechos de Azure AD, puede ver a quién se le han asignado los paquetes de acceso, su directiva y su estado. Si un paquete de acceso tiene una directiva adecuada, también puede asignar directamente al usuario un paquete de acceso. En este artículo se explica cómo consultar, agregar y eliminar asignaciones en un paquete de acceso.

## <a name="view-who-has-an-assignment"></a>Ver quién tiene una asignación

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Asignaciones** para ver una lista de las asignaciones activas.

    ![Lista de asignaciones a un paquete de acceso](./media/entitlement-management-access-package-assignments/assignments-list.png)

1. Haga clic en una asignación específica para ver detalles adicionales.

1. Para ver una lista de asignaciones que no tenían aprovisionados correctamente todos los roles de recursos, haga clic en el filtro de estado y seleccione **Entrega**.

    Puede ver detalles adicionales sobre los errores de entrega localizando la solicitud del usuario correspondiente en la página **Solicitudes**.

1. Para ver las asignaciones expiradas, haga clic en el filtro de estado y seleccione **Expiradas**.

1. Para descargar un archivo CSV de la lista filtrada, haga clic en **Descargar**.

## <a name="directly-assign-a-user"></a>Asignar directamente un usuario

En algunos casos, es posible que quiera asignar directamente usuarios específicos a un paquete de acceso para que los usuarios no tengan que pasar por el proceso de solicitar el paquete de acceso. Para asignar directamente usuarios, el paquete de acceso debe tener una directiva que permita las asignaciones directas de administrador.

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En el menú de la izquierda, haga clic en **Asignaciones**.

1. Haga clic en **Nueva asignación** para abrir Add user to access package (Agregar usuario al paquete de acceso).

    ![Asignaciones - Add user to access package (Agregar usuario al paquete de acceso)](./media/entitlement-management-access-package-assignments/assignments-add-user.png)

1. Haga clic en **Agregar usuarios** para seleccionar los usuarios a los que quiere asignar el paquete de acceso.

1. En la lista **Seleccionar directiva**, seleccione una directiva que tenga la configuración [Ninguno (solo para las asignaciones directas del administrador)](entitlement-management-access-package-request-policy.md#none-administrator-direct-assignments-only).

    Si este paquete de acceso no tiene este tipo de directiva, haga clic en **Crear nueva directiva** para agregar una.

1. Establezca la fecha y hora en la que quiere que comience y finalice la asignación de los usuarios seleccionados. Si no se proporciona una fecha de finalización, se utilizará la configuración del ciclo de vida de la directiva.

1. Opcionalmente, proporcione una justificación para la asignación directa para mantener un registro.

1. Haga clic en **Agregar** para asignar directamente los usuarios seleccionados al paquete acceso.

    Transcurridos unos instantes, haga clic en **Actualizar** para ver los usuarios en la lista Asignaciones.

## <a name="next-steps"></a>Pasos siguientes

- [Modificar la configuración de solicitud de un paquete de acceso](entitlement-management-access-package-request-policy.md)
- [Ver informes y registros](entitlement-management-reports.md)
