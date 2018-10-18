---
title: Eliminación del proveedor de recursos de SQL en Azure Stack | Microsoft Docs
description: Obtenga información sobre cómo quitar el proveedor de recursos de SQL de la implementación de Azure Stack.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 42d8a5a8073d2650b9e023305472f28d4f1c738f
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580106"
---
# <a name="remove-the-sql-resource-provider"></a>Eliminación del proveedor de recursos de SQL

Antes de quitar el proveedor de recursos de SQL, debe quitar todas las dependencias del proveedor. También necesitará una copia del paquete de implementación que se utilizó para instalar el proveedor de recursos.

Hay varias tareas de limpieza que debe realizar antes de ejecutar el script _DeployMySqlProvider.ps1_ para quitar el proveedor de recursos.
Los inquilinos son responsables de las siguientes tareas de limpieza:

* Eliminar todas sus bases de datos del proveedor de recursos. (Eliminar las bases de datos de inquilino no elimina los datos).
* Anular el registro desde el espacio de nombres del proveedor de recursos.

El administrador es responsable de las siguientes tareas de limpieza:

* Eliminar los servidores de hospedaje del proveedor de recursos SQL.
* Eliminar los planes que hagan referencia al proveedor de recursos SQL.
* Eliminar las cuotas asociadas al proveedor de recursos SQL.

## <a name="to-remove-the-sql-resource-provider"></a>Proceso para quitar el proveedor de recursos de SQL

1. Compruebe que ha quitado todas las dependencias del proveedor de recursos de SQL existentes.

   > [!NOTE]
   > La desinstalación del proveedor de recursos de SQL continuará incluso si los recursos dependientes utilizan el proveedor de recursos.
  
2. Obtenga una copia del binario del proveedor de recursos de SQL y ejecute el extractor automático para extraer el contenido en un directorio temporal.

3. Abra una nueva ventana de consola de PowerShell con privilegios elevados y cambie al directorio en el que extrajo los archivos binarios del proveedor de recursos de SQL.

4. Ejecute el script DeploySqlProvider.ps1 con los siguientes parámetros:

    * **Desinstalación**. Quita el proveedor de recursos y todos los recursos asociados.
    * **PrivilegedEndpoint**. Dirección IP o nombre DNS del punto de conexión con privilegios.
    * **AzureEnvironment**. El entorno de Azure usado para implementar Azure Stack. Requerido solo para implementaciones de Azure AD.
    * **CloudAdminCredential**. Credencial del administrador de la nube, necesaria para el acceso al punto de conexión con privilegios.
    * **AzCredential**. Credencial de la cuenta de administrador de servicio de Azure Stack. Use las mismas credenciales que para la implementación de Azure Stack.

## <a name="next-steps"></a>Pasos siguientes

[Oferta de App Services como PaaS](azure-stack-app-service-overview.md)
