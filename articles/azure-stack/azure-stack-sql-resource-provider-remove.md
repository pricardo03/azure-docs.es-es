---
title: Eliminación del proveedor de recursos de SQL en Azure Stack | Microsoft Docs
description: Obtenga información sobre cómo puede quitar el proveedor de recursos de SQL de la implementación de Azure Stack.
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
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 9f90201cad0f74923460c2f25eff4de98dc6690a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294787"
---
# <a name="removing-the-mysql-resource-provider"></a>Eliminación del proveedor de recursos de MySQL  
Antes de quitar el proveedor de recursos de SQL, es esencial quitar antes todas las dependencias.

## <a name="remove-the-mysql-resource-provider"></a>Eliminación del proveedor de recursos de MySQL 

1. Compruebe que ha quitado las dependencias del proveedor de recursos de SQL existentes.

  > [!NOTE]
  > La desinstalación del proveedor de recursos de SQL continuará incluso si los recursos dependientes utilizan el proveedor de recursos. 
  
2. Asegúrese de tener el paquete de implementación original que descargó para esta versión del adaptador del proveedor de recursos de SQL.
3. Vuelva a ejecutar el script de implementación con los siguientes elementos:
    - Uso del parámetro -Uninstall
    - Dirección IP o nombre DNS del punto de conexión con privilegios.
    - Credencial del administrador de la nube, necesaria para el acceso al punto de conexión con privilegios.
    - Credenciales de la cuenta de administrador de servicio de Azure Stack. Use las mismas credenciales que para la implementación de Azure Stack.

## <a name="next-steps"></a>Pasos siguientes
[Oferta de App Services como PaaS](azure-stack-app-service-overview.md)
