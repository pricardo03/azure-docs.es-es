---
title: Uso de scripts de implementación en plantillas | Microsoft Docs
description: Use scripts de implementación en plantillas de Azure Resource Manager.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 01/22/2020
ms.author: jgao
ms.openlocfilehash: 125fefbb1d83db8b6114b2d09f5bd6da885159ba
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76547649"
---
# <a name="use-deployment-scripts-in-templates-preview"></a>Uso de scripts de implementación en plantillas (versión preliminar)

Aprenda a usar scripts de implementación en plantillas de recursos de Azure. Con un nuevo tipo de recurso denominado `Microsoft.Resources/deploymentScripts`, los usuarios pueden ejecutar scripts de implementación en implementaciones de plantilla y revisar los resultados de la ejecución. Estos scripts se pueden usar para realizar pasos personalizados tales como:

- Adición de usuarios a un directorio
- Creación de un registro de aplicación
- Realización de operaciones de plano de datos como, por ejemplo, copiar blobs o una base de datos de inicialización
- Búsqueda y validación de una clave de licencia
- Creación de un certificado autofirmado.
- Creación de un objeto en Azure AD
- Búsqueda de bloques de direcciones IP en el sistema personalizado

Ventajas del script de implementación:

- Fácil de programar, usar y depurar. Puede desarrollar scripts de implementación en sus entornos de desarrollo favoritos. Los scripts se pueden insertar en plantillas o en archivos de script externos.
- Puede especificar el lenguaje y la plataforma del script. Actualmente, solo se admiten scripts de implementación de Azure PowerShell en el entorno de Linux.
- Permite especificar las identidades que se usan para ejecutar los scripts. Actualmente, solo se admiten [identidades asignadas por el usuario de Azure](../../active-directory/managed-identities-azure-resources/overview.md).
- Permite pasar cuatro argumentos de la línea de comandos al script.
- Puede especificar salidas de script y pasarlas de nuevo a la implementación.

