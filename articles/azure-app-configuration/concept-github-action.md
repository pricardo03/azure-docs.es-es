---
title: Uso de Acciones de GitHub con la sincronización de Azure App Configuration
description: Uso de Acciones de GitHub para desencadenar una actualización de la instancia de App Configuration cuando se realizan acciones definidas en un repositorio de GitHub
author: lisaguthrie
ms.author: lcozzens
ms.date: 01/14/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: 269ae5630d1524cb8f89d3af8728892079f6eb5f
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76899629"
---
# <a name="sync-your-app-configuration-instance-using-github-actions"></a>Sincronización de la instancia de App Configuration mediante Acciones de GitHub
Azure App Configuration usa Acciones de GitHub para actualizar una instancia de App Configuration cuando se desencadena mediante una acción realizada en un repositorio de GitHub. Puede aprovechar los flujos de trabajo de GitHub para actualizar la configuración de la aplicación, lo que permite la integración de las actualizaciones de configuración de aplicaciones en el mismo flujo de trabajo que se usa para actualizar el código de la aplicación.

Un [flujo de trabajo](https://help.github.com/articles/about-github-actions#workflow) de las Acciones de GitHub es un proceso automatizado que se define en el repositorio de GitHub. Este proceso indica a GitHub cómo crear e implementar el proyecto de GitHub. Azure App Configuration proporciona la acción de *sincronización de Azure App Configuration* para habilitar las actualizaciones en una instancia de configuración de App Configuration cuando se realizan cambios en el repositorio de origen. 

Un archivo YAML (.yml) define un flujo de trabajo en la ruta de acceso `/.github/workflows/` del repositorio. En esta definición se incluyen los diversos pasos y parámetros que definen el flujo de trabajo.

Los eventos de GitHub, como una inserción en un repositorio, pueden desencadenar un flujo de trabajo de Acciones de GitHub.  Azure proporciona la *sincronización de Azure App Configuration* para que pueda desencadenar una actualización de una instancia de App Configuration cuando se produce una acción de GitHub especificada. Esto permite que los equipos aprovechen las características principales de GitHub al insertar, revisar o bifurcar los archivos de configuración de la aplicación de la misma forma que con el código de la aplicación.

La [documentación](https://help.github.com/actions/automating-your-workflow-with-github-actions/configuring-a-workflow) de GitHub proporciona una vista detallada de los flujos de trabajo y las acciones de GitHub. 

## <a name="enable-github-actions-in-your-repository"></a>Habilitación de Acciones de GitHub en el repositorio
Para empezar a usar esta acción de GitHub, vaya al repositorio y seleccione la pestaña **Acciones**. Busque y seleccione la acción de GitHub en Marketplace. Para ello, busque "Sincronización de Azure App Configuration". 

> [!div class="mx-imgBorder"]
> ![Selección de la pestaña Acción](media/find-github-action.png)

> [!div class="mx-imgBorder"]
> ![Selección de la acción de sincronización de configuración de la aplicación](media/app-configuration-sync-action.png)

## <a name="sync-configuration-files-after-a-push"></a>Sincronización de los archivos de configuración después de una inserción
Esta acción sincroniza los archivos de Azure App Configuration cuando se inserta un cambio en `appsettings.json`. Cuando un desarrollador realiza un cambio y lo inserta en `appsettings.json`, la acción de sincronización de App Configuration actualiza la instancia de App Configuration con los nuevos valores.

La primera sección de este flujo de trabajo especifica que la acción se desencadena *en* una *inserción* que contiene `appsettings.json` para la bifurcación *maestra*. En la segunda sección se enumeran los trabajos que se ejecutan una vez que se desencadena la acción. La acción desprotege los archivos pertinentes y actualiza la instancia de App Configuration con la cadena de conexión almacenada como un secreto en el repositorio.  Para más información sobre el uso de secretos en Github, consulte [este artículo](https://help.github.com/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets) sobre la creación y el uso de secretos cifrados.

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your                        
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }} 
          separator: ':' 
```

## <a name="use-a-dynamic-label-on-sync"></a>Uso de una etiqueta dinámica en la sincronización
La acción anterior simplemente actualizó la instancia de App Configuration cada vez que se actualiza `appsettings.json`. Esta acción inserta una etiqueta dinámica en cada sincronización, lo que garantiza que pueda identificarse de forma única.  Esto permite que los cambios de código se asignen rápidamente a los cambios de configuración.

La primera sección de este flujo de trabajo especifica que la acción se desencadena *en* una *inserción* que contiene `appsettings.json` para la bifurcación *maestra*. En la segunda sección se ejecuta un trabajo que crea una etiqueta única para la actualización de la configuración según el hash de confirmación. Después, el trabajo actualiza la instancia de App Configuration con los nuevos valores y la etiqueta única para esta actualización.

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # Creates a label based on the branch name and the first 8 characters          
      # of the commit hash 
      - id: determine_label 
        run: echo ::set-output name=LABEL::"${GITHUB_REF#refs/*/}/${GITHUB_SHA:0:8}" 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          label: ${{ steps.determine_label.outputs.LABEL }} 
```

## <a name="use-strict-sync"></a>Uso de una sincronización estricta
Cuando el modo estricto está habilitado, la sincronización garantiza que la instancia de App Configuration coincida exactamente con el archivo de configuración para el prefijo y la etiqueta especificados. Se eliminan los pares clave-valor con el mismo prefijo y etiqueta que no están en el archivo de configuración. 
 
Si el modo estricto no está habilitado, la sincronización solo establecerá los valores de clave del archivo de configuración. No se eliminará ningún par clave-valor. 

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          label: 'Label' 
          prefix: 'Prefix:' 
          strict: true 
```

## <a name="use-max-depth-to-limit-github-action"></a>Uso de la profundidad máxima para limitar la acción de GitHub
El comportamiento predeterminado de los atributos JSON anidados es aplanar todo el objeto.  El código JSON siguiente define este par clave-valor:

| Clave | Value |
| --- | --- |
| Object:Inner:InnerKey | InnerValue |

```json
{ "Object": 
    { "Inner":
        {
        "InnerKey": "InnerValue"
        }
    }
}
```
Si el objeto anidado está pensado para ser el valor insertado en la instancia de configuración, puede usar el valor de *profundidad* para detener el acoplamiento con la profundidad adecuada. 

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          depth: 2 
```

Dada una profundidad de 2, el ejemplo anterior devuelve ahora el siguiente par clave-valor:

| Clave | Value |
| --- | --- |
| Object:Inner | {"InnerKey":"InnerValue"} |

## <a name="understand-action-inputs"></a>Descripción de las entradas de acción
Los parámetros de entrada especifican los datos usados por la acción durante el tiempo de ejecución.  La tabla siguiente contiene los parámetros de entrada aceptados por la sincronización de App Configuration y los valores esperados para cada uno.  Para más información sobre las entradas de acción para Acciones de GitHub, consulte la [documentación](https://help.github.com/actions/automating-your-workflow-with-github-actions/metadata-syntax-for-github-actions#inputs) de GitHub.

> [!Note]
> Los identificadores de entradas no distinguen mayúsculas de minúsculas.


| Nombre de entrada | ¿Necesario? | Value |
|----|----|----|
| configurationFile | Sí | Ruta de acceso al archivo de configuración en el repositorio, con respecto a la raíz de este.  Se admiten patrones Glob y pueden incluir varios archivos. |
| format | Sí | Formato del archivo de configuración.  Los formatos válidos son: Propiedades JSON, YAML. |
| connectionString | Sí | Cadena de conexión para la instancia de App Configuration. La cadena de conexión debe almacenarse como un secreto en el repositorio de GitHub y solo se debe usar el nombre de secreto en el flujo de trabajo. |
| separador | Sí | Separador utilizado al aplanar el archivo de configuración en pares de clave y valor.  Los valores válidos son: , ; : - _ __ / |
| prefix | No | Prefijo que se va a agregar al inicio de las claves. |
| etiqueta | No | Etiqueta que se usa al establecer pares de clave-valor. Si no se especifica, se usa una etiqueta null. |
| strict | No | Valor booleano que determina si está habilitado el modo estricto. El valor predeterminado es false. |
| depth | No | Profundidad máxima para aplanar el archivo de configuración.  La profundidad debe ser un número positivo.  El valor predeterminado no tendrá una profundidad máxima. |
| etiquetas | No | Especifica el conjunto de etiquetas en los pares de clave-valor.  El formato esperado es un formato de cadena de un objeto JSON con la siguiente forma: { [propertyName: string]: string; } Cada propiedad nombre-valor se convierte en una etiqueta. |

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido sobre la acción de GitHub de sincronización de App Configuration y del modo en que se puede usar para automatizar las actualizaciones de la instancia de App Configuration. Para información sobre cómo reacciona Azure App Configuration a los cambios en los pares de clave-valor, continúe con el siguiente [artículo](./concept-app-configuration-event.md).
