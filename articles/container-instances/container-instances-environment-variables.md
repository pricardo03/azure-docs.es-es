---
title: Establecimiento de variables de entorno en Azure Container Instances
description: Aprenda a establecer variables de entorno en los contenedores que ejecuta en Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/17/2019
ms.author: danlep
ms.openlocfilehash: 4a4b19338d96094f28b4f4bedd8042723f67f10a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59994782"
---
# <a name="set-environment-variables-in-container-instances"></a>Establecer variables de entorno en las instancias de contenedor

El establecimiento de variables de entorno en las instancias de contenedor le permite proporcionar configuración dinámica de la aplicación o el script ejecutados por el contenedor. Esto es casi lo mismo que el argumento de línea de comandos `--env` para `docker run`. 

Para establecer las variables de entorno en un contenedor, debe especificarlas al crear una instancia del contenedor. En este artículo se muestra ejemplos de cómo configurar variables de entorno cuando se inicia un contenedor con el [CLI de Azure](#azure-cli-example), [Azure PowerShell](#azure-powershell-example)y el [portal Azure](#azure-portal-example). 

Por ejemplo, si ejecuta Microsoft [aci-wordcount] [ aci-wordcount] imagen de contenedor, puede modificar su comportamiento mediante la especificación de las variables de entorno siguientes:

*NumWords*: El número de palabras enviadas a STDOUT.

*MinLength*: El número mínimo de caracteres en una palabra para que se tenga en cuenta. Un número mayor omite palabras comunes como "de" y "la".

Si tiene que pasar secretos como variables de entorno, Azure Container Instances admite [valores seguros](#secure-values) tanto en contenedores Windows como Linux.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-cli-example"></a>Ejemplo de la CLI de Azure

Para ver el resultado predeterminado de la [aci-wordcount] [ aci-wordcount] contenedor, ejecútelo en primer lugar con este [crear contenedor az] [ az-container-create] (no de comando variables de entorno especificadas):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure
```

Para modificar la salida, inicie un segundo contenedor con el argumento `--environment-variables` agregado y especifique valores para las variables *NumWords* y *MinLength*. (En este ejemplo, se supone que está ejecutando la CLI en un shell de Bash o en Azure Cloud Shell. Si usa el símbolo del sistema de Windows, especifique las variables con comillas dobles, como `--environment-variables "NumWords"="5" "MinLength"="8"`).

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables 'NumWords'='5' 'MinLength'='8'
```

Una vez que el estado de ambos contenedores se muestre como *Finalizado* (use [az container show][az-container-show] para comprobar el estado), muestre sus registros con [az container logs][az-container-logs] para ver la salida.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

La salida de los contenedores muestra cómo ha modificado el comportamiento de script del segundo contenedor estableciendo las variables de entorno.

```console
azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="azure-powershell-example"></a>Ejemplo de Azure PowerShell

El establecimiento de las variables de entorno en PowerShell es similar a la CLI, pero usa el argumento de línea de comandos `-EnvironmentVariable`.

En primer lugar, inicie la [aci-wordcount] [ aci-wordcount] contenedor en su configuración predeterminada con este [New AzContainerGroup] [ new-Azcontainergroup] comando:

```azurepowershell-interactive
New-AzContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image mcr.microsoft.com/azuredocs/aci-wordcount:latest
```

Ahora ejecute el siguiente [New AzContainerGroup] [ new-Azcontainergroup] comando. Este especifica las variables de entorno *NumWords* y *MinLength* después de rellenar una variable de matriz, `envVars`:

```azurepowershell-interactive
$envVars = @{'NumWords'='5';'MinLength'='8'}
New-AzContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image mcr.microsoft.com/azuredocs/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Una vez que el estado de ambos contenedores es *Terminated* (usar [Get AzContainerInstanceLog] [ azure-instance-log] para comprobar el estado), extraer sus registros con el [ Get-AzContainerInstanceLog] [ azure-instance-log] comando.

```azurepowershell-interactive
Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

La salida de cada contenedor muestra cómo ha modificado la ejecución del script mediante el contenedor estableciendo las variables de entorno.

```console
PS Azure:\> Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

Azure:\
PS Azure:\> Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## <a name="azure-portal-example"></a>Ejemplo de Azure Portal

Para establecer las variables de entorno cuando se inicia un contenedor en Azure portal, puede especificarlas en el **avanzadas** página cuando se crea el contenedor.

1. En el **avanzadas** , establezca el **directiva de reinicio** a *en caso de error*
2. En **variables de entorno**, escriba `NumWords` con un valor de `5` para la primera variable y escriba `MinLength` con un valor de `8` para la segunda variable. 
1. Seleccione **revisión + crear** para comprobar y, a continuación, implementar el contenedor.

![Página del portal que muestra el botón Habilitar de la entorno variable y cuadros de texto][portal-env-vars-01]

Para ver los registros del contenedor, en **configuración** seleccione **contenedores**, a continuación, **registros**. De forma similar a la salida que se muestra en las secciones de la CLI y PowerShell anteriores, puede ver cómo se ha modificado el comportamiento del script mediante las variables de entorno. Se muestran solo cinco palabras, cada una con una longitud mínima de ocho caracteres.

![Portal que muestra la salida de los registros de contenedor][portal-env-vars-02]

## <a name="secure-values"></a>Protección de valores

Los objetos con valores seguros están diseñados para contener información confidencial, como contraseñas o claves de la aplicación. Utilizar valores seguros para las variables de entorno es más confiable y más flexible que incluirlas en la imagen de su contenedor. Otra opción consiste en utilizar volúmenes secretos, según se describe en [Montaje de un volumen secreto en Azure Container Instances](container-instances-volume-secret.md).

Las variables de entorno con valores seguros no son visibles en las propiedades del contenedor, solo se tiene acceso a los valores desde dentro del contenedor. Por ejemplo, las propiedades del contenedor que se visualizan en Azure Portal o con la CLI de Azure solo muestran el nombre de una variable segura, no su valor.

Establezca una variable de entorno segura especificando la propiedad `secureValue` en lugar del valor `value` habitual para el tipo de variable. Las dos variables definidas en el siguiente YAML demuestran los dos tipos de variable.

### <a name="yaml-deployment"></a>Implementación de YAML

Cree un archivo `secure-env.yaml` con el siguiente fragmento de código.

```yaml
apiVersion: 2018-10-01
location: eastus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - name: 'NOTSECRET'
          value: 'my-exposed-value'
        - name: 'SECRET'
          secureValue: 'my-secret-value'
      image: nginx
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Ejecute el siguiente comando para implementar el grupo de contenedores con YAML (ajuste el nombre del grupo de recursos según sea necesario):

```azurecli-interactive
az container create --resource-group myResourceGroup --file secure-env.yaml
```

### <a name="verify-environment-variables"></a>Verificación de las variables de entorno

Ejecute el comando [az container show][az-container-show] para consultar las variables de entorno del contenedor:

```azurecli-interactive
az container show --resource-group myResourceGroup --name securetest --query 'containers[].environmentVariables'
```

La respuesta JSON muestra la clave y el valor de la variable de entorno no segura, sino únicamente el nombre de la variable de entorno segura:

```json
[
  [
    {
      "name": "NOTSECRET",
      "secureValue": null,
      "value": "my-exposed-value"
    },
    {
      "name": "SECRET",
      "secureValue": null,
      "value": null
    }
  ]
]
```

Puede comprobar que se ha establecido la variable de entorno segura con el comando [az container exec][az-container-exec], que permite ejecutar un comando en un contenedor en ejecución. Ejecute el siguiente comando para iniciar una sesión de Bash interactiva en el contenedor:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name securetest --exec-command "/bin/bash"
```

Cuando haya abierto un shell interactivo dentro del contenedor, tendrá acceso al valor de la variable `SECRET`:

```console
root@caas-ef3ee231482549629ac8a40c0d3807fd-3881559887-5374l:/# echo $SECRET
my-secret-value
```

## <a name="next-steps"></a>Pasos siguientes

Los escenarios basados en tareas, como el procesamiento por lotes de un gran conjunto de datos con varios contenedores, pueden aprovechar las variables de entorno personalizadas en tiempo de ejecución. Para obtener más información sobre la ejecución de contenedores basados en tareas, consulte [ejecutar tareas en contenedores con las directivas de reinicio](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/az.containerinstance/get-azcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-Az-ps
[new-Azcontainergroup]: /powershell/module/az.containerinstance/new-azcontainergroup
[portal]: https://portal.azure.com
