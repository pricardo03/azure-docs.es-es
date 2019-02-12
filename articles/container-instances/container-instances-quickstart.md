---
title: 'Guía de inicio rápido: Ejecución de una aplicación en Azure Container Instances: CLI'
description: En esta guía de inicio rápido, va a usar la CLI de Azure para implementar una aplicación de contenedor de Docker que se ejecutará en un contenedor aislado de Azure Container Instances
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 93a41610035d91774256410cea6af1d06b085d30
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55562069"
---
# <a name="quickstart-run-a-container-application-in-azure-container-instances-with-the-azure-cli"></a>Guía de inicio rápido: Ejecución de una aplicación de contenedor en Azure Container Instances con la CLI de Azure

Use Azure Container Instances para ejecutar contenedores de Docker en Azure con sencillez y velocidad. No es necesario implementar máquinas virtuales ni usar una plataforma de orquestación de contenedores completa como Kubernetes. En esta guía de inicio rápido, va a usar la CLI de Azure para crear un contenedor en Azure y hacer que su aplicación esté disponible con un nombre de dominio completo (FQDN). Unos pocos segundos después de ejecutar un comando de implementación único, puede ir a la aplicación en ejecución:

![Aplicación implementada en Azure Container Instances vista en el explorador][aci-app-browser]

Si no tiene ninguna suscripción a Azure, cree una [cuenta gratuita][azure-account] antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para completar esta guía de inicio rápido. Si desea usarlos de forma local, necesita la versión 2.0.27 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Las instancias de contenedores de Azure, al igual que otros recursos de Azure, se deben implementar en un grupo de recursos. Los grupos de recursos le permiten organizar y administrar los recursos relacionados de Azure.

Primero, cree un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus* con el siguiente comando [az group create][az-group-create]:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Crear un contenedor

Ahora que tiene un grupo de recursos, puede ejecutar un contenedor en Azure. Para crear una instancia de contenedor con la CLI de Azure, proporcione un nombre de grupo de recursos, un nombre de instancia de contenedor y una imagen de contenedor de Docker para el comando [az container create][az-container-create]. En este inicio rápido, usará la imagen de `microsoft/aci-helloworld` del registro público de Docker Hub. Esta imagen empaqueta una pequeña aplicación web escrita en Node.js que sirve una página HTML estática.

Puede exponer los contenedores en Internet mediante la especificación para que se abran uno o varios puertos, o la especificación de una etiqueta de nombre DNS o ambas. En este inicio rápido, se implementa un contenedor con una etiqueta de nombre DNS para que la aplicación web sea públicamente accesible.

Ejecute el siguiente comando para iniciar una instancia de contenedor. El valor `--dns-name-label` debe ser único dentro de la región de Azure en la que cree la instancia. Si recibe un mensaje de error "DNS name label not available" (La etiqueta de nombre DNS no está disponible), pruebe otra etiqueta de nombre DNS diferente.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --dns-name-label aci-demo --ports 80
```

En unos segundos, debería recibir una respuesta de la CLI de Azure que indica que la implementación ha finalizado. Compruebe su estado con el comando [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Al ejecutar el comando, se muestra el nombre completo del dominio (FQDN) y su estado de aprovisionamiento.

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Si el estado `ProvisioningState` del contenedor es **Correcto**, vaya a su FQDN en el explorador. Si ve una página web parecida a la siguiente, enhorabuena. Ha implementado correctamente una aplicación que se ejecuta en un contenedor de Docker en Azure.

![Captura de pantalla del explorador que muestra una aplicación en ejecución en una instancia de contenedor de Azure][aci-app-browser]

Si al principio no aparece la aplicación, espere unos segundos mientras se propaga el DNS y, a continuación, pruebe a actualizar el explorador.

## <a name="pull-the-container-logs"></a>Extracción de los registros del contenedor

Cuando necesite solucionar problemas de un contenedor o de la aplicación que este ejecuta (o simplemente ver la salida), empiece por ver los registros de la instancia de contenedor.

Extraiga los registros de la instancia de contenedor con el comando [az container logs][az-container-logs]:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

En la salida se muestran los registros del contenedor y se mostrarán las solicitudes HTTP GET generadas al ver la aplicación en el explorador.

```console
$ az container logs --resource-group myResourceGroup --name mycontainer
listening on port 80
::ffff:10.240.255.105 - - [01/Oct/2018:18:25:51 +0000] "GET / HTTP/1.0" 200 1663 "-" "-"
::ffff:10.240.255.106 - - [01/Oct/2018:18:31:04 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36"
::ffff:10.240.255.106 - - [01/Oct/2018:18:31:04 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36"
```

## <a name="attach-output-streams"></a>Flujos de salida como adjuntos

Además de ver los registros, puede asociar la salida estándar local y los flujos de error estándar a los del contenedor.

En primer lugar, ejecute el comando [az container attach][az-container-attach] para asociar la consola local a los flujos de salida del contenedor:

```azurecli-interactive
az container attach --resource-group myResourceGroup -n mycontainer
```

Una vez asociados, actualice el explorador varias veces para generar algunas salidas adicionales. Cuando haya terminado, desconecte la consola con `Control+C`. Debería ver una salida similar a la siguiente:

```console
$ az container attach --resource-group myResourceGroup -n mycontainer
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-15 21:17:59+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Created container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45
(count: 1) (last timestamp: 2018-03-15 21:18:06+00:00) Started container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45

Start streaming logs:
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:44 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:47 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con el contenedor, puede eliminarlo con el comando [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Para comprobar que se ha eliminado el contenedor, ejecute el comando [az container list](/cli/azure/container#az-container-list):

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

El contenedor **mycontainer** no debe aparecer en la salida del comando. Si no cuenta con ningún otro contenedor en el grupo de recursos, no se muestra ninguna salida.

Si ha terminado con el grupo de recursos *myResourceGroup* y todos los recursos que contiene, elimínelo con el comando [az group delete][az-group-delete]:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado una instancia de contenedor de Azure mediante una imagen en un repositorio público de Docker Hub. Si quiere compilar una imagen de contenedor e implementarla desde un registro de contenedor privado de Azure, vaya al tutorial de Azure Container Instances.

> [!div class="nextstepaction"]
> [Tutorial de Azure Container Instances](./container-instances-tutorial-prepare-app.md)

Para probar opciones para ejecutar contenedores en un sistema de orquestación en Azure, consulte los inicios rápidos de [Azure Kubernetes Service (AKS)][container-service].

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/
[node-js]: http://nodejs.org

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-container-list]: /cli/azure/container#az-container-list
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
