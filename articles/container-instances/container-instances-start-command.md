---
title: Usar una línea de comandos de inicia en Azure Container Instances
description: Reemplazar el punto de entrada configurado en una imagen de contenedor cuando se implementa una instancia de contenedor de Azure
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: 78136a081e52ef3f12d672d01449ce616534462e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60537656"
---
# <a name="set-the-command-line-in-a-container-instance-to-override-the-default-command-line-operation"></a>Establecer la línea de comandos en una instancia de contenedor para anular la operación de línea de comandos predeterminada

Cuando se crea una instancia de contenedor, opcionalmente, especifique un comando para reemplazar la instrucción de línea de comandos predeterminada incluida en la imagen de contenedor. Este comportamiento es similar a la `--entrypoint` argumento de línea de comandos `docker run`.

Como la configuración como [variables de entorno](container-instances-environment-variables.md) de container instances, especifica una línea de comandos de inicia es útil para los trabajos por lotes donde es necesario preparar cada contenedor dinámicamente con la configuración específica de la tarea.

## <a name="command-line-guidelines"></a>Instrucciones de línea de comandos

* De forma predeterminada, la línea de comandos especifica un *único proceso que se inicie sin un shell* en el contenedor. Por ejemplo, la línea de comandos puede ejecutar un script de Python o el archivo ejecutable. 

* Para ejecutar varios comandos, iniciar la línea de comandos mediante el establecimiento de un entorno de shell en el sistema operativo del contenedor (ejemplos: `bin/sh`, `/bin/bash`, `cmd`). Siga las convenciones del shell para combinar varios comandos que se ejecutarán en secuencia.

* Dependiendo de la configuración del contenedor, debe establecer una ruta de acceso completa a la línea de comandos ejecutable o argumentos.

* Establecer un adecuado [directiva de reinicio](container-instances-restart-policy.md) para la instancia del contenedor, dependiendo de si la línea de comandos especifica una tarea de ejecución prolongada o una tarea de una sola ejecución. Por ejemplo, una directiva de reinicio de `Never` o `OnFailure` se recomienda para una tarea de una sola ejecución. 

* Si necesita información sobre el punto de entrada predeterminado establecido en una imagen de contenedor, use el [inspeccionar la imagen de docker](https://docs.docker.com/engine/reference/commandline/image_inspect/) comando.

## <a name="command-line-syntax"></a>Sintaxis de la línea de comandos

La sintaxis de línea de comandos varía dependiendo de la API de Azure o una herramienta que se usa para crear las instancias. Si especifica un entorno de shell, observe también las convenciones de sintaxis de comando del shell.

* [Crear contenedor AZ] [ az-container-create] comando: Pasar una cadena con el `--command-line` parámetro. Ejemplo: `--command-line "python myscript.py arg1 arg2"`).

* [Nuevo-AzureRmContainerGroup] [ new-azurermcontainergroup] cmdlet de PowerShell de Azure: Pasar una cadena con el `-Command` parámetro. Ejemplo: `-Command "echo hello"`.

* Azure Portal: En el **invalidación comando** la propiedad de la configuración del contenedor, proporcionar una lista separada por comas de cadenas sin comillas. Ejemplo: `python, myscript.py, arg1, arg2`). 

* Plantilla de Resource Manager o archivo YAML o uno de los SDK de Azure: Especifique la propiedad de línea de comandos como una matriz de cadenas. Ejemplo: la matriz JSON `["python", "myscript.py", "arg1", "arg2"]` en una plantilla de Resource Manager. 

  Si está familiarizado con [Dockerfile](https://docs.docker.com/engine/reference/builder/) sintaxis, este formato es similar a la *exec* formato de la instrucción CMD.

### <a name="examples"></a>Ejemplos

|    |  Azure CLI   | Portal | Plantilla | 
| ---- | ---- | --- | --- |
| Único comando | `--command-line "python myscript.py arg1 arg2"` | **Invalidación del comando**: `python, myscript.py, arg1, arg2` | `"command": ["python", "myscript.py", "arg1", "arg2"]` |
| Varios comandos | `--command-line "/bin/bash -c 'mkdir test; touch test/myfile; tail -f /dev/null'"` |**Invalidación del comando**: `/bin/bash, -c, mkdir test; touch test/myfile; tail -f /dev/null` | `"command": ["/bin/bash", "-c", "mkdir test; touch test/myfile; tail -f /dev/null"]` |

## <a name="azure-cli-example"></a>Ejemplo de la CLI de Azure

Por ejemplo, modificar el comportamiento de la [microsoft/aci-wordcount] [ aci-wordcount] imagen de contenedor, que analiza el texto de Shakespeare *Hamlet* para encontrar con más frecuencia las repeticiones de palabras. En lugar de analizar *Hamlet*, puede establecer una línea de comandos que señala a un origen de texto diferente.

Para ver el resultado de la [microsoft/aci-wordcount] [ aci-wordcount] contenedor cuando analiza el texto predeterminado, ejecútelo con el siguiente [crear contenedor az] [ az-container-create] comando. No se especifica ninguna línea de comandos de inicio, por lo que se ejecuta el comando del contenedor predeterminado. Por motivos ilustrativos, este ejemplo se establece [variables de entorno](container-instances-environment-variables.md) para buscar las palabras 3 principales que son menos a cinco letras largo:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --environment-variables NumWords=3 MinLength=5 \
    --restart-policy OnFailure
```

Una vez que se muestra el estado del contenedor como *Terminated* (usar [show de contenedor az] [ az-container-show] para comprobar el estado), mostrar el registro con [registros del contenedor az] [ az-container-logs] para ver el resultado.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
```

Salida:

```console
[('HAMLET', 386), ('HORATIO', 127), ('CLAUDIUS', 120)]
```

Ahora configure un segundo contenedor de ejemplo para analizar texto diferente mediante la especificación de una línea de comandos diferente. El script de Python ejecutado por el contenedor, *wordcount.py*, acepta una dirección URL como argumento y procesa el contenido de la página en lugar del predeterminado.

Por ejemplo, para determinar la parte superior 3 palabras que están al menos cinco letras largo de *Romeo y Julieta*:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=3 MinLength=5 \
    --command-line "python wordcount.py http://shakespeare.mit.edu/romeo_juliet/full.html"
```

Una vez más, cuando el contenedor muestra el estado *Terminado*, puede ver la salida en los registros del contenedor:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

Salida:

```console
[('ROMEO', 177), ('JULIET', 134), ('CAPULET', 119)]
```

## <a name="next-steps"></a>Pasos siguientes

Escenarios basados en tareas, como un conjunto de datos grande con varios contenedores, de procesamiento por lotes pueden beneficiarse de las líneas de comandos personalizadas en tiempo de ejecución. Para obtener más información sobre la ejecución de contenedores basados en tareas, consulte [ejecutar tareas en contenedores con las directivas de reinicio](container-instances-restart-policy.md).

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com
