---
title: Descargar una lista de grupos en el portal de Azure Active Directory | Microsoft Docs
description: Descargue las propiedades de grupo de forma masiva en el centro de administración de Azure en Azure Active Directory.
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
ms.openlocfilehash: b1d0f94e9cf9b91bc365586dafc147dc73b17876
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70914750"
---
# <a name="bulk-download-a-list-of-groups-preview-in-azure-active-directory"></a>Descarga masiva de una lista de grupos (versión preliminar) en Azure Active Directory

Mediante el portal de Azure Active Directory (Azure AD), puede descargar de forma masiva la lista de todos los grupos de su organización en un archivo de valores separados por comas (CSV).

> [!NOTE]
> Las operaciones masivas de Azure AD son una característica en vista previa (GB) pública de Azure AD y están disponibles con cualquier plan de licencias de Azure AD de pago. Para obtener más información sobre los términos de uso de las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="bulk-download-service-limits"></a>Límites del servicio de descarga masiva

La ejecución de cada actividad masiva para descargar una lista de grupos puede durar hasta una hora. Esto permite la descarga de una lista de al menos 300 000 grupos.

## <a name="to-download-a-list-of-groups"></a>Descargar una lista de grupos

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador en la organización.
1. En Azure AD, seleccione **Grupos** > **Descargar grupos**.
1. En la página de **descarga de grupos**, seleccione **Iniciar** para recibir un archivo CSV que enumere los grupos.

   ![El comando para descargar grupos está en la página Todos los grupos](./media/groups-bulk-download/bulk-download.png)

## <a name="check-download-status"></a>Comprobar el estado de la descarga

Puede ver el estado de todas las solicitudes masivas pendientes en la página de **resultados de la operación masiva (versión preliminar)** .

   ![La página de resultados de la operación masiva muestra el estado de la solicitud masiva](./media/groups-bulk-download/bulk-center.png)

## <a name="next-steps"></a>Pasos siguientes

- [Quitar miembros de un grupo de forma masiva](groups-bulk-remove-members.md)
- [Descarga de miembros de un grupo](groups-bulk-download-members.md)
