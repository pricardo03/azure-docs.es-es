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
ms.openlocfilehash: 6c2204c572ca1f74f8060d0b6176df69359fe69a
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901380"
---
# <a name="bulk-delete-users-preview-in-azure-active-directory"></a>Eliminación masiva de usuarios (versión preliminar) en Azure Active Directory

Azure Active Directory (Azure AD) admite operaciones de creación y eliminación de usuarios en bloque, invitar usuarios en bloque y la descarga de listas de usuarios, grupos y miembros del grupo.

## <a name="to-bulk-delete-users"></a>Para eliminar usuarios en masa

1. Inicie sesión en la organización de Azure AD con una cuenta que sea la del administrador de usuarios de la organización.
1. En Azure AD, seleccione **Usuarios** > **Eliminación masiva**.
1. En la página **Eliminación masiva de usuarios**, seleccione **Descargar** para recibir un archivo .csv válido de propiedades de usuario y, a continuación, agregue los usuarios que quiere eliminar.
1. Cuando termine de editar el archivo .csv o si tiene uno propio listo para cargar, seleccione el archivo en **Cargue el archivo .csv** para validarlo.

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
