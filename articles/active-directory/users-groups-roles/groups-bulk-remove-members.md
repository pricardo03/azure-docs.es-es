---
title: 'Eliminación masiva de los miembros de un grupo mediante la carga de un archivo csv: Azure Active Directory | Microsoft Docs'
description: Agregue usuarios de forma masiva en el Centro de administración de Azure.
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
ms.openlocfilehash: b153db3570f10ad5ad130dedd0bd20fe22776ed6
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910715"
---
# <a name="bulk-remove-group-members-preview-in-azure-active-directory"></a>Eliminación masiva de miembros de un grupo (versión preliminar) en Azure Active Directory

Mediante el portal de Azure Active Directory (Azure AD), puede quitar una gran cantidad de miembros de un grupo mediante el uso de un archivo de valores separados por comas (CSV) para quitar dichos miembros de forma masiva.

> [!NOTE]
> Las operaciones masivas de Azure AD son una característica en vista previa (GB) pública de Azure AD y están disponibles con cualquier plan de licencias de Azure AD de pago. Para más información sobre los términos de uso de las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="bulk-removal-service-limits"></a>Límites del servicio de eliminación masiva

La ejecución de cada actividad masiva para quitar una lista de miembros de grupo puede durar hasta una hora. Esto permite la eliminación de una lista de al menos 40 000 miembros.

## <a name="to-bulk-remove-group-members"></a>Para quitar miembros de un grupo de forma masiva

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador de usuarios en la organización. Los propietarios de grupos también pueden quitar de forma masiva miembros de los grupos que poseen.
1. En Azure AD, seleccione **Grupos** > **Todos los grupos**.
1. Abra el grupo del que va a quitar miembros y luego seleccione **Miembros**.
1. En la página **Miembros**, seleccione **Quitar miembros** para descargar, actualizar y cargar un archivo CSV en el que se enumeran los miembros que desea quitar del grupo.

   ![El comando Quitar miembros está en la página de perfil del grupo](./media/groups-bulk-remove-members/remove-panel.png)

## <a name="check-removal-status"></a>Comprobación del estado de eliminación

Puede ver el estado de todas las solicitudes masivas pendientes en la **Resultados de la operación masiva (versión preliminar)** .

   ![La página Resultados de la operación masiva muestra el estado de la solicitud masiva](./media/groups-bulk-remove-members/bulk-center.png)

## <a name="next-steps"></a>Pasos siguientes

- [Importación masiva de miembros de grupo](groups-bulk-import-members.md)
- [Descarga de miembros de un grupo](groups-bulk-download-members.md)
