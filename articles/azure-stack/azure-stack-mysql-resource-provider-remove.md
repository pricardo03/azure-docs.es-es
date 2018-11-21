---
title: Eliminación del proveedor de recursos de MySQL en Azure Stack | Microsoft Docs
description: Obtenga información sobre cómo puede quitar el proveedor de recursos de MySQL de la implementación de Azure Stack.
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
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 7d3b0e179972464a1ed857c576ca8a7c8fc2e162
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686813"
---
# <a name="remove-the-mysql-resource-provider"></a>Eliminación del proveedor de recursos de MySQL

Antes de quitar el proveedor de recursos de MySQL, debe quitar todas las dependencias del proveedor. También necesitará una copia del paquete de implementación que se utilizó para instalar el proveedor de recursos.

  |Versión mínima de Azure Stack|MySQL RP, versión|
  |-----|-----|
  |Versión 1808 (1.1808.0.97)|[MySQL RP, versión 1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
  |Versión 1804 (1.0.180513.1)|[MySQL RP, versión 1.1.24.0](https://aka.ms/azurestackmysqlrp11240)
  |     |     |

## <a name="dependency-cleanup"></a>Limpieza de dependencias

Hay varias tareas de limpieza que debe realizar antes de ejecutar el script DeployMySqlProvider.ps1 para quitar el proveedor de recursos.

Los inquilinos de Azure Stack son responsables de las siguientes tareas de limpieza:

* Eliminar todas sus bases de datos del proveedor de recursos. (Eliminar las bases de datos de inquilino no elimina los datos).
* Anular el registro desde el espacio de nombres del proveedor.

El operador de Azure Stack es responsable de las siguientes tareas de limpieza:

* Eliminar los servidores host del adaptador de MySQL.
* Eliminar los planes que hacen referencia el adaptador de MySQL.
* Eliminar cualquier cuota asociada al adaptador de MySQL.

## <a name="to-remove-the-mysql-resource-provider"></a>Para quitar el proveedor de recursos de MySQL

1. Compruebe que ha quitado todas las dependencias existentes del proveedor de recursos de MySQL.

   >[!NOTE]
   >La desinstalación del proveedor de recursos de MySQL continuará incluso si los recursos dependientes utilizan el proveedor de recursos.
  
2. Obtenga una copia del binario del proveedor de recursos de MySQL y ejecute el extractor automático para extraer el contenido en un directorio temporal.
3. Obtenga una copia del binario del proveedor de recursos de SQL y ejecute el extractor automático para extraer el contenido en un directorio temporal.
4. Abra una nueva ventana de consola de PowerShell con privilegios elevados y cambie al directorio en el que extrajo los archivos binarios del proveedor de recursos de MySQL.
5. Ejecute el script DeployMySqlProvider.ps1 con los siguientes parámetros:
    - **Desinstalación**. Quita el proveedor de recursos y todos los recursos asociados.
    - **PrivilegedEndpoint**. Dirección IP o nombre DNS del punto de conexión con privilegios.
    - **AzureEnvironment**. El entorno de Azure usado para implementar Azure Stack. Requerido solo para implementaciones de Azure AD.
    - **CloudAdminCredential**. Credencial del administrador de la nube, necesaria para el acceso al punto de conexión con privilegios.
    - **DirectoryTenantID**
    - **AzCredential**. Credencial de la cuenta de administrador de servicio de Azure Stack. Use las mismas credenciales que para la implementación de Azure Stack.

## <a name="next-steps"></a>Pasos siguientes

[Oferta de App Services como PaaS](azure-stack-app-service-overview.md)