> [!NOTE]
> El script de implementación se encuentra actualmente en versión preliminar. Para usarlo, debe [registrarse en la versión preliminar](https://aka.ms/armtemplatepreviews).

> [!IMPORTANT]
> En el mismo grupo de recursos que se usa para la ejecución de scripts y la solución de problemas, se crean dos recursos de script de implementación, una cuenta de almacenamiento y una instancia de contenedor. Estos recursos los suele eliminar el servicio de scripts cuando la ejecución del script de implementación llega a un estado terminal. Los recursos se le facturarán hasta que se eliminen. Para más información, consulte [Limpieza de los recursos del script de implementación](#clean-up-deployment-script-resources).

## <a name="prerequisites"></a>Prerequisites

- **Una identidad administrada asignada por el usuario con el rol de colaborador en el nivel de suscripción**. Esta identidad se usa para ejecutar scripts de implementación. Para crear una, consulte [Creación de una identidad administrada asignada por el usuario mediante Azure Portal](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) o [la CLI de Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) o [Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md). Necesitará el identificador de identidad al implementar la plantilla. El formato de la identidad es:

  ```json
  /subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<IdentityID>
  ```

  Use el script de PowerShell siguiente para obtener el identificador; para ello, proporcione el nombre del grupo de recursos y el nombre de la identidad.

  ```azurepowershell-interactive
  $idGroup = Read-Host -Prompt "Enter the resource group name for the managed identity"
  $idName = Read-Host -Prompt "Enter the name of the managed identity"

  $id = (Get-AzUserAssignedIdentity -resourcegroupname $idGroup -Name idName).Id
  ```

- **Azure PowerShell versión 2.7.0, 2.8.0 o 3.0.0**. No necesita estas versiones para implementar plantillas. Pero estas versiones son necesarias para probar los scripts de implementación de forma local. Consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Puede usar una imagen de Docker preconfigurada.  Consulte [Configuración del entorno de desarrollo](#configure-development-environment).

## <a name="resource-schema"></a>Esquema de recursos

El código JSON siguiente es un ejemplo.  [Aquí](/azure/templates/microsoft.resources/deploymentscripts) puede encontrar el esquema de plantilla más reciente.

```json
{
  "type": "Microsoft.Resources/deploymentScripts",
  "apiVersion": "2019-10-01-preview",
  "name": "myDeploymentScript",
  "location": "[resourceGroup().location]",
  "kind": "AzurePowerShell",
  "identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
      "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID": {}
    }
  },
  "properties": {
    "forceUpdateTag": 1,
    "azPowerShellVersion": "2.8",
    "arguments": "[concat('-name ', parameters('name'))]",
    "scriptContent": "
      param([string] $name)
      $output = 'Hello {0}' -f $name
      Write-Output $output
      $DeploymentScriptOutputs = @{}
      $DeploymentScriptOutputs['text'] = $output
    ",
    "primaryScriptUri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.json",
    "supportingScriptUris":[],
    "timeout": "PT30M",
    "cleanupPreference": "OnSuccess",
    "retentionInterval": "P1D"
  }
}
```

> [!NOTE]
> Este ejemplo se ofrece con fines de demostración.  **scriptContent** y **primaryScriptUris** no pueden coexistir en una plantilla.

Detalles de los valores de propiedad:

- **Identidad**: el servicio de scripts de implementación utiliza una identidad administrada asignada por el usuario para ejecutar los scripts. Actualmente, solo se admiten identidades asignadas por el usuario.
- **kind**: especifique el tipo de script. Actualmente, solo se admite el script de Azure PowerShell. El valor es **AzurePowerShell**.
- **forceUpdateTag**: el cambio de este valor entre implementaciones de plantilla obliga a que se vuelva a ejecutar el script de implementación. Utilice la función newGuid() o utcNow() que hay que establecer como defaultValue de un parámetro. Para más información, consulte la sección [Ejecución de un script varias veces](#run-script-more-than-once).
- **azPowerShellVersion**: especifique la versión del módulo de Azure PowerShell que se va a usar. Actualmente, el script de implementación es compatible con las versiones 2.7.0, 2.8.0 y 3.0.0.
- **arguments**: Especifique los valores de los parámetros. Los valores se separan con espacios.
- **scriptContent**: especifique el contenido del script. Para ejecutar un script externo, use `primaryScriptUri` en su lugar. Para ver ejemplos, consulte [Uso scripts en línea](#use-inline-scripts) y [Uso de scripts externos](#use-external-scripts).
- **primaryScriptUri**: especifique una dirección URL de acceso público para el script principal de PowerShell con la extensión de archivo de PowerShell compatible.
- **supportingScriptUris**: especifique una matriz de direcciones URL de acceso público para admitir los archivos de PowerShell a los que se llamará en `ScriptContent` o `PrimaryScriptUri`.
- **timeout**: especifique el tiempo máximo de ejecución de scripts permitido en [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). El valor predeterminado es **P1D**.
- **cleanupPreference**. especifique la preferencia de limpieza de recursos de implementación cuando la ejecución del script llegue a un estado terminal. La configuración predeterminada es **Siempre**, lo que significa que se eliminan los recursos a pesar del estado terminal (correcto, error, cancelado). Para obtener más información, vea el artículo sobre [limpieza de los recursos del script de implementación](#clean-up-deployment-script-resources).
- **retentionInterval**: especifique el intervalo durante el que el servicio conserva los recursos del script de implementación cuando este llega a un estado terminal. Los recursos del script de implementación se eliminarán cuando expire este periodo. La duración se basa en el [patrón ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). El valor predeterminado es **P1D**, lo que significa siete días. Esta propiedad se usa cuando cleanupPreference se establece en *OnExpiration*. La propiedad *OnExpiration* no está habilitada actualmente. Para obtener más información, vea el artículo sobre [limpieza de los recursos del script de implementación](#clean-up-deployment-script-resources).

## <a name="use-inline-scripts"></a>Uso de scripts en línea

La plantilla siguiente tiene un recurso definido con el tipo `Microsoft.Resources/deploymentScripts`.

[!code-json[](~/resourcemanager-templates/deployment-script/deploymentscript-helloworld.json?range=1-54)]

> [!NOTE]
> Dado que los scripts de implementación en línea se incluyen entre comillas dobles, las cadenas dentro de los scripts de implementación deben incluirse entre comillas simples. El carácter de escape de PowerShell es **&#92;** . También puede plantearse la posibilidad de usar la sustitución de cadenas tal y como se muestra en el ejemplo anterior de JSON. Consulte el valor predeterminado del nombre de parámetro.

El script toma un parámetro y genera el valor del parámetro. **DeploymentScriptOutputs** se usa para almacenar las salidas.  En la sección de salidas, la línea **value** muestra cómo acceder a los valores almacenados. `Write-Output` se utiliza con fines de depuración. Para obtener información sobre cómo acceder al archivo de salida, vea [Depuración de scripts de implementación](#debug-deployment-scripts).  Para ver las descripciones de propiedades, consulte [Esquema de recursos](#resource-schema).

Para ejecutar el script, seleccione **Pruébelo** para abrir Cloud Shell y luego pegue el código siguiente en el panel de Shell.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the name of the resource group to be created"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$id = Read-Host -Prompt "Enter the user-assigned managed identity ID"

New-AzResourceGroup -Name $resourceGroupName -Location $location

New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.json" -identity $id

Write-Host "Press [ENTER] to continue ..."
```

El resultado tendrá una apariencia similar a la siguiente:

![Salida del script Hola mundo de implementación de la plantilla de Resource Manager](./media/deployment-script-template/resource-manager-template-deployment-script-helloworld-output.png)

## <a name="use-external-scripts"></a>Uso de scripts externos

Además de los scripts en línea, también puede usar archivos de script externos. Actualmente solo se admiten scripts de PowerShell con la extensión de archivo **ps1**. Para usar archivos de script externos, reemplace `scriptContent` por `primaryScriptUri`. Por ejemplo:

```json
"primaryScriptURI": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
```

Para ver un ejemplo, seleccione [aquí](https://github.com/Azure/azure-docs-json-samples/blob/master/deployment-script/deploymentscript-helloworld-primaryscripturi.json).

Los archivos de script externos deben ser accesibles.  Para proteger los archivos de script que se almacenan en cuentas de Azure Storage, consulte [Tutorial: Protección de los artefactos de las implementaciones de plantillas de Azure Resource Manager](./template-tutorial-secure-artifacts.md).

## <a name="use-supporting-scripts"></a>Uso de scripts auxiliares

Puede separar las lógicas complicadas en uno o varios archivos de script auxiliar. En caso necesario, la propiedad `supportingScriptURI` le permite especificar una matriz de identificadores URI para los archivos de script auxiliar:

```json
"scriptContent": "
    ...
    ./Create-Cert.ps1
    ...
"

"supportingScriptUris": [
  "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/create-cert.ps1"
],
```

Se puede llamar a archivos de script auxiliar desde scripts en línea y archivos de script principal.

Los archivos auxiliares se copian en azscripts/azscriptinput en el tiempo de ejecución. Use la ruta de acceso relativa para hacer referencia a los archivos auxiliares desde scripts en línea y archivos de script principal.

## <a name="work-with-outputs-from-deployment-scripts"></a>Trabajo con salidas de los scripts de implementación

En la plantilla siguiente se muestra cómo pasar valores entre dos recursos deploymentScripts:

[!code-json[](~/resourcemanager-templates/deployment-script/deploymentscript-basic.json?range=1-84)]

En el primer recurso, defina una variable denominada **$DeploymentScriptOutputs** y úsela para almacenar los valores de salida. Para acceder al valor de salida de otro recurso dentro de la plantilla, use:

```json
reference('<ResourceName>').output.text
```

## <a name="debug-deployment-scripts"></a>Depuración de scripts de implementación

El servicio de scripts crea una [cuenta de almacenamiento](../../storage/common/storage-account-overview.md) y una [instancia de contenedor](../../container-instances/container-instances-overview.md) para la ejecución de scripts. Ambos recursos incluyen el sufijo **azscripts** en los nombres de los recursos.

![Nombres de recursos del script de implementación de la plantilla de Resource Manager](./media/deployment-script-template/resource-manager-template-deployment-script-resources.png)

El script de usuario, los resultados de la ejecución y el archivo stdout se almacenan en los recursos compartidos de archivos de la cuenta de almacenamiento. Hay una carpeta llamada **azscripts**. En la carpeta hay otras dos carpetas para los archivos de entrada y salida: **azscriptinput** y **azscriptoutput**.

La carpeta de salida contiene un archivo **executionresult.json** y el archivo de salida del script. Puede ver el mensaje de error de ejecución del script en **executionresult.json**. El archivo de salida solo se crea cuando el script se ejecuta correctamente. La carpeta de entrada contiene un archivo de script de PowerShell del sistema y los archivos de script de implementación del usuario. Puede reemplazar el archivo de script de implementación de usuario por uno revisado y volver a ejecutar el script de implementación en la instancia de Azure Container.

Puede obtener la información de implementación de recursos del script de implementación en el nivel de grupo de recursos y en el nivel de suscripción mediante API REST:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>?api-version=2019-10-01-preview
```

```rest
/subscriptions/<SubscriptionID>/providers/microsoft.resources/deploymentScripts?api-version=2019-10-01-preview
```

En el ejemplo siguiente se usa [ARMClient](https://github.com/projectkudu/ARMClient):

```azurepowershell
armclient login
armclient get /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourcegroups/myrg/providers/microsoft.resources/deploymentScripts/myDeployementScript?api-version=2019-10-01-preview
```

La salida es parecida a esta:

[!code-json[](~/resourcemanager-templates/deployment-script/deploymentscript-status.json?range=1-48)]

La salida muestra el estado de implementación y los identificadores de recursos del script de implementación.

La siguiente API REST devuelve el registro:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>/logs?api-version=2019-10-01-preview
```

Solo funciona antes de que se eliminen los recursos del script de implementación.

Para ver el recurso deploymentScripts en el portal, seleccione **Mostrar tipos ocultos**:

![Script de implementación de la plantilla de Resource Manager, mostrar tipos ocultos, portal](./media/deployment-script-template/resource-manager-deployment-script-portal-show-hidden-types.png)

## <a name="clean-up-deployment-script-resources"></a>Limpieza de los recursos del script de implementación

El script de implementación crea una cuenta de almacenamiento y una instancia de contenedor que se usan para ejecutar scripts de implementación y almacenar información de depuración. Estos dos recursos se crean en el mismo grupo de recursos que los recursos aprovisionados y el servicio de scripts los eliminará cuando expire el script. Puede controlar el ciclo de vida de estos recursos.  Hasta que se eliminen, se le facturarán ambos recursos. Para obtener información sobre los precios, consulte los [precios de Container Instances](https://azure.microsoft.com/pricing/details/container-instances/) y los [precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

El ciclo de vida de estos recursos se controla mediante las siguientes propiedades de la plantilla:

- **cleanupPreference**: Preferencia de limpieza cuando la ejecución del script llega a un estado terminal.  Los valores admitidos son:

  - **Always**: Elimine los recursos cuando la ejecución del script llegue a un estado terminal. Puesto que el recurso deploymentScripts todavía puede estar presente después de que se limpien los recursos, el script del sistema copiará los resultados de la ejecución del script (por ejemplo, los valores stdout, outputs, return, etc.) en DB antes de que se eliminen los recursos.
  - **OnSuccess**: Elimine los recursos solo cuando la ejecución del script sea correcta. Todavía puede acceder a los recursos para encontrar la información de depuración.
  - **OnExpiration**: Elimine los recursos solo cuando la configuración de **retentionInterval** haya expirado. Esta propiedad está deshabilitada actualmente.

- **retentionInterval**: especifique el intervalo de tiempo que se conservará un recurso de script y tras el que expirará y se eliminará.

> [!NOTE]
> No se recomienda usar los recursos del script de implementación con otros fines.

## <a name="run-script-more-than-once"></a>Ejecución del script más de una vez

La ejecución del script de implementación es una operación idempotente. Si no se cambia ninguna de las propiedades del recurso deploymentScripts (incluido el script en línea), el script no se ejecutará al volver a implementar la plantilla. El servicio de scripts de implementación compara los nombres de recursos de la plantilla con los recursos existentes en el mismo grupo de recursos. Hay dos opciones si quiere ejecutar el mismo script de implementación varias veces:

- Cambie el nombre del recurso deploymentScripts. Por ejemplo, use la función de plantilla [utcNow](./template-functions-string.md#utcnow) como nombre del recurso o como parte del nombre del recurso. Al cambiar el nombre del recurso se crea otro recurso deploymentScripts. Es conveniente mantener un historial de ejecución del script.

    > [!NOTE]
    > La función utcNow solo puede utilizarse en el valor predeterminado de un parámetro.

- Especifique otro valor en la propiedad de plantilla `forceUpdateTag`.  Por ejemplo, use utcNow como valor.

> [!NOTE]
> Escriba los scripts de implementación que son idempotentes. Esto garantiza que si se ejecutan de nuevo accidentalmente, no van a provocar cambios en el sistema. Por ejemplo, si el script de implementación se usa para crear un recurso de Azure, compruebe que el recurso no exista antes de crearlo, para que el script se realice correctamente o no se vuelva a crear el recurso.

## <a name="configure-development-environment"></a>Configuración del entorno de desarrollo

Actualmente, el script de implementación admite las versiones 2.7.0, 2.8.0 y 3.0.0 de Azure PowerShell.  Si tiene un equipo Windows, puede instalar una de las versiones de Azure PowerShell admitidas y empezar a desarrollar y probar los scripts de implementación.  Si no tiene un equipo Windows o no tiene una de estas versiones Azure PowerShell instaladas, puede usar una imagen de contenedor de Docker preconfigurada. En el procedimiento siguiente se muestra cómo configurar la imagen de Docker en Windows. En el caso de Linux y Mac, puede encontrar la información en Internet.

1. Instale [Docker Desktop](https://www.docker.com/products/docker-desktop) en el equipo de desarrollo.
1. Abra Docker Desktop.
1. Seleccione el icono de Docker Desktop en la barra de tareas y elija **Configuración**.
1. Seleccione **Shared Drives**, una unidad local que quiera que esté disponible para los contenedores y luego elija **Aplicar**

    ![Unidad compartida del script de implementación de la plantilla de Resource Manager](./media/deployment-script-template/resource-manager-deployment-script-docker-setting-drive.png)

1. Escriba sus credenciales de Windows en el símbolo del sistema.
1. Abra una ventana de terminal, ya sea desde el símbolo del sistema o desde Windows PowerShell (no use PowerShell ISE).
1. Extraiga la imagen del contenedor de scripts de implementación en el equipo local:

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    En este ejemplo se usa la versión 2.7.0.

1. Ejecute la imagen de Docker localmente.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    Reemplace **&lt;letra de unidad del host >** y **&lt;nombre del directorio host >** por una carpeta existente en la unidad compartida.  La carpeta se asigna a la carpeta **/data** del contenedor. Por ejemplo, para asignar D:\docker:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    **-it** significa mantener activa la imagen del contenedor.

1. Seleccione **Share it** (Compartir) cuando reciba un mensaje.
1. Ejecute un script de PowerShell tal y como se muestra en la siguiente captura de pantalla (dado que tiene un archivo helloworld.ps1 en d:\docker folder.)

    ![CMD de Docker del script de implementación de plantilla de Resource Manager](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

Después de que el script de PowerShell se pruebe correctamente, puede usarlo como script de implementación.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a usar scripts de implementación. Para seguir un tutorial sobre scripts de implementación:

> [!div class="nextstepaction"]
> [Tutorial: Uso de scripts de implementación en plantillas de Azure Resource Manager](./template-tutorial-deployment-script.md).