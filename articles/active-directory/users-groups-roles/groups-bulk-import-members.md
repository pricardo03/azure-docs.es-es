---
title: 'Importación masiva para agregar miembros a un grupo: Azure Active Directory | Microsoft Docs'
description: Agregue miembros de grupo de forma masiva en el Centro de administración de Azure Active Directory.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 09/11/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0335522b398c22fb395305b33bac5a56ba565ee2
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910729"
---
# <a name="bulk-import-group-members-preview-in-azure-active-directory"></a>Importación masiva de los miembros del grupo (versión preliminar) en Azure Active Directory

Mediante el portal de Azure Active Directory (Azure AD), puede agregar una gran cantidad de miembros a un grupo mediante el uso de un archivo de valores separados por comas (CSV) para importar dichos miembros de forma masiva.

> [!NOTE]
> Las operaciones masivas de Azure AD son una característica en vista previa (GB) pública de Azure AD y están disponibles con cualquier plan de licencias de Azure AD de pago. Para más información sobre los términos de uso de las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="bulk-import-service-limits"></a>Límites del servicio de importación de forma masiva

La ejecución de cada actividad masiva para importar una lista de miembros de grupo puede durar hasta una hora. Esto permite la importación de una lista de al menos 40 000 miembros.

## <a name="to-bulk-import-group-members"></a>Para importar de forma masiva miembros de grupo

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador de usuarios en la organización. Los propietarios de grupos también pueden importar de forma masiva miembros de los grupos que poseen.
1. En Azure AD, seleccione **Grupos** > **Todos los grupos**.
1. Abra el grupo al que va a agregar miembros y luego seleccione **Miembros**.
1. En la página **Miembros**, seleccione **Importar miembros** para descargar, actualizar y cargar un archivo CSV en el que se enumeran los miembros que desea importar al grupo.

   ![El comando Importar miembros está en la página de perfil del grupo](./media/groups-bulk-import-members/import-panel.png)

## <a name="check-import-status"></a>Comprobación del estado de importación

Puede ver el estado de todas las solicitudes masivas pendientes en la **Resultados de la operación masiva (versión preliminar)** .

   ![La página Resultados de la operación masiva muestra el estado de la solicitud masiva](./media/groups-bulk-import-members/bulk-center.png)

## <a name="next-steps"></a>Pasos siguientes

- [Quitar miembros de un grupo de forma masiva](groups-bulk-remove-members.md)
- [Descarga de miembros de un grupo](groups-bulk-download-members.md)
