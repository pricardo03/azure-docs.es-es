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
ms.openlocfilehash: 08708c23f9c8f4c4a8fc9f2f0aa5cd20d8333a42
ms.sourcegitcommit: b03516d245c90bca8ffac59eb1db522a098fb5e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71146365"
---
# <a name="bulk-remove-group-members-preview-in-azure-active-directory"></a>Eliminación masiva de miembros de un grupo (versión preliminar) en Azure Active Directory

Mediante el portal de Azure Active Directory (Azure AD), puede quitar una gran cantidad de miembros de un grupo mediante el uso de un archivo de valores separados por comas (CSV) para quitar dichos miembros de forma masiva.

> [!NOTE]
> Las operaciones masivas de Azure AD son una característica en vista previa (GB) pública de Azure AD y están disponibles con cualquier plan de licencias de Azure AD de pago. Para obtener más información sobre los términos de uso de las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="to-bulk-remove-group-members"></a>Para quitar miembros de un grupo de forma masiva

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador de usuarios en la organización. Los propietarios de grupos también pueden quitar de forma masiva miembros de los grupos que poseen.
1. En Azure AD, seleccione **Grupos** > **Todos los grupos**.
1. Abra el grupo del que va a quitar miembros y luego seleccione **Miembros**.
1. En la página **Miembros**, seleccione **Quitar miembros**.
1. En la página **Eliminación masiva de los miembros del grupo**, seleccione **Descargar** para obtener la plantilla del archivo CSV con las propiedades de miembro de grupo requeridas.

   ![El comando Quitar miembros está en la página de perfil del grupo](./media/groups-bulk-remove-members/remove-panel.png)

1. Abra el archivo CSV y agregue una línea para cada miembro del grupo que quiera quitar del grupo (los valores necesarios son Id. de objeto de miembro o Nombre principal del usuario). A continuación, guarde el archivo.

   ![El archivo CSV contiene los nombres y los identificadores de los miembros que se quitarán.](./media/groups-bulk-remove-members/csv-file.png)

1. En la página **Eliminación masiva de los miembros del grupo (versión preliminar)** , en **Cargue el archivo csv**, vaya al archivo. Al seleccionarlo, comienza su validación.
1. Cuando finalice la validación del contenido del archivo, aparecerá el mensaje **Archivo cargado correctamente** en la página de importación en bloque. Si hay errores, debe corregirlos para poder enviar el trabajo.
1. Cuando el archivo supere la validación, seleccione **Enviar** para iniciar la operación masiva de Azure que quita los miembros del grupo.
1. Cuando la operación de supresión finalice, verá una notificación que indicará que la operación masiva se realizó correctamente.

## <a name="check-removal-status"></a>Comprobación del estado de eliminación

Puede ver el estado de todas las solicitudes masivas pendientes en la **Resultados de la operación masiva (versión preliminar)** .

   ![La página de resultados de la operación masiva muestra el estado de la solicitud masiva](./media/groups-bulk-remove-members/bulk-center.png)

Para obtener más información sobre cada elemento de línea de la operación masiva, seleccione los valores de las columnas **Número de elementos correctos**, **Número de errores** o **Total de solicitudes**. Si se produjeron errores, se mostrarán sus motivos.

## <a name="bulk-removal-service-limits"></a>Límites del servicio de eliminación masiva

La ejecución de cada actividad masiva para quitar una lista de miembros de grupo puede durar hasta una hora. Esto permite la eliminación de una lista de al menos 40 000 miembros.

## <a name="next-steps"></a>Pasos siguientes

- [Importación masiva de miembros de grupo](groups-bulk-import-members.md)
- [Descarga de miembros de un grupo](groups-bulk-download-members.md)
- [Descargar una lista de todos los grupos](groups-bulk-download.md)
