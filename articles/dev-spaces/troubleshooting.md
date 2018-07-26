---
title: Solución de problemas | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
manager: douge
ms.openlocfilehash: 4dee39b56cf0f6494f6e79c70b85bbf711d33d65
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044601"
---
# <a name="troubleshooting-guide"></a>Guía de solución de problemas

Esta guía contiene información sobre problemas habituales que pueden surgir al usar Azure Dev Spaces.

## <a name="error-service-cannot-be-started"></a>Error "No se puede iniciar el servicio".

Es posible que vea este error cuando el código del servicio no se inicia. La causa suele estar en el código de usuario. Para obtener más información de diagnóstico, realice los siguientes cambios en los comandos y valores de configuración:

En la línea de comandos:

1. Al usar _azds.exe_, emplee la opción de línea de comandos detallada, y utilice la opción de línea de comandos de salida para especificar el formato de salida.
 
    ```cmd
    azds up --verbose --output json
    ```

En Visual Studio:

1. Abra **Herramientas > Opciones** y en **Proyectos y soluciones**, elija **Build and Run** (Compilar y ejecutar).
2. Cambie la configuración de **Detalles de la salida de la compilación del proyecto de MSBuild** a **Detallado** o **Diagnóstico**.

    ![Captura de pantalla del cuadro de diálogo Opciones de herramientas](media/common/VerbositySetting.PNG)

## <a name="error-required-tools-and-configurations-are-missing"></a>Error que indica que faltan las herramientas y configuraciones necesarias

Este error puede producirse al iniciar VS Code: "[Azure Dev Spaces] Faltan las herramientas y configuraciones necesarias para compilar y depurar "[nombre de proyecto]"".
El error significa que azds.exe no está en la variable de entorno PATH, tal como se muestra en VS Code.

### <a name="try"></a>Pruebe lo siguiente:

Inicie VS Code desde un símbolo del sistema donde la variable de entorno PATH esté establecida correctamente.

## <a name="error-azds-is-not-recognized-as-an-internal-or-external-command-operable-program-or-batch-file"></a>El error de "azds" no se reconoce como un comando interno o externo, programa operable o archivo por lotes
 
Es posible que vea este error si azds.exe no está instalado o configurado correctamente.

### <a name="try"></a>Pruebe lo siguiente:

1. Compruebe la ubicación %ProgramFiles%/Microsoft SDKs\Azure\Azure Dev Spaces CLI (Preview) de azds.exe. Si está ahí, agregue esa ubicación a la variable de entorno PATH.
2. Si azds.exe no está instalado, ejecute el siguiente comando:

    ```cmd
    az aks use-dev-spaces -n <cluster-name> -g <resource-group>
    ```

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Error "Error de conexión en dirección ascendente o desconexión o restablecimiento antes de los encabezados"
Puede ver este error al intentar acceder al servicio. Por ejemplo, cuando vaya a la dirección URL del servicio en un explorador. 

### <a name="reason"></a>Motivo 
El puerto de contenedor no está disponible. Las causas de este problema pueden ser: 
* El contenedor está todavía en proceso de creación e implementación. Este problema puede producirse si ejecuta `azds up` o inicia el depurador y luego intenta acceder al contenedor antes de que se haya implementado correctamente.
* La configuración del puerto no es coherente en _Dockerfile_, el gráfico de Helm y cualquier otro código de servidor que abre un puerto.

