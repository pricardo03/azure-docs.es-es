---
title: Prueba de la definición de interfaz de usuario para Azure Managed Applications | Microsoft Docs
description: Describe cómo probar la experiencia del usuario para la creación de la aplicación administrada de Azure a través del portal.
author: tfitzmac
ms.service: managed-applications
ms.topic: conceptual
ms.date: 05/26/2019
ms.author: tomfitz
ms.openlocfilehash: 99ca319910be2cb20214172826eb40361abe72f0
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257669"
---
# <a name="test-your-portal-interface-for-azure-managed-applications"></a>Probar la interfaz del portal de Azure Managed Applications

Después de [crear el archivo createUiDefinition.json](create-uidefinition-overview.md) de la aplicación administrada, debe probar la experiencia del usuario. Para simplificar las pruebas, use un entorno de recinto de seguridad que se carga el archivo en el portal. No es necesario implementar la aplicación administrada. El espacio aislado presenta la interfaz de usuario en la experiencia del portal actual, pantalla completa. O bien, puede usar un script de PowerShell para probar la interfaz, pero utiliza una vista del portal heredada. En este artículo se muestran ambos enfoques. El recinto de seguridad es la manera recomendada para obtener una vista previa de la interfaz.

## <a name="prerequisites"></a>Requisitos previos

* Un archivo **createUiDefinition.json**. Si no tiene este archivo, copie el [archivo de ejemplo](https://github.com/Azure/azure-quickstart-templates/blob/master/100-marketplace-sample/createUiDefinition.json).

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="use-sandbox"></a>Uso de espacio aislado

1. Abra el [crear espacio aislado de definición de interfaz de usuario](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade).

   ![Mostrar espacio aislado](./media/test-createuidefinition/show-sandbox.png)

1. Reemplace la definición vacía con el contenido del archivo createUiDefinition.json. Seleccione **Preview**.

   ![Seleccione Vista previa](./media/test-createuidefinition/select-preview.png)

1. Se muestra el formulario creado. Puede recorrer la experiencia del usuario y rellene los valores.

   ![Mostrar formulario](./media/test-createuidefinition/show-ui-form.png)

### <a name="troubleshooting"></a>solución de problemas

Si no se muestra el formulario después de seleccionar **Preview**, puede tener un error de sintaxis. Busque el indicador rojo en la barra de desplazamiento a la derecha y navegar hasta él.

![Mostrar error de sintaxis](./media/test-createuidefinition/show-syntax-error.png)

Si no se muestra el formulario y, en su lugar, verá un icono de una nube con la colocación de anulación, el formulario tiene un error, como una propiedad que falta. Abra las herramientas de desarrollo Web en el explorador. La **consola** muestra mensajes importantes sobre la interfaz.

![Mostrar error](./media/test-createuidefinition/show-error.png)

## <a name="use-test-script"></a>Usar script de prueba

Para probar la interfaz en el portal, copie uno de los siguientes scripts en la máquina local:

* [Script de carga en PowerShell](https://github.com/Azure/azure-quickstart-templates/blob/master/SideLoad-CreateUIDefinition.ps1)
* [Script de carga en la CLI de Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/sideload-createuidef.sh)

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

El script abre una nueva pestaña en el explorador. Muestra el portal con la interfaz para crear la aplicación administrada.

![Visualización del portal](./media/test-createuidefinition/view-portal.png)

Proporcione los valores para los campos. Cuando termine, verá los valores que se han pasado a la plantilla.

![Mostrar valores](./media/test-createuidefinition/show-json.png)

Puede utilizar estos valores como el archivo de parámetros para probar la plantilla de implementación.

Si el portal se bloquea en la pantalla de resumen, podría haber un error en la sección de salida. Por ejemplo, puede que haya hecho referencia un control que no existe. Si un parámetro en la salida está vacío, el parámetro es posible que se hace referencia a una propiedad que no existe. Por ejemplo, la referencia al control es válida, pero la referencia de propiedad no es válida.

## <a name="test-your-solution-files"></a>Prueba de los archivos de la solución

Ahora que ha comprobado que la interfaz del portal funciona como se espera, es el momento de validar que el archivo createUiDefinition está correctamente integrado con el archivo mainTemplate.json. Puede ejecutar una prueba del script de validación para probar el contenido de los archivos de la solución, incluido el archivo createUiDefinition. El script valida la sintaxis JSON, comprueba si hay expresiones regex en los campos de texto y se asegura de que los valores de salida de la interfaz del portal coincidan con los parámetros de la plantilla. Para más información sobre cómo ejecutar este script, consulte [Ejecución de comprobaciones de validación estática para plantillas](https://github.com/Azure/azure-quickstart-templates/tree/master/test/template-validation-tests).

## <a name="next-steps"></a>Pasos siguientes

Después de validar la interfaz de su portal, infórmese sobre cómo hacer que la [aplicación administrada de Azure esté disponible en Marketplace](publish-marketplace-app.md).
