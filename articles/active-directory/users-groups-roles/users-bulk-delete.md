---
title: Eliminación masiva de usuarios (versión preliminar) en el portal de Azure Active Directory | Microsoft Docs
description: Eliminación masiva de usuarios en el Centro de administración de Azure de Azure Active Directory
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 08/15/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb01b46d61b6ba99c3ec9c537dccc350074f5e05
ms.sourcegitcommit: b03516d245c90bca8ffac59eb1db522a098fb5e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71146428"
---
# <a name="bulk-delete-users-preview-in-azure-active-directory"></a>Eliminación masiva de usuarios (versión preliminar) en Azure Active Directory

Mediante el portal de Azure Active Directory (Azure AD), puede quitar una gran cantidad de miembros a un grupo mediante el uso de un archivo de valores separados por comas (CSV) para eliminar usuarios de forma masiva.

## <a name="to-bulk-delete-users"></a>Para eliminar usuarios en masa

1. Inicie sesión en la organización de Azure AD con una cuenta que sea la del administrador de usuarios de la organización.
1. En Azure AD, seleccione **Usuarios** > **Eliminación masiva**.
1. En la página **Eliminación masiva de usuarios**, seleccione **Descargar** para recibir un archivo .csv válido de propiedades de usuario y, a continuación, agregue los usuarios que quiere eliminar.

   ![El archivo CSV contiene los nombres y los identificadores de los usuarios que se eliminarán.](./media/users-bulk-delete/delete-csv-file.png)

1. Cuando termine de editar el archivo .csv, seleccione el archivo en **Cargue el archivo .csv** para validarlo.

   ![Selección de un archivo .csv local en el que se indican los usuarios que quiere eliminar](./media/users-bulk-delete/bulk-delete.png)

1. Cuando se valida el contenido del archivo, debe corregir los errores para poder enviar el trabajo.
1. Cuando el archivo pase la validación, seleccione **Enviar** para iniciar el trabajo por lotes de Azure que elimina los usuarios. Si hay errores, puede descargar y ver el archivo de resultados en la página de resultados de la operación masiva. El archivo contiene el motivo de cada error.

## <a name="check-status"></a>Comprobar estado

Puede ver el estado de todas las solicitudes masivas pendientes en la página de **resultados de la operación masiva (versión preliminar)** .

   ![Comprobación del estado de carga en la página de resultados de la operación masiva](./media/users-bulk-delete/bulk-center.png)

A continuación, puede comprobar si los usuarios eliminados existen en la organización de Azure AD en Azure Portal o mediante PowerShell.

## <a name="verify-deleted-users-in-the-azure-portal"></a>Comprobación de usuarios eliminados en Azure Portal

1. Inicie sesión en Azure Portal con una cuenta que sea la del administrador de usuarios de la organización.
1. En el panel de navegación, seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Usuarios**.
1. En **Mostrar**, seleccione **Todos los usuarios** y compruebe que los usuarios eliminados ya no se incluyen.

### <a name="verify-deleted-users-with-powershell"></a>Comprobación de usuarios eliminados con PowerShell

Ejecute el siguiente comando:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Compruebe que los usuarios que ha eliminado ya no se incluyen.

## <a name="next-steps"></a>Pasos siguientes

- [Adición masiva de usuarios](users-bulk-add.md)
- [Descarga de la lista de usuarios](users-bulk-download.md)
- [Restauración de usuarios masiva](users-bulk-restore.md)
