---
title: Ejecución de la instancia de Azure Functions desde un paquete | Microsoft Docs
description: Para que el sistema en tiempo de ejecución de Azure Functions ejecute sus funciones, monte un archivo del paquete de implementación que contenga los archivos de proyecto de la aplicación de función.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
editor: ''
tags: ''
ms.service: functions
ms.devlang: multiple
ms.topic: conceptual
ms.workload: na
ms.date: 08/22/2018
ms.author: glenga
ms.openlocfilehash: 2b9cc3618bf21eac268e3c25f08b80124d52e6af
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669298"
---
# <a name="run-your-azure-functions-from-a-package-file"></a>Ejecución de la instancia de Azure Functions desde un archivo de paquete

> [!NOTE]
> La funcionalidad descrita en este artículo está actualmente en versión preliminar; no está disponible para Functions en Linux.

En Azure, puede ejecutar las funciones directamente desde un archivo del paquete de implementación de la aplicación de función. La otra opción consiste en implementar los archivos de proyecto de la función en el directorio `d:\home\site\wwwroot` de la aplicación de función.

En este artículo se describen las ventajas de ejecutar las funciones desde un paquete. También se muestra cómo habilitar esta funcionalidad en la aplicación de función.

## <a name="benefits-of-running-from-a-package-file"></a>Ventajas de la ejecución desde un archivo de paquete
  
Son varias las ventajas de ejecutar las funciones desde un archivo de paquete:

+ Se reduce el riesgo de problemas de bloqueo de copia de archivos.
+ Se pueden implementar en una aplicación de producción (con reinicio).
+ Puede estar seguro de los archivos que se ejecutan en la aplicación.
+ Mejora el rendimiento de las [implementaciones de Azure Resource Manager](functions-infrastructure-as-code.md).
+ Puede reducir el tiempo de arranque en frío de las funciones de JavaScript.

Para más información, consulte [este anuncio](https://github.com/Azure/app-service-announcements/issues/84).

## <a name="enabling-functions-to-run-from-a-package"></a>Ejecución de las funciones desde un paquete

Para permitir la ejecución de la aplicación de función desde un paquete, solo tiene que agregar un valor `WEBSITE_RUN_FROM_ZIP` a la configuración de la aplicación de función. El valor `WEBSITE_RUN_FROM_ZIP` puede tener uno de los siguientes valores:

| Valor  | DESCRIPCIÓN  |
|---------|---------|
|**`<url>`**  | Ubicación del archivo de paquete específico que desea ejecutar. Cuando se usa Blob Storage, debe usar un contenedor privado con una [firma de acceso compartido (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#attach-a-storage-account-by-using-a-shared-access-signature-sas) para permitir que sistema en tiempo de ejecución de Functions acceda al paquete. Puede usar el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) para cargar archivos de paquete en la cuenta de Blob Storage.         |
| **`1`**  | Ejecución desde un archivo de paquete en la carpeta `d:\home\data\SitePackages` de la aplicación de función. Esta opción requiere que la carpeta tenga también un archivo denominado `packagename.txt`. Este archivo contiene solo el nombre del archivo de paquete en la carpeta, sin ningún espacio en blanco. |

A continuación se muestra una aplicación de función configurada para ejecutarse desde un archivo .zip que se hospeda en Azure Blob Storage:

![Valor de aplicación WEBSITE_RUN_FROM_ZIP](./media/run-functions-from-deployment-package/run-from-zip-app-setting-portal.png)

> [!NOTE]
> Actualmente, solo se admiten archivos de paquete .zip.

## <a name="integration-with-zip-deployment"></a>Integración con la implementación de archivos ZIP

La [implementación de archivos ZIP][Zip deployment for Azure Functions] es una característica de Azure App Service que le permite implementar su proyecto de aplicación de función en el directorio `wwwroot`. El proyecto se empaqueta como un archivo de implementación .zip. Las mismas API se pueden usar para implementar el paquete en la carpeta `d:\home\data\SitePackages`. Con el valor de configuración de la aplicación `WEBSITE_RUN_FROM_ZIP` de `1`, las API de implementación de archivos ZIP copian el paquete en la carpeta `d:\home\data\SitePackages` en lugar de extraer los archivos en `d:\home\site\wwwroot`. También se crea el archivo `packagename.txt`. La aplicación de función se ejecuta a continuación desde el paquete después del reinicio, y `wwwroot` pasa a ser de solo lectura. Para más información sobre la implementación de archivos ZIP, consulte [Implementación para insertar archivos ZIP en Azure Functions](deployment-zip-push.md).

## <a name="adding-the-websiterunfromzip-setting"></a>Incorporación del valor WEBSITE_RUN_FROM_ZIP

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Implementación continua para Azure Functions](functions-continuous-deployment.md)

[Zip deployment for Azure Functions]: deployment-zip-push.md
