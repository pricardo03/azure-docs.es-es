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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 150d1c40463aa04527bdd6e356a4c24ef68b02ef
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301905"
---
# <a name="remove-the-sql-resource-provider"></a>Eliminación del proveedor de recursos de SQL

Antes de quitar el proveedor de recursos de SQL, debe quitar todas las dependencias del proveedor. También necesitará una copia del paquete de implementación que se utilizó para instalar el proveedor de recursos.

## <a name="to-remove-the-sql-resource-provider"></a>Proceso para quitar el proveedor de recursos de SQL

1. Compruebe que ha quitado todas las dependencias del proveedor de recursos de SQL existentes.

   > [!NOTE]
   > La desinstalación del proveedor de recursos de SQL continuará incluso si los recursos dependientes utilizan el proveedor de recursos.
  
2. Obtenga una copia del binario del proveedor de recursos de SQL y ejecute el extractor automático para extraer el contenido en un directorio temporal.

3. Abra una nueva ventana de consola de PowerShell con privilegios elevados y cambie al directorio en el que extrajo los archivos binarios del proveedor de recursos de SQL.

4. Ejecute el script DeploySqlProvider.ps1 con los siguientes parámetros:

    - **Desinstalación**. Quita el proveedor de recursos y todos los recursos asociados.
    - **PrivilegedEndpoint**. Dirección IP o nombre DNS del punto de conexión con privilegios.
    - **CloudAdminCredential**. Credencial del administrador de la nube, necesaria para el acceso al punto de conexión con privilegios.
    - **AzCredential**. Credencial de la cuenta de administrador de servicio de Azure Stack. Use las mismas credenciales que para la implementación de Azure Stack.

## <a name="next-steps"></a>Pasos siguientes

[Oferta de App Services como PaaS](azure-stack-app-service-overview.md)
