---
title: Uso de Acciones de GitHub para crear actualizaciones de código en Azure Functions
description: Obtenga información sobre cómo usar Acciones de GitHub para definir un flujo de trabajo para crear e implementar proyectos de Azure Functions en GitHub.
author: ahmedelnably
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: aelnably
ms.openlocfilehash: 8e9e1189c3eb9de273926645ad0d4cfde5ba1c49
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71260040"
---
# <a name="continuous-delivery-by-using-github-action"></a>Entrega continua con Acciones de GitHub

[Acciones de GitHub](https://github.com/features/actions) le permite definir un flujo de trabajo para crear e implementar automáticamente su código de funciones en la aplicación de funciones de Azure. 

> [!IMPORTANT]  
> Acciones de GitHub está actualmente en versión beta. Primero debe [registrarse para unirse a la versión preliminar](https://github.com/features/actions) mediante su cuenta de GitHub.

En Acciones de GitHub, un [flujo de trabajo](https://help.github.com/articles/about-github-actions#workflow) es un proceso automatizado que define en su repositorio de GitHub. Este proceso indica a GitHub cómo crear e implementar su proyecto de aplicación de funciones en GitHub. 

Un archivo YAML (.yml) define un flujo de trabajo en la ruta de acceso `/.github/workflows/` de su repositorio. En esta definición se incluyen los diversos pasos y parámetros que componen el flujo de trabajo. 

Para un flujo de trabajo de Azure Functions, el archivo tiene tres secciones: 

| Sección | Tareas |
| ------- | ----- |
| **Autenticación** | <ol><li>Defina una entidad de servicio.</li><li>Cree un secreto de GitHub.</li></ol>|  
| **Compilar** | <ol><li>Configure el entorno.</li><li>Compile la aplicación de función.</li></ol> |
| **Implementación** | <ol><li>Implemente la aplicación de función.</li></ol>| 

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Puede crear una [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) mediante el comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) de la [CLI de Azure](/cli/azure/). Puede ejecutar este comando mediante [Azure Cloud Shell](https://shell.azure.com) en Azure Portal o seleccionando el botón **Pruébelo**.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>/providers/Microsoft.Web/sites/<APP_NAME> --sdk-auth
```

En este ejemplo, reemplace los marcadores de posición del recurso por su identificador de suscripción, grupo de recursos y nombre de la aplicación de función. La salida son las credenciales de asignación de roles que proporcionan acceso a su aplicación de funciones. Copie este objeto JSON, el cual puede usar para autenticarse desde GitHub.

> [!IMPORTANT]
> Siempre es recomendable conceder acceso mínimo. Es por ello que el ámbito del ejemplo anterior se limita a la aplicación de funciones específica y no a todo el grupo de recursos.

## <a name="configure-the-github-secret"></a>Configuración del secreto de GitHub

1. En [GitHub](https://github.com), examine su repositorio, seleccione **Configuración** > **Secretos** > **Agregar un nuevo secreto**.

    ![Agregar secreto](media/functions-how-to-github-actions/add-secret.png)

1. Use `AZURE_CREDENTIALS` para el **Nombre** y la salida del comando copiada para **Valor** y, a continuación, seleccione **Agregar secreto**. 

Ahora GitHub puede autenticarse en su aplicación de funciones de Azure.

## <a name="set-up-the-environment"></a>Configuración del entorno 

La configuración del entorno puede realizarse mediante una de las acciones de configuración de publicación.

|Idioma | Acción de configuración |
|---------|---------|
|**.NET**     | `actions/setup-dotnet` |
|**Java**    | `actions/setup-java` |
|**JavaScript**     | `actions/setup-node` |
|**Python**   | `actions/setup-python` |

En los siguientes ejemplos se muestra la parte del flujo de trabajo que configura el entorno para los diversos lenguajes admitidos:

**JavaScript**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
```

**Python**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
```

**.NET**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Dotnet 2.2.300
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '2.2.300'
```

**Java**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```

## <a name="build-the-function-app"></a>Creación de la aplicación de funciones

Esto depende del lenguaje y, para los lenguajes admitidos por Azure Functions, esta sección deben ser los pasos de compilación estándar de cada lenguaje.

En los siguientes ejemplos se muestra la parte del flujo de trabajo que crea la aplicación de funciones, en los diversos lenguajes admitidos:

**JavaScript**

```yaml
    - name: 'Run npm'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please change your directory for npm in pushd
        pushd .
        npm install
        npm run build --if-present
        npm run test --if-present
        popd
```

**Python**

```yaml
    - name: 'Run pip'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please change your directory for pip in pushd
        pushd .
        python -m pip install --upgrade pip
        pip install -r requirements.txt --target=".python_packages/lib/python3.6/site-packages"
        popd
```

**.NET**

```yaml
    - name: 'Run dotnet build'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please consider using pushd to change your path
        pushd .
        dotnet build --configuration Release --output ./output
        popd
```

**Java**

```yaml
    - name: 'Run mvn'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please change your directory for maven build in pushd
        pushd . ./POM_ARTIFACT_ID
        mvn clean package
        mvn azure-functions:package
        popd
```

## <a name="deploy-the-function-app"></a>Implementación de la aplicación de función

Para implementar su código en una aplicación de funciones, tendrá que usar la acción `Azure/functions-action`. Esta acción tiene dos parámetros:

|Parámetro |Explicación  |
|---------|---------|
|**_app-name_** | (Obligatorio) El nombre de su aplicación de funciones. |
|_**slot-name**_ | (Opcional) El nombre de la [ranura de implementación](functions-deployment-slots.md) en la que desea realizar la implementación. La ranura ya debe estar definida en su aplicación de funciones. |


En el siguiente ejemplo se usa la versión 1 de la `functions-action`:

```yaml
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: PLEASE_REPLACE_THIS_WITH_YOUR_FUNCTION_APP_NAME
```

## <a name="next-steps"></a>Pasos siguientes

Para ver un flujo de trabajo completo .yaml, consulte uno de los archivos del [repositorio de ejemplos de flujo de trabajo de Acciones de GitHub de Azure](https://github.com/Azure/actions-workflow-samples) con `functionapp` en el nombre. Puede usar estos ejemplos como punto de partida para su flujo de trabajo.

> [!div class="nextstepaction"]
> [Más información sobre Acciones de GitHub](https://help.github.com/en/articles/about-github-actions)
