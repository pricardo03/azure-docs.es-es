---
title: Restauración masiva de usuarios (versión preliminar) en el portal de Azure Active Directory | Microsoft Docs
description: Restauración de usuarios en bloque en el Centro de administración de Azure AD de Azure Active Directory
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
ms.openlocfilehash: f53ade09c5e2e7db0499122526a1de482af9378f
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901352"
---
# <a name="bulk-restore-deleted-users-preview-in-azure-active-directory"></a>Restauración masiva de usuarios (versión preliminar) en Azure Active Directory

Azure Active Directory (Azure AD) admite operaciones de creación y eliminación de usuarios en bloque, invitar usuarios en bloque y la descarga de listas de usuarios, grupos y miembros del grupo.

## <a name="to-bulk-restore-users"></a>Para restaurar usuarios en masa

1. [Inicie sesión en la organización de Azure AD](https://aad.portal.azure.com) con una cuenta que sea la del administrador de usuarios de la organización.
1. En Azure AD, seleccione **Usuarios** > **Usuarios eliminados**.
1. En la página **Usuarios eliminados**, seleccione **Restauración masiva** para cargar un archivo .csv válido de las propiedades de los usuarios que se van a restaurar.

   ![Selección del comando de restauración masiva en la página Usuarios eliminados](./media/users-bulk-restore/bulk-restore.png)

1. Cuando termine de editar el archivo .csv o si tiene uno propio listo para cargar, seleccione el archivo en **Cargue el archivo .csv** para validarlo.

   ![Selección de un archivo .csv local en el que se indican los usuarios que quiere agregar](./media/users-bulk-restore/upload-button.png)

1. Cuando se valide el contenido del archivo, corrija la información del archivo y vuelva a enviarlo si hay errores. El envío de un archivo válido inicia el trabajo de carga de datos automáticamente.
1. Después de que el archivo pase la validación, seleccione **Enviar** para iniciar el trabajo por lotes de Azure que restaura los usuarios. Si hay errores, puede descargar y ver el archivo de resultados en la página de resultados de la operación masiva. El archivo contiene el motivo de cada error.

## <a name="check-status"></a>Comprobar estado

Puede ver el estado de todas las solicitudes masivas pendientes en la página de **resultados de la operación masiva (versión preliminar)** .

   ![Comprobación del estado de carga en la página de resultados de la operación masiva](./media/users-bulk-restore/bulk-center.png)

A continuación, puede comprobar si los usuarios restaurados existen en la organización de Azure AD en Azure Portal o mediante PowerShell.

## <a name="view-restored-users-in-the-azure-portal"></a>Visualización de los usuarios restaurados en Azure Portal

1. [Inicie sesión en el Centro de administración de Azure AD](https://aad.portal.azure.com) con una cuenta que sea la del administrador de usuarios de la organización.
1. En el panel de navegación, seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Usuarios**.
1. En **Mostrar**, seleccione **Todos los usuarios** y compruebe que se incluyen los usuarios restaurados.

### <a name="view-users-with-powershell"></a>Visualización de usuarios con PowerShell

Ejecute el siguiente comando:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Debería ver que se han incluido los usuarios restaurados.

## <a name="next-steps"></a>Pasos siguientes

- [Importación en bloque de usuarios](users-bulk-add.md)
- [Eliminación masiva de usuarios](users-bulk-delete.md)
- [Descarga de la lista de usuarios](users-bulk-download.md)
