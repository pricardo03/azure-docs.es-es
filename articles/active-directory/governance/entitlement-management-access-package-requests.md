---
title: Consulta y administración de solicitudes para un paquete de acceso en la administración de derechos de Azure AD (versión preliminar) - Azure Active Directory
description: Aprenda a consultar, a volver a procesar y a cancelar solicitudes de un paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.openlocfilehash: f7211fea4781904b9d97428ecf00b57970e4b57d
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72430284"
---
# <a name="view-and-manage-requests-for-an-access-package-in-azure-ad-entitlement-management-preview"></a>Consulte y administre solicitudes de un paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En la administración de derechos de Azure AD, puede ver quién ha solicitado los paquetes de acceso, su directiva y su estado. En este artículo, se explica cómo puede consultar, volver a procesar y cancelar solicitudes de un paquete de acceso.

## <a name="view-requests"></a>Ver solicitudes

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Solicitudes**.

1. Haga clic en una solicitud específica para ver detalles adicionales.

    ![Lista de solicitudes de un paquete de acceso](./media/entitlement-management-access-package-requests/requests-list.png)

## <a name="view-a-requests-delivery-errors"></a>Visualización de errores de entrega de una solicitud

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Solicitudes**.

1. Seleccione la solicitud que quiere ver.

    Si la solicitud tiene errores de entrega, su estado será **Undelivered** (Sin enviar) y su subestado, **Entregado parcialmente**.

    Si hay errores de entrega, en el panel de detalles de la solicitud, habrá un recuento de los errores de entrega.

1. Haga clic en el recuento para ver todos los errores de entrega de la solicitud.

## <a name="reprocess-a-request"></a>Nuevo procesamiento de una solicitud

Si se produce un error en una solicitud, puede intentar procesarla de nuevo. Solo se pueden volver a procesar las solicitudes que tienen el estado **Error en la entrega** o **Entregado parcialmente** y una fecha de finalización inferior a una semana.

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Solicitudes**.

1. Haga clic en la solicitud que desee procesar de nuevo.

1. En el panel de detalles de la solicitud, haga clic en **Volver a procesar solicitud**.

    ![Nuevo procesamiento de una solicitud que no se ha ejecutado correctamente](./media/entitlement-management-access-package-requests/reprocess-request.png)

## <a name="cancel-a-pending-request"></a>Cancelación de una solicitud pendiente

Solo se pueden cancelar las solicitudes pendientes que aún no se hayan entregado o en cuya entrega se hayan producido errores.

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Solicitudes**.

1. Haga clic en la solicitud que desee cancelar.

1. En el panel de detalles de la solicitud, haga clic en **Cancelar solicitud**.

## <a name="next-steps"></a>Pasos siguientes

- [Modificación de la configuración de solicitudes y aprobación de un paquete de acceso](entitlement-management-access-package-request-policy.md)
- [Visualización, incorporación y eliminación de asignaciones en un paquete de acceso](entitlement-management-access-package-assignments.md)