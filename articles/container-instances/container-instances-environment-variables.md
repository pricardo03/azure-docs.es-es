---
title: Establecimiento de variables de entorno en Azure Container Instances
description: Aprenda a establecer variables de entorno en los contenedores que ejecuta en Azure Container Instances.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/07/2018
ms.author: marsma
ms.openlocfilehash: bc30352f50344031f8356d2be1b800dd035f12ad
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830469"
---
# <a name="set-environment-variables"></a>Establecimiento de variables de entorno

El establecimiento de variables de entorno en las instancias de contenedor le permite proporcionar configuración dinámica de la aplicación o el script ejecutados por el contenedor. Para establecer las variables de entorno en un contenedor, debe especificarlas al crear una instancia del contenedor. Se pueden establecer variables de entorno cuando se inicia un contenedor con la [CLI de Azure](#azure-cli-example), [Azure PowerShell](#azure-powershell-example) y [Azure Portal](#azure-portal-example).

Por ejemplo, si ejecuta la imagen de contenedor [microsoft/aci-wordcount][aci-wordcount], puede modificar el comportamiento de la misma mediante la especificación de las siguientes variables de entorno:

*NumWords*: el número de palabras enviadas a STDOUT.

*MinLength*: el número mínimo de caracteres en una palabra para que se tenga en cuenta. Un número mayor omite palabras comunes como "de" y "la".

Si necesita pasar información confidencial como las variables de entorno, Azure Container Instances permite [proteger valores](#secure-values) para los contenedores Windows y Linux.

## <a name="azure-cli-example"></a>Ejemplo de la CLI de Azure

Para ver la salida predeterminada del contenedor [microsoft/aci-wordcount][aci-wordcount], ejecútelo en primer lugar con el comando [az container create][az-container-create] (sin variables de entorno especificadas):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Para modificar la salida, inicie un segundo contenedor con el argumento `--environment-variables` agregado, especificando valores para las variables *NumWords* y *MinLength*:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
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

En primer lugar, inicie el contenedor [microsoft/aci-wordcount][aci-wordcount] en su configuración predeterminada con el comando [New-AzureRmContainerGroup][new-azurermcontainergroup]:

```azurepowershell-interactive
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image microsoft/aci-wordcount:latest
```

Ahora ejecute el siguiente comando [New-AzureRmContainerGroup][new-azurermcontainergroup]. Este especifica las variables de entorno *NumWords* y *MinLength* después de rellenar una variable de matriz, `envVars`:

```azurepowershell-interactive
$envVars = @{NumWords=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Una vez que el estado de ambos contenedores es *Finalizado* (use [Get-AzureRmContainerInstanceLog][azure-instance-log] para comprobar dicho estado), extraiga sus registros con el comando [Get-AzureRmContainerInstanceLog][azure-instance-log].

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

La salida de cada contenedor muestra cómo ha modificado la ejecución del script mediante el contenedor estableciendo las variables de entorno.

```console
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
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
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## <a name="azure-portal-example"></a>Ejemplo de Azure Portal

Para establecer las variables de entorno cuando inicia un contenedor en Azure Portal, especifíquelas en la página **Configuración** cuando crea el contenedor.

Cuando se implementa con el portal, actualmente está limitado a tres variables y debe especificarla con este formato: `"variableName":"value"`.

Para ver un ejemplo, inicie el contenedor [microsoft/aci-wordcount][aci-wordcount] con las variables *NumWords* y *MinLength*.

1. En **Configuración**, establezca **Directiva de reinicio** en *En caso de error*.
2. Escriba `"NumWords":"5"` para la primera variable, seleccione **Sí** en **Add additional environment variables** (Agregar más variables de entorno) y escriba `"MinLength":"8"` para la segunda variable. Seleccione **Aceptar** para comprobar y luego implemente el contenedor.

![Página del portal que muestra el botón Habilitar de la entorno variable y cuadros de texto][portal-env-vars-01]

Para ver los registros del contenedor, en **CONFIGURACIÓN**, seleccione **Contenedores** y luego **Registros**. De forma similar a la salida que se muestra en las secciones de la CLI y PowerShell anteriores, puede ver cómo se ha modificado el comportamiento del script mediante las variables de entorno. Se muestran solo cinco palabras, cada una con una longitud mínima de ocho caracteres.

![Portal que muestra la salida de los registros de contenedor][portal-env-vars-02]

## <a name="secure-values"></a>Protección de valores
Los objetos con valores seguros están diseñados para contener información confidencial, como contraseñas o claves de la aplicación. Utilizar valores seguros para las variables de entorno es más confiable y más flexible que incluirlas en la imagen de su contenedor. Otra opción consiste en utilizar volúmenes secretos, según se describe en [Montaje de un volumen secreto en Azure Container Instances](container-instances-volume-secret.md).

La protección de las variables de entorno con valores seguros no revelará el valor seguro en las propiedades de su contenedor, por lo que solo se puede acceder al valor desde dentro del contenedor. Por ejemplo, las propiedades del contenedor visualizadas en Azure Portal o con la CLI de Azure no mostrarán una variable de entorno con un valor seguro.

Establezca una variable de entorno segura especificando la propiedad `secureValue` en lugar del valor `value` habitual para el tipo de variable. Las dos variables definidas en el siguiente YAML demuestran los dos tipos de variable.

### <a name="yaml-deployment"></a>Implementación de YAML

Cree un archivo `secure-env.yaml` con el siguiente fragmento de código.

```yaml
apiVersion: 2018-06-01
location: westus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - "name": "SECRET"
          "secureValue": "my-secret-value"
        - "name": "NOTSECRET"
          "value": "my-exposed-value"
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

Ejecute el siguiente comando para implementar el grupo de contenedores con YAML.

```azurecli-interactive
az container create --resource-group myRG --name securetest -f secure-env.yaml
```

### <a name="verify-environment-variables"></a>Verificación de las variables de entorno

Ejecute el comando siguiente para consultar las variables de entorno de su contenedor.

```azurecli-interactive
az container show --resource-group myRG --name securetest --query 'containers[].environmentVariables`
```

La respuesta JSON con los detalles de este contenedor mostrará solo la variable de entorno no segura y la clave de la variable de entorno segura.

```json
  "environmentVariables": [
    {
      "name": "NOTSECRET",
      "value": "my-exposed-value"
    },
    {
      "name": "SECRET"
    }
```

Puede revisar si la variable de entorno segura tiene establecido el comando `exec`, que permite ejecutar un comando desde dentro de un contenedor en ejecución. 

Ejecute el siguiente comando para iniciar una sesión de Bash interactiva con el contenedor.
```azurecli-interactive
az container exec --resource-group myRG --name securetest --exec-command "/bin/bash"
```

Desde dentro del contenedor, imprima la variable de entorno con el siguiente comando de Bash.
```bash
echo $SECRET
```

## <a name="next-steps"></a>Pasos siguientes

Los escenarios basados en tareas, como el procesamiento por lotes de un gran conjunto de datos con varios contenedores, pueden aprovechar las variables de entorno personalizadas en tiempo de ejecución. Para más información sobre cómo ejecutar contenedores basados en tareas, consulte [Ejecución de tareas en contenedores en Azure Container Instances](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com