### <a name="try"></a>Pruebe lo siguiente:
1. Si el contenedor está en proceso de creación o implementación, puede esperar entre dos y tres segundos e intentar acceder al servicio de nuevo. 
1. Compruebe la configuración del puerto. Los números de puerto especificados deben ser **idénticos** en todos los recursos siguientes:
    * **Dockerfile:** especificado por la instrucción `EXPOSE`.
    * **[Gráfico de Helm](https://docs.helm.sh):** especificado por los valores `externalPort` y `internalPort` valores para un servicio (a menudo se encuentra en un archivo `values.yml`).
    * Los puertos abiertos en el código de aplicación, por ejemplo, en Node.js: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Archivo de configuración no encontrado
Ejecuta `azds up` y aparece el siguiente error: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Motivo
Debe ejecutar `azds up` desde el directorio raíz del código que desea ejecutar y debe inicializar la carpeta de código para que se ejecute con Azure Dev Spaces.

### <a name="try"></a>Pruebe lo siguiente:
1. Cambie el directorio actual a la carpeta raíz que contiene el código de servicio. 
1. Si no tiene un archivo _azds.yaml_ en la carpeta de código, ejecute `azds prep` para generar los recursos de Docker, Kubernetes y Azure Dev Spaces.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Error: "El programa de canalización "azds" terminó inesperadamente con el código 126."
El inicio del depurador de VS Code a veces produce este error. Este es un problema conocido.

### <a name="try"></a>Pruebe lo siguiente:
1. Cierre y vuelva a abrir VS Code.
2. Vuelva a presionar F5.


## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>Error de depuración "El tipo de depuración "coreclr" configurado no es compatible."
La ejecución del depurador de VS Code informa del error `Configured debug type 'coreclr' is not supported.`.

### <a name="reason"></a>Motivo
No tiene la extensión de VS Code para Azure Dev Spaces instalada en la máquina de desarrollo.

### <a name="try"></a>Pruebe lo siguiente:
Instale la [extensión de VS Code para Azure Dev Spaces](get-started-netcore.md).

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>No se encontró el tipo o el nombre de espacio de nombres "MyLibrary"

### <a name="reason"></a>Motivo 
El contexto de compilación está en el nivel de proyecto o servicio de forma predeterminada, por lo que un proyecto de biblioteca que está usando no se encontrará.

### <a name="try"></a>Pruebe lo siguiente:
¿Qué debe hacer?:
1. Modifique el archivo _azds.yaml_ para establecer el contexto de compilación en el nivel de la solución.
2. Modifique los archivos _Dockerfile_ y _Dockerfile.develop_ para hacer referencia a los archivos _.csproj_ correctamente, en relación con el nuevo contexto de compilación.
3. Coloque un archivo _.dockerignore_ junto al archivo .sln y realice las modificaciones según sea necesario.

Puede encontrar un ejemplo en https://github.com/sgreenmsft/buildcontextsample.

## <a name="microsoftdevspacesregisteraction-authorization-error"></a>Error de autorización "Microsoft.DevSpaces/register/action"
Podría ver el error siguiente error cuando administra Azure Dev Spaces o si trabaja en una suscripción de Azure para la que no tiene acceso de tipo Propietario o Colaborador.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Motivo
La suscripción de Azure seleccionada no ha registrado el espacio de nombres `Microsoft.DevSpaces`.

### <a name="try"></a>Pruebe lo siguiente:
Un usuario con acceso de tipo Propietario o Colaborador a la suscripción de Azure puede ejecutar el siguiente comando de la CLI de Azure para registrar manualmente el espacio de nombres `Microsoft.DevSpaces`:

```cmd
az provider register --namespace Microsoft.DevSpaces
```

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure Dev Spaces no parece usar mi Dockerfile existente para crear un contenedor 

### <a name="reason"></a>Motivo
Azure Dev Spaces puede configurarse para que apunte a un _Dockerfile_ específico en el proyecto. Si parece que Azure Dev Spaces no usa el _Dockerfile_ que se espera para crear los contenedores, podría tener que indicar explícitamente a Azure Dev Spaces dónde se encuentra. 

### <a name="try"></a>Pruebe lo siguiente:
Abra el archivo _azds.yaml_ que Azure Dev Spaces generó en el proyecto. Utilice la directiva `configurations->develop->build->dockerfile` para que apuntar al Dockerfile que desea usar:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```