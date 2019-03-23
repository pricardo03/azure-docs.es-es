---
title: Implementación de grupos de varios contenedores en Azure Container Instances con la CLI de Azure y YAML
description: Obtenga información sobre cómo implementar un grupo de contenedores con varios contenedores en Azure Container Instances mediante la CLI de Azure y un archivo YAML.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/21/2019
ms.author: danlep
ms.openlocfilehash: 10f2340bd85da3dabcd50d51a4dd56d58d31675b
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372444"
---
# <a name="deploy-a-multi-container-container-group-with-yaml"></a>Implementación de un grupo de contenedores con varios contenedores mediante YAML

Azure Container Instances admite la implementación de varios contenedores en un solo host mediante un [grupo de contenedores](container-instances-container-groups.md). Los grupos de contenedores con varios contenedores son útiles cuando se crea un sidecar de aplicación para el registro, la supervisión o cualquier otra configuración donde un servicio necesite un segundo proceso asociado.

Hay dos métodos para implementar grupos con varios contenedores mediante la CLI de Azure:

* Implementación del archivo YAML (este artículo)
* [Implementación de la plantilla de Resource Manager](container-instances-multi-container-group.md)

Dada la naturaleza más concisa del formato YAML, la implementación con un archivo YAML se recomienda cuando la implementación incluye *solo* instancias de contenedor. Si necesita implementar más recursos de servicio de Azure (por ejemplo, un recurso compartido de Azure Files) en el momento de la implementación de una instancia de contenedor, se recomienda implementar la plantilla de Resource Manager.

> [!NOTE]
> Los grupos de varios contenedores están restringidos actualmente a los contenedores Linux. Aunque estamos trabajando para traer todas las características a los contenedores Windows, puede encontrar diferencias en la plataforma actual en la [disponibilidad de cuotas y regiones de Azure Container Instances](container-instances-quotas.md).

## <a name="configure-the-yaml-file"></a>Configuración del archivo YAML

Para implementar un grupo de contenedores con varios contenedores con el comando [az container create][az-container-create] en la CLI de Azure, debe especificar la configuración del grupo de contenedores en un archivo YAML y, a continuación, pasar el archivo YAML como un parámetro al comando.

Para empezar, copie el siguiente archivo YAML en un nuevo archivo denominado **deploy-aci.yaml**.

En este archivo YAML, se define un grupo de contenedores denominado "myContainerGroup" con dos contenedores, una dirección IP pública y dos puertos expuestos. Los contenedores se implementan desde imágenes públicas de Microsoft. El primer contenedor del grupo ejecuta una aplicación web accesible desde Internet. El segundo contenedor, el sidecar, realiza solicitudes HTTP periódicamente a la aplicación web que se ejecuta en el primer contenedor a través de la red local del grupo de contenedores.

```YAML
apiVersion: 2018-10-01
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
      - port: 8080
  - name: aci-tutorial-sidecar
    properties:
      image: mcr.microsoft.com/azuredocs/aci-tutorial-sidecar
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
    - protocol: tcp
      port: '8080'
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

## <a name="deploy-the-container-group"></a>Implementación del grupo de contenedores

Cree un grupo de recursos con el comando [az group create][az-group-create]:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implemente el grupo de contenedores con el comando [az container create][az-container-create] y pase el archivo YAML como un argumento:

```azurecli-interactive
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```

Al cabo de unos segundos, debe recibir una respuesta inicial de Azure.

## <a name="view-deployment-state"></a>Visualización del estado de la implementación

Para ver el estado de la implementación, use el siguiente comando [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Si desea ver la aplicación en ejecución, vaya a su dirección IP en el explorador. Por ejemplo, la dirección IP es `52.168.26.124` en esta salida de ejemplo:

```bash
Name              ResourceGroup    Status    Image                                                                                               IP:ports              Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  --------------------------------------------------------------------------------------------------  --------------------  ---------  ---------------  --------  ----------
myContainerGroup  danlep0318r      Running   mcr.microsoft.com/azuredocs/aci-tutorial-sidecar,mcr.microsoft.com/azuredocs/aci-helloworld:latest  20.42.26.114:80,8080  Public     1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-logs"></a>Ver registros

Visualice la salida del registro de un contenedor con el comando [az container logs][az-container-logs]. El argumento `--container-name` especifica el contenedor del que se van a extraer registros. En este ejemplo, se especifica el primer contenedor.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Salida:

```console
listening on port 80
::1 - - [21/Mar/2019:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Para ver los registros para el contenedor sidecar, ejecute el mismo comando especificando el segundo nombre del contenedor.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Salida:

```console
Every 3s: curl -I http://localhost                          2019-03-21 20:36:41

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Thu, 21 Mar 2019 20:36:41 GMT
Connection: keep-alive
```

Como puede ver, el sidecar realiza periódicamente una solicitud HTTP a la aplicación web principal a través de la red local del grupo para asegurarse de que se está ejecutando. Este ejemplo de sidecar podría ampliarse para desencadenar una alerta si recibe un código de respuesta HTTP distinto de 200 OK.

## <a name="deploy-from-private-registry"></a>Implementación desde el registro privado

Para usar un registro de imagen de contenedor privado, incluya el siguiente archivo YAML con los valores modificados para su entorno:

```YAML
  imageRegistryCredentials:
  - server: imageRegistryLoginServer
    username: imageRegistryUsername
    password: imageRegistryPassword
```

Por ejemplo, el archivo YAML siguiente implementa un grupo de contenedores con un único contenedor cuya imagen se extrajo de un registro privado de Azure Container Registry denominado "myregistry":

```YAML
apiVersion: 2018-10-01
location: eastus
name: myContainerGroup2
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: myregistry.azurecr.io/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
  imageRegistryCredentials:
  - server: myregistry.azurecr.io
    username: myregistry
    password: REGISTRY_PASSWORD
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

## <a name="export-container-group-to-yaml"></a>Exportación del grupo de contenedores a YAML

Puede exportar la configuración de un grupo de contenedores existente a un archivo YAML mediante el comando de la CLI de Azure [az container export][az-container-export].

La exportación, que resulta útil para conservar la configuración de un grupo de contenedores, permite almacenar las configuraciones de grupos de contenedores en el control de versiones para la "configuración como código". O bien, use el archivo exportado como punto de partida al desarrollar una nueva configuración en YAML.

Exporte la configuración del grupo de contenedores que creó anteriormente mediante la emisión del siguiente comando [az container export][az-container-export]:

```azurecli-interactive
az container export --resource-group myResourceGroup --name myContainerGroup --file deployed-aci.yaml
```

Si el comando es correcto, no se muestra ninguna salida, pero puede ver el contenido del archivo para ver el resultado. Por ejemplo, las primeras líneas con `head`:

```console
$ head deployed-aci.yaml
additional_properties: {}
apiVersion: '2018-06-01'
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se explican los pasos necesarios para implementar una instancia de contenedor de Azure de varios contenedores. Para obtener una experiencia integral de Azure Container Instances, incluido el uso de un registro de Azure privado, consulte el tutorial de Azure Container Instances.

> [!div class="nextstepaction"]
> [Tutorial de Azure Container Instances][aci-tutorial]

<!-- LINKS - External -->
[cli-issue-6525]: https://github.com/Azure/azure-cli/issues/6525

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-create]: /cli/azure/container#az-container-create
[az-container-export]: /cli/azure/container#az-container-export
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
