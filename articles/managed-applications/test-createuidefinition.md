---
title: Prueba de la definición de interfaz de usuario para Azure Managed Applications | Microsoft Docs
description: Describe cómo probar la experiencia del usuario para la creación de la aplicación administrada de Azure a través del portal.
services: managed-applications
documentationcenter: na
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2018
ms.author: tomfitz
ms.openlocfilehash: c88bdce64e88f8639da2c4ebb01f4594fccff8a0
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "42747095"
---
# <a name="test-azure-portal-interface-for-your-managed-application"></a>Prueba de la interfaz de Azure Portal para la aplicación administrada
Después de [crear el archivo createUiDefinition.json](create-uidefinition-overview.md) para la aplicación administrada de Azure, debe probar la experiencia del usuario. Para simplificar las pruebas, use un script que cargue el archivo en el portal. No es necesario implementar la aplicación administrada.

## <a name="prerequisites"></a>Requisitos previos

* Un archivo **createUiDefinition.json**. Si no tiene este archivo, copie el [archivo de ejemplo](https://github.com/Azure/azure-quickstart-templates/blob/master/100-marketplace-sample/createUiDefinition.json) y guárdelo localmente.

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="download-test-script"></a>Descarga del script de prueba

Para probar la interfaz en el portal, copie uno de los siguientes scripts en la máquina local:

* [Script de carga en PowerShell](https://github.com/Azure/azure-quickstart-templates/blob/master/SideLoad-CreateUIDefinition.ps1)
* [Script de carga en la CLI de Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/sideload-createuidef.sh)

## <a name="run-script"></a>Ejecución de script

Para ver el archivo de interfaz en el portal, ejecute el script descargado. El script crea una cuenta de almacenamiento en la suscripción de Azure y carga el archivo createUiDefinition.json en la cuenta de almacenamiento. La cuenta de almacenamiento se crea la primera vez que ejecute el script o si la cuenta de almacenamiento se ha eliminado. Si ya existe la cuenta de almacenamiento en su suscripción de Azure, el script la reutiliza. El script abre el portal y carga el archivo de la cuenta de almacenamiento.

Proporcione una ubicación para la cuenta de almacenamiento y especifique la carpeta que contiene el archivo createUiDefinition.json.

Para PowerShell, use:

```powershell
.\SideLoad-CreateUIDefinition.ps1 `
  -StorageResourceGroupLocation southcentralus `
  -ArtifactsStagingDirectory .\100-Marketplace-Sample
```

Para la CLI de Azure, utilice:

```azurecli
./sideload-createuidef.sh \
  -l southcentralus \
  -a .\100-Marketplace-Sample
```

Si el archivo createUiDefinition.json está en la misma carpeta que el script y ya ha creado la cuenta de almacenamiento, no necesita proporcionar dichos parámetros.

Para PowerShell, use:

```powershell
.\SideLoad-CreateUIDefinition.ps1
```

Para la CLI de Azure, utilice:

```azurecli
./sideload-createuidef.sh
```

## <a name="test-your-interface"></a>Prueba de la interfaz

El script abre una nueva pestaña en el explorador. Muestra el portal con la interfaz para crear la aplicación administrada.

![Visualización del portal](./media/test-createuidefinition/view-portal.png)

Antes de rellenar los campos, abra Web Developer Tools en el explorador. La **consola** muestra mensajes importantes sobre la interfaz.

![Selección de la consola](./media/test-createuidefinition/select-console.png)

Si la definición de la interfaz tiene un error, verá la descripción en la consola.

![Mostrar error](./media/test-createuidefinition/show-error.png)

Proporcione los valores para los campos. Cuando termine, verá los valores que se han pasado a la plantilla.

![Mostrar valores](./media/test-createuidefinition/show-json.png)

Puede utilizar estos valores como el archivo de parámetros para probar la plantilla de implementación.

## <a name="troubleshooting-the-interface"></a>Solución de problemas de la interfaz

Algunos errores comunes que pueden aparecer son:

* El portal no carga la interfaz. En su lugar, muestra un icono de una nube con una gota. Por lo general, aparece este icono cuando hay algún error de sintaxis en el archivo. Abra el archivo en VS Code o en otro editor de JSON que tenga la validación de esquemas y busque errores de sintaxis.

* El portal se bloquea en la pantalla de resumen. Normalmente, esta interrupción se produce cuando existe algún error en la sección de salida. Por ejemplo, puede que haya hecho referencia un control que no existe.

* Un parámetro de la salida está vacío. El parámetro puede haber hecho referencia a una propiedad que no existe. Por ejemplo, la referencia al control es válida, pero la referencia de propiedad no es válida.

## <a name="test-your-solution-files"></a>Prueba de los archivos de la solución

Ahora que ha comprobado que la interfaz del portal funciona como se espera, es el momento de validar que el archivo createUiDefinition está correctamente integrado con el archivo mainTemplate.json. Puede ejecutar una prueba del script de validación para probar el contenido de los archivos de la solución, incluido el archivo createUiDefinition. El script valida la sintaxis JSON, comprueba si hay expresiones regex en los campos de texto y se asegura de que los valores de salida de la interfaz del portal coincidan con los parámetros de la plantilla. Para más información sobre cómo ejecutar este script, consulte [Ejecución de comprobaciones de validación estática para plantillas](https://github.com/Azure/azure-quickstart-templates/tree/master/test/template-validation-tests).

## <a name="next-steps"></a>Pasos siguientes

Después de validar la interfaz de su portal, infórmese sobre cómo hacer que la [aplicación administrada de Azure esté disponible en Marketplace](publish-marketplace-app.md).
